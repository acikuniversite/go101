# Ders 8: Go'da Goroutine ve Concurrency

Bu derste Go dilinde goroutine'ler ve concurrency (eşzamanlılık) kavramlarını öğreneceksiniz. Bu temel ders, goroutine'lerin nasıl çalıştığını ve Go dilindeki concurrency yapılarının nasıl kullanıldığını anlamanızı sağlayacak.

## İçerik
- Goroutine Nedir?
- Goroutine Kullanımı
- Concurrency ve Paralellik
- Kanal (Channel) Kullanımı
- Select İfadesi
- WaitGroup Kullanımı
- Mutex ve RWMutex Kullanımı

### 1. Goroutine Nedir?

Goroutine, Go dilinde hafif iş parçacıklarıdır. Çok az kaynak kullanarak eşzamanlı olarak çalışabilen küçük görevler olarak düşünülebilir. Bir fonksiyonun goroutine olarak çalıştırılması, bu fonksiyonun ayrı bir iş parçacığı gibi davranmasını sağlar, ancak Go dilinde bu iş parçacıkları daha hafif ve verimlidir.

```go
package main

import (
    "fmt"
    "time"
)

func printMessage() {
    fmt.Println("Goroutine çalışıyor!")
}

func main() {
    go printMessage() // Goroutine olarak çalıştırılıyor
    time.Sleep(1 * time.Second) // Ana goroutine'nin bitmesini beklemek için
    fmt.Println("Main fonksiyonu tamamlandı")
}
```


### 2. Goroutine Kullanımı

Goroutine'ler, bir fonksiyonu eşzamanlı olarak çalıştırmak istediğiniz her durumda kullanılabilir. Bir fonksiyonu goroutine olarak çalıştırmak için go anahtar kelimesini kullanmanız yeterlidir. Bu, fonksiyonun anında yürütülmeye başlamasını sağlar, ancak ana programın yürütülmesiyle aynı anda devam eder.

```go
package main

import (
    "fmt"
    "time"
)

func countToTen() {
    for i := 1; i <= 10; i++ {
        fmt.Println(i)
        time.Sleep(500 * time.Millisecond)
    }
}

func main() {
    go countToTen()
    fmt.Println("Main fonksiyonu devam ediyor...")
    time.Sleep(6 * time.Second)
}
```

### 3. Concurrency ve Paralellik

Concurrency, aynı anda birden fazla işlemin yürütülebileceği anlamına gelir. Go dilinde, goroutine'ler sayesinde birden fazla iş aynı anda çalıştırılabilir. Bu, programların daha verimli çalışmasını sağlar.

Paralellik ise, bu eşzamanlı işlerin gerçekten aynı anda fiziksel olarak farklı işlemcilerde çalıştırılmasıdır. Concurrency, paralellik ile aynı şey değildir, ancak paralelliğe olanak tanır.

```go 
package main

import (
    "fmt"
    "time"
)

func sayHello() {
    fmt.Println("Merhaba!")
    time.Sleep(1 * time.Second)
}

func sayGoodbye() {
    fmt.Println("Hoşça kal!")
    time.Sleep(1 * time.Second)
}

func main() {
    go sayHello()
    go sayGoodbye()

    time.Sleep(2 * time.Second)
    fmt.Println("Main fonksiyonu tamamlandı")
}
```

### 4. Kanal (Channel) Kullanımı

Kanallar (channels), goroutine'ler arasında veri iletmek için kullanılan yapılandırmalardır. Bir goroutine bir kanala veri gönderir, diğer bir goroutine ise bu veriyi alır. Bu, eşzamanlı işlemler arasında güvenli veri iletişimi sağlar.

```go

package main

import (
    "fmt"
)

func sendMessage(ch chan string) {
    ch <- "Merhaba, Kanal!"
}

func main() {
    messageChannel := make(chan string)

    go sendMessage(messageChannel)

    message := <-messageChannel
    fmt.Println(message)
}

```
Bu örnekte, sendMessage fonksiyonu bir mesajı messageChannel kanalına gönderir. Ana fonksiyon bu mesajı kanaldan alır ve yazdırır. Bu, goroutine'ler arasında veri iletişimini gösteren basit bir örnektir.

### 5. Select İfadesi

`select` ifadesi, birden fazla kanal işlemini beklemek ve bu işlemlerden biri gerçekleştiğinde yürütmek için kullanılır. Bu, goroutine'lerin belirli koşullarda nasıl tepki vereceğini kontrol etmenin güçlü bir yoludur.

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
        ch1 <- "ch1'den mesaj"
    }()

    go func() {
        time.Sleep(1 * time.Second)
        ch2 <- "ch2'den mesaj"
    }()

    select {
    case msg1 := <-ch1:
        fmt.Println(msg1)
    case msg2 := <-ch2:
        fmt.Println(msg2)
    case <-time.After(3 * time.Second):
        fmt.Println("Timeout gerçekleşti")
    }
}
```

Bu örnekte, select ifadesi, iki kanaldan gelen mesajları bekler ve bu mesajlardan hangisi önce gelirse onu işleyerek ekrana yazdırır. Eğer belirli bir süre içinde mesaj alınmazsa, "Timeout gerçekleşti" mesajı gösterilir.

### 6. WaitGroup Kullanımı

`sync` paketi içinde yer alan `WaitGroup` yapısı, goroutine'lerin tamamlanmasını beklemek için kullanılır. Bu yapı, bir veya daha fazla goroutine'nin tamamlanmasını beklemek için kullanılır.

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done() // WaitGroup sayacını azalt
    fmt.Printf("Worker %d started\n", id)
    time.Sleep(time.Second)
    fmt.Printf("Worker %d finished\n", id)
}

func main() {
    var wg sync.WaitGroup

    for i := 1; i <= 5; i++ {
        wg.Add(1)
        go worker(i, &wg)
    }

    wg.Wait() // Tüm goroutine'lerin bitmesini bekler
    fmt.Println("Tüm işler tamamlandı")
}

```

Bu örnekte, worker fonksiyonu 5 farklı goroutine olarak çalıştırılır ve wg.Wait() ifadesi, tüm worker goroutine'lerinin tamamlanmasını bekler.

### 7. Mutex ve RWMutex Kullanımı

Mutex ve RWMutex, Go dilinde eşzamanlılık sorunlarını çözmek için kullanılan senkronizasyon yapılarıdır. Mutex, bir veriye aynı anda sadece bir goroutine'nin erişmesine izin verirken, RWMutex, bir veriye aynı anda birden fazla okuma işlemine izin verirken yazma işlemlerini sadece tek bir goroutine'nin yapmasını sağlar.

```go
package main

import (
	"sync"
	"time"
)

func incrementCounter(counter *int, mutex *sync.Mutex) {
	mutex.Lock()
	*counter++
	mutex.Unlock()
}

func failIncrementCounter(counter *int) {
	*counter++
}

func main() {
	var withoutMutexCounter int = 0
	var withoutMutexCounter2 int = 0
	var withoutMutexCounter3 int = 0
	var withoutMutexCounter4 int = 0
	var withoutMutexCounter5 int = 0
	var withMutexCounter int
	var mutex sync.Mutex

	for i := 0; i < 1000; i++ {
		go incrementCounter(&withMutexCounter, &mutex)
		go failIncrementCounter(&withoutMutexCounter)
		go failIncrementCounter(&withoutMutexCounter2)
		go failIncrementCounter(&withoutMutexCounter3)
		go failIncrementCounter(&withoutMutexCounter4)
		go failIncrementCounter(&withoutMutexCounter5)
	}
	time.Sleep(5 * time.Second)
	mutex.Lock()
	println("With Mutex Counter:", withMutexCounter)
	mutex.Unlock()

	println("Without Mutex Counter:", withoutMutexCounter)
	println("Without Mutex Counter2:", withoutMutexCounter2)
	println("Without Mutex Counter3:", withoutMutexCounter3)
	println("Without Mutex Counter4:", withoutMutexCounter4)
	println("Without Mutex Counter5:", withoutMutexCounter5)

}
```


```shell
>> go run main.go

With Mutex Counter: 1000

Without Mutex Counter: 876
Without Mutex Counter2: 987
Without Mutex Counter3: 564
Without Mutex Counter4: 923
Without Mutex Counter5: 997
```

Bu örnekte, `sync.Mutex` yapısı kullanılarak `incrementCounter` fonksiyonu ile güvenli bir şekilde bir sayacı artırırken, `failIncrementCounter` fonksiyonu ile aynı işlemi güvenli olmayan bir şekilde yapmaktadır. Bu durumda, güvenli olmayan şekilde artırılan sayacıların değerlerinde hatalar oluşmaktadır.


