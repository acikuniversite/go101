# Ders 6: Haritalar (Maps)

Bu derste Go dilinde haritaları (maps) öğreneceksiniz.

## İçerik

- Harita Tanımlama (Map Definition)
- Harita İşlemleri (Map Operations)
- Harita Örnekleri (Map Examples)

## Harita Tanımlama (Map Definition)

Go dilinde haritalar, anahtar-değer (key-value) çiftlerini saklamak için kullanılır. Haritalar `map` anahtar kelimesi ile tanımlanır. Örneğin:

```go
package main

import "fmt"

func main() {
    var m map[string]int
    m = make(map[string]int)
    fmt.Println(m)
}
```

Yukarıdaki kodda, `m` adında bir harita tanımlanmıştır. Bu harita `string` türünde anahtarlar ve `int` türünde değerler saklar.

## Harita İşlemleri (Map Operations)

### Değer Ekleme ve Güncelleme

Bir haritaya değer eklemek veya güncellemek için anahtar kullanılır:

```go
m["a"] = 1
m["b"] = 2
m["a"] = 3 // "a" anahtarının değerini günceller
```

### Değer Okuma

Bir haritadan değer okumak için anahtar kullanılır:

```go
value := m["a"]
fmt.Println(value) // 3
```

### Değer Silme

Bir haritadan değer silmek için `delete` fonksiyonu kullanılır:

```go
delete(m, "a")
fmt.Println(m) // "a" anahtarı silinmiştir
```

### Anahtarın Varlığını Kontrol Etme

Bir anahtarın haritada olup olmadığını kontrol etmek için iki değer döndüren bir okuma işlemi yapılır:

```go
value, exists := m["b"]
if exists {
    fmt.Println("Anahtar mevcut:", value)
} else {
    fmt.Println("Anahtar mevcut değil")
}
```

## Harita Örnekleri (Map Examples)

### Basit Bir Harita Örneği

```go
package main

import "fmt"

func main() {
    m := make(map[string]int)
    m["k1"] = 7
    m["k2"] = 13

    fmt.Println("map:", m)

    v1 := m["k1"]
    fmt.Println("v1: ", v1)

    fmt.Println("len:", len(m))

    delete(m, "k2")
    fmt.Println("map:", m)

    _, prs := m["k2"]
    fmt.Println("prs:", prs)

    n := map[string]int{"foo": 1, "bar": 2}
    fmt.Println("map:", n)
}
```

Bu örnekte, çeşitli harita işlemleri gösterilmiştir: değer ekleme, okuma, silme ve anahtarın varlığını kontrol etme.


### Teorikten Pratiğe Ödevler

1. Öğrenci bilgilerini saklayan bir harita tanımlayın. Haritada öğrenci numarası (int) anahtar, öğrenci adı (string) ve öğrenci notu (float64) değerleri olsun. Haritaya en az 3 öğrenci ekleyin ve haritayı ekrana yazdırın. (Öğrenci numarası, adı ve notu farklı olmalıdır.)
2. Bir haritada, bir öğrencinin notunu güncelleyin ve haritayı ekrana yazdırın.
3. Bir haritadan bir öğrenciyi silin ve haritayı ekrana yazdırın.
4. Bir haritada, bir öğrencinin var olup olmadığını kontrol edin ve sonucu ekrana yazdırın.
5. Bir haritada, bir öğrencinin notunu okuyun ve ekrana yazdırın.

Sonraki Ders: [
