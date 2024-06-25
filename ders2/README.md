# Ders 2: Temel Go Söz Dizimi ve Veri Tipleri (Basic Go Syntax and Data Types)

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

### Sabitler (Constants)

Sabitler `const` anahtar kelimesi ile tanımlanır:

```go
const pi = 3.14
```

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

## Kontrol Yapıları (Control Structures)

### If-Else

```go
if age >= 18 {
    fmt.Println("You are an adult.")
} else {
    fmt.Println("You are a minor.")
}
```

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

### For Döngüsü (For Loop)

```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}
```

### Range

```go
numbers := []int{1, 2, 3, 4, 5}
for index, value := range numbers {
    fmt.Println(index, value)
}
```

Bu dersin sonunda, Go programlama dilinin temel söz dizimini ve veri tiplerini öğrenmiş olacaksınız. Bir sonraki derste fonksiyonlar ve paketler hakkında daha fazla bilgi edineceksiniz.

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