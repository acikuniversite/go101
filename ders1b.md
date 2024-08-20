# Ders 1: Temel Go Söz Dizimi ve Veri Tipleri (Basic Go Syntax and Data Types)

Bu derste Go programlama dilinin temel söz dizimini ve veri tiplerini öğreneceksiniz.

## İçerik

- Go programlarının yapısı (structure of Go programs)
- Değişkenler ve sabitler (variables and constants)
- Temel veri tipleri (basic data types)
- Operatörler (operators)
- Kontrol yapıları (control structures)

## Go Programlarının Yapısı (Structure of Go Programs)

Go programları genellikle aşağıdaki yapıya sahiptir:

```go
package main

import "fmt"

func main() {
    // Kodlar buraya gelecek
}
```

- `package main`: Programın ana paketini belirtir.
- `import "fmt"`: Gerekli paketleri içe aktarır.
- `func main()`: Programın giriş noktası olan ana fonksiyonu tanımlar.

## Değişkenler ve Sabitler (Variables and Constants)

### Değişkenler (Variables)

Go'da değişkenler `var` anahtar kelimesi ile tanımlanır:

```go
var x int
x = 5
```

Kısa yol olarak `:=` operatörü ile de tanımlanabilir:

```go
y := 10
```

```go

### Sabitler (Constants)

Sabitler `const` anahtar kelimesi ile tanımlanır:

```go
const pi = 3.14
```

<details>
<summary><b>Ders Alıştırması: 2 farklı değişken ve bir tane sabit tanımlayın ve ekrana yazdırın.</b></summary>

```go
package main

import "fmt"

func main() {
    var x int
    x = 5
    fmt.Println(x)

    y := 10
    fmt.Println(y)
	
    const pi = 3.14
    fmt.Println(pi)
}
```
</details>

## Temel Veri Tipleri (Basic Data Types)

Go'da temel veri tipleri şunlardır:

- `int`: Tamsayı
- `float64`: Ondalıklı sayı
- `string`: Metin
- `bool`: Mantıksal değer (true/false)

Örnek:

```go
var age int = 30
var name string = "Ali"
var isStudent bool = true
var gpa float64 = 3.75
```


<details>
<summary><b>Ders Alıştırması: bir öğrencinin adını soyadını telefon numarasını ve final notunu kayıt altına alın.</b></summary>
<pre>
package main

import "fmt"

func main() {

	    var name string
        var surname string
        var phone string
        var finalNotu int
		
		fmt.Println("Öğrenci Adı: ")
		fmt.Scanln(&name)
		
		fmt.Println("Öğrenci Soyadı: ")
		fmt.Scanln(&surname)
		
		fmt.Println("Öğrenci Telefon Numarası: ")
		fmt.Scanln(&phone)
		
		fmt.Println("Öğrenci Final Notu: ")
		fmt.Scanln(&finalNotu)
}
</pre>
</details>

## Operatörler (Operators)

Go'da yaygın olarak kullanılan operatörler şunlardır:

- Aritmetik Operatörler: `+`, `-`, `*`, `/`, `%`
- Karşılaştırma Operatörleri: `==`, `!=`, `<`, `>`, `<=`, `>=`
- Mantıksal Operatörler: `&&`, `||`, `!`

Örnek:

```go
a := 10
b := 20

sum := a + b
isEqual := (a == b)
isGreater := (a > b)
```

<details>
<summary><b>Ders Alıştırması: x için `x^2+2x+5` formülünü kullanarak x'in değerini hesaplayın.</b></summary>

```go
package main

import "fmt"

func main()  {
    fmt.Println("x değerini girin: ")
    var x int
    fmt.Scanln(&x)
	
	fmt.Printf("Sonuç: %d\n", x*x + 2*x + 1)
}

```

</details>

## Kontrol Yapıları (Control Structures)

### If-Else

```go
if age >= 18 {
    fmt.Println("You are an adult.")
} else {
    fmt.Println("You are a minor.")
}
```

<details>
<summary><b>Ders Alıştırması: Bir öğrencinin vize ve final notuna göre geçip geçmediğini kontrol edin. (vize %40 final %60)</b></summary>

```go
package main

import "fmt"

func main() {
	var vizeNotu, finalNotu float64
	fmt.Println("Vize notunu girin: ")
	fmt.Scanln(&vizeNotu)
	fmt.Println("Final notunu girin: ")
	fmt.Scanln(&finalNotu)
	
	result := vizeNotu*0.4 + finalNotu*0.6
	if result >= 50 {
        fmt.Println("Geçtiniz.")
    } else {
		        fmt.Println("Kaldınız.")
	}
}
```

</details>

### Switch

```go
switch day := "Monday"; day {
case "Monday":
    fmt.Println("Start of the work week.")
case "Friday":
    fmt.Println("End of the work week.")
default:
    fmt.Println("Midweek day.")
}
```

<details>
<summary><b>Ders Alıştırması: Bir öğrencinin öğrenim kredisini hangi gün alacağını söyleyen uygulama (TC Kimlik numarasının sonu “0” olanlar 6 Aralık, “2” olanlar 7 Aralık, “4” olanlar 8 Aralık, “6” olanlar 9 Aralık, “8” olanlar 10 Aralık'ta ödemelerini alabiliyor.)</b></summary>

```go
package main

import "fmt"

func main() {
	fmt.Println("TC Kimlil Numaranınız giriniz ")
	var tcNo int
	fmt.Scanln(&tcNo)
	
	switch tcNo % 10 {
	case 0:
        fmt.Println("6 Aralık'ta öğrenim kredinizi alabilirsiniz.")
	case 2:
        fmt.Println("7 Aralık'ta öğrenim kredinizi alabilirsiniz.")
    case 4:
        fmt.Println("8 Aralık'ta öğrenim kredinizi alabilirsiniz.")
    case 6:
        fmt.Println("9 Aralık'ta öğrenim kredinizi alabilirsiniz.")
    case 8:
        fmt.Println("10 Aralık'ta öğrenim kredinizi alabilirsiniz.")
    default:
        fmt.Println("Öğrenim kredinizi alabileceğiniz bir gün bulunmamaktadır.")
	}
	
}
```

</details>

### For Döngüsü (For Loop)

```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}
```

<details>
<summary><b>Ders Alıştırması: 1'den 10'a kadar olan sayıları ekrana yazdırın.</b></summary>

```go

package main

import "fmt"

func main() {
    for i := 1; i <= 10; i++ {
        fmt.Println(i)
    }
}
```

</details>

### Range

```go
numbers := []int{1, 2, 3, 4, 5}
for index, value := range numbers {
    fmt.Println(index, value)
}
```

<details>
<summary><b>Ders Alıştırması: 1'den 5'e kadar olan sayıları bir dilimde (slice) tanımlayın ve dilimdek elemanları her adım toplayıp ekrana yazdırın.</b></summary>

```go
package main

import "fmt"

func main() {
    numbers := []int{1, 2, 3, 4, 5}
    sum := 0
    for _, value := range numbers {
        sum += value
    }
    fmt.Println(sum)
}
```

</details>

## Teorikten Pratiğe Ödev:

1. Değişkenler: Bir tamsayı ve bir metin değişkeni tanımlayın ve bu değişkenleri ekrana yazdırın.
2. Değişkenler: Bir mantıksal ve bir ondalıklı sayı değişkeni tanımlayın ve bu değişkenleri toplayıp ekranı yazdırın.
3. Sabitler: Pi sayısını temsil eden bir sabit tanımlayın ve ekrana yazdırın.
4. Operatörler: İki tamsayı değişkeni tanımlayın ve bu değişkenler üzerinde aritmetik işlemler yaparak sonuçları ekrana yazdırın.
5. Kontrol Yapıları: Bir tamsayı değişkeni tanımlayın ve bu değişkenin pozitif, negatif veya sıfır olduğunu kontrol eden bir program yazın.
6. Döngüler: 1'den 10'a kadar olan sayıları ekrana yazdıran bir program yazın.
7. Range: Bir dilim (slice) tanımlayın ve bu dilimdeki elemanları ekrana yazdırın.
8. Görevleri tamamladıktan sonra ödevinizi fork edilmiş reponuzda `ders2` klasörü altında `main.go` dosyası olarak kaydedin ve pull request oluşturun.
9. Pull request linkini ödev teslim formunda paylaşın.


## Sonraki Ders

[Fonksiyonlar ve Paketler (Functions and Packages)](../ders3)