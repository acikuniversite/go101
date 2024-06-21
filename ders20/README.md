# Ders 20: Go'da JWT ve Auth

Bu dersimizde, Go dilinde JSON Web Token (JWT) kullanarak kimlik doğrulama işlemlerini nasıl gerçekleştireceğimizi öğreneceğiz. JWT, kullanıcıların kimliklerini doğrulamak ve güvenli bir şekilde bilgi alışverişi yapmak için kullanılan bir standarttır.

## İçindekiler

1. JWT Nedir?
2. JWT Nasıl Çalışır?
3. Go'da JWT Kullanımı
4. JWT ile Kimlik Doğrulama Örneği
5. Sonuç

## 1. JWT Nedir?

JWT (JSON Web Token), iki taraf arasında güvenli bilgi alışverişi için kullanılan bir JSON tabanlı erişim belirtecidir. JWT, üç parçadan oluşur: header (başlık), payload (yük) ve signature (imza).

## 2. JWT Nasıl Çalışır?

JWT, kullanıcı kimlik bilgilerini içeren bir token oluşturur ve bu token'ı imzalar. İmzalanmış token, kullanıcıya gönderilir ve kullanıcı bu token'ı her istekte sunar. Sunucu, token'ı doğrulayarak kullanıcının kimliğini doğrular.

## 3. Go'da JWT Kullanımı

Go dilinde JWT kullanmak için `github.com/dgrijalva/jwt-go` paketini kullanacağız. Bu paket, JWT oluşturma ve doğrulama işlemlerini kolaylaştırır.

### Kurulum

```sh
go get github.com/dgrijalva/jwt-go
```

### JWT Oluşturma

```go
package main

import (
    "fmt"
    "time"
    "github.com/dgrijalva/jwt-go"
)

func createToken() (string, error) {
    mySigningKey := []byte("secret")

    token := jwt.NewWithClaims(jwt.SigningMethodHS256, jwt.MapClaims{
        "username": "user1",
        "exp":      time.Now().Add(time.Hour * 1).Unix(),
    })

    tokenString, err := token.SignedString(mySigningKey)
    if err != nil {
        return "", err
    }

    return tokenString, nil
}

func main() {
    token, err := createToken()
    if err != nil {
        fmt.Println("Error creating token:", err)
        return
    }

    fmt.Println("Generated Token:", token)
}
```

### JWT Doğrulama

```go
package main

import (
    "fmt"
    "github.com/dgrijalva/jwt-go"
)

func validateToken(tokenString string) (*jwt.Token, error) {
    mySigningKey := []byte("secret")

    token, err := jwt.Parse(tokenString, func(token *jwt.Token) (interface{}, error) {
        return mySigningKey, nil
    })

    if err != nil {
        return nil, err
    }

    if !token.Valid {
        return nil, fmt.Errorf("invalid token")
    }

    return token, nil
}

func main() {
    tokenString := "your.jwt.token.here"

    token, err := validateToken(tokenString)
    if err != nil {
        fmt.Println("Error validating token:", err)
        return
    }

    fmt.Println("Token is valid:", token)
}
```

## 4. JWT ile Kimlik Doğrulama Örneği

Aşağıda, JWT kullanarak basit bir kimlik doğrulama örneği bulunmaktadır. Bu örnekte, kullanıcı giriş yapar ve bir JWT alır. Daha sonra, bu token'ı kullanarak korunan bir kaynağa erişir.

### Kullanıcı Girişi ve Token Alma

```go
package main

import (
    "fmt"
    "net/http"
    "github.com/dgrijalva/jwt-go"
    "time"
)

var mySigningKey = []byte("secret")

func login(w http.ResponseWriter, r *http.Request) {
    token := jwt.NewWithClaims(jwt.SigningMethodHS256, jwt.MapClaims{
        "username": "user1",
        "exp":      time.Now().Add(time.Hour * 1).Unix(),
    })

    tokenString, err := token.SignedString(mySigningKey)
    if err != nil {
        http.Error(w, "Error creating token", http.StatusInternalServerError)
        return
    }

    w.Write([]byte(tokenString))
}

func home(w http.ResponseWriter, r *http.Request) {
    tokenString := r.Header.Get("Authorization")

    token, err := validateToken(tokenString)
    if err != nil {
        http.Error(w, "Invalid token", http.StatusUnauthorized)
        return
    }

    w.Write([]byte(fmt.Sprintf("Welcome %s!", token.Claims.(jwt.MapClaims)["username"])))
}

func main() {
    http.HandleFunc("/login", login)
    http.HandleFunc("/home", home)

    http.ListenAndServe(":8080", nil)
}
```

## 5. Sonuç

Bu dersimizde, Go dilinde JWT kullanarak kimlik doğrulama işlemlerini nasıl gerçekleştireceğimizi öğrendik. JWT, güvenli ve verimli bir kimlik doğrulama yöntemi sunar. Bu bilgileri kullanarak kendi projelerinizde güvenli kimlik doğrulama mekanizmaları oluşturabilirsiniz.
