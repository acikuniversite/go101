# Ders 21: Go ile JSON İşlemleri

Bu derste, Go dilinde JSON verilerini nasıl işleyebileceğinizi öğreneceksiniz. JSON verilerini nasıl serileştireceğinizi (marshal) ve serileştirilmiş JSON verilerini nasıl çözeceğinizi (unmarshal) göreceksiniz.

## İçindekiler

1. [Giriş](#giriş)
2. [JSON Serileştirme (Marshalling)](#json-serileştirme-marshalling)
3. [JSON Çözme (Unmarshalling)](#json-çözme-unmarshalling)
4. [Örnek Uygulama](#örnek-uygulama)
5. [Sonuç](#sonuç)

## Giriş

JSON (JavaScript Object Notation), veri değişimi için yaygın olarak kullanılan hafif bir formattır. Go dilinde JSON verilerini işlemek oldukça kolaydır ve `encoding/json` paketi bu işlemler için gerekli fonksiyonları sağlar.

## JSON Serileştirme (Marshalling)

Go dilinde bir veri yapısını JSON formatına dönüştürmek için `json.Marshal` fonksiyonu kullanılır. Aşağıda basit bir örnek bulunmaktadır:

```go
package main

import (
    "encoding/json"
    "fmt"
)

type Kisi struct {
    Ad    string `json:"ad"`
    Yas   int    `json:"yas"`
    Email string `json:"email"`
}

func main() {
    kisi := Kisi{
        Ad:    "Ahmet",
        Yas:   30,
        Email: "ahmet@example.com",
    }

    jsonData, err := json.Marshal(kisi)
    if err != nil {
        fmt.Println(err)
        return
    }

    fmt.Println(string(jsonData))
}
```

Yukarıdaki kodda, `Kisi` yapısı JSON formatına dönüştürülür ve ekrana yazdırılır.

## JSON Çözme (Unmarshalling)

JSON verilerini Go veri yapısına dönüştürmek için `json.Unmarshal` fonksiyonu kullanılır. Aşağıda basit bir örnek bulunmaktadır:

```go
package main

import (
    "encoding/json"
    "fmt"
)

type Kisi struct {
    Ad    string `json:"ad"`
    Yas   int    `json:"yas"`
    Email string `json:"email"`
}

func main() {
    jsonData := `{"ad":"Ahmet","yas":30,"email":"ahmet@example.com"}`

    var kisi Kisi
    err := json.Unmarshal([]byte(jsonData), &kisi)
    if err != nil {
        fmt.Println(err)
        return
    }

    fmt.Printf("%+v\n", kisi)
}
```

Yukarıdaki kodda, JSON formatındaki veri `Kisi` yapısına dönüştürülür ve ekrana yazdırılır.

## Örnek Uygulama

Aşağıda JSON serileştirme ve çözme işlemlerini bir arada gösteren bir örnek uygulama bulunmaktadır:

```go
package main

import (
    "encoding/json"
    "fmt"
    "log"
)

type Kisi struct {
    Ad    string `json:"ad"`
    Yas   int    `json:"yas"`
    Email string `json:"email"`
}

func main() {
    // JSON Serileştirme
    kisi := Kisi{
        Ad:    "Ahmet",
        Yas:   30,
        Email: "ahmet@example.com",
    }

    jsonData, err := json.Marshal(kisi)
    if err != nil {
        log.Fatalf("JSON serileştirme hatası: %s", err)
    }

    fmt.Println("Serileştirilmiş JSON:", string(jsonData))

    // JSON Çözme
    var yeniKisi Kisi
    err = json.Unmarshal(jsonData, &yeniKisi)
    if err != nil {
        log.Fatalf("JSON çözme hatası: %s", err)
    }

    fmt.Printf("Çözülmüş veri: %+v\n", yeniKisi)
}
```

Bu örnekte, `Kisi` yapısı önce JSON formatına dönüştürülür, ardından tekrar `Kisi` yapısına çözülür.

## Sonuç

Bu derste, Go dilinde JSON verilerini nasıl serileştireceğinizi ve çözeceğinizi öğrendiniz. JSON işlemleri, veri değişimi ve API geliştirme gibi birçok alanda oldukça kullanışlıdır. Bu temel bilgileri kullanarak daha karmaşık JSON işlemleri gerçekleştirebilirsiniz.
