# Ders 26: Go ile Unit Test Yazma

Bu derste, Go dilinde birim testlerini nasıl yazabileceğinizi öğreneceksiniz. Test paketlerini kullanarak kodunuzu nasıl test edebileceğinizi ve testlerinizi nasıl organize edebileceğinizi göreceksiniz.

## İçindekiler

1. [Giriş](#giriş)
2. [Test Paketini Kullanma](#test-paketini-kullanma)
3. [Basit Birim Testi Yazma](#basit-birim-testi-yazma)
4. [Tablolu Testler](#tablolu-testler)
5. [Test Kapsamı](#test-kapsamı)
6. [Mocking](#mocking)
7. [Sonuç](#sonuç)

## Giriş

Birim testleri, yazılım geliştirme sürecinin önemli bir parçasıdır. Go dilinde, `testing` paketini kullanarak birim testleri yazabiliriz. Bu ders boyunca, Go dilinde birim testlerinin nasıl yazılacağını ve organize edileceğini öğreneceksiniz.

## Test Paketini Kullanma

Go dilinde test yazmak için `testing` paketini kullanırız. Bu paket, test fonksiyonlarını tanımlamak ve çalıştırmak için gerekli araçları sağlar.

```go
import "testing"
```

## Basit Birim Testi Yazma

Basit birim testleri yazmak için, test fonksiyonlarını `*_test.go` dosyalarında tanımlarız. Test fonksiyonları `Test` ile başlamalı ve `*testing.T` parametresini almalıdır.

Örnek olarak, basit bir toplama fonksiyonunu test edelim:

```go
// main.go
package main

func Add(a, b int) int {
    return a + b
}
```

```go
// main_test.go
package main

import "testing"

func TestAdd(t *testing.T) {
    result := Add(2, 3)
    expected := 5

    if result != expected {
        t.Errorf("Add(2, 3) = %d; want %d", result, expected)
    }
}
```

## Tablolu Testler

Tablolu testler, birden fazla test durumunu tek bir test fonksiyonunda tanımlamak için kullanılır. Bu yöntem, testlerinizi daha düzenli ve okunabilir hale getirir.

```go
func TestAddTableDriven(t *testing.T) {
    var tests = []struct {
        a, b int
        want int
    }{
        {1, 1, 2},
        {1, 2, 3},
        {2, 2, 4},
        {2, 3, 5},
    }

    for _, tt := range tests {
        testname := fmt.Sprintf("%d+%d", tt.a, tt.b)
        t.Run(testname, func(t *testing.T) {
            result := Add(tt.a, tt.b)
            if result != tt.want {
                t.Errorf("got %d, want %d", result, tt.want)
            }
        })
    }
}
```

## Test Kapsamı

Test kapsamı, kodunuzun ne kadarının test edildiğini gösterir. Go'da test kapsamını ölçmek için `go test -cover` komutunu kullanabilirsiniz.

```sh
go test -cover
```

## Mocking

Mocking, bağımlılıkları izole etmek ve test etmek için sahte nesneler kullanma işlemidir. Go'da mocking yapmak için çeşitli kütüphaneler kullanabilirsiniz, örneğin `gomock`.

## Sonuç

Bu derste, Go dilinde birim testlerinin nasıl yazılacağını öğrendiniz. Test paketini kullanarak basit ve tablolu testler yazabilir, test kapsamını ölçebilir ve mocking yapabilirsiniz. Birim testleri, kodunuzun kalitesini artırmak ve hataları erken tespit etmek için önemlidir.
