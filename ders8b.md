# Ders 8: Goroutine Advance Patterns

Bu derste, Go dilinde goroutine'lerin daha gelişmiş kullanım şekillerini ve desenlerini inceleyeceğiz. Goroutine'ler, Go dilinin en önemli özelliklerinden biridir ve paralel programlama yapmamızı sağlar. Ancak, goroutine'leri doğru bir şekilde kullanmak ve yönetmek önemlidir. Bu derste, goroutine''lerin daha gelişmiş kullanım şekillerini ve desenlerini öğreneceğiz.

## Patterns

### 1. Fan-Out, Fan-In

Fan-Out, Fan-In deseni, birçok goroutine'i başlatıp, bu goroutine'lerin çalışmasını beklemek ve sonuçlarını birleştirmek için kullanılır. Bu desen, birçok goroutine'i paralel olarak çalıştırmamızı sağlar ve sonuçlarını birleştirmemizi sağlar.

**Nerede Kulalnılır?**: Örnek olarak, birçok veriyi işleyip, sonuçlarını birleştirmemiz gereken durumlarda kullanılabilir.

**Örnek**: Birçok sayıyı toplayıp, sonucu döndüren bir fonksiyon yazalım.

```go
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func sum(a, b int) int {
    time.Sleep(time.Duration(rand.Intn(100)) * time.Millisecond)
    return a + b
}

func main() {
    rand.Seed(time.Now().UnixNano())

    // Fan-Out
    ch := make(chan int)
    for i := 0; i < 10; i++ {
        go func() {
            ch <- sum(rand.Intn(10), rand.Intn(10))
        }()
    }

    // Fan-In
    sum := 0
    for i := 0; i < 10; i++ {
        sum += <-ch
    }

    fmt.Println("Sum:", sum)
}
```

Yukarıdaki örnekte, `sum` fonksiyonu, iki sayıyı toplayıp, sonucu döndürüyor. `main` fonksiyonunda, 10 goroutine başlatıp, bu goroutine'lerin sonuçlarını birleştiriyoruz.

### 2. Pipeline

Pipeline deseni, veriyi adım adım işlemenizi sağlayan bir yapı oluşturur. Her adım bir goroutine tarafından temsil edilir ve bir adımın çıktısı, bir sonraki adımın girdisi olur. Bu sayede, işlemler bir sıraya konulur ve her adım bağımsız bir şekilde çalışır.

**Nerede Kullanılır?**: Bu desen, veri işleme akışlarının birden fazla aşamaya bölünmesi gerektiğinde kullanılır. Örneğin, bir dosyadan veri okuyup, bu veriyi işleyip, sonuçları kaydetmek için kullanılabilir.

Örnek: Sayıları bir dizi halinde işleyip, her sayıyı ikiye katlayan bir pipeline yapalım.

```go
package main

import (
    "fmt"
)

func generator(nums ...int) <-chan int {
    out := make(chan int)
    go func() {
        for _, n := range nums {
            out <- n
        }
        close(out)
    }()
    return out
}

func multiplyByTwo(in <-chan int) <-chan int {
    out := make(chan int)
    go func() {
        for n := range in {
            out <- n * 2
        }
        close(out)
    }()
    return out
}

func main() {
    nums := generator(1, 2, 3, 4, 5)

    result := multiplyByTwo(nums)

    for n := range result {
        fmt.Println(n)
    }
}

```

Yukarıdaki örnekte, `generator` fonksiyonu, veriyi oluşturan bir goroutine'i temsil eder. `multiplyByTwo` fonksiyonu, veriyi işleyen bir goroutine'i temsil eder. `main` fonksiyonunda, bu iki fonksiyonu bir pipeline oluşturacak şekilde kullanıyoruz.

### 3. Worker Pool

Worker Pool deseni, belirli sayıda goroutine'in belirli bir iş yükünü paralel olarak işlemesini sağlar. Bu desen, işlemci kaynaklarını daha verimli kullanmak ve aşırı goroutine başlatılmasını önlemek için kullanılır.

**Nerede Kullanılır?:** Büyük veri setlerini işlerken veya yoğun I/O işlemleri yapılırken kullanılır.

**Örnek:** Bir dizi işin aynı anda çalışan birkaç goroutine tarafından işlenmesini sağlayan bir worker pool oluşturun.

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func worker(id int, jobs <-chan int, results chan<- int, wg *sync.WaitGroup) {
    defer wg.Done()
    for j := range jobs {
        fmt.Printf("Worker %d started job %d\n", id, j)
        time.Sleep(time.Second)
        fmt.Printf("Worker %d finished job %d\n", id, j)
        results <- j * 2
    }
}

func main() {
    jobs := make(chan int, 10)
    results := make(chan int, 10)
    var wg sync.WaitGroup

    for w := 1; w < 4; w++ {
        wg.Add(1)
        go worker(w, jobs, results, &wg)
    }

    for j := 1; j <= 9; j++ {
        jobs <- j
    }
    close(jobs)

    wg.Wait()
    close(results)

    for result := range results {
        fmt.Println("Result:", result)
    }
}
```

Bu örnekte, 3 tane worker aynı anda çalışarak iş listesinde yer alan işleri paralel olarak işler.

### 4. Select with Timeouts

`select` ifadesi, Go dilinde birden fazla kanal işlemine aynı anda beklemeyi sağlar. Bununla birlikte, belirli bir süre sonra `timeout` gerçekleşmesi durumunda işlem yapılmasını sağlayan select with timeouts deseni, eş zamanlı işlemlerinizin tıkanmasını önler.

**Nerede Kullanılır?:** Birden fazla kaynaktan veri beklerken, belirli bir sürede sonuç alınamaması durumunda bir işlem yapmak gerektiğinde kullanılır.

**Örnek:** İki ayrı işlemden veri bekleyip, belirli bir süre içinde veri gelmezse timeout yapalım

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)

    go func() {
        time.Sleep(2 * time.Second)
        ch1 <- "result from ch1"
    }()

    go func() {
        time.Sleep(1 * time.Second)
        ch2 <- "result from ch2"
    }()

    select {
    case res := <-ch1:
        fmt.Println(res)
    case res := <-ch2:
        fmt.Println(res)
    case <-time.After(3 * time.Second):
        fmt.Println("timeout")
    }
}
```

Bu örnekte, iki goroutine ayrı ayrı işlemler yapar ve sonuçları select bloğunda beklenir. Eğer 3 saniye içinde bir sonuç alınmazsa, "timeout" mesajı yazdırılır.

### 5. Cancellation with Context

Context, Go'da goroutine'lerin çalışmasını iptal etmek veya belirli bir durum ortaya çıktığında işlemi durdurmak için kullanılan bir yapıdır. Bu desen, uzun süreli işlemlerde iptal desteği sağlamak için idealdir.

**Nerede Kullanılır?:** Uzun süren işlemleri belirli koşullarda durdurmanız gerektiğinde kullanılır, örneğin, bir HTTP isteğinin süresi dolduğunda.

**Örnek:** Belirli bir süre sonra iptal edilen bir iş akışı oluşturalım.

````go 
package main

import (
    "context"
    "fmt"
    "time"
)

func doWork(ctx context.Context) {
    for {
        select {
        case <-ctx.Done():
            fmt.Println("Work cancelled")
            return
        default:
            fmt.Println("Working...")
            time.Sleep(500 * time.Millisecond)
        }
    }
}

func main() {
    ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
    defer cancel()

    go doWork(ctx)

    time.Sleep(3 * time.Second)
}
````

Bu örnekte, `doWork` fonksiyonu, belirli bir süre boyunca çalışır ve ardından iptal edilir. `main` fonksiyonunda, `doWork` fonksiyonu bir goroutine olarak çalıştırılır ve 3 saniye sonra iptal edilir.

### 6. Bounded Concurrency

Bounded Concurrency deseni, goroutine sayısını sınırlayarak sistem kaynaklarını daha verimli kullanmayı sağlar. Bu desen, aynı anda çalışabilecek goroutine sayısını kontrol altına alarak kaynak tüketimini optimize eder.

**Nerede Kullanılır?:** Sistem kaynaklarının sınırlandırılması gereken yoğun işlemlerde kullanılır.

**Örnek:** Aynı anda çalışan goroutine sayısını sınırlayan bir yapı oluşturalım.

```go

package main

import (
    "fmt"
    "sync"
    "time"
)

func worker(id int, wg *sync.WaitGroup, semaphore chan struct{}) {
    defer wg.Done()
    semaphore <- struct{}{}
    fmt.Printf("Worker %d started\n", id)
    time.Sleep(2 * time.Second)
    fmt.Printf("Worker %d finished\n", id)
    <-semaphore
}

func main() {
    var wg sync.WaitGroup
    semaphore := make(chan struct{}, 3)

    for i := 1; i <= 10; i++ {
        wg.Add(1)
        go worker(i, &wg, semaphore)
    }

    wg.Wait()
}

```

Bu örnekte, 3 tane goroutine aynı anda çalışır ve diğer goroutine'ler sırayla çalışır. Bu sayede, aynı anda çalışan goroutine sayısı sınırlanmış olur.

### 7. Error Handling in Goroutines
Goroutine'ler içinde hata yönetimi, programın sağlamlığı için önemlidir. Bu desen, goroutine'lerin ürettiği hataların uygun bir şekilde işlenmesini sağlar.

**Nerede Kullanılır?**: Paralel işlemlerde ortaya çıkan hataların merkezi bir şekilde yönetilmesi gerektiğinde kullanılır.

**Örnek:** Goroutine'lerin hatalarını toplamak ve işlemek için bir yapı oluşturalım.


```go

package main

import (
    "fmt"
    "sync"
)

func worker(id int, wg *sync.WaitGroup, errChan chan<- error) {
    defer wg.Done()
    if id%2 == 0 {
        errChan <- fmt.Errorf("error in worker %d", id)
    } else {
        fmt.Printf("Worker %d done successfully\n", id)
    }
}

func main() {
    var wg sync.WaitGroup
    errChan := make(chan error, 10)

    for i := 1; i <= 10; i++ {
        wg.Add(1)
        go worker(i, &wg, errChan)
    }

    wg.Wait()
    close(errChan)

    for err := range errChan {
        fmt.Println("Caught error:", err)
    }
}

```

Bu örnekte, hata oluştuğunda bu hatalar errChan kanalına gönderilir ve main fonksiyonunda işlenir.

Bu desenler, Go dilinde paralel programlama yaparken karşılaşabileceğiniz yaygın problemleri çözmek için kullanabileceğiniz güçlü araçlardır. Her bir deseni iyi anlamak, uygulamalarınızın performansını ve güvenilirliğini artıracaktır.
