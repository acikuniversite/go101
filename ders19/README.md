# Ders 19: Go'da Docker ve CI/CD

Bu derste, Go projelerinde Docker kullanımı ve CI/CD (Continuous Integration/Continuous Deployment) süreçlerini nasıl uygulayacağımızı öğreneceğiz.

## İçindekiler

1. Docker Nedir?
2. Dockerfile Oluşturma
3. Docker Compose Kullanımı
4. CI/CD Nedir?
5. GitHub Actions ile CI/CD Pipeline Oluşturma

## 1. Docker Nedir?

Docker, uygulamaları konteynerler içinde çalıştırmamızı sağlayan bir platformdur. Konteynerler, uygulamanın tüm bağımlılıklarıyla birlikte çalışmasını sağlar ve bu sayede uygulamanın her ortamda aynı şekilde çalışmasını garanti eder.

## 2. Dockerfile Oluşturma

Dockerfile, Docker imajlarını oluşturmak için kullanılan bir betik dosyasıdır. Aşağıda basit bir Go uygulaması için Dockerfile örneği bulunmaktadır:

```dockerfile
# Base image olarak Go'yu kullan
FROM golang:1.16

# Çalışma dizinini ayarla
WORKDIR /app

# Go mod ve sum dosyalarını kopyala
COPY go.mod go.sum ./

# Bağımlılıkları indir
RUN go mod download

# Uygulama dosyalarını kopyala
COPY . .

# Uygulamayı derle
RUN go build -o main .

# Uygulamayı çalıştır
CMD ["./main"]
```

## 3. Docker Compose Kullanımı

Docker Compose, çoklu konteyner uygulamalarını tanımlamak ve çalıştırmak için kullanılır. Aşağıda basit bir `docker-compose.yml` dosyası örneği bulunmaktadır:

```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "8080:8080"
```

## 4. CI/CD Nedir?

CI/CD, yazılım geliştirme süreçlerini otomatikleştiren bir yöntemdir. CI, kodun sürekli olarak entegre edilmesini sağlarken, CD kodun sürekli olarak dağıtılmasını sağlar.

## 5. GitHub Actions ile CI/CD Pipeline Oluşturma

GitHub Actions, CI/CD süreçlerini GitHub üzerinde otomatikleştirmek için kullanılan bir araçtır. Aşağıda basit bir GitHub Actions workflow dosyası örneği bulunmaktadır:

```yaml
name: Go CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Go
      uses: actions/setup-go@v2
      with:
        go-version: 1.16

    - name: Install dependencies
      run: go mod download

    - name: Build
      run: go build -v ./...

    - name: Run tests
      run: go test -v ./...
```

Bu dersin sonunda, Docker ve CI/CD süreçlerini Go projelerinizde nasıl kullanacağınızı öğrenmiş olacaksınız.

### Sonraki Ders

[# Ders 20: Go'da JWT ve Auth](../ders20/README.md)