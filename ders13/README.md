# Advanced Patterns in Go (Go'da İleri Desenler)

Bu ders, Go programlama dilinde ileri seviye desenleri ve bu desenlerin nasıl kullanılacağını kapsamaktadır. Aşağıdaki konular ele alınacaktır:

## Context (Bağlam)

Context, Go'da uzun süren işlemleri yönetmek ve iptal etmek için kullanılır. Context paketini kullanarak, işlemleri iptal edebilir, zaman aşımı belirleyebilir ve işlemler arasında veri aktarabilirsiniz.

### Örnek Kod:
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
        fmt.Println("işlem tamamlandı")
    case <-ctx.Done():
        fmt.Println("işlem iptal edildi:", ctx.Err())
    }
}
```

## Fan-out/Fan-in Patterns (Fan-out/Fan-in Desenleri)

Fan-out, bir işi birden fazla iş parçacığına bölmek anlamına gelirken, Fan-in bu iş parçacıklarının sonuçlarını birleştirmek anlamına gelir.

### Örnek Kod:
```go
package main

import (
    "fmt"
    "sync"
)

func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Printf("Worker %d başladı\n", id)
}

func main() {
    var wg sync.WaitGroup
    for i := 1; i <= 5; i++ {
        wg.Add(1)
        go worker(i, &wg)
    }
    wg.Wait()
    fmt.Println("Tüm işler tamamlandı")
}
```

## Pipeline Patterns (Pipeline Desenleri)

Pipeline deseni, verilerin bir dizi aşamadan geçirilerek işlendiği bir yapıdır.

### Örnek Kod:
```go
package main

import "fmt"

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

func square(in <-chan int) <-chan int {
    out := make(chan int)
    go func() {
        for n := range in {
            out <- n * n
        }
        close(out)
    }()
    return out
}

func main() {
    in := generator(2, 3, 4)
    out := square(in)

    for n := range out {
        fmt.Println(n)
    }
}
```

## Mutex Patterns (Mutex Desenleri)

Mutex, aynı anda birden fazla iş parçacığının bir kaynağa erişimini kontrol etmek için kullanılır.

### Örnek Kod:
```go
package main

import (
    "fmt"
    "sync"
)

var (
    counter int
    mu      sync.Mutex
)

func increment(wg *sync.WaitGroup) {
    defer wg.Done()
    mu.Lock()
    counter++
    mu.Unlock()
}

func main() {
    var wg sync.WaitGroup
    for i := 0; i < 1000; i++ {
        wg.Add(1)
        go increment(&wg)
    }
    wg.Wait()
    fmt.Println("Counter değeri:", counter)
}
```

## Worker Pool Patterns (Worker Havuz Desenleri)

Worker Pool, belirli sayıda iş parçacığının bir iş kuyruğundan görevler aldığı bir yapıdır.

### Örnek Kod:
```go
package main

import (
    "fmt"
    "sync"
)

func worker(id int, jobs <-chan int, wg *sync.WaitGroup) {
    defer wg.Done()
    for j := range jobs {
        fmt.Printf("Worker %d, job %d\n", id, j)
    }
}

func main() {
    jobs := make(chan int, 100)
    var wg sync.WaitGroup

    for w := 1; w <= 3; w++ {
        wg.Add(1)
        go worker(w, jobs, &wg)
    }

    for j := 1; j <= 9; j++ {
        jobs <- j
    }
    close(jobs)

    wg.Wait()
}
```

## Semaphores Patterns (Semafor Desenleri)

Semaforlar, belirli sayıda iş parçacığının bir kaynağa erişimini kontrol etmek için kullanılır.

### Örnek Kod:
```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func main() {
    var sem = make(chan struct{}, 2)
    var wg sync.WaitGroup

    for i := 1; i <= 5; i++ {
        wg.Add(1)
        go func(i int) {
            defer wg.Done()
            sem <- struct{}{}
            fmt.Printf("Goroutine %d başladı\n", i)
            time.Sleep(2 * time.Second)
            fmt.Printf("Goroutine %d bitti\n", i)
            <-sem
        }(i)
    }

    wg.Wait()
}
```

## Barrier Patterns (Bariyer Desenleri)

Barrier, belirli bir sayıda iş parçacığının belirli bir noktaya kadar ilerlemesini ve ardından birlikte devam etmesini sağlar.

### Örnek Kod:
```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var wg sync.WaitGroup
    var mu sync.Mutex
    var count int

    for i := 1; i <= 5; i++ {
        wg.Add(1)
        go func(i int) {
            defer wg.Done()
            fmt.Printf("Goroutine %d başladı\n", i)
            mu.Lock()
            count++
            if count == 5 {
                fmt.Println("Tüm goroutine'ler bariyere ulaştı")
            }
            mu.Unlock()
        }(i)
    }

    wg.Wait()
}
```

## WaitGroup Patterns (WaitGroup Desenleri)

WaitGroup, bir grup goroutine'in tamamlanmasını beklemek için kullanılır.

### Örnek Kod:
```go
package main

import (
    "fmt"
    "sync"
)

func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Printf("Worker %d başladı\n", id)
}

func main() {
    var wg sync.WaitGroup
    for i := 1; i <= 5; i++ {
        wg.Add(1)
        go worker(i, &wg)
    }
    wg.Wait()
    fmt.Println("Tüm işler tamamlandı")
}
```

