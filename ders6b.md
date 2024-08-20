# Ders 6: Auth ve JWT

Bu derste, Go dilinde JWT (JSON Web Token) kullanarak kimlik doğrulama işlemlerini gerçekleştireceğiz. JWT, web uygulamalarında kullanıcı kimlik doğrulaması için kullanılan bir standarttır. Bu derste, JWT'yi nasıl kullanacağımızı ve Go dilinde nasıl uygulayacağımızı öğreneceksiniz.

## İçindekiler

1. Authentication Nedir?
2. JWT Nedir?
3. JWT Oluşturma
4. JWT Doğrulama
5. JWT ile Kimlik Doğrulama
6. JWT ile Middleware Kullanımı

## Authentication Nedir?

Authentication, bir kullanıcının kimliğini doğrulama işlemidir. Web uygulamalarında, kullanıcıların kimliklerini doğrulamak için farklı yöntemler kullanılır. Bunlar arasında, kullanıcı adı ve şifre, OAuth, JWT gibi yöntemler bulunur.

## Authorization Nedir?

Authorization, bir kullanıcının belirli bir kaynağa erişim yetkisini doğrulama işlemidir. Authentication işleminden sonra, Authorization işlemi gerçekleştirilir. Bu işlemde, kullanıcının belirli bir kaynağa erişim yetkisi olup olmadığı kontrol edilir.

## JWT Nedir?

JWT (JSON Web Token), web uygulamalarında kullanıcı kimlik doğrulaması için kullanılan bir standarttır. JWT, JSON formatında veri taşıyan bir token'dır. Bu token, kullanıcı kimliğini doğrulamak için kullanılır.

JWT, üç bölümden oluşur: Header, Payload ve Signature. Header bölümünde, token'ın türü ve algoritması belirtilir. Payload bölümünde, kullanıcı bilgileri gibi veriler bulunur. Signature bölümünde, token'ın doğruluğunu kontrol etmek için kullanılan bir imza bulunur.

- Header: `{"alg": "HS256", "typ": "JWT"}`
- Payload: `{"sub": "1234567890", "name": "John Doe", "admin": true}`
- Signature: `HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)`

Bu üç bölüm, bir araya getirilerek bir JWT oluşturulur. Bu JWT, web uygulamalarında kimlik doğrulama işlemlerinde kullanılır.

## JWT Oluşturma

JWT oluşturmak için, `github.com/dgrijalva/jwt-go` paketini kullanabiliriz. Bu paket, JWT oluşturmak ve doğrulamak için gerekli fonksiyonları sağlar.

```go
package main

import (
    "fmt"
    "github.com/dgrijalva/jwt-go"
)

func main() {
    token := jwt.New(jwt.SigningMethodHS256)
    claims := token.Claims.(jwt.MapClaims)
    claims["sub"] = "1234567890"
    claims["name"] = "John Doe"
    claims["admin"] = true
    tokenString, err := token.SignedString([]byte("secret"))
    if err != nil {
        fmt.Println(err)
    }
    fmt.Println(tokenString)
}
```

Yukarıdaki kod, bir JWT oluşturur ve ekrana yazdırır. Bu

## JWT Doğrulama

JWT doğrulamak için, `github.com/dgrijalva/jwt-go` paketini kullanabiliriz. Bu paket, JWT oluşturmak ve doğrulamak için gerekli fonksiyonları sağlar.

```go
package main

import (
    "fmt"
    "github.com/dgrijalva/jwt-go"
)

func main(){
    tokenString := "token_string_here"
    token, err := jwt.Parse(tokenString, func(token *jwt.Token) (interface{}, error) {
        return []byte("secret"), nil
    })
    if err == nil && token.Valid {
        fmt.Println("Token is valid")
    } else {
        fmt.Println("Token is invalid")
    }

}

```

Yukarıdaki kod, bir JWT'yi doğrular ve ekrana yazdırır. Bu kod, JWT'nin geçerli olup olmadığını kontrol eder.

## JWT ile Kimlik Doğrulama

JWT'yi kullanarak kimlik doğrulama işlemlerini gerçekleştirebiliriz. Bu işlemde, kullanıcıya bir JWT verilir ve bu JWT, kullanıcının kimliğini doğrulamak için kullanılır.

```go
package main

import (
    "fmt"
    "net/http"
    "github.com/dgrijalva/jwt-go"
)

func homePage(w http.ResponseWriter, r *http.Request) {
    tokenString := r.Header.Get("Authorization")
    token, err := jwt.Parse(tokenString, func(token *jwt.Token) (interface{}, error) {
        return []byte("secret"), nil
    })
    if err == nil && token.Valid {
        fmt.Fprintf(w, "Token is valid")
    } else {
        fmt.Fprintf(w, "Token is invalid")
    }
}


func handleRequests() {
    http.HandleFunc("/", homePage)
    http.ListenAndServe(":8080", nil)
}

func main() {
    handleRequests()
}
```

Yukarıdaki kod, bir HTTP sunucusu oluşturur ve `/` endpoint'ine gelen istekleri işler. Bu kod, gelen isteklerdeki JWT'yi doğrular ve ekrana yazdırır.

### Access Token ve Refresh Token

JWT ile kimlik doğrulama işlemlerinde, genellikle iki tür token kullanılır: Access Token ve Refresh Token. Access Token, kullanıcının kimliğini doğrulamak için kullanılır ve kısa süreli geçerlidir. Refresh Token ise, Access Token'ın süresi dolduğunda yeni bir Access Token almak için kullanılır.

- Access Token: Kullanıcının kimliğini doğrulamak için kullanılır.
- Refresh Token: Access Token'ın süresi dolduğunda yeni bir Access Token almak için kullanılır.

## Neden Refresh Token Kullanmalıyız?
- Uzun ömürlü access token’lar, çalınmaları durumunda ciddi güvenlik riskleri oluşturabilir. Örneğin, bir saldırgan bu token’ı ele geçirirse, süresi dolana kadar yetkisiz erişim sağlayabilir.
- Kısa Ömürlü access token’lar (erişim jetonları), güvenlik nedeniyle iyi bir tercih olsada, kısa bir süre için geçerli olacağı için (örneğin, 15 dakika veya 1 saat). Ancak, kullanıcı uzun süreli oturumlarda kalmak istediğinde, bu sürenin dolması kullanıcıyı tekrar giriş yapmak zorunda bırakabilir.
- Refresh token, kullanıcıların uzun süreli oturumlarını korumalarına olanak tanırken, güvenlik risklerini de en aza indirir ve kullanıcıların yeni access token’lar almasını sağlar.

#### Çalışma Mimarisi

1. Kullanıcı, kullanıcı adı ve şifresi ile kimlik doğrulama işlemi gerçekleştirir.
2. Sunucu, kullanıcı bilgilerini kontrol eder ve Access Token ve Refresh Token oluşturur.
3. Kullanıcı, Access Token ve Refresh Token'ı alır ve bu token'ları kullanarak kimlik doğrulama işlemlerini gerçekleştirir.
4. Access Token'ın süresi dolduğunda, kullanıcı Refresh Token'ı kullanarak yeni bir Access Token alır.
5. Bu işlem, kullanıcı çıkış yapana kadar devam eder.
6. Kullanıcı çıkış yaptığında, Access Token ve Refresh Token geçersiz hale gelir.
7. Kullanıcı, tekrar kimlik doğrulama işlemi gerçirene kadar yeni bir Access Token ve Refresh Token alır.


## JWT ile Middleware Kullanımı

JWT'yi kullanarak middleware oluşturabiliriz. Bu middleware, gelen isteklerdeki JWT'yi doğrular ve isteği işler.

```go

package main

import (
    "fmt"
    "net/http"
    "github.com/dgrijalva/jwt-go"
)

func authMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        tokenString := r.Header.Get("Authorization")
        token, err := jwt.Parse(tokenString, func(token *jwt.Token) (interface{}, error) {
            return []byte("secret"), nil
        })
        if err == nil && token.Valid {
            next.ServeHTTP(w, r)
        } else {
            http.Error(w, "Unauthorized", http.StatusUnauthorized)
        }
    })
}

func homePage(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Home Page")
}

func handleRequests() {
    http.Handle("/", authMiddleware(http.HandlerFunc(homePage)))
    http.ListenAndServe(":8080", nil)
}

func main() {
    handleRequests()
}
```


Yukarıdaki kod, bir middleware oluşturur ve bu middleware, gelen isteklerdeki JWT'yi doğrular. Eğer JWT geçerli ise, isteği işler. Eğer JWT geçerli değil ise, `Unauthorized` hatası döner.


## Sonuç

Bu derste, Go dilinde JWT kullanarak kimlik doğrulama işlemlerini gerçekleştirdik. JWT, web uygulamalarında kullanıcı kimlik doğrulaması için kullanılan bir standarttır. Bu derste, JWT'yi nasıl kullanacağımızı ve Go dilinde nasıl uygulayacağımızı öğrendiniz. JWT'nin kullanımı, web uygulamalarında güvenli kimlik doğrulama işlemleri gerçekleştirmek için oldukça önemlidir. Bu derste öğrendiklerinizi uygulayarak, web uygulamalarınızda güvenli kimlik doğrulama işlemleri gerçekleştirebilirsiniz.