# Ders 9: Generic Yapılar ve Fonksiyonlar (Generics) 

Bu derste Go dilinde generic yapıları (generics) öğreneceğiz.

## Generics Nedir?

Generic yapılar, bir programlama dilinde farklı veri tipleri üzerinde çalışabilen ve aynı kod bloğunu farklı veri tipleri için kullanmamızı sağlayan yapılar olarak tanımlanabilir. Generic yapılar, aynı kod bloğunu farklı veri tipleri için kullanmamızı sağlar ve bu sayede kod tekrarını azaltır.

Go dilinde generic yapılar, Go 1.18 sürümü ile birlikte desteklenmeye başlanmıştır. Generic yapılar, Go dilinde `type` anahtar kelimesi ile tanımlanır ve `type` anahtar kelimesi ile tanımlanan generic yapılar, farklı veri tipleri üzerinde çalışabilir.

## Generic Fonksiyonlar

Go dilinde generic fonksiyonlar, farklı veri tipleri üzerinde çalışabilen fonksiyonlar olarak tanımlanabilir. Generic fonksiyonlar, `type` anahtar kelimesi ile tanımlanır ve farklı veri tipleri üzerinde çalışabilir.

Örneğin, aşağıdaki örnekte `topla` fonksiyonu, farklı veri tipleri üzerinde çalışabilen bir generic fonksiyondur:

```go
package main

import "fmt"

func topla[T any](a, b T) T {
    return a + b
}

func main() {
    fmt.Println(topla(5, 3)) // 8
    fmt.Println(topla(5.5, 3.3)) // 8.8
    fmt.Println(topla("Hello, ", "World!")) // Hello, World!
}
```

Yukarıdaki örnekte `topla` fonksiyonu, farklı veri tipleri üzerinde çalışabilen bir generic fonksiyondur. `topla` fonksiyonu, `T` adında bir generic tip alır ve bu generic tip, `a` ve `b` parametrelerinin veri tiplerini belirler. `topla` fonksiyonu, `a` ve `b` parametrelerini toplar ve sonucu döndürür.

## Generic Yapılar

Go dilinde generic yapılar, farklı veri tipleri üzerinde çalışabilen yapılar olarak tanımlanabilir. Generic yapılar, `type` anahtar kelimesi ile tanımlanır ve farklı veri tipleri üzerinde çalışabilir.

Örneğin, aşağıdaki örnekte `Stack` adında bir generic yapı tanımlanmıştır:

```go

package main

import "fmt"

type Stack[T any] []T

func (s *Stack[T]) Push(value T) {
    *s = append(*s, value)
}

func (s *Stack[T]) Pop() T {
    if len(*s) == 0 {
        return nil
    }
    value := (*s)[len(*s)-1]
    *s = (*s)[:len(*s)-1]
    return value
}

func main() {
    var s Stack[int]
    s.Push(1)
    s.Push(2)
    s.Push(3)
    fmt.Println(s.Pop()) // 3
    fmt.Println(s.Pop()) // 2
    fmt.Println(s.Pop()) // 1
}
```

Yukarıdaki örnekte `Stack` adında bir generic yapı tanımlanmıştır. `Stack` yapısı, `T` adında bir generic tip alır ve bu generic tip, `Stack` yapısının elemanlarının veri tiplerini belirler. `Stack` yapısı, `Push` ve `Pop` metodları ile eleman ekleme ve çıkarma işlemlerini gerçekleştirir.

## Generic Fonksiyon ve Yapılar ile Çalışma

Generic fonksiyonlar ve yapılar, farklı veri tipleri üzerinde çalışabilen yapılar olduğu için, bu yapılar ile çalışırken dikkatli olunmalıdır. Generic yapılar ve fonksiyonlar, farklı veri tipleri üzerinde çalışabilen yapılar olduğu için, bu yapılar ile çalışırken veri tiplerine dikkat edilmelidir.

Örneğin, aşağıdaki örnekte `Stack` yapısına `int` veri tipi yerine `string` veri tipi ile eleman eklemeye çalışıldığında hata alınır:

```go

package main

import "fmt"

type Stack[T any] []T

func (s *Stack[T]) Push(value T) {
    *s = append(*s, value)
}

func (s *Stack[T]) Pop() T {
    if len(*s) == 0 {
        return nil
    }
    value := (*s)[len(*s)-1]
    *s = (*s)[:len(*s)-1]
    return value
}

func main() {
    var s Stack[int]
    s.Push(1)
    s.Push(2)
    s.Push(3)
    fmt.Println(s.Pop()) // 3
    fmt.Println(s.Pop()) // 2
    fmt.Println(s.Pop()) // 1

    var s2 Stack[string]
    s2.Push("Hello")
    s2.Push("World")
    fmt.Println(s2.Pop()) // World
    fmt.Println(s2.Pop()) // Hello
}
```

Yukarıdaki örnekte `Stack` yapısına `int` veri tipi yerine `string` veri tipi ile eleman eklemeye çalışıldığında hata alınır. Bu nedenle, generic yapılar ve fonksiyonlar ile çalışırken veri tiplerine dikkat edilmelidir.

## Sonuç

Bu derste Go dilinde generic yapıları (generics) öğrendik. Generic yapılar, farklı veri tipleri üzerinde çalışabilen ve aynı kod bloğunu farklı veri tipleri için kullanmamızı sağlayan yapılar olarak tanımlanabilir. Generic yapılar, Go dilinde `type` anahtar kelimesi ile tanımlanır ve `type` anahtar kelimesi ile tanımlanan generic yapılar, farklı veri tipleri üzerinde çalışabilir.

## Teorikten Pratiğe Ödev:

1. `Queue` adında bir generic yapı tanımlayın. Bu yapı, `Push` ve `Pop` metodları ile eleman ekleme ve çıkarma işlemlerini gerçekleştirmelidir.
2. `Queue` yapısını kullanarak farklı veri tipleri üzerinde çalışan bir kuyruk oluşturun.
3. Oluşturduğunuz kuyruğa eleman ekleyin ve çıkarın.
4. Kuyruğun elemanlarını ekrana yazdırın.

Queue Nedir: [Queue](https://en.wikipedia.org/wiki/Queue_(abstract_data_type))
FIFO Nedir: [FIFO](https://en.wikipedia.org/wiki/FIFO_(computing_and_electronics))

## Sonraki Ders: [Ders 10: Hata 
