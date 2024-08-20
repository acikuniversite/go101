# Ders 5: Yapılar (Structs)

Bu derste Go dilinde yapıları (structs) öğreneceksiniz.

## İçerik

- Yapı Tanımlama (Struct Definition)
- Yapı İşlemleri (Struct Operations)
- Yapı Örnekleri (Struct Examples)

## Yapı Tanımlama (Struct Definition)

Go dilinde yapı (struct), birden fazla veriyi bir arada tutmak için kullanılan veri yapısıdır. Aşağıda bir yapı tanımlama örneği bulunmaktadır:

```go
type Kisi struct {
    Isim    string
    Yas     int
    Meslek  string
}
```

Bu örnekte, `Kisi` adında bir yapı tanımlanmıştır. Bu yapı, `Isim`, `Yas` ve `Meslek` adında üç alana sahiptir.

## Yapı İşlemleri (Struct Operations)

Yapılar üzerinde çeşitli işlemler yapabilirsiniz. Örneğin, bir yapı örneği oluşturabilir ve alanlarına değer atayabilirsiniz:

```go
func main() {
    kisi := Kisi{
        Isim:   "Ahmet",
        Yas:    30,
        Meslek: "Mühendis",
    }

    fmt.Println(kisi)
}
```

Bu kod parçasında, `Kisi` yapısının bir örneği oluşturulmuş ve `Isim`, `Yas` ve `Meslek` alanlarına değerler atanmıştır.

### Yapı Alanlarına Erişim

Yapı alanlarına erişmek için nokta (`.`) operatörünü kullanabilirsiniz:

```go
fmt.Println("İsim:", kisi.Isim)
fmt.Println("Yaş:", kisi.Yas)
fmt.Println("Meslek:", kisi.Meslek)
```

### Yapı Fonksiyonları

Yapılar için fonksiyonlar tanımlayabilirsiniz. Örneğin, `Kisi` yapısı için bir fonksiyon tanımlayalım:

```go
func (k Kisi) Bilgi() string {
    return fmt.Sprintf("%s, %d yaşında, %s olarak çalışıyor.", k.Isim, k.Yas, k.Meslek)
}

func main() {
    kisi := Kisi{
        Isim:   "Ahmet",
        Yas:    30,
        Meslek: "Mühendis",
    }

    fmt.Println(kisi.Bilgi())
}
```

Bu örnekte, `Kisi` yapısı için `Bilgi` adında bir fonksiyon tanımlanmıştır. Bu fonksiyon, yapının alanlarını kullanarak bir bilgi metni döndürür.

## Yapı Örnekleri (Struct Examples)

Aşağıda farklı yapı örnekleri bulunmaktadır:

### Adres Yapısı

```go
type Adres struct {
    Sokak  string
    Sehir  string
    Ulke   string
}
```

### Kitap Yapısı

```go
type Kitap struct {
    Baslik     string
    Yazar      string
    SayfaSayisi int
}
```

Bu ders kapsamında, Go dilinde yapıların nasıl tanımlandığını, kullanıldığını ve üzerinde nasıl işlemler yapıldığını öğrendiniz. Yapılar, verileri organize etmek ve yönetmek için güçlü bir araçtır.

### Teorikten Pratiğe Ödevler (Sektörel & Gerçek Hayat Örneği ile)

Ozet: Bir öğrenci ve öğrertmen ilişkisi tutan struct tanımlama

1. Bir öğrenci yapısı tanımlayın. Bu yapı, öğrencinin adını, numarasını ve notlarını saklamalıdır ve öğretmen bilgisini de içermelidir.
2. Her öğretmen bir öğrencidir ve her öğretmenin bir çok öğrencisi olabilir (struct ogrenciler array tutmalı)
3. Öğrenci ve öğretmen yapısını kullanarak öğrenci ve öğretmen bilgilerini ekrana yazdırın.
4. AddOgrenci ve AddOgretmen fonksiyonları ile öğrenci ve öğretmen ekleyin.
5. 
