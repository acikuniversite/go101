# Mikroservisler ve Klasör Yapısı Golang

Bu ders, mikroservis mimarisini ve Go dilinde mikroservislerin nasıl yapılandırılacağını ele alır. Mikroservislerin avantajları, dezavantajları ve Go dilinde mikroservislerin nasıl oluşturulacağına dair örnekler sunar.

## Mikroservis Nedir?

Mikroservis mimarisi, büyük ve karmaşık uygulamaları daha küçük, bağımsız ve yönetilebilir servislere bölmeyi amaçlayan bir yazılım geliştirme yaklaşımıdır. Her mikroservis, belirli bir işlevi yerine getirir ve diğer mikroservislerle iletişim kurarak bütün bir sistemi oluşturur.

### Avantajları

- **Bağımsız Geliştirme:** Her mikroservis bağımsız olarak geliştirilebilir ve dağıtılabilir.
- **Ölçeklenebilirlik:** Mikroservisler, ihtiyaç duyulan bileşenlerin bağımsız olarak ölçeklenmesine olanak tanır.
- **Hata İzolasyonu:** Bir mikroservisteki hata, diğer mikroservisleri etkilemez.

### Dezavantajları

- **Dağıtık Sistem Karmaşıklığı:** Mikroservisler, dağıtık sistemlerin yönetiminde ek karmaşıklıklar getirir.
- **Ağ Gecikmeleri:** Mikroservisler arasındaki iletişim, ağ gecikmelerine neden olabilir.

## Go ile Mikroservis Oluşturma

Go, mikroservis geliştirme için ideal bir dildir. Basitliği, performansı ve güçlü standart kütüphanesi ile mikroservislerin hızlı ve verimli bir şekilde geliştirilmesini sağlar.

### Proje Klasör Yapısı

Mikroservis projelerinde düzenli bir klasör yapısı, kodun okunabilirliğini ve yönetilebilirliğini artırır. Aşağıda, tipik bir Go mikroservis projesi için önerilen klasör yapısı verilmiştir:

```
/my-microservice
|-- /cmd
|   |-- /service
|       |-- main.go
|-- /pkg
|   |-- /api
|   |   |-- api.go
|   |-- /service
|       |-- service.go
|-- /internal
|   |-- /config
|   |   |-- config.go
|   |-- /database
|       |-- database.go
|-- go.mod
|-- go.sum
```

### Örnek Mikroservis

Aşağıda, basit bir "Merhaba Dünya" mikroservisinin Go ile nasıl oluşturulacağına dair bir örnek verilmiştir.

#### main.go

```go
package main

import (
    "fmt"
    "net/http"
)

func helloHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Merhaba Dünya!")
}

func main() {
    http.HandleFunc("/", helloHandler)
    http.ListenAndServe(":8080", nil)
}
```

#### api.go

```go
package api

import (
    "fmt"
    "net/http"
)

func HelloHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Merhaba Dünya!")
}
```

#### service.go

```go
package service

import (
    "net/http"
    "my-microservice/pkg/api"
)

func Start() {
    http.HandleFunc("/", api.HelloHandler)
    http.ListenAndServe(":8080", nil)
}
```

#### config.go

```go
package config

import (
    "os"
)

func GetPort() string {
    port := os.Getenv("PORT")
    if port == "" {
        port = "8080"
    }
    return port
}
```

#### database.go

```go
package database

import (
    "database/sql"
    _ "github.com/lib/pq"
)

func Connect() (*sql.DB, error) {
    connStr := "user=username dbname=mydb sslmode=disable"
    return sql.Open("postgres", connStr)
}
```

Bu örnek, basit bir mikroservisin nasıl yapılandırılacağını ve Go dilinde nasıl kodlanacağını göstermektedir. Daha karmaşık mikroservisler için, bu temel yapı genişletilebilir ve ek bileşenler eklenebilir.
