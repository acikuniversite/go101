# Ders 6: API Geliştirme

Bu derste, Go dilinde API geliştirme konusunu ele alacağız. API'ler, uygulamalar arasında veri alışverişini sağlayan arayüzlerdir. Bu dersin sonunda, basit bir RESTful API oluşturmayı öğreneceksiniz.

## İçindekiler

1. [Giriş](#giriş)
2. [HTTP Sunucusu Kurulumu](#http-sunucusu-kurulumu)
3. [RESTful API Nedir?](#restful-api-nedir)
4. [Basit Bir RESTful API Oluşturma](#basit-bir-restful-api-oluşturma)
5. [CRUD İşlemleri](#crud-işlemleri)
6. [Ortam Değişkenleri ve Yapılandırma](#ortam-değişkenleri-ve-yapılandırma)
7. [Hata Yönetimi](#hata-yönetimi)
8. [Test Etme](#test-etme)
9. [Sonuç](#sonuç)

## Giriş

API'ler, uygulamalar arasında veri alışverişini sağlayan arayüzlerdir. RESTful API'ler, HTTP protokolünü kullanarak veri alışverişi yapar ve genellikle JSON formatında veri döner.

## HTTP Sunucusu Kurulumu

Go dilinde HTTP sunucusu kurmak oldukça basittir. `net/http` paketini kullanarak bir HTTP sunucusu oluşturabiliriz.

```go
package main

import (
    "fmt"
    "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Merhaba, Dünya!")
}

func main() {
    http.HandleFunc("/", handler)
    http.ListenAndServe(":8080", nil)
}
```

Yukarıdaki kod, 8080 portunda çalışan basit bir HTTP sunucusu oluşturur.

## RESTful API Nedir?

REST (Representational State Transfer), HTTP protokolünü kullanarak veri alışverişi yapmayı sağlayan bir mimari stildir. RESTful API'ler, HTTP metodlarını (GET, POST, PUT, DELETE) kullanarak CRUD (Create, Read, Update, Delete) işlemlerini gerçekleştirir.

## Basit Bir RESTful API Oluşturma

Şimdi, basit bir RESTful API oluşturalım. Bu API, kullanıcı bilgilerini saklayacak ve CRUD işlemlerini gerçekleştirecek.

```go
package main

import (
    "encoding/json"
    "net/http"
    "strconv"
    "github.com/gorilla/mux"
)

type User struct {
    ID    int    `json:"id"`
    Name  string `json:"name"`
    Email string `json:"email"`
}

var users []User

func getUsers(w http.ResponseWriter, r *http.Request) {
    json.NewEncoder(w).Encode(users)
}

func getUser(w http.ResponseWriter, r *http.Request) {
    params := mux.Vars(r)
    id, _ := strconv.Atoi(params["id"])
    for _, user := range users {
        if user.ID == id {
            json.NewEncoder(w).Encode(user)
            return
        }
    }
    http.NotFound(w, r)
}

func createUser(w http.ResponseWriter, r *http.Request) {
    var user User
    _ = json.NewDecoder(r.Body).Decode(&user)
    user.ID = len(users) + 1
    users = append(users, user)
    json.NewEncoder(w).Encode(user)
}

func updateUser(w http.ResponseWriter, r *http.Request) {
    params := mux.Vars(r)
    id, _ := strconv.Atoi(params["id"])
    for index, user := range users {
        if user.ID == id {
            users = append(users[:index], users[index+1:]...)
            var updatedUser User
            _ = json.NewDecoder(r.Body).Decode(&updatedUser)
            updatedUser.ID = id
            users = append(users, updatedUser)
            json.NewEncoder(w).Encode(updatedUser)
            return
        }
    }
    http.NotFound(w, r)
}

func deleteUser(w http.ResponseWriter, r *http.Request) {
    params := mux.Vars(r)
    id, _ := strconv.Atoi(params["id"])
    for index, user := range users {
        if user.ID == id {
            users = append(users[:index], users[index+1:]...)
            break
        }
    }
    json.NewEncoder(w).Encode(users)
}

func main() {
    router := mux.NewRouter()
    router.HandleFunc("/users", getUsers).Methods("GET")
    router.HandleFunc("/users/{id}", getUser).Methods("GET")
    router.HandleFunc("/users", createUser).Methods("POST")
    router.HandleFunc("/users/{id}", updateUser).Methods("PUT")
    router.HandleFunc("/users/{id}", deleteUser).Methods("DELETE")
    http.ListenAndServe(":8080", router)
}
```

Yukarıdaki kod, kullanıcı bilgilerini saklayan ve CRUD işlemlerini gerçekleştiren basit bir RESTful API oluşturur.

## CRUD İşlemleri

CRUD işlemleri, veri tabanında yapılan temel işlemlerdir. Bu işlemler şunlardır:

- **Create**: Yeni bir kayıt oluşturur.
- **Read**: Mevcut kayıtları okur.
- **Update**: Mevcut bir kaydı günceller.
- **Delete**: Mevcut bir kaydı siler.

## Ortam Değişkenleri ve Yapılandırma

API'lerinizi yapılandırmak için ortam değişkenlerini kullanabilirsiniz. Bu, API'nizin farklı ortamlarda (geliştirme, test, üretim) çalışmasını kolaylaştırır.

```go
import (
    "os"
    "log"
)

func main() {
    port := os.Getenv("PORT")
    if port == "" {
        log.Fatal("Port is not set.")
    }
    http.ListenAndServe(":"+port, router)
}
```

## Hata Yönetimi

API'lerde hata yönetimi önemlidir. Kullanıcılara anlamlı hata mesajları döndürmek, API'nizin kullanılabilirliğini artırır.

```go
func getUser(w http.ResponseWriter, r *http.Request) {
    params := mux.Vars(r)
    id, err := strconv.Atoi(params["id"])
    if err != nil {
        http.Error(w, "Invalid user ID", http.StatusBadRequest)
        return
    }
    for _, user := range users {
        if user.ID == id {
            json.NewEncoder(w).Encode(user)
            return
        }
    }
    http.NotFound(w, r)
}
```
### Gin Framework

Gin, Go dilinde hızlı ve basit bir web framework'tür. Gin, RESTful API'ler oluşturmak için kullanışlıdır.

```go
import "github.com/gin-gonic/gin"

func main() {
    r := gin.Default()
    r.GET("/users", getUsers)
    r.GET("/users/:id", getUser)
    r.POST("/users", createUser)
    r.PUT("/users/:id", updateUser)
    r.DELETE("/users/:id", deleteUser)
    r.Run(":8080")
}
```

### Sonraki Ders

[# Ders 18: Testler ve Performans](../ders18/README.md)