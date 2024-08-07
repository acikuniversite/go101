# Ders 6: Pointerlar (Pointers)

## Pointerlar (Pointers) Nedir?

Go dilinde, bir değişkenin bellek adresini tutan ve bu bellek adresine erişim sağlayan bir veri türüne _pointer_ denir. Pointerlar, bir değişkenin bellek adresini tutarlar ve bu bellek adresine erişim sağlarlar. Pointerlar, bir değişkenin değerini değiştirmek için kullanılır ve bu sayede değişkenin değeri üzerinde işlem yapılabilir.

## Pointerlar Nasıl Tanımlanır?

Pointer tanımlama sırasında `*` karakteri kullanılır ve adresini tutacağımız değişken tipinin pointerı olduğunu belirtiriz.

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
<summary><b>Alıştırma: 2 farklı değişken tanımlayın ve birer pointer oluşturarak bu değişkenlerin bellek adreslerini ekrana yazdırın.</b></summary>

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

<details>
<summary><b>Alıştırma:  Bir değişkenin değerini tutan pointerın değerini ekranan yazdırın.</b></summary>

```go
package main

import "fmt"

func main() {
    var x int = 5
    var p *int

    p = &x

    fmt.Println("Pointerın değeri:", *p)
}
```

</details>

## Pointer'ın Pointerı

Go dilinde, bir pointerın pointerını tanımlamak için `**` operatörü kullanılır. Bir pointerın pointerı, bir pointerın bellek adresini tutar ve bu bellek adresine erişim sağlar.

```go
package main

import "fmt"

func main() {
    var x int = 5
    var p *int
	var q **int
	
    p = &x
	q = &p
	
    fmt.Println("Değişkenin değeri:", x)
    fmt.Println("Değişkenin bellek adresi:", &x)
    fmt.Println("Pointerın bellek adresi:", p)
    fmt.Println("Pointerın değeri:", *p)
	fmt.Println("Pointerın Pointerı bellek adresi:", q)
    fmt.Println("Pointerın Pointerı değeri:", *q)
	fmt.Println("Pointerın Pointerı'nın Pointerı değeri:", **q)
	fmt.Println("Pointerın Pointerı'nın Pointerı'nın değeri:", ***&q)
}
```

## Pointer'ın Nil Olması

Go dilinde, bir pointerın değeri `nil` olabilir. Bir pointerın değeri `nil` olduğunda, bu pointerın bellek adresi yoktur ve bu nedenle bu pointerın değeri `nil` olarak kabul edilir. Bir pointerın değeri `nil` olduğunda, bu pointerın bellek adresi yoktur ve bu nedenle bu pointerın değeri `nil` olarak kabul edilir.

```go
package main

import "fmt"

func main() {
    var p *int

    fmt.Println("Pointerın değeri:", p)
}
```

## 

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

## usafe Paketi

Go dilinde, pointerlar standart yapıda aktifken, pointerlar üzerinde işlem yapmak için `unsafe` paketini kullanabiliriz. `unsafe` paketi, pointerlar üzerinde işlem yapmak için kullanılır ve bu sayede pointerlar üzerinde işlem yapabiliriz.

```go
package main

import (
    "fmt"
    "unsafe"
)

func main() {
    var x int = 5
    var p *int

    p = &x

    fmt.Println("Değişkenin değeri:", x)
    fmt.Println("Değişkenin bellek adresi:", &x)
    fmt.Println("Pointerın bellek adresi:", p)
    fmt.Println("Pointerın değeri:", *p)
    fmt.Println("Pointerın boyutu:", unsafe.Sizeof(p))
}
```

### usafe ile neler yapabiliriz?

- Pointer Dönüşümü: Pointerları farklı veri türlerine dönüştürebiliriz. `p2 := (*float64)(unsafe.Pointer(p1))`
- Pointer Aritmetiği: Pointerlar üzerinde aritmetik işlemler yapabiliriz. `p2 := uintptr(unsafe.Pointer(p1)) + 1`
- Bellek Adresi: Pointerın bellek adresini alabiliriz. `fmt.Println("Pointerın bellek adresi:", unsafe.Pointer(p))`
- Struct Alanlarının Ofsetini Almak: Bir struct alanının ofsetini öğrenebiliriz. `fmt.Println("x değişkeninin ofseti:", unsafe.Offsetof(x))`
- Bellek Alignments (Bellek Hizalaması): Bellek hizalamasını kontrol ederek performans optimizasyonları yapabilirsiniz. `fmt.Println("x değişkeninin hizalaması:", unsafe.Alignof(x))`
- Zero-copy Slice Manipülasyonu: Zero-copy slice manipülasyonu, verilerin kopyalanmadan doğrudan bellekteki konumları üzerinden işlenmesini sağlar. Bu yaklaşım, performansı artırır ve bellek kullanımını optimize eder. Bu sayede arrayden slice dönüşümü veya struct tipinin slice dönüşümü yapabiliriz.
```
 array := [5]int{1, 2, 3, 4, 5}
slice := *(*[]int)(unsafe.Pointer(&array))
fmt.Println(slice)
```
- Custom Allocators (Özel Bellek Ayırıcılar): Custom Allocators (Özel Bellek Ayırıcılar), programların bellek yönetimini daha ince ayrıntılarla kontrol etmelerini sağlayan mekanizmalardır. Golang'de, bellek yönetimi genellikle yerleşik bellek ayırıcıları ve çöp toplayıcı (garbage collector) tarafından yönetilir. Ancak bazı durumlarda, özel bellek ayırıcılar kullanmak performansı artırabilir veya belirli bellek kullanım gereksinimlerini karşılayabilir.
```go
package main

import "unsafe"

type MyAllocator struct {
	pool []byte
	idx  uintptr
}

func (a *MyAllocator) Alloc(size uintptr) unsafe.Pointer {
	if a.idx+size > uintptr(len(a.pool)) {
		return nil
	}
	p := unsafe.Pointer(&a.pool[a.idx])
	a.idx += size
	return p
}

func main() {
	alloc := MyAllocator{pool: make([]byte, 1024)}
	p := alloc.Alloc(10)
	fmt.Println(p)
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

## Pointerlar ve Dilimler

Go dilinde, dilimler, bir dizinin bir alt kümesini tutan bir veri türüdür. Dilimler, bir dizinin bir alt kümesini tutarlar ve bu alt kümesine bellek adresleri üzerinden erişim sağlarlar. Dilimler, bir dizinin bir alt kümesini tutarlar ve bu alt kümesine bellek adresleri üzerinden erişim sağlarlar. Dilimler, bir dizinin bir alt kümesini tutarlar ve bu alt kümesine bellek adresleri üzerinden erişim sağlarlar.

```go
package main

import "fmt"

func main() {
    var dizi [5]int = [5]int{1, 2, 3, 4, 5}
    var dilim []int = dizi[1:4]
    var p *int

    p = &dilim[0]

    fmt.Println("Dilimin ilk elemanı:", dilim[0])
    fmt.Println("Dilimin ilk elemanının bellek adresi:", p)
    fmt.Println("Dilimin ikinci elemanı:", dilim[1])
    fmt.Println("Dilimin ikinci elemanının bellek adresi:", p+1)
}
```

<details>
<summary><b>Alıştırma: 5 elemanlı bir dilimin 10 kapasitesi olduğunu varsayalım, kapasitesi arttığı zaman dilimin bellek adresinin değişip değişmediğini kontrol edin. Dilim kapasitesi 1000e kadar çıkartın</b></summary>

```go
package main

import (
	"fmt"
	"unsafe"
)

func main() {
	var dilim = make([][10]int, 1, 1) // Başlangıç kapasitesi 1
	previousCapacity := cap(dilim)
	previousAddress := unsafe.Pointer(&dilim[0])
	fmt.Printf("Başlangıç kapasitesi: %d, Dilimin ilk elemanının adresi: %p\n", previousCapacity, previousAddress)
	count := 0

	for {
		dilim = append(dilim, [10]int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10})
		if cap(dilim) != previousCapacity {
			newAddress := unsafe.Pointer(&dilim[0])
			if newAddress != previousAddress {
				fmt.Printf("Kapasite değişti! Yeni kapasite: %d, Yeni dilimin ilk elemanının adresi: %p\n", cap(dilim), newAddress)
				previousAddress = newAddress
				count++
			}
			previousCapacity = cap(dilim)
		}
		if count == 100 {
			break
		}
	}
}
```


Sonraki Ders [Ders 7: Yapılar (Structs)](./ders7) için [tıklayınız](./ders7)

