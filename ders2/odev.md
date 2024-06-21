# Ders 2: Ödev

Bu ödevde, Go programlama dilinde temel veri tiplerini ve kontrol yapılarını kullanmayı öğreneceksiniz.

## Ödev Adımları

### Adım 1: Temel Veri Tipleri

1. Bir metin editörü kullanarak `main.go` dosyasını oluşturun ve aşağıdaki kodu ekleyin:
   ```go
   package main

   import "fmt"

   func main() {
       var age int = 30
       var name string = "Ali"
       var isStudent bool = true
       var gpa float64 = 3.75

       fmt.Println("Yaş:", age)
       fmt.Println("İsim:", name)
       fmt.Println("Öğrenci mi?:", isStudent)
       fmt.Println("GPA:", gpa)
   }
   ```
2. Terminal veya komut istemcisinde `go run main.go` komutunu çalıştırarak programınızı çalıştırın.

### Adım 2: Kontrol Yapıları

1. `main.go` dosyasını aşağıdaki kod ile güncelleyin:
   ```go
   package main

   import "fmt"

   func main() {
       var age int = 30

       if age < 18 {
           fmt.Println("Çocuk")
       } else if age < 65 {
           fmt.Println("Yetişkin")
       } else {
           fmt.Println("Yaşlı")
       }

       for i := 0; i < 5; i++ {
           fmt.Println("Döngüdeyim:", i)
       }

       day := "Monday"
       switch day {
       case "Monday":
           fmt.Println("Start of the work week.")
       case "Friday":
           fmt.Println("End of the work week.")
       default:
           fmt.Println("Midweek day.")
       }

       numbers := []int{1, 2, 3, 4, 5}
       for index, value := range numbers {
           fmt.Println(index, value)
       }
   }
   ```
2. Terminal veya komut istemcisinde `go run main.go` komutunu çalıştırarak programınızı çalıştırın.

### Teslim

- `main.go` dosyanızın içeriğini ve programın çıktısını bir dosyaya ekleyin.
- Bu dosyayı ödev teslim sistemi üzerinden gönderin.

Tebrikler! Go'da temel veri tiplerini ve kontrol yapılarını başarıyla kullandınız.
