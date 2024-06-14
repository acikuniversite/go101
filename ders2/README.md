# Ders 2: Temel Go Sözdizimi (Basic Go Syntax)

Bu derste Go dilinin temel sözdizimini (syntax) öğreneceksiniz.

## İçerik

- Go programının yapısı (structure)
- Temel sözdizimi kuralları (basic syntax rules)
- Değişken tanımlama (variable declaration)
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
