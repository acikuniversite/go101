# Ders 6: Pointerlar (Pointers)

## Pointerlar (Pointers) Nedir?

Go dilinde, bir değişkenin bellek adresini tutan ve bu bellek adresine erişim sağlayan bir veri türüne _pointer_ denir. Pointerlar, bir değişkenin bellek adresini tutarlar ve bu bellek adresine erişim sağlarlar. Pointerlar, bir değişkenin değerini değiştirmek için kullanılır ve bu sayede değişkenin değeri üzerinde işlem yapılabilir.

## Pointerlar Nasıl Tanımlanır?

Pointerlar, bir değişkenin bellek adresini tutan ve bu bellek adresine erişim sağlayan bir veri türüdür. Pointerlar, bir değişkenin bellek adresini tutarlar ve bu bellek adresine erişim sağlarlar. Pointerlar, bir değişkenin değerini değiştirmek için kullanılır ve bu sayede değişkenin değeri üzerinde işlem yapılabilir.

```go
package main

import "fmt"

func main() {
    var x int = 5
    var p *int

    p = &x

    fmt.Println("Değişkenin değeri:", x)
    fmt.Println("Değişkenin bellek adresi:", &x)
    fmt.Println("Pointerın bellek adresi:", p)
    fmt.Println("Pointerın değeri:", *p)
}
```

<details>
<summary><b>Alıştırma: 2 farklı değişken tanımlayın ve bir pointer oluşturarak bu değişkenlerin bellek adreslerini ekrana yazdırın.</b></summary>

```go
package main

import "fmt"

func main() {
    var x int = 5
    var y int = 10
    var p *int
    var q *int

    p = &x
    q = &y

    fmt.Println("x değişkeninin bellek adresi:", p)
    fmt.Println("y değişkeninin bellek adresi:", q)
}
```

</details>

## & ve * Operatörleri

- `&` operatörü: Bir değişkenin bellek adresini almak için kullanılır.
- `*` operatörü: Bir pointerın değerini almak için kullanılır.

```
fmt.Scanln(&x) // x değişkeninin bellek adresini alır bundan dolayı da scan fonksiyonu x değişkeninin değerini değiştirebilir.
```

## Pointerlar ve Fonksiyonlar

Go dilinde fonksiyonlar, değerler üzerinde işlem yaparlar ve bu işlemler sonucunda değerler değişebilir. Fonksiyonlar, değerler üzerinde işlem yaparken, değerlerin kopyaları üzerinde işlem yaparlar ve bu nedenle fonksiyonlar, değişkenlerin orijinal değerlerini değiştiremezler. Ancak, fonksiyonlara pointerlar aracılığıyla değişkenlerin bellek adresleri gönderilirse, fonksiyonlar bu bellek adresleri üzerinde işlem yapabilir ve değişkenlerin orijinal değerlerini değiştirebilirler.

```go
package main

import "fmt"

func degistir(x *int) {
    *x = 10
}

func main() {
    var x int = 5

    fmt.Println("Değişkenin değeri:", x)
    degistir(&x)
    fmt.Println("Değişkenin yeni değeri:", x)
}
```

Yukarıdaki örnekte, `degistir` fonksiyonu, bir pointer alır ve bu pointerın değerini değiştirir. `main` fonksiyonunda, `x` adında bir değişken tanımlanır ve bu değişkenin değeri `5` olarak atanır. Daha sonra, `degistir` fonksiyonu çağrılarak, `x` değişkeninin bellek adresi gönderilir ve `degistir` fonksiyonu, bu bellek adresi üzerinde işlem yaparak, `x` değişkeninin değerini `10` olarak değiştirir. Sonuç olarak, `main` fonksiyonunda, `x` değişkeninin değeri `10` olarak yazdırılır.

<details>
<summary><b>Alıştırma: Değişkenin değerini ve adresini gönderdiğimiz iki farklı fonksiyonun çıktılarının farklı olduğunu gösteren bir örnek yazın.</b></summary>

```go
package main

import "fmt"

func degistir(x *int) {
    *x = 10
}

func degistir2(x int) {
    x = 10
}

func main() {
    var x int = 5

    fmt.Println("Değişkenin değeri:", x)
    degistir(&x)
    fmt.Println("Değişkenin yeni değeri:", x)

    fmt.Println("Değişkenin değeri:", x)
    degistir2(x)
    fmt.Println("Değişkenin yeni değeri:", x)
}
```

</details>

## Pointerlar ve Diziler

Go dilinde, diziler, bellekte ardışık olarak sıralanmış veri öğelerini tutan bir veri türüdür. Diziler, aynı türdeki veri öğelerini tutarlar ve bu veri öğelerine bellek adresleri üzerinden erişim sağlarlar. Diziler, bellekte ardışık olarak sıralanmış veri öğelerini tutarlar ve bu veri öğelerine bellek adresleri üzerinden erişim sağlarlar. Diziler, bir veri türünden oluşan bir veri kümesini tutarlar ve bu veri kümesine bellek adresleri üzerinden erişim sağlarlar.

```go

package main

import "fmt"

func main() {
    var dizi [5]int = [5]int{1, 2, 3, 4, 5}
    var p *int

    p = &dizi[0]

    fmt.Println("Dizinin ilk elemanı:", dizi[0])
    fmt.Println("Dizinin ilk elemanının bellek adresi:", p)
    fmt.Println("Dizinin ikinci elemanı:", dizi[1])
    fmt.Println("Dizinin ikinci elemanının bellek adresi:", p+1)
}
```


<details>
<summary><b>Alıştırma: 5 elemanlı bir dizi tanımlayın ve bu dizinin bellek adreslerini ekrana yazdırın.</b></summary>

```go

package main

import "fmt"

func main() {
    var dizi [5]int = [5]int{1, 2, 3, 4, 5}
    var p *int

    for i := 0; i < len(dizi); i++ {
        p = &dizi[i]
        fmt.Println("Dizinin", i+1, ". elemanının bellek adresi:", p)
    }
}
```
</details>

Sonraki Ders [Ders 7: Yapılar (Structs)](./ders7) için [tıklayınız](./ders7)

