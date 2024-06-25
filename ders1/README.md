# Ders 1: Go Kurulumu ve Çalışma Ortamı (Setup and Environment)

Bu derste Go programlama dilini kurmayı ve çalışma ortamını (environment) ayarlamayı öğreneceksiniz.

## İçerik

- Go'nun kurulumu (installation)
- Go çalışma ortamının ayarlanması (environment setup)
- İlk Go programının yazılması (writing the first Go program)

## Go'nun Kurulumu (Installation)

### Adım 1: Go'yu İndirin

Go'nun en son sürümünü [resmi Go web sitesinden](https://golang.org/dl/) indirin. İşletim sisteminize uygun olanı seçin (Windows, macOS, Linux).

### Adım 2: Go'yu Kurun

İndirdiğiniz dosyayı çalıştırarak Go'yu kurun. Kurulum sihirbazı sizi adım adım yönlendirecektir.

### Adım 3: Kurulumu Doğrulayın

Kurulumun başarılı olup olmadığını doğrulamak için terminal veya komut istemcisini açın ve aşağıdaki komutu çalıştırın:

```sh
go version
```

Bu komut, yüklü Go sürümünü gösterecektir.

## Go Çalışma Ortamının Ayarlanması (Environment Setup)

### Adım 1: Go Path Ayarları

Go çalışma ortamını ayarlamak için `GOPATH` ve `GOROOT` çevre değişkenlerini ayarlamanız gerekebilir. Genellikle `GOROOT` otomatik olarak ayarlanır, ancak `GOPATH`'i manuel olarak ayarlamanız gerekebilir.

`GOPATH`, Go projelerinizin ve bağımlılıklarının saklanacağı yerdir. Örneğin, `~/go` dizinini kullanabilirsiniz.

### Adım 2: Çevre Değişkenlerini Ayarlayın

`GOPATH`'i ayarlamak için aşağıdaki adımları izleyin:

#### macOS/Linux:

```sh
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

Bu komutları `.bashrc`, `.zshrc` veya `.profile` dosyanıza ekleyin.

#### Windows:

Sistem özelliklerinden çevre değişkenlerini açın ve yeni bir `GOPATH` değişkeni ekleyin. Değer olarak `C:\Users\<kullanıcı_adı>\go` kullanabilirsiniz.

## İlk Go Programının Yazılması (Writing the First Go Program)

### Adım 1: Proje Dizini Oluşturun

Öncelikle, proje dizininizi oluşturun:

```sh
mkdir -p $GOPATH/src/hello
cd $GOPATH/src/hello
```

### Adım 2: `main.go` Dosyasını Oluşturun

Bir metin editörü kullanarak `main.go` dosyasını oluşturun ve aşağıdaki kodu ekleyin:

```go
package main

import "fmt"

func main() {
    fmt.Println("Merhaba, Dünya!")
}
```

### Adım 3: Programı Çalıştırın

Terminal veya komut istemcisinde aşağıdaki komutu çalıştırarak programınızı çalıştırın:

```sh
go run main.go
```

Eğer her şey doğru ayarlandıysa, terminalde "Merhaba, Dünya!" çıktısını görmelisiniz.

Tebrikler! İlk Go programınızı başarıyla yazdınız ve çalıştırdınız.


## Teorikten Pratiğe Ödev:

1. Go'nun kurulumunu gerçekleştirin ve çalışma ortamınızı ayarlayın.
2. Çıktı olarak `Açık Üniversiteye Hoş Geldiniz!` yazan bir Go programı yazın.
3. Programı çalıştırarak çıktıyı kontrol edin.
4. Programı farklı bir çıktı ile güncelleyin ve tekrar çalıştırın.
5. Görevleri tamamladıktan sonra ödevinizi fork edilmiş reponuzda `ders1` klasörü altında `main.go` dosyası olarak kaydedin ve pull request oluşturun.
6. Pull request linkini ödev teslim formunda paylaşın.

## Sonraki Ders

Sonraki derste [Temel Go Söz Dizimi ve Veri Tipleri (Basic Go Syntax and Data Types)](../ders2) ve veri tiplerini öğreneceksiniz.