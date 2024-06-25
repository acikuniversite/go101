# Ders 11: Go'da Goroutine ve Concurrency

Bu derste Go dilinde goroutine'ler ve concurrency (eşzamanlılık) kavramlarını öğreneceksiniz.

## İçerik

- Goroutine Nedir?
- Goroutine Kullanımı
- Concurrency ve Paralellik
- Kanal (Channel) Kullanımı

## Goroutine Nedir?

Goroutine, Go dilinde hafif bir iş parçacığıdır. Goroutine'ler, aynı anda birden fazla işlemi gerçekleştirmek için kullanılır. `go` anahtar kelimesi ile başlatılırlar.

```go
package main

import (
    "fmt"
    "time"
)

func sayHello() {
    for i := 0; i < 5; i++ {
        time.Sleep(100 * time.Millisecond)
        fmt.Println("Merhaba")
    }
}

func main() {
    go sayHello()
    time.Sleep(600 * time.Millisecond)
    fmt.Println("Goroutine bitti")
}
```

## Goroutine Kullanımı

Goroutine'ler, fonksiyonları eşzamanlı olarak çalıştırmak için kullanılır. `go` anahtar kelimesi ile bir fonksiyon çağrıldığında, bu fonksiyon yeni bir goroutine olarak çalışır.

```go
package main

import (
    "fmt"
    "time"
)

func printNumbers() {
    for i := 1; i <= 5; i++ {
        time.Sleep(100 * time.Millisecond)
        fmt.Println(i)
    }
}

func main() {
    go printNumbers()
    time.Sleep(600 * time.Millisecond)
    fmt.Println("Goroutine bitti")
}
```

## Concurrency ve Paralellik

Concurrency, aynı anda birden fazla işlemi gerçekleştirme yeteneğidir. Paralellik ise bu işlemlerin aynı anda fiziksel olarak gerçekleştirilmesidir. Go, concurrency'yi destekler ve paralelliği de mümkün kılar.

## Kanal (Channel) Kullanımı

Kanal (channel), goroutine'ler arasında veri alışverişi yapmak için kullanılır. Kanallar, `chan` anahtar kelimesi ile tanımlanır.

```go
package main

import (
    "fmt"
)

func sum(a []int, c chan int) {
    total := 0
    for _, v := range a {
        total += v
    }
    c <- total
}

func main() {
    a := []int{1, 2, 3, 4, 5}
    c := make(chan int)
    go sum(a, c)
    result := <-c
    fmt.Println(result)
}
```

Bu dersin sonunda, Go dilinde goroutine'ler ve concurrency ile ilgili temel kavramları öğrenmiş olacaksınız.

### Sonraki Ders

[# Ders 12: Goroutine Advanced Patterns (İleri Goroutine Desenleri)](../ders12/README.md)