# Ders 28: Go ile CLI Uygulamaları Geliştirme

Bu derste, Go dilinde komut satırı arayüzü (CLI) uygulamaları geliştirmeyi öğreneceksiniz. Bayraklar, argümanlar ve alt komutlar gibi CLI bileşenlerini nasıl kullanabileceğinizi göreceksiniz.

## İçindekiler

1. [Giriş](#giriş)
2. [Bayraklar ve Argümanlar](#bayraklar-ve-argümanlar)
3. [Alt Komutlar](#alt-komutlar)
4. [Örnek Proje](#örnek-proje)
5. [Sonuç](#sonuç)

## Giriş

Komut satırı arayüzü (CLI) uygulamaları, kullanıcıların terminal üzerinden programlarla etkileşime geçmesini sağlar. Go dilinde CLI uygulamaları geliştirmek oldukça basittir ve bu ders boyunca temel kavramları ve pratik örnekleri inceleyeceğiz.

## Bayraklar ve Argümanlar

Go dilinde bayraklar ve argümanlar `flag` paketi kullanılarak yönetilir. Bayraklar, komut satırında belirli seçenekleri belirtmek için kullanılırken, argümanlar genellikle dosya adları veya diğer girdiler gibi ek bilgileri içerir.

### Bayrak Kullanımı

```go
package main

import (
    "flag"
    "fmt"
)

func main() {
    // Bayrak tanımlama
    name := flag.String("name", "Dünya", "Greet someone by name")
    age := flag.Int("age", 0, "Specify age")

    // Bayrakları ayrıştırma
    flag.Parse()

    // Bayrakları kullanma
    fmt.Printf("Merhaba %s! Yaşınız %d.\n", *name, *age)
}
```

### Argüman Kullanımı

```go
package main

import (
    "flag"
    "fmt"
    "os"
)

func main() {
    // Bayrakları tanımlama ve ayrıştırma
    flag.Parse()

    // Argümanları alma
    args := os.Args[1:]

    // Argümanları kullanma
    for i, arg := range args {
        fmt.Printf("Argüman %d: %s\n", i, arg)
    }
}
```

## Alt Komutlar

Alt komutlar, bir CLI uygulamasının farklı işlevlerini yönetmek için kullanılır. Örneğin, `git` komutunda `clone`, `commit` ve `push` gibi alt komutlar bulunur. Go dilinde alt komutları yönetmek için `flag` paketini kullanabiliriz.

### Alt Komut Kullanımı

```go
package main

import (
    "flag"
    "fmt"
    "os"
)

func main() {
    // Alt komutları tanımlama
    helloCmd := flag.NewFlagSet("hello", flag.ExitOnError)
    byeCmd := flag.NewFlagSet("bye", flag.ExitOnError)

    // Alt komut bayraklarını tanımlama
    helloName := helloCmd.String("name", "Dünya", "Greet someone by name")
    byeName := byeCmd.String("name", "Dünya", "Say goodbye to someone by name")

    // Alt komutları ayrıştırma
    if len(os.Args) < 2 {
        fmt.Println("Alt komut belirtmelisiniz: hello veya bye")
        os.Exit(1)
    }

    switch os.Args[1] {
    case "hello":
        helloCmd.Parse(os.Args[2:])
        fmt.Printf("Merhaba %s!\n", *helloName)
    case "bye":
        byeCmd.Parse(os.Args[2:])
        fmt.Printf("Hoşçakal %s!\n", *byeName)
    default:
        fmt.Println("Bilinmeyen alt komut:", os.Args[1])
        os.Exit(1)
    }
}
```

## Örnek Proje

Bu bölümde, yukarıda öğrendiğimiz kavramları kullanarak basit bir CLI uygulaması geliştireceğiz. Bu uygulama, kullanıcının adını ve yaşını alarak bir karşılama mesajı yazdıracak ve alt komutlar kullanarak farklı mesajlar gösterecek.

### Proje Yapısı

```
cli-app/
├── main.go
```

### main.go

```go
package main

import (
    "flag"
    "fmt"
    "os"
)

func main() {
    // Alt komutları tanımlama
    greetCmd := flag.NewFlagSet("greet", flag.ExitOnError)
    infoCmd := flag.NewFlagSet("info", flag.ExitOnError)

    // Alt komut bayraklarını tanımlama
    greetName := greetCmd.String("name", "Dünya", "Greet someone by name")
    infoName := infoCmd.String("name", "Dünya", "Provide info about someone")
    infoAge := infoCmd.Int("age", 0, "Specify age")

    // Alt komutları ayrıştırma
    if len(os.Args) < 2 {
        fmt.Println("Alt komut belirtmelisiniz: greet veya info")
        os.Exit(1)
    }

    switch os.Args[1] {
    case "greet":
        greetCmd.Parse(os.Args[2:])
        fmt.Printf("Merhaba %s!\n", *greetName)
    case "info":
        infoCmd.Parse(os.Args[2:])
        fmt.Printf("%s, yaşınız %d.\n", *infoName, *infoAge)
    default:
        fmt.Println("Bilinmeyen alt komut:", os.Args[1])
        os.Exit(1)
    }
}
```

## Sonuç

Bu derste, Go dilinde CLI uygulamaları geliştirmenin temellerini öğrendik. Bayraklar, argümanlar ve alt komutlar gibi temel bileşenleri inceledik ve basit bir CLI uygulaması geliştirdik. Bu bilgilerle, kendi CLI araçlarınızı geliştirebilir ve terminal üzerinden kullanıcılarla etkileşime geçebilirsiniz.

### Sonraki Ders

[# Ders 29: Go ile Mikroservis Mimarisi](../ders29/README.md)