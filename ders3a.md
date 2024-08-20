# Ders 3: Diziler ve Dilimler (Arrays and Slices)

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

<details>
<summary><b>Ders Alıştırması: 9 boşluğu olan bir dilim tanımlayın ve bu dilime 1'den 9'a kadar olan sayıları ekleyin. Sonucu ekrana yazdırın.</b></summary>

```go
package main

import "fmt"

func main()  {
    dizi := make([]int, 9)
	    for i := 0; i < 9; i++ {
        dizi[i] = i + 1
    }
    fmt.Println(dizi)
}

```

</details>

### [9]int ile make([]int, 9) arasındaki fark nedir?

- `make([]int, 9)` ifadesi, 9 elemanlı bir dilim oluşturur ve uzunluğu 9'dur.
- `[9]int` ifadesi, 9 elemanlı bir dizi oluşturur ve uzunluğu 9'dur.

ikisi arasında en büyük fark, dilimlerin dinamik uzunluğa sahip olmasıdır. Bu nedenle, dilimlerin uzunluğu ve kapasitesi değiştirilebilirken, dizilerin uzunluğu sabittir.

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

<details>
<summary> Alıştırma: 0 elemanlı bir dilim tanımlayın ve bu dilime elemanlar ekleyin.</summary>
    
```go
package main

import "fmt"

func main() {
	numbers := make([]int, 0, 5)
	var girdi int = 0
	for {
		fmt.Scanln(&girdi)
		if girdi == -1 {
			break
		}
		numbers = append(numbers, girdi)
	}
	fmt.Println("Dilim:", numbers)
	fmt.Println("Uzunluk:", len(numbers))
	fmt.Println("Kapasite:", cap(numbers))
}

```

</details>
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

<details>
<summary>Alıştırma: bir ev krokisi çizmek için 6x6 boyutunda bir matris oluşturun ve bu matrisin transpozunu alın. Sonuçları ekrana yazdırın.</summary>

```go
package main

import "fmt"

func def()  {
    kroki := [6][6]int{
		{1,1, 1, 1, 1, 1},
		{1,0, 0, 0, 0, 1},
		{1,0, 1, 1, 0, 0},
		{1,0, 1, 1, 0, 1},
		{1,0, 0, 0, 0, 1},
		{1,1, 1, 1, 1, 1},
    }
}
```
</details>

## Teorikten Pratiğe Ödev:

1. Bir tamsayı dizisi tanımlayın ve bu dizinin elemanlarını ekrana yazdırın.
2. Bir dilim tanımlayın ve bu dilime elemanlar ekleyerek ekrana yazdırın.
3. İki boyutlu bir tamsayı dizisi tanımlayın ve bu dizinin elemanlarını ekrana yazdırın.
4. Bir dilim üzerinde dilim uzunluğu ve kapasitesi hakkında denemeler yapın.
5. İki boyutlu bir dizi üzerinde işlemler yaparak sonuçları ekrana yazdırın.
6. 6x6 boyutunda bir matris oluşturun bu matrisin Transpozunu alın ve Transpozu ile orijinal matrisi topalayın. Sonucu ekrana yazdırın.

6x6 Matris:
```
1 1 1 1 1 1
2 4 6 8 10 12
3 9 12 15 18 21
4 16 20 24 28 32
5 25 30 35 40 45
6 36 42 48 54 60
```


Sonraki Ders: [Ders 5: Gelişmiş Fonksiyonlar (Advanced Functions)](../ders5x/README.md)



