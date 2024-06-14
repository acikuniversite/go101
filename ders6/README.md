# Ders 6: Diziler ve Dilimler (Arrays and Slices)

Bu derste Go dilinde diziler (arrays) ve dilimleri (slices) öğreneceksiniz.

## İçerik

- Dizi tanımlama (array definition)
- Dilim tanımlama (slice definition)
- Dilim işlemleri (slice operations)

## Dizi Tanımlama (Array Definition)

Go dilinde diziler sabit uzunlukta ve aynı türden elemanlar içeren veri yapılarıdır. Bir dizi tanımlamak için aşağıdaki sözdizimini kullanabilirsiniz:

```go
var dizi [5]int
```

Bu örnekte, `dizi` adında ve 5 tamsayı (int) eleman içeren bir dizi tanımlanmıştır. Dizinin elemanlarına indeks numarası ile erişebilirsiniz:

```go
dizi[0] = 10
fmt.Println(dizi[0]) // 10
```

## Dilim Tanımlama (Slice Definition)

Dilimler, dizilerin dinamik boyutlu versiyonlarıdır. Bir dilim tanımlamak için aşağıdaki sözdizimini kullanabilirsiniz:

```go
var dilim []int
```

Dilimler, dizilerden farklı olarak başlangıçta belirli bir uzunluğa sahip değildir. Eleman ekleyerek dilimi büyütebilirsiniz:

```go
dilim = append(dilim, 10)
fmt.Println(dilim[0]) // 10
```

## Dilim İşlemleri (Slice Operations)

Dilimler üzerinde çeşitli işlemler yapabilirsiniz. Örneğin, bir dilimin belirli bir kısmını almak için dilimleme operatörünü kullanabilirsiniz:

```go
dizi := [5]int{1, 2, 3, 4, 5}
dilim := dizi[1:4]
fmt.Println(dilim) // [2 3 4]
```

Dilimler ayrıca uzunluk (length) ve kapasite (capacity) bilgilerine sahiptir:

```go
fmt.Println(len(dilim)) // 3
fmt.Println(cap(dilim)) // 4
```

Bu dersin sonunda, Go dilinde diziler ve dilimler ile ilgili temel kavramları öğrenmiş olacaksınız. Bu konular, Go dilinde veri yapıları ve algoritmalar üzerinde çalışırken oldukça önemlidir.
