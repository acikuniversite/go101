# Ders 5: Gelişmiş Fonksiyonlar (Advanced Functions)

Bu derste iki ders önce gördüğümüz fonksiyonlar konusunun daha derinlemesine incelenmesi hedeflenmektedir. Fonksiyonlar konusunda daha karmaşık yapılar ve fonksiyonlar üzerinde işlemler ele alınacaktır.

## Değişken Sayıda Parametre Alan Fonksiyonlar (Variadic Functions)

Bir fonksiyonun kaç tane parametre alacağını önceden belirlemek yerine, fonksiyonun değişken sayıda parametre almasını sağlayabiliriz. Bu tür fonksiyonlara _variadic functions_ denir ve Go dilinde bu tür fonksiyonlar tanımlanırken parametre tipinin önüne üç nokta (`...`) işareti konulur.

Örneğin, aşağıdaki fonksiyon `topla` fonksiyonu, değişken sayıda parametre alabilmektedir:

```go

func topla(sayilar ...int) int {
    toplam := 0
    for _, sayi := range sayilar {
        toplam += sayi
    }
    return toplam
}

func main() {
    fmt.Println(topla(1, 2, 3, 4, 5)) // 15
    fmt.Println(topla(1, 2, 3)) // 6
    fmt.Println(topla(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)) // 55
}
```

Yukarıdaki örnekte `topla` fonksiyonu, değişken sayıda parametre alabilmekteden ve bu parametrelerin toplamını döndüren bir fonksiyondur. Fonksiyonun parametre tipi `int` olmasına rağmen, fonksiyonun kaç tane parametre alacağı önceden belirlenmemiştir. Fonksiyon çağrıldığında, fonksiyona verilen parametreler bir _slice_ olarak ele alınır ve fonksiyon bu _slice_ üzerinde dolaşarak işlem yapar.

## İsimlendirilmiş Dönüş Değerleri (Named Return Values)

Go dilinde bir fonksiyonun dönüş değerlerine isim verilebilir. Bu sayede fonksiyonun dönüş değerlerini döndürürken, dönüş değerlerinin ne anlama geldiği daha açık bir şekilde ifade edilebilir. İsimlendirilmiş dönüş değerleri, fonksiyonun başında tanımlanır ve fonksiyonun sonunda `return` ifadesi kullanılarak dönüş değerlerine değer atanır.

Örneğin, aşağıdaki `bol` fonksiyonu, iki sayının bölümünü ve kalanını döndüren bir fonksiyondur:

```go
func bol(bolunen, bolen int) (bolum, kalan int) {
    bolum = bolunen / bolen
    kalan = bolunen % bolen
    return
}

func main() {
    bolum, kalan := bol(10, 3)
    fmt.Printf("Bölüm: %d, Kalan: %d\n", bolum, kalan) // Bölüm: 3, Kalan: 1
}
```

Yukarıdaki örnekte `bol` fonksiyonu, iki sayının bölümünü ve kalanını döndüren bir fonksiyondur. Fonksiyonun dönüş değerlerine isim verilmiş ve fonksiyon sonunda `return` ifadesi kullanılarak dönüş değerlerine değer atanmıştır. Fonksiyon çağrıldığında, dönüş değerlerine isim verilmiş olduğu için, dönüş değerlerine isimler üzerinden erişilebilir.

## Defer İfadesi

Go dilinde `defer` ifadesi, bir fonksiyonun sonunda çalıştırılması gereken bir ifadeyi belirtmek için kullanılır. `defer` ifadesi, bir fonksiyonun sonunda çalıştırılacak ifadeyi belirtir ve bu ifade, fonksiyonun sonunda çalıştırılır. `defer` ifadesi, fonksiyonun sonunda çalıştırılacak ifadeyi belirtmek için kullanılır ve bu ifade, fonksiyonun sonunda çalıştırılır.

Örneğin, aşağıdaki örnekte `dosyaKapat` fonksiyonu, bir dosyayı kapatmak için kullanılan bir fonksiyondur. Bu fonksiyon, dosya kapatma işlemini gerçekleştirdikten sonra dosyanın kapatıldığını ekrana yazdırmaktadır:

```go
func dosyaKapat() {
    fmt.Println("Dosya kapatılıyor...")
	defer fmt.Println("Dosya kapatıldı.")
}

func main() {
    fmt.Println("Dosya açılıyor...")
    defer dosyaKapat()
    fmt.Println("Dosya işlemleri yapılıyor...")
}
```

Yukarıdaki örnekte `dosyaKapat` fonksiyonu, bir dosyayı kapatmak için kullanılan bir fonksiyondur. Bu fonksiyon, dosya kapatma işlemini gerçekleştirdikten sonra dosyanın kapatıldığını ekrana yazdırmaktadır. `dosyaKapat` fonksiyonu, `defer` ifadesi ile `main` fonksiyonunda çağrılmıştır. `defer` ifadesi, `dosyaKapat` fonksiyonunu `main` fonksiyonunun sonunda çalıştırmak üzere işaretlemiştir. Bu nedenle, `main` fonksiyonu çalıştırıldığında, `dosyaKapat` fonksiyonu `main` fonksiyonunun sonunda çalıştırılmıştır.

## Fonksiyonlar İçinde Fonksiyonlar (Nested Functions)

Go dilinde bir fonksiyon, başka bir fonksiyonun içinde tanımlanabilir. Bu tür fonksiyonlara _nested functions_ denir. _Nested functions_ tanımlanırken, fonksiyonun adı belirtilmez ve fonksiyonun adı yerine sadece fonksiyonun parametreleri ve dönüş değerleri belirtilir.

Örneğin, aşağıdaki örnekte `topla` fonksiyonu içinde `cikar` fonksiyonu tanımlanmıştır:

```go
func topla(a, b int) int {
    cikar := func(x, y int) int {
        return x - y
    }
    return a + b + cikar(a, b)
}

func main() {
    fmt.Println(topla(5, 3)) // 10
}
```

Yukarıdaki örnekte `topla` fonksiyonu içinde `cikar` fonksiyonu tanımlanmıştır. `cikar` fonksiyonu, iki sayıyı çıkaran bir fonksiyondur ve `topla` fonksiyonu içinde tanımlanmıştır. `topla` fonksiyonu, iki sayıyı topladıktan sonra, `cikar` fonksiyonunu çağırarak iki sayıyı çıkarmış ve sonucu döndürmüştür.

## Fonksiyon girdisi olarak fonksiyonlar (Functions as Parameters)

Go dilinde bir fonksiyon, başka bir fonksiyonun girdisi olarak kullanılabilir. Bu tür fonksiyonlara _higher-order functions_ denir. _Higher-order functions_ tanımlanırken, fonksiyonun parametre olarak alacağı fonksiyonun tipi belirtilir ve bu fonksiyon, fonksiyonun içinde çağrılabilir.

Örneğin, aşağıdaki örnekte `hesapla` fonksiyonu, iki sayı ve bir işlem fonksiyonu alarak, bu iki sayı üzerinde işlem yapar:

```go
func topla(a, b int) int {
    return a + b
}

func cikar(a, b int) int {
    return a - b
}

func hesapla(a, b int, f func(int, int) int) int {
    return f(a, b)
}

func main() {
    fmt.Println(hesapla(5, 3, topla)) // 8
    fmt.Println(hesapla(5, 3, cikar)) // 2
}
```

Yukarıdaki örnekte `hesapla` fonksiyonu, iki sayı ve bir işlem fonksiyonu alarak, bu iki sayı üzerinde işlem yapar. `hesapla` fonksiyonu, `f` adında bir fonksiyon parametresi alır ve bu fonksiyonu `a` ve `b` parametreleri üzerinde çağırarak sonucu döndürür. `main` fonksiyonunda, `hesapla` fonksiyonu, `topla` ve `cikar` fonksiyonları ile çağrılmıştır.


## Derinlemesine Recursive Fonksiyonlar (Deeply Recursive Functions)

Go dilinde fonksiyonlar, kendilerini çağırarak derinlemesine (recursive) çalışabilirler. Ancak, Go dilinde derinlemesine çalışan fonksiyonlar için bir sınırlama vardır. Bu sınırlama, derinlemesine çalışan fonksiyonların yığın (stack) boyutunu sınırlar ve bu sınırlama, derinlemesine çalışan fonksiyonların yığın taşmasını (stack overflow) önler.

Örneğin, aşağıdaki örnekte `faktoriyel` fonksiyonu, bir sayının faktöriyelini hesaplamak için derinlemesine çalışan bir fonksiyondur:

```go
func faktoriyel(n int) int {
    if n == 0 {
        return 1
    }
    return n * faktoriyel(n-1)
}

func main() {
    fmt.Println(faktoriyel(5)) // 120
}
```

### Teorikten Pratiğe Ödev:

1. Bir tamsayı dizisi alarak bu dizinin elemanlarını toplayan bir fonksiyon yazın. Bu fonksiyon, değişken sayıda parametre alabilmelidir.
2. İki adet çalışan maaş hesaplama (brüt maaş, net maaş) fonksiyonu yazın. Bu fonksiyonlar, çalışanın brüt maaşını alarak net maaşını hesaplamalıdır. Brüt maaş, net maaş hesaplama fonksiyonuna parametre olarak verilmelidir.`fmt.Println(25000, hesaplaNetMaas)`
3. Bir fonksiyon alarak bu fonksiyonu 10 defa çalıştıran bir fonksiyon yazın.
4. Bir fonksiyon alarak bu fonksiyonu belirli bir süre aralığında çalıştıran bir fonksiyon yazın. (Örneğin, 1 saniye aralıklarla 10 defa çalıştırma)

[Ders 6: Haritalar (Maps))](../ders5/README.md)


