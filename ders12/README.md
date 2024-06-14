# Ders 12: Goroutine Advanced Patterns (İleri Goroutine Desenleri)

Bu derste, Go dilinde ileri seviye goroutine desenlerini inceleyeceğiz. Goroutine'ler, Go dilinin eşzamanlılık modelinin temel taşlarıdır ve doğru kullanıldığında çok güçlü olabilirler. Bu derste aşağıdaki konuları ele alacağız:

1. **Goroutine'lerin Temelleri**
2. **Goroutine Havuzları**
3. **Goroutine İletişimi ve Senkronizasyonu**
4. **Goroutine Sızıntıları ve Yönetimi**
5. **Gerçek Dünya Uygulamaları**

## 1. Goroutine'lerin Temelleri

Goroutine'ler, Go dilinde hafif iş parçacıklarıdır. `go` anahtar kelimesi ile başlatılırlar. Örneğin:

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

Yukarıdaki kodda, `sayHello` fonksiyonu bir goroutine olarak başlatılır ve ana program 1 saniye uyutulur, böylece goroutine'in çalışması için zaman tanınır.

## 2. Goroutine Havuzları

Goroutine havuzları, belirli sayıda goroutine'in görevleri işlemek için yeniden kullanıldığı bir desendir. Bu, kaynak kullanımını optimize eder. Örneğin:

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

Bu kodda, 5 işçi goroutine başlatılır ve her biri bir görev gerçekleştirir.

## 3. Goroutine İletişimi ve Senkronizasyonu

Goroutine'ler arasında iletişim ve senkronizasyon için kanallar kullanılır. Örneğin:

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

Bu kodda, bir goroutine bir mesaj gönderir ve ana goroutine bu mesajı alır.

## 4. Goroutine Sızıntıları ve Yönetimi

Goroutine sızıntıları, başlatılan ancak asla sonlanmayan goroutine'lerdir. Bu, bellek sızıntılarına yol açabilir. Goroutine'leri dikkatli yönetmek önemlidir. Örneğin:

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

Bu kodda, bir goroutine belirli bir süre sonra sonlanır.

## 5. Gerçek Dünya Uygulamaları

Goroutine'ler, web sunucuları, veri işleme boru hatları ve daha birçok uygulamada kullanılır. Örneğin, bir web sunucusunda her gelen istek için bir goroutine başlatılabilir.

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

Bu kodda, her gelen HTTP isteği için bir goroutine başlatılır ve istek işlenir.

Bu dersin sonuna geldik. Goroutine'lerin gücünü ve esnekliğini anlamak, Go dilinde etkili ve verimli programlar yazmanıza yardımcı olacaktır.

