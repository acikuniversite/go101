# Ders 29: Go ile Mikroservis Mimarisi

Bu derste, Go dilinde mikroservis mimarisi ile uygulama geliştirmeyi öğreneceksiniz. Mikroservislerin nasıl tasarlanacağını, geliştirileceğini ve yönetileceğini göreceksiniz.

## İçindekiler

1. Mikroservis Mimarisi Nedir?
2. Go ile Mikroservis Geliştirme
3. Proje Yapısı
4. Örnek Kodlar
5. Test ve Dağıtım

## 1. Mikroservis Mimarisi Nedir?

Mikroservis mimarisi, büyük ve karmaşık uygulamaları daha küçük, bağımsız ve yönetilebilir servislere bölme yaklaşımıdır. Her mikroservis, belirli bir işlevi yerine getirir ve diğer mikroservislerle iletişim kurarak bütünsel bir uygulama oluşturur.

## 2. Go ile Mikroservis Geliştirme

Go, mikroservis geliştirme için ideal bir dildir çünkü hızlı, verimli ve taşınabilir uygulamalar oluşturmanıza olanak tanır. Go'nun güçlü standart kütüphanesi ve geniş topluluk desteği, mikroservislerinizi hızlı bir şekilde geliştirmenizi sağlar.

## 3. Proje Yapısı

Mikroservis projeleriniz için aşağıdaki gibi bir yapı önerilir:

```
/project
    /service1
        main.go
        handler.go
        ...
    /service2
        main.go
        handler.go
        ...
    /shared
        utils.go
        ...
```

## 4. Örnek Kodlar

### main.go

```go
package main

import (
    "log"
    "net/http"
    "github.com/gorilla/mux"
)

func main() {
    r := mux.NewRouter()
    r.HandleFunc("/hello", HelloHandler).Methods("GET")
    log.Fatal(http.ListenAndServe(":8080", r))
}
```

### handler.go

```go
package main

import (
    "fmt"
    "net/http"
)

func HelloHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, World!")
}
```

## 5. Test ve Dağıtım

Mikroservislerinizi test etmek için Go'nun `testing` paketini kullanabilirsiniz. Dağıtım için ise Docker ve Kubernetes gibi araçlar kullanarak mikroservislerinizi konteynerize edebilir ve ölçeklendirebilirsiniz.

### Test Örneği

```go
package main

import (
    "net/http"
    "net/http/httptest"
    "testing"
)

func TestHelloHandler(t *testing.T) {
    req, err := http.NewRequest("GET", "/hello", nil)
    if err != nil {
        t.Fatal(err)
    }

    rr := httptest.NewRecorder()
    handler := http.HandlerFunc(HelloHandler)
    handler.ServeHTTP(rr, req)

    if status := rr.Code; status != http.StatusOK {
        t.Errorf("handler returned wrong status code: got %v want %v", status, http.StatusOK)
    }

    expected := "Hello, World!"
    if rr.Body.String() != expected {
        t.Errorf("handler returned unexpected body: got %v want %v", rr.Body.String(), expected)
    }
}
```

### Dockerfile

```dockerfile
FROM golang:1.16-alpine

WORKDIR /app

COPY . .

RUN go build -o main .

CMD ["./main"]
```

### Kubernetes Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: go-microservice
spec:
  replicas: 3
  selector:
    matchLabels:
      app: go-microservice
  template:
    metadata:
      labels:
        app: go-microservice
    spec:
      containers:
      - name: go-microservice
        image: your-docker-image
        ports:
        - containerPort: 8080
```
