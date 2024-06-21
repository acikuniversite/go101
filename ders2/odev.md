# Ders 2: Ödev

Bu ödevde, Go programlama dilinde temel veri tiplerini ve kontrol yapılarını kullanmayı öğreneceksiniz.

## Ödev Adımları

### Adım 1: Temel Veri Tipleri

1. Bir metin editörü kullanarak `main.go` dosyasını oluşturun ve aşağıdaki kodu ekleyin:
   ```go
   package main

   import "fmt"

   func main() {
       var age int = 25
       var name string = "Ali"
       var isStudent bool = true

       fmt.Println("Yaş:", age)
       fmt.Println("İsim:", name)
       fmt.Println("Öğrenci mi?:", isStudent)
   }
   ```
2. Terminal veya komut istemcisinde `go run main.go` komutunu çalıştırarak programınızı çalıştırın.

### Adım 2: Kontrol Yapıları

1. `main.go` dosyasını aşağıdaki kod ile güncelleyin:
   ```go
   package main

   import "fmt"

   func main() {
       var age int = 25

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
   }
   ```
2. Terminal veya komut istemcisinde `go run main.go` komutunu çalıştırarak programınızı çalıştırın.

### Teslim

- `main.go` dosyanızın içeriğini ve programın çıktısını bir dosyaya ekleyin.
- Bu dosyayı ödev teslim sistemi üzerinden gönderin.

Tebrikler! Go'da temel veri tiplerini ve kontrol yapılarını başarıyla kullandınız.
