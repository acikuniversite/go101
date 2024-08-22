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

### Ogrenci Yoklama Yapısı

Hikaye: Acikuniversite öğrencileri, derslerine katılım durumlarını takip etmek için bir yoklama sistemi oluşturmak istiyorlar. Bu sistemde, her öğrenci için bir yoklama yapısı tanımlanmalıdır.

```go
type Ogrenci struct {
    Ad        string
    Soyad     string
    DiscordID string
}

type Ders struct {
    Ad      string
}

type Yoklama struct {
    Ogrenci *Ogrenci // *Ogrenci  yerine Ogrenci da kullanılabilir, bellek kullanımı artar. direction ihtiyacı olursa Ogrenci kullanılabilir.  
    Ders    *Ders   // *Ders yerine Ders da kullanılabilir, bellek kullanımı artar. direction ihtiyacı olursa Ders kullanılabilir. 
    Tarih   time.Time
    Durum   bool
}

// y Yoklama dedik çünkü yoklama yapısının içindeki sadece değeri okuyacağız.
func (y Yoklama) String() string {
    return fmt.Sprintf("%s adlı öğrenci, %s dersine %s tarihinde %v.", y.Ogrenci.Ad, y.Ders.Ad, y.Tarih, y.Durum)
}

// y *Yoklama dememiz gerekiyor çünkü yoklama yapısının içindeki durumu değiştireceğiz.
func (y *Yoklama) YoklamaAl() {
    y.Durum = true
}

func (o *Ogrenci) YoklamaYap(d *Ders, t time.Time) *Yoklama {
    return &Yoklama{
        Ogrenci: o,
        Ders:    d,
        Tarih:   t,
        Durum:   false,
    }
}

func main() {
    ders := &Ders{Ad: "Go Programlama Dili"}
    yoklamalar := []*Yoklama{}
	tarih := time.Now()
	ogrenciler := []*Ogrenci{
        {Ad: "Ahmet", Soyad: "Yılmaz", DiscordID: "123456"},
        {Ad: "Mehmet", Soyad: "Kaya", DiscordID: "654321"},
        {Ad: "Ayşe", Soyad: "Demir", DiscordID: "987654"},
    }

    for _, ogrenci := range ogrenciler {
        yoklama := ogrenci.YoklamaYap(ders, tarih)
        yoklamalar = append(yoklamalar, yoklama)
    }

    for _, yoklama := range yoklamalar {
        fmt.Println(yoklama)
    }
}
```


| Koşul | Direction | Indirection                                               |
| --- | --- |-----------------------------------------------------------|
| Bellek Kullanımı | Yüksek | Düşük                                                     |
| Küçük Struct'lar | Uygun | Genellikle Gereksiz                                       |
| Büyük Struct'lar | Bellek Tüketimi Artar | Bellek Tasarrufu Sağlar                                   |
| Kopyalama Maliyeti | Yüksek | Düşük                                                     |
| Veri Paylaşımı | Kopyalar Bağımsız | Aynı Veriyi Paylaşır                                      |
| Veri Bütünlüğü | Daha Güvenli (Bağımsız Kopyalar) | Dikkat Gerektirir (Tüm Referanslar Etkilenir)             |
| Kodun Basitliği ve Okunabilirlik | Daha Basit ve Anlaşılır | Karmaşık (Pointer Yönetimi Gerekir)                       |
| Performans (Erişim Hızı) | Doğrudan Erişim, Daha Hızlı | Dolaylı Erişim, Biraz Daha Yavaş(İhmal edilebilir oranda) |
| Nil (Geçersiz) Pointer Riski | Yok | Var                                                       |

