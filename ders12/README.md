# Ders 12: Go’da İleri Desenler (Advanced Patterns in Go)

Bu derste, Go programlama dilinde ileri seviye desenleri inceleyeceğiz. Bu desenler, etkili, eşzamanlı ve bakımı kolay kodlar yazmanıza yardımcı olacaktır. Ele alınacak konular şunlardır:

1.	Goroutine’lerin Temelleri
2.	Goroutine Havuzları
3.	Goroutine İletişimi ve Senkronizasyonu
4.	Goroutine Sızıntıları ve Yönetimi
5.	Gerçek Dünya Uygulamaları
6.	Bağlam Yönetimi (Context Management)
7.	Fan-out/Fan-in Desenleri
8.	Boru Hattı Desenleri (Pipeline Patterns)
9.	Mutex Desenleri
10.	İşçi Havuz Desenleri (Worker Pool Patterns)
11.	Semafor Desenleri
12.	Bariyer Desenleri
13.	WaitGroup Desenleri


## 1. Goroutine’lerin Temelleri

Goroutine’ler, Go dilinde hafif iş parçacıklarıdır ve go anahtar kelimesi ile başlatılırlar. Örneğin:

```go
package main

import (
    "fmt"
    "time"
)

func sayHello() {
    fmt.Println("Merhaba!")
}

func main() {
    go sayHello()
    time.Sleep(1 * time.Second)
}
```

Yukarıdaki örnekte, `sayHello` fonksiyonu bir goroutine olarak başlatılır ve ana program 1 saniye uyutulur, böylece goroutine’in çalışması için zaman tanınır.

## 2. Goroutine Havuzları

Goroutine havuzları, belirli sayıda goroutine’in görevleri işlemek için yeniden kullanıldığı bir desendir. Bu, kaynak kullanımını optimize eder. Örneğin:

```go
package main

import (
    "fmt"
    "sync"
)

func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Printf("Worker %d started\n", id)
    // İşlem yapılacak kod
    fmt.Printf("Worker %d finished\n", id)
}

func main() {
    var wg sync.WaitGroup
    for i := 1; i <= 5; i++ {
        wg.Add(1)
        go worker(i, &wg)
    }
    wg.Wait()
}
```

Yukarıdaki örnekte, `worker` fonksiyonu bir goroutine olarak başlatılır ve `sync.WaitGroup` kullanılarak goroutine’lerin bitişini beklenir.


## 3.Goroutine İletişimi ve Senkronizasyonu

Goroutine’ler arasında iletişim ve senkronizasyon için kanallar kullanılır. Örneğin:

```go

package main

import "fmt"

func main() {
    messages := make(chan string)

    go func() {
        messages <- "Merhaba, Dünya!"
    }()

    msg := <-messages
    fmt.Println(msg)
}
```

Yukarıdaki örnekte, bir kanal oluşturulur ve bir goroutine içinde bu kanala bir mesaj gönderilir. Ana programda ise bu kanaldan mesaj alınır ve ekrana yazdırılır.


## 4. Goroutine Sızıntıları ve Yönetimi

Goroutine sızıntıları, başlatılan ancak asla sonlanmayan goroutine’lerdir. Bu, bellek sızıntılarına yol açabilir. Goroutine’leri dikkatli yönetmek önemlidir. Örneğin:

```go

package main

import (
    "context"
    "fmt"
    "time"
)

func main() {
    ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
    defer cancel()

    go func() {
        select {
        case <-ctx.Done():
            fmt.Println("Goroutine sonlandı")
        }
    }()

    time.Sleep(3 * time.Second)
}

```

Yukarıdaki örnekte, bir bağlam (context) oluşturulur ve bir goroutine içinde bu bağlamın bitişini beklenir. Ana programda ise 3 saniye uyutulur ve goroutine’in bitişini beklenir.

Eğerki aşağıdaki gibi bir kod yazarsak goroutine sızıntısı oluşurdu çünkü goroutine bitmediği için program sonlanmazdı.

```go

package main

import (
    "fmt"
    "time"
)

func main() {
    go func() {
        for {
            fmt.Println("Çalışıyor...")
            time.Sleep(1 * time.Second)
        }
    }()

    time.Sleep(3 * time.Second)
}

```

## 5. Gerçek Dünya Uygulamaları

Goroutine’ler, web sunucuları, veri işleme boru hatları ve daha birçok uygulamada kullanılır. Örneğin, bir web sunucusunda her gelen istek için bir goroutine başlatılabilir.

```go
package main

import (
    "fmt"
    "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Merhaba, Dünya!")
}

func main() {
    http.HandleFunc("/", handler)
    http.ListenAndServe(":8080", nil)
}
```

Bu örnekte, bir HTTP sunucusu oluşturulur ve her gelen istek için bir goroutine başlatılır.

## 6. Bağlam Yönetimi (Context Management)

Context, Go’da uzun süren işlemleri yönetmek ve iptal etmek için kullanılır. Context paketini kullanarak, işlemleri iptal edebilir, zaman aşımı belirleyebilir ve işlemler arasında veri aktarabilirsiniz.

```go

package main

import (
    "context"
    "fmt"
    "time"
)

func main() {
    ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
    defer cancel()

    select {
    case <-time.After(1 * time.Second):
        fmt.Println("İşlem tamamlandı")
    case <-ctx.Done():
        fmt.Println("İşlem iptal edildi:", ctx.Err())
    }
}

```

Yukarıdaki örnekte, bir bağlam oluşturulur ve 2 saniye içinde işlem tamamlanır. Eğer işlem 2 saniyeden fazla sürerse, işlem iptal edilir.

## 7. Fan-out/Fan-in Desenleri

Fan-out/fan-in deseni, birçok goroutine’i başlatarak işlemi paralel olarak yürütme ve ardından sonuçları birleştirme işlemidir. Bu desen, işlemi hızlandırmak ve kaynakları etkin bir şekilde kullanmak için kullanılır.

```go

package main

import (
    "fmt"
    "math/rand"
    "sync"
    "time"
)

func producer(ch chan int) {
    defer close(ch)
    for i := 0; i < 5; i++ {
        ch <- rand.Intn(100)
    }
}

func consumer(ch chan int, wg *sync.WaitGroup) {
    defer wg.Done()
    for num := range ch {
        fmt.Println(num)
    }
}

func main() {
    ch := make(chan int)
    var wg sync.WaitGroup

    wg.Add(1)
    go producer(ch)

    for i := 0; i < 3; i++ {
        wg.Add(1)
        go consumer(ch, &wg)
    }

    wg.Wait()
}

```

Yukarıdaki örnekte, bir üretici goroutine ve üç tüketici goroutine başlatılır. Üretici, rastgele sayılar üretir ve tüketici bu sayıları ekrana yazdırır.


## 8. Boru Hattı Desenleri (Pipeline Patterns)

Boru hattı desenleri, bir işlemi birkaç adımda parçalara ayırarak işlemi hızlandırmak ve optimize etmek için kullanılır. Bu desen, veri işleme ve dönüşüm süreçlerinde yaygın olarak kullanılır.

```go

package main

import (
    "fmt"
    "sync"
)

func producer(ch chan int) {
    defer close(ch)
    for i := 0; i < 5; i++ {
        ch <- i
    }
}

func square(ch chan int, wg *sync.WaitGroup) {
    defer wg.Done()
    for num := range ch {
        ch <- num * num
    }
}

func printer(ch chan int) {
    for num := range ch {
        fmt.Println(num)
    }
}

func main() {
    ch := make(chan int)
    var wg sync.WaitGroup

    wg.Add(1)
    go producer(ch)

    for i := 0; i < 3; i++ {
        wg.Add(1)
        go square(ch, &wg)
    }

    go printer(ch)

    wg.Wait()
}

```

Yukarıdaki örnekte, bir üretici goroutine, üç dönüştürücü goroutine ve bir yazıcı goroutine başlatılır. Üretici, sayıları üretir, dönüştürücü sayıları karesine çevirir ve yazıcı sayıları ekrana yazdırır.


## 9. Mutex Desenleri

Mutex desenleri, eşzamanlı erişimi kontrol etmek ve paylaşılan verilere güvenli bir şekilde erişmek için kullanılır. Mutex, bir veri yapısına erişim izni verir ve diğer goroutine’lerin beklemesini sağlar.

```go

package main

import (
    "fmt"
    "sync"
)

func main() {
    var mu sync.Mutex
    var data = make(map[string]string)

    mu.Lock()
    data["key"] = "value"
    mu.Unlock()

    mu.Lock()
    fmt.Println(data["key"])
    mu.Unlock()
}

```

Yukarıdaki örnekte, bir mutex oluşturulur ve bir veri yapısına erişim izni verilir. Mutex, veri yapısına erişim sırasında kilitlenir ve diğer goroutine’lerin beklemesini sağlar.


## 10. İşçi Havuz Desenleri (Worker Pool Patterns)

İşçi havuz desenleri, belirli sayıda işçi goroutine’inin işleri işlemek için kullanıldığı bir desendir. Bu desen, iş yükünü dengelemek ve kaynakları etkin bir şekilde kullanmak için kullanılır.

```go

package main

import (
    "fmt"
    "sync"
)

func worker(id int, jobs <-chan int, results chan<- int) {
    for job := range jobs {
        fmt.Printf("Worker %d started job %d\n", id, job)
        results <- job * 2
        fmt.Printf("Worker %d finished job %d\n", id, job)
    }
}

func main() {
    const numJobs = 5
    const numWorkers = 3

    jobs := make(chan int, numJobs)
    results := make(chan int, numJobs)

    var wg sync.WaitGroup

    for i := 1; i <= numWorkers; i++ {
        wg.Add(1)
        go func(i int) {
            defer wg.Done()
            worker(i, jobs, results)
        }(i)
    }

    for i := 1; i <= numJobs; i++ {
        jobs <- i
    }

    close(jobs)

    wg.Wait()

    for i := 1; i <= numJobs; i++ {
        result := <-results
        fmt.Println(result)
    }
}

```

Yukarıdaki örnekte, işçi havuzu deseni kullanılarak işçi goroutine’leri başlatılır ve iş yükü işlenir. İşçi goroutine’leri, işleri işler ve sonuçları bir kanala yazar.


## 11. Semafor Desenleri

Semafor desenleri, belirli sayıda kaynağa erişimi kontrol etmek ve eşzamanlı erişimi sınırlamak için kullanılır. Bu desen, kaynak kullanımını dengelemek ve aşırı yüklenmeyi önlemek için kullanılır.

```go

package main

import (
    "fmt"
    "sync"
)

func main() {
    const numWorkers = 3
    const numJobs = 5

    sem := make(chan struct{}, numWorkers)
    var wg sync.WaitGroup

    for i := 1; i <= numJobs; i++ {
        sem <- struct{}{}
        wg.Add(1)
        go func(i int) {
            defer func() { <-sem }()
            defer wg.Done()
            fmt.Printf("Worker %d started\n", i)
            fmt.Printf("Worker %d finished\n", i)
        }(i)
    }

    wg.Wait()
}

```

Yukarıdaki örnekte, bir semafor oluşturulur ve belirli sayıda işçi goroutine başlatılır. Semafor, belirli sayıda kaynağa erişimi kontrol eder ve eşzamanlı erişimi sınırlar.


## 12. Bariyer Desenleri

Bariyer desenleri, belirli sayıda goroutine’in belirli bir noktada beklemesini sağlar. Bu desen, eşzamanlı işlemleri senkronize etmek ve sonuçları birleştirmek için kullanılır.

```go

package main

import (
    "fmt"
    "sync"
)

func main() {
    const numWorkers = 3
    var wg sync.WaitGroup

    for i := 1; i <= numWorkers; i++ {
        wg.Add(1)
        go func(i int) {
            defer wg.Done()
            fmt.Printf("Worker %d started\n", i)
            fmt.Printf("Worker %d finished\n", i)
        }(i)
    }

    wg.Wait()
}

```

Yukarıdaki örnekte, bir bariyer oluşturulur ve belirli sayıda işçi goroutine başlatılır. Bariyer, işçi goroutine’lerin belirli bir noktada beklemesini sağlar.

### Sonraki Ders

[# Ders 13: Reflection ve Dinamik Tipler (Reflection and Dynamic Types)](../ders13/README.md)


