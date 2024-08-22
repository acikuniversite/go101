# Ders 5: Generic Yapılar ve Fonksiyonlar (Generics) 

Bu derste Go dilinde generic yapıları (generics) öğreneceğiz.

## Generics Nedir?

Generic yapılar, bir programlama dilinde farklı veri tipleri üzerinde çalışabilen ve aynı kod bloğunu farklı veri tipleri için kullanmamızı sağlayan yapılar olarak tanımlanabilir. Generic yapılar, aynı kod bloğunu farklı veri tipleri için kullanmamızı sağlar ve bu sayede kod tekrarını azaltır.

Go dilinde generic yapılar, Go 1.18 sürümü ile birlikte desteklenmeye başlanmıştır. Generic yapılar, Go dilinde `type` anahtar kelimesi ile tanımlanır ve `type` anahtar kelimesi ile tanımlanan generic yapılar, farklı veri tipleri üzerinde çalışabilir.

## Olaylı Generic Eklenmesi

| <img src="go13.png" style="width: 250px;"> | <img src="go12.png" style="width: 250px;"> | <img src="go11.png" style="width: 250px;"> |
|--------------------------------------------|--------------------------------------------|--------------------------------------------|
 | Rob Pike                                   |Ken Thompson| Robert Griesemer                           |

Golang'de generics'in eklenme süreci oldukça ilginç ve uzun bir hikaye. Başlangıçta, Golang'in oluşturucuları Rob Pike, Ken Thompson ve Robert Griesemer, dili olabildiğince basit ve hızlı yapmak istiyorlardı. Bu yüzden, ilk versiyonlarında generics gibi karmaşık özelliklere yer vermediler. Onların hedefi, özellikle sistem programlamasında kullanılacak, anlaşılması kolay bir dil oluşturmaktı.

Ancak, Golang'in popülerliği arttıkça, topluluk içinde generics desteği için ciddi bir talep oluşmaya başladı. Geliştiriciler, "Neden her seferinde aynı türdeki kodu farklı veri tipleri için yeniden yazmak zorundayım?" gibi sorular sormaya başladılar. Özellikle büyük projelerde bu durum sıkıcı ve verimsiz hale geliyordu.

Golang topluluğu, generics'i istemeye başladı, ancak Rob Pike ve ekibi bu özelliği eklemekte tereddütlüydü. Onlar, dilin basitlik felsefesine sadık kalmak istiyorlardı ve generics'in dili karmaşıklaştırabileceğinden endişeliydiler. Bu yüzden, uzun bir süre boyunca "Hayır, generics eklemeyeceğiz" dediler.

Yıllar boyunca bu konu tartışılmaya devam etti. Golang geliştiricileri, "interface" ve "reflection" gibi mevcut özelliklerle pek çok sorunu çözmeye çalıştılar. Ama topluluk içinde, özellikle büyük projelerde çalışanlar, generics olmadan kodun tekrarlandığını ve bu durumun verimliliği düşürdüğünü belirtti.

Nihayetinde, 2019 yılında Ian Lance Taylor ve Robert Griesemer, generics eklemek için bir tasarım önerisi sundular. Bu tasarım, generics'in dili çok karmaşık hale getirmeden eklenebileceğini gösteriyordu. Rob Pike ve ekibi de bu öneriyi kabul etti ve üzerinde çalışmaya başladılar.

2022 yılında, Golang 1.18 sürümüyle birlikte generics resmi olarak dile eklendi. Bu, topluluk tarafından büyük bir sevinçle karşılandı. Artık geliştiriciler, tekrar eden kod yazmak zorunda kalmadan farklı veri tipleriyle çalışabiliyorlardı.

Rob Pike ve ekibi, bu özelliği eklerken dilin basitliğini korumayı başardılar. Böylece, hem topluluğun istediği generics eklendi, hem de Golang'in temel felsefesi bozulmadan kaldı.

Sonuç olarak, generics'in eklenmesi, Golang'in daha esnek ve güçlü bir dil haline gelmesini sağladı ve dilin kullanıcı kitlesi genişledi. Ama bu süreçte Rob Pike ve ekibi, dili karmaşıklaştırmamak için uzun süre direndiler ve sonunda toplulukla uzlaşıp doğru bir denge buldular

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


### Any ve Comparable Kısıtlamaları

- `any`: `any` kısıtlaması, fonksiyonun herhangi bir veri tipi üzerinde çalışabilmesini sağlar.
- `comparable`: `comparable` kısıtlaması, fonksiyonun karşılaştırılabilir veri tipleri üzerinde çalışabilmesini sağlar.
- `numeric`: `numeric` kısıtlaması, fonksiyonun sayısal veri tipleri üzerinde çalışabilmesini sağlar.
- 

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

## Struct[T any] Yapısı

Go dilinde generic yapılar, `struct` yapısı ile de tanımlanabilir. Örneğin, aşağıdaki örnekte `Pair` adında bir generic `struct` tanımlanmıştır:

```go
package main

import "fmt"

type Pair[T any] struct {
    First  T
    Second T
}

func main() {
    p := Pair[int]{First: 1, Second: 2}
    fmt.Println(p) // {1 2}

    p2 := Pair[string]{First: "Hello", Second: "World"}
    fmt.Println(p2) // {Hello World}
}
```

## Gerçek Dünya Uygulamaları

VOIP Cihazlardan gelen telefon açıldı, kapandı, cevap verildi, meşgul edildi gibi olayları takip eden bir uygulama yazmak istediğinizi düşünün. Bunun yanında farklı operatörlerin Kullanıcı ID tanımlamasında bazısı string bazısı integer tanımlama yaptığını gördük, bu durumda da generic yapılar kullanarak bu durumu çözebilirsiniz.

```go
package main

import "fmt"

type CallEvent[F, T any] struct {
	From F
	To   T
	Type string
}

func (c CallEvent[F, T]) Normalize() CallEvent[string, string] {
	return CallEvent[string, string]{From: fmt.Sprint(c.From), To: fmt.Sprint(c.To), Type: c.Type}
}

func main() {
	call1 := CallEvent[int, string]{From: 5418902324, To: "654321", Type: "Answered"}
	fmt.Println(call1.Normalize()) // {5418902324 654321 Answered}

	call2 := CallEvent[string, string]{From: "awqe12", To: "654321", Type: "Missed"}
	fmt.Println(call2.Normalize()) // {123456 654321 Missed}
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

