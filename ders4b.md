# Ders 4: Hata Yönetimi (Error Handling)

Bu derste Go dilinde hata yönetimini (error handling) öğreneceksiniz.

## İçerik

- Hata tanımlama (error definition)
- Hata yakalama ve işleme (error catching and handling)
- Özel hata türleri (custom error types)

## Hata Tanımlama (Error Definition)

Go dilinde hatalar `error` arayüzü (interface) kullanılarak tanımlanır. `error` arayüzü, sadece bir `Error()` metoduna sahiptir. Bu metod, hata mesajını döner.

```go
package main

import (
    "errors"
    "fmt"
)

func main() {
    err := errors.New("Bu bir hatadır")
    fmt.Println(err)
}
```

## Hata Yakalama ve İşleme (Error Catching and Handling)

Hataları yakalamak ve işlemek için `if` deyimi kullanılır. Hata döndüren bir fonksiyon çağrıldığında, dönen hata kontrol edilir ve uygun şekilde işlenir.

```go
package main

import (
    "fmt"
    "strconv"
)

func main() {
    i, err := strconv.Atoi("42a")
    if err != nil {
        fmt.Println("Hata:", err)
        return
    }
    fmt.Println("Başarılı:", i)
}
```

## Özel Hata Türleri (Custom Error Types)

Kendi hata türlerinizi tanımlayarak daha anlamlı hata mesajları oluşturabilirsiniz. Bu, `struct` ve `Error()` metodunu kullanarak yapılır.

```go
package main

import (
    "fmt"
)

type MyError struct {
    Msg string
}

func (e *MyError) Error() string {
    return e.Msg
}

func main() {
    err := &MyError{Msg: "Bu özel bir hatadır"}
    fmt.Println(err)
}
```

Bu dersin sonunda, Go dilinde hata yönetimi ile ilgili temel kavramları öğrenmiş olacaksınız.

### Sonraki Ders

[# Ders 11: Go'da Goroutine ve Concurrency](../ders11/README.md)