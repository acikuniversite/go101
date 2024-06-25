# Ders 18: Testler ve Performans

Bu derste, Go dilinde test yazma ve performans optimizasyonu konularını ele alacağız. Testler, yazılım geliştirme sürecinin önemli bir parçasıdır ve kodunuzun doğru çalıştığını doğrulamanıza yardımcı olur. Performans optimizasyonu ise uygulamanızın daha hızlı ve verimli çalışmasını sağlar.

## İçindekiler

1. [Test Yazma](#test-yazma)
2. [Benchmarking](#benchmarking)
3. [Profiling](#profiling)
4. [Performans İyileştirme](#performans-iyileştirme)

## Test Yazma

Go dilinde test yazmak oldukça basittir. `testing` paketini kullanarak test fonksiyonları oluşturabilirsiniz. Test fonksiyonları `Test` ile başlamalı ve `*testing.T` parametresini almalıdır.

### Örnek Test Fonksiyonu

```go
package main

import (
    "testing"
)

func Add(a, b int) int {
    return a + b
}

func TestAdd(t *testing.T) {
    result := Add(2, 3)
    if result != 5 {
        t.Errorf("Add(2, 3) = %d; want 5", result)
    }
}
```

Yukarıdaki örnekte, `Add` fonksiyonunu test eden bir test fonksiyonu oluşturduk. `go test` komutunu kullanarak testleri çalıştırabilirsiniz.

## Benchmarking

Benchmarking, kodunuzun performansını ölçmek için kullanılır. `testing` paketinde `Benchmark` fonksiyonları oluşturabilirsiniz. Bu fonksiyonlar `Benchmark` ile başlamalı ve `*testing.B` parametresini almalıdır.

### Örnek Benchmark Fonksiyonu

```go
package main

import (
    "testing"
)

func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(2, 3)
    }
}
```

Yukarıdaki örnekte, `Add` fonksiyonunun performansını ölçen bir benchmark fonksiyonu oluşturduk. `go test -bench .` komutunu kullanarak benchmark testlerini çalıştırabilirsiniz.

## Profiling

Profiling, uygulamanızın hangi kısımlarının en fazla kaynak tükettiğini belirlemenize yardımcı olur. Go dilinde profiling yapmak için `pprof` paketini kullanabilirsiniz.

### CPU Profiling

```go
package main

import (
    "os"
    "runtime/pprof"
    "testing"
)

func TestMain(m *testing.M) {
    f, err := os.Create("cpu.prof")
    if err != nil {
        panic(err)
    }
    pprof.StartCPUProfile(f)
    defer pprof.StopCPUProfile()
    m.Run()
}
```

Yukarıdaki örnekte, CPU profilini oluşturmak için `pprof` paketini kullandık. `go test -cpuprofile=cpu.prof` komutunu kullanarak CPU profilini oluşturabilirsiniz.

## Performans İyileştirme

Performans iyileştirme, uygulamanızın daha hızlı ve verimli çalışmasını sağlar. Profiling sonuçlarına göre, en fazla kaynak tüketen kısımları optimize edebilirsiniz.

### Örnek Performans İyileştirme

```go
package main

import (
    "testing"
)

func OptimizedAdd(a, b int) int {
    return a + b
}

func BenchmarkOptimizedAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        OptimizedAdd(2, 3)
    }
}
```

Yukarıdaki örnekte, `Add` fonksiyonunu optimize ettik ve performansını ölçtük. Profiling sonuçlarına göre, daha fazla iyileştirme yapabilirsiniz.


### Sonraki Ders

[# Ders 19: Go'da Docker ve CI/CD](../ders19/README.md)