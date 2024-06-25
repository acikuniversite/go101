# Ders 13: Reflection ve Dinamik Tipler (Reflection and Dynamic Types)

Bu derste, Go programlama dilinde reflection ve dinamik tiplerin nasıl kullanıldığını inceleyeceğiz. Reflection, çalışma zamanında türler ve arayüzler hakkında bilgi edinmenizi sağlar ve dinamik tipler ile birlikte, esnek ve güçlü kodlar yazmanıza olanak tanır. Ele alınacak konular şunlardır:

1.	Reflection Nedir?
2.	Reflection Kullanımı
3.	Struct Alanlarına Erişim ve Manipülasyon
4.	Dinamik Tipler ve interface{} Kullanımı
5.	Reflection ile Tip Kontrolü ve Dönüşümler
6.	Reflection Performansına Dikkat
7.	Gerçek Dünya Uygulamaları


### 1. Reflection Nedir?

Reflection, çalışma zamanında bir programın kendi yapısı ve davranışı hakkında bilgi edinme yeteneğidir. Go dilinde, reflect paketi kullanılarak gerçekleştirilir.

### 2. Reflection Kullanımı

reflect paketini kullanarak tür ve değer bilgilerini elde edebilirsiniz. Örneğin:

```go
package main

import (
    "fmt"
    "reflect"
)

func main() {
    var x float64 = 3.4
    t := reflect.TypeOf(x)
    v := reflect.ValueOf(x)
    fmt.Println("Type:", t)
    fmt.Println("Value:", v)
}
```

Bu kodda, TypeOf ve ValueOf fonksiyonları kullanılarak x değişkeninin türü ve değeri elde edilir.

### 3. Struct Alanlarına Erişim ve Manipülasyon

Reflection kullanarak struct alanlarına erişebilir ve bu alanları manipüle edebilirsiniz. Örneğin:

```go
package main

import (
    "fmt"
    "reflect"
)

type Person struct {
    Name string
    Age  int
}

func main() {
    p := Person{"Alice", 30}
    v := reflect.ValueOf(&p).Elem()

    typeOfP := v.Type()

    for i := 0; i < v.NumField(); i++ {
        f := v.Field(i)
        fmt.Printf("%d: %s %s = %v\n", i, typeOfP.Field(i).Name, f.Type(), f.Interface())
    }

    v.FieldByName("Age").SetInt(31)
    fmt.Println("Updated Person:", p)
}
```

Bu kodda, Person struct’ının alanlarına reflection ile erişilir ve Age alanı güncellenir.

### 4. Dinamik Tipler ve interface{} Kullanımı

Go’da interface{} türü, herhangi bir türdeki değeri tutabilir. Bu, dinamik tiplerle çalışmayı kolaylaştırır.

```go
package main

import "fmt"

func PrintAnything(val interface{}) {
    fmt.Println("Value:", val)
}

func main() {
    PrintAnything(42)
    PrintAnything("Hello")
    PrintAnything([]int{1, 2, 3})
}
```

Bu kodda, PrintAnything fonksiyonu interface{} türünde bir parametre alır ve herhangi bir türdeki değeri yazdırabilir.

### 5. Reflection ile Tip Kontrolü ve Dönüşümler

Reflection kullanarak bir değerin türünü kontrol edebilir ve gerekli dönüşümleri yapabilirsiniz.

```go
package main

import (
    "fmt"
    "reflect"
)

func main() {
    var x interface{} = 3.4

    if reflect.TypeOf(x).Kind() == reflect.Float64 {
        fmt.Println("x is a float64")
    }

    if v, ok := x.(float64); ok {
        fmt.Println("x as float64:", v)
    }
}
```
Bu kodda, reflect.TypeOf kullanılarak x değişkeninin türü kontrol edilir ve tür dönüşümü yapılır.

6. Reflection Performansına Dikkat

Reflection güçlü bir araç olmasına rağmen, performans maliyetleri olabilir. Bu nedenle, reflection kullanırken performans üzerinde dikkatli olunmalıdır.

7. Gerçek Dünya Uygulamaları

Reflection, özellikle framework geliştirmede, veri doğrulama ve dönüştürmede yaygın olarak kullanılır. Örneğin, JSON serileştirme ve seriden çıkarma işlemlerinde reflection kullanılır.

```go
package main

import (
    "encoding/json"
    "fmt"
    "reflect"
)

type Person struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}

func main() {
    p := Person{"Bob", 25}
    data, _ := json.Marshal(p)
    fmt.Println("JSON:", string(data))

    var p2 Person
    json.Unmarshal(data, &p2)
    fmt.Println("Struct:", p2)
}
```

Bu kodda, Person struct’ı JSON’a dönüştürülür ve tekrar struct’a seriden çıkarılır.

Bu ders, Go’da reflection ve dinamik tiplerin gücünü ve esnekliğini anlamanıza yardımcı olacaktır.

### Sonraki Ders

[# Mikroservisler ve Klasör Yapısı Golang
](../ders14/README.md)