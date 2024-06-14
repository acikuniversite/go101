# Ders 4: Kontrol Yapıları (Control Structures)

Bu derste Go dilinde kontrol yapıları (control structures) öğreneceksiniz.

## İçerik

- If-else yapıları (if-else structures)
- Switch-case yapıları (switch-case structures)
- Döngüler (loops)
# Ders 4: Diziler ve Dilimler (Arrays and Slices)

Bu derste Go dilinde diziler (arrays) ve dilimleri (slices) öğreneceksiniz.

## İçerik

- Diziler (Arrays)
- Dilimler (Slices)
- Dilim Operasyonları (Slice Operations)
- Çok Boyutlu Diziler (Multidimensional Arrays)

## Diziler (Arrays)

Diziler, sabit uzunlukta ve aynı türdeki elemanlardan oluşan veri yapılarıdır. Go dilinde diziler aşağıdaki şekilde tanımlanır:

### Dizi Tanımlama

```go
package main

import "fmt"

func main() {
    var numbers [5]int
    numbers[0] = 1
    numbers[1] = 2
    numbers[2] = 3
    numbers[3] = 4
    numbers[4] = 5

    fmt.Println(numbers)
}
```

Yukarıdaki örnekte, `numbers` adında bir tamsayı dizisi tanımlanmış ve elemanları atanmıştır.

## Dilimler (Slices)

Dilimler, dizilerin dinamik versiyonlarıdır ve uzunlukları değiştirilebilir. Dilimler, dizilerin bir alt kümesini temsil eder.

### Dilim Tanımlama

```go
package main

import "fmt"

func main() {
    numbers := []int{1, 2, 3, 4, 5}
    fmt.Println(numbers)
}
```

Yukarıdaki örnekte, `numbers` adında bir dilim tanımlanmış ve elemanları atanmıştır.

### Dilim Operasyonları (Slice Operations)

Dilimler üzerinde çeşitli işlemler yapabilirsiniz. Örneğin, dilimlerin uzunluğunu ve kapasitesini öğrenebilir, dilimlere eleman ekleyebilirsiniz.

```go
package main

import "fmt"

func main() {
    numbers := []int{1, 2, 3}
    numbers = append(numbers, 4, 5)

    fmt.Println("Dilim:", numbers)
    fmt.Println("Uzunluk:", len(numbers))
    fmt.Println("Kapasite:", cap(numbers))
}
```

Yukarıdaki örnekte, `append` fonksiyonu kullanılarak `numbers` dilimine elemanlar eklenmiştir.

## Çok Boyutlu Diziler (Multidimensional Arrays)

Go dilinde çok boyutlu diziler de tanımlanabilir. Örneğin, iki boyutlu bir dizi aşağıdaki şekilde tanımlanır:

### İki Boyutlu Dizi Tanımlama

```go
package main

import "fmt"

func main() {
    var matrix [2][3]int
    matrix[0][0] = 1
    matrix[0][1] = 2
    matrix[0][2] = 3
    matrix[1][0] = 4
    matrix[1][1] = 5
    matrix[1][2] = 6

    fmt.Println(matrix)
}
```

Yukarıdaki örnekte, `matrix` adında iki boyutlu bir tamsayı dizisi tanımlanmış ve elemanları atanmıştır.

Bu dersin sonunda, Go dilinde diziler ve dilimleri nasıl kullanacağınızı öğrenmiş olacaksınız. Bir sonraki derste daha ileri seviye konulara geçeceğiz.
