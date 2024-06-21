# Ders 1: Ödev

Bu ödevde, Go programlama dilini kurmayı ve basit bir Go programı yazmayı öğreneceksiniz.

## Ödev Adımları

### Adım 1: Go'nun Kurulumu

1. [Resmi Go web sitesinden](https://golang.org/dl/) işletim sisteminize uygun olan Go sürümünü indirin.
2. İndirdiğiniz dosyayı çalıştırarak Go'yu kurun.
3. Terminal veya komut istemcisini açın ve `go version` komutunu çalıştırarak kurulumun başarılı olup olmadığını doğrulayın.

### Adım 2: Go Çalışma Ortamının Ayarlanması

1. `GOPATH` ve `GOROOT` çevre değişkenlerini ayarlayın.
   - macOS/Linux:
     ```sh
     export GOPATH=$HOME/go
     export PATH=$PATH:$GOPATH/bin
     ```
     Bu komutları `.bashrc`, `.zshrc` veya `.profile` dosyanıza ekleyin.
   - Windows:
     Sistem özelliklerinden çevre değişkenlerini açın ve yeni bir `GOPATH` değişkeni ekleyin. Değer olarak `C:\Users\<kullanıcı_adı>\go` kullanabilirsiniz.

### Adım 3: İlk Go Programının Yazılması

1. Proje dizininizi oluşturun:
   ```sh
   mkdir -p $GOPATH/src/hello
   cd $GOPATH/src/hello
   ```
2. Bir metin editörü kullanarak `main.go` dosyasını oluşturun ve aşağıdaki kodu ekleyin:
   ```go
   package main

   import "fmt"

   func main() {
       fmt.Println("Merhaba, Dünya!")
   }
   ```
3. Terminal veya komut istemcisinde `go run main.go` komutunu çalıştırarak programınızı çalıştırın.

### Teslim

- `main.go` dosyanızın içeriğini ve `go version` komutunun çıktısını bir dosyaya ekleyin.
- Bu dosyayı ödev teslim sistemi üzerinden gönderin.

Tebrikler! İlk Go programınızı başarıyla yazdınız ve çalıştırdınız.
