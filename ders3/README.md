# Ders 3: Fonksiyonlar ve Paketler (Functions and Packages)

Bu derste Go dilinde fonksiyonları ve paketleri öğreneceksiniz.

## İçerik

- Fonksiyonlar (Functions)
- Fonksiyon Parametreleri ve Geri Dönüş Değerleri (Function Parameters and Return Values)
- Paketler (Packages)
- Dahili Paketler (Standard Library Packages)
- Kendi Paketlerinizi Oluşturma (Creating Your Own Packages)

## Fonksiyonlar (Functions)

Fonksiyonlar, belirli bir görevi yerine getiren kod bloklarıdır. Go dilinde fonksiyonlar `func` anahtar kelimesi ile tanımlanır.

### Fonksiyon Tanımlama

```go
package main

import "fmt"

func greet(name string) {
    fmt.Println("Merhaba,", name)
}

func main() {
    greet("Ali")
}
```

Yukarıdaki örnekte, `greet` adında bir fonksiyon tanımlanmış ve `main` fonksiyonunda çağrılmıştır.

### Fonksiyon Parametreleri ve Geri Dönüş Değerleri (Function Parameters and Return Values)

Fonksiyonlar parametre alabilir ve değer döndürebilir.

```go
package main

import "fmt"

func add(a int, b int) int {
    return a + b
}

func main() {
    sum := add(5, 3)
    fmt.Println("Toplam:", sum)
}
```

Yukarıdaki örnekte, `add` fonksiyonu iki tamsayı parametre alır ve bu parametrelerin toplamını döner.

## Paketler (Packages)

Go programları paketler halinde organize edilir. Her Go dosyası bir paket bildirimi ile başlar.

### Dahili Paketler (Standard Library Packages)

Go'nun standart kütüphanesi birçok kullanışlı paket içerir. Örneğin, `fmt` paketi formatlı I/O işlemleri için kullanılır.

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    fmt.Println("Pi sayısı:", math.Pi)
}
```

### Kendi Paketlerinizi Oluşturma (Creating Your Own Packages)

Kendi paketlerinizi oluşturabilir ve kullanabilirsiniz. Örneğin, `greetings` adında bir paket oluşturalım.

#### greetings/greetings.go

```go
package greetings

import "fmt"

func Hello(name string) {
    fmt.Println("Merhaba,", name)
}
```

#### main.go

```go
package main

import (
    "greetings"
)

func main() {
    greetings.Hello("Ali")
}
```

Bu şekilde, `greetings` paketini `main` paketinde kullanabilirsiniz.

Bu dersin sonunda, Go dilinde fonksiyonları ve paketleri nasıl kullanacağınızı öğrenmiş olacaksınız. Bir sonraki derste daha ileri seviye konulara geçeceğiz.

## Teorikten Pratiğe Ödev:

1. Bir fonksiyon tanımlayın ve bu fonksiyonu çağırarak ekrana bir metin yazdırın.
2. Bir fonksiyon tanımlayın ve bu fonksiyona iki tamsayı parametre göndererek bu parametrelerin toplamını ekrana yazdırın.
3. Go'nun standart kütüphanesinden math ve time paketlerini kullanarak birkaç örnek uygulama yazın.
4. Kendi paketinizi oluşturun ve bu paketi kullanarak bir fonksiyon çağırın.