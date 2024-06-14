# Ders 9: Arayüzler (Interfaces)

Bu derste Go dilinde arayüzleri (interfaces) öğreneceksiniz.

## İçerik

- Arayüz Tanımlama (Interface Definition)
- Arayüz İşlemleri (Interface Operations)
- Arayüz Örnekleri (Interface Examples)

## Arayüz Tanımlama (Interface Definition)

Go dilinde arayüzler, bir veya daha fazla yöntemi (method) içeren türlerdir. Arayüzler, belirli bir işlevselliği tanımlamak için kullanılır ve bu işlevselliği gerçekleştiren türler tarafından uygulanır.

### Örnek

```go
package main

import "fmt"

// Arayüz tanımlama
type Shaper interface {
    Area() float64
}

// Dikdörtgen yapısı
type Rectangle struct {
    width, height float64
}

// Dikdörtgenin alanını hesaplayan yöntem
func (r Rectangle) Area() float64 {
    return r.width * r.height
}

func main() {
    r := Rectangle{width: 5, height: 3}
    var s Shaper
    s = r
    fmt.Println("Dikdörtgenin alanı:", s.Area())
}
```

## Arayüz İşlemleri (Interface Operations)

Arayüzler, belirli bir işlevselliği tanımlamak ve bu işlevselliği gerçekleştiren türler arasında ortak bir sözleşme sağlamak için kullanılır. Arayüzler, türlerin birbirleriyle nasıl etkileşime gireceğini belirler.

### Örnek

```go
package main

import "fmt"

// Arayüz tanımlama
type Printer interface {
    Print()
}

// Mesaj yapısı
type Message struct {
    text string
}

// Mesajın içeriğini yazdıran yöntem
func (m Message) Print() {
    fmt.Println(m.text)
}

func main() {
    m := Message{text: "Merhaba, Dünya!"}
    var p Printer
    p = m
    p.Print()
}
```

## Arayüz Örnekleri (Interface Examples)

Arayüzler, Go dilinde çok çeşitli senaryolarda kullanılabilir. Aşağıda, arayüzlerin farklı kullanım örneklerini bulabilirsiniz.

### Örnek 1: Çoklu Arayüzler

```go
package main

import "fmt"

// Arayüz tanımlama
type Reader interface {
    Read() string
}

type Writer interface {
    Write(string)
}

// Okuyucu/Yazıcı yapısı
type ReadWriter struct {
    content string
}

func (rw *ReadWriter) Read() string {
    return rw.content
}

func (rw *ReadWriter) Write(text string) {
    rw.content = text
}

func main() {
    rw := &ReadWriter{}
    var r Reader = rw
    var w Writer = rw

    w.Write("Merhaba, Go!")
    fmt.Println(r.Read())
}
```

### Örnek 2: Boş Arayüz (Empty Interface)

```go
package main

import "fmt"

// Boş arayüz
type Any interface{}

func main() {
    var a Any
    a = 5
    fmt.Println(a)

    a = "Merhaba"
    fmt.Println(a)

    a = true
    fmt.Println(a)
}
```

Bu ders, Go dilinde arayüzlerin nasıl tanımlandığını, kullanıldığını ve farklı senaryolarda nasıl uygulandığını anlamanıza yardımcı olacaktır.
