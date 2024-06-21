# Ders 23: Go'da Clean Architecture

Bu derste, Go dilinde Clean Architecture (Temiz Mimari) prensiplerini nasıl uygulayabileceğinizi öğreneceksiniz. Clean Architecture, yazılım projelerinde bağımlılıkları minimize ederek daha sürdürülebilir ve test edilebilir kod yazmayı amaçlar.

## İçindekiler

1. [Clean Architecture Nedir?](#clean-architecture-nedir)
2. [Katmanlar ve Sorumluluklar](#katmanlar-ve-sorumluluklar)
3. [Go'da Clean Architecture Uygulaması](#goda-clean-architecture-uygulaması)
4. [Örnek Proje](#örnek-proje)

## Clean Architecture Nedir?

Clean Architecture, Robert C. Martin (Uncle Bob) tarafından ortaya atılan bir yazılım mimarisi prensibidir. Bu prensip, yazılım projelerinde bağımlılıkları minimize ederek daha sürdürülebilir ve test edilebilir kod yazmayı amaçlar. Clean Architecture, dört ana katmandan oluşur:

- **Entities (Varlıklar)**: İş kurallarını ve nesnelerini içerir.
- **Use Cases (Kullanım Durumları)**: Uygulamanın iş mantığını içerir.
- **Interface Adapters (Arayüz Adaptörleri)**: Veri ve iş mantığı arasındaki dönüşümleri içerir.
- **Frameworks & Drivers (Çerçeveler ve Sürücüler)**: Dış bağımlılıkları içerir.

## Katmanlar ve Sorumluluklar

### Entities (Varlıklar)

Varlıklar, iş kurallarını ve nesnelerini içerir. Bu katman, uygulamanın en iç katmanıdır ve diğer katmanlardan bağımsızdır.

### Use Cases (Kullanım Durumları)

Kullanım durumları, uygulamanın iş mantığını içerir. Bu katman, varlıklar katmanına bağımlıdır ancak arayüz adaptörleri ve çerçeveler katmanına bağımlı değildir.

### Interface Adapters (Arayüz Adaptörleri)

Arayüz adaptörleri, veri ve iş mantığı arasındaki dönüşümleri içerir. Bu katman, kullanım durumları katmanına bağımlıdır ve çerçeveler katmanına veri sağlar.

### Frameworks & Drivers (Çerçeveler ve Sürücüler)

Çerçeveler ve sürücüler, dış bağımlılıkları içerir. Bu katman, en dış katmandır ve diğer katmanlara bağımlıdır.

## Go'da Clean Architecture Uygulaması

Go dilinde Clean Architecture uygulamak için, her katmanı ayrı bir paket olarak organize edebilirsiniz. Örneğin:

```
/entities
    /user.go
/usecases
    /user_usecase.go
/interfaces
    /user_controller.go
    /user_presenter.go
/frameworks
    /database
        /user_repository.go
    /web
        /router.go
```

## Örnek Proje

Aşağıda, Go dilinde Clean Architecture prensiplerini uygulayan basit bir kullanıcı yönetim sistemi örneği bulunmaktadır.

### entities/user.go

```go
package entities

type User struct {
    ID    int
    Name  string
    Email string
}
```

### usecases/user_usecase.go

```go
package usecases

import "example.com/project/entities"

type UserUseCase struct {
    UserRepository UserRepository
}

func (uc *UserUseCase) CreateUser(name, email string) (*entities.User, error) {
    user := &entities.User{
        Name:  name,
        Email: email,
    }
    err := uc.UserRepository.Save(user)
    if err != nil {
        return nil, err
    }
    return user, nil
}

type UserRepository interface {
    Save(user *entities.User) error
}
```

### interfaces/user_controller.go

```go
package interfaces

import (
    "example.com/project/usecases"
    "net/http"
)

type UserController struct {
    UserUseCase usecases.UserUseCase
}

func (uc *UserController) CreateUser(w http.ResponseWriter, r *http.Request) {
    name := r.FormValue("name")
    email := r.FormValue("email")
    user, err := uc.UserUseCase.CreateUser(name, email)
    if err != nil {
        http.Error(w, err.Error(), http.StatusInternalServerError)
        return
    }
    w.WriteHeader(http.StatusCreated)
    w.Write([]byte("User created: " + user.Name))
}
```

### frameworks/database/user_repository.go

```go
package database

import (
    "example.com/project/entities"
    "errors"
)

type UserRepository struct {
    users map[int]*entities.User
}

func NewUserRepository() *UserRepository {
    return &UserRepository{
        users: make(map[int]*entities.User),
    }
}

func (repo *UserRepository) Save(user *entities.User) error {
    if user.ID == 0 {
        user.ID = len(repo.users) + 1
    }
    repo.users[user.ID] = user
    return nil
}
```

### frameworks/web/router.go

```go
package web

import (
    "example.com/project/interfaces"
    "net/http"
)

func NewRouter(userController *interfaces.UserController) *http.ServeMux {
    router := http.NewServeMux()
    router.HandleFunc("/users", userController.CreateUser)
    return router
}
```

Bu örnek proje, Go dilinde Clean Architecture prensiplerini uygulayarak basit bir kullanıcı yönetim sistemi oluşturur. Bu yapı, kodun daha sürdürülebilir, test edilebilir ve genişletilebilir olmasını sağlar.
