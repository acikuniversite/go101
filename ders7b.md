# Ders 7: Golang ile Veritabanı İşlemleri (Database Operations)

Bu derste, Golang kullanarak veritabanı işlemlerini nasıl gerçekleştireceğimizi öğreneceğiz. Aşağıdaki konulara değineceğiz:

1. Veritabanı Bağlantısı Kurma
2. CRUD (Create, Read, Update, Delete) İşlemleri
3. SQL Sorguları Çalıştırma
4. Hata Yönetimi

## 1. Veritabanı Bağlantısı Kurma

Golang'da veritabanı bağlantısı kurmak için `database/sql` paketini ve ilgili sürücüyü kullanacağız. Örneğin, PostgreSQL kullanıyorsak `lib/pq` sürücüsünü kullanabiliriz.

```go
package main

import (
    "database/sql"
    _ "github.com/lib/pq"
    "log"
)

const (
    host     = "localhost"
    port     = 5432
    user     = "yourusername"
    password = "yourpassword"
    dbname   = "yourdbname"
)

func main() {
    psqlInfo := fmt.Sprintf("host=%s port=%d user=%s password=%s dbname=%s sslmode=disable",
        host, port, user, password, dbname)

    db, err := sql.Open("postgres", psqlInfo)
    if err != nil {
        log.Fatal(err)
    }
    defer db.Close()

    err = db.Ping()
    if err != nil {
        log.Fatal(err)
    }

    fmt.Println("Successfully connected!")
}
```

## 2. CRUD İşlemleri

### Create (Oluşturma)

```go
sqlStatement := `
INSERT INTO users (age, email, first_name, last_name)
VALUES ($1, $2, $3, $4)
RETURNING id`
id := 0
err := db.QueryRow(sqlStatement, 30, "test@example.com", "John", "Doe").Scan(&id)
if err != nil {
    log.Fatal(err)
}
fmt.Println("New record ID is:", id)
```

### Read (Okuma)

```go
sqlStatement := `SELECT id, email FROM users WHERE id=$1;`
var email string
var id int
row := db.QueryRow(sqlStatement, 1)
switch err := row.Scan(&id, &email); err {
case sql.ErrNoRows:
    fmt.Println("No rows were returned!")
case nil:
    fmt.Println(id, email)
default:
    log.Fatal(err)
}
```

### Update (Güncelleme)

```go
sqlStatement := `
UPDATE users
SET email = $2
WHERE id = $1;`
res, err := db.Exec(sqlStatement, 1, "newemail@example.com")
if err != nil {
    log.Fatal(err)
}
count, err := res.RowsAffected()
if err != nil {
    log.Fatal(err)
}
fmt.Println(count, "rows affected")
```

### Delete (Silme)

```go
sqlStatement := `
DELETE FROM users
WHERE id = $1;`
res, err := db.Exec(sqlStatement, 1)
if err != nil {
    log.Fatal(err)
}
count, err := res.RowsAffected()
if err != nil {
    log.Fatal(err)
}
fmt.Println(count, "rows affected")
```

## 3. SQL Sorguları Çalıştırma

Golang'da SQL sorguları çalıştırmak için `Query` ve `QueryRow` metodlarını kullanabiliriz. `Query` birden fazla satır dönerken, `QueryRow` tek bir satır döner.

### 4. Repository Tasarım Deseni

Repository tasarım deseni, veritabanı işlemlerini soyutlamak ve kodu daha okunabilir ve test edilebilir hale getirmek için kullanılır. Repository tasarım desenini kullanarak, veritabanı işlemlerini ayrı bir katmana taşıyabiliriz.

```go
package repository

import (
    "database/sql"
    "log"
)

type UserRepository struct {
    db *sql.DB
}

func NewUserRepository(db *sql.DB) *UserRepository {
    return &UserRepository{db}
}

func (r *UserRepository) FindByID(id int) (string, error) {
    var email string
    sqlStatement := `SELECT email FROM users WHERE id=$1;`
    row := r.db.QueryRow(sqlStatement, id)
    switch err := row.Scan(&email); err {
    case sql.ErrNoRows:
        return "", nil
    case nil:
        return email, nil
    default:
        log.Fatal(err)
        return "", err
    }
}
```
### 6. ORM Kullanımı

ORM (Object-Relational Mapping) kütüphaneleri, veritabanı işlemlerini gerçekleştirirken kod yazmayı ve veri modellerini yönetmeyi kolaylaştırır. Ancak, ORM kullanmanın hem avantajları hem de dezavantajları vardır.

### Görülecek Konular
1. Gorm Kütüphanesi 
2. XORM Kütüphanesi

### 6. Gorm Kütüphanesi

Gorm, Go programlama dili için ORM (Object-Relational Mapping) kütüphanesidir. Gorm, veritabanı işlemlerini daha kolay ve hızlı bir şekilde gerçekleştirmemizi sağlar.

Gorm kütüphanesini kullanmak için öncelikle `gorm` paketini yüklememiz gerekmektedir.

```bash
go get -u gorm.io/gorm
```

Gorm kütüphanesini kullanarak veritabanı işlemlerini gerçekleştirmek için aşağıdaki adımları takip edebiliriz:

1. Veritabanı Bağlantısı Kurma

```go
import (
    "gorm.io/driver/postgres"
    "gorm.io/gorm"
)

func main() {
    dsn := "host=localhost user=gorm password=gorm dbname=gorm port=9920 sslmode=disable TimeZone=Asia/Shanghai"
    db, err := gorm.Open(postgres.Open(dsn), &gorm.Config{})
    if err != nil {
        panic("failed to connect database")
    }
    defer db.Close()
}
```

2. Model Tanımlama

```go
type User struct {
    gorm.Model
    Name  string
    Email string
}
```

3. CRUD İşlemleri

```go
// Create
db.Create(&User{Name: "John", Email: "aaa@aaa.com"})

// Read
var user User

db.First(&user, 1) // find user with integer primary key

// Update - update user's name to `aaa` and email to `bbb`

db.Model(&user).Update("Name", "aaa").Update("Email", "bbb")

// Delete - delete user
db.Delete(&user, 1)
```

Gorm kütüphanesini kullanarak veritabanı işlemlerini gerçekleştirmek daha kolay ve hızlı olabilir. Ancak, performans ve esneklik açısından `database/sql` paketini kullanmak daha avantajlı olabilir.

### 7. XORM Kütüphanesi

XORM, Go programlama dili için ORM (Object-Relational Mapping) kütüphanesidir. XORM, veritabanı işlemlerini daha kolay ve hızlı bir şekilde gerçekleştirmemizi sağlar.

XORM kütüphanesini kullanmak için öncelikle `xorm.io/xorm` paketini yüklememiz gerekmektedir.

```bash
go get -u xorm.io/xorm
```

XORM kütüphanesini kullanarak veritabanı işlemlerini gerçekleştirmek için aşağıdaki adımları takip edebiliriz:

1. Veritabanı Bağlantısı Kurma

```go
import (
    "xorm.io/xorm"
    _ "github.com/lib/pq"
)

func main() {
    engine, err := xorm.NewEngine("postgres", "user=test dbname=test sslmode=disable")
    if err != nil {
        panic(err)
    }
    defer engine.Close()
}
```

2. Model Tanımlama

```go
type User struct {
    Id   int64
    Name string
    Age  int
}
```

3. CRUD İşlemleri

```go
// Create
user := User{Name: "John", Age: 30}
affected, err := engine.Insert(&user)

// Read
user := User{Id: 1}
if has, err := engine.Get(&user) ; err == nil && has {
    fmt.Printf("user: %v\n", user)
}

// Update
user := User{Name: "John"}

affected, err := engine.Where("id = ?", 1).Update(&user)

// Delete
user := User{Id: 1}

affected, err := engine.Delete(&user)
```

XORM kütüphanesini kullanarak veritabanı işlemlerini gerçekleştirmek daha kolay ve hızlı olabilir. Ancak, performans ve esneklik açısından `database/sql` paketini kullanmak daha avantajlı olabilir.

## Neden ORM Kullanmalıyız / Kullanmamalıyız?

### Avantajları:
**Kolaylık:** ORM kütüphaneleri, SQL yazmak zorunda kalmadan veritabanı işlemleri yapmanıza olanak tanır. Bu, özellikle SQL'de tecrübesiz geliştiriciler için faydalıdır.

**Modülerlik:** ORM'ler veri modellerini nesne olarak temsil eder ve bu nesnelerle doğrudan çalışmanıza olanak tanır. Bu, kodun daha modüler olmasını sağlar.

**Bakım Kolaylığı:** ORM kütüphaneleri, veritabanı işlemlerini soyutladığı için veritabanı değiştirilse bile iş mantığında çok az değişiklik yapmanız gerekir. Bu da bakım sürecini kolaylaştırır.

**Otomatik Migrasyonlar:** Birçok ORM kütüphanesi, veri tabanı şemalarını otomatik olarak yönetmenize yardımcı olur. Bu, özellikle veri tabanı şemasında değişiklikler yapıldığında faydalıdır.

### Dezavantajları

**Performans Kaybı**: ORM kütüphaneleri, düşük seviyeli SQL sorgularının yerine geçer. Bu, bazı durumlarda performans kaybına neden olabilir, çünkü ORM'ler her duruma en uygun SQL sorgusunu oluşturmayabilir.

**Esneklik Eksikliği**: ORM kütüphaneleri, karmaşık ve optimize edilmiş SQL sorguları yazmanızı zorlaştırabilir. Özellikle performansın kritik olduğu büyük projelerde, ORM'ler yeterince esnek olmayabilir.

**Öğrenme Eğrisi**: ORM kullanmak, SQL ve veritabanı yönetimi konusunda tecrübeli geliştiriciler için öğrenilmesi gereken yeni bir kavram seti sunar. Bu da bir öğrenme eğrisi yaratabilir.

Büyük Firmalar ORM Kullanıyor mu?
Büyük firmalar genellikle ORM kütüphanelerini kullanır, ancak kullanım şekilleri projelerin ihtiyaçlarına göre değişir. Özellikle hızlı geliştirme ve prototip oluşturma aşamalarında ORM'ler tercih edilir. Ancak, performansın kritik olduğu durumlarda, özellikle büyük veri kümeleri veya karmaşık sorgular gerektiren projelerde, ORM kullanımını sınırlamak ya da tamamen SQL kullanmak yaygındır.

**Örneğin:**

- Facebook ve Twitter gibi büyük teknoloji firmaları, ORM kullanırken belirli durumlarda manuel SQL sorgularına başvururlar. Bu, performans kritik olduğunda ORM'nin yerine geçer.
- Google gibi firmalar, performansın ön planda olduğu büyük veri işlemleri için ORM yerine direkt olarak optimize edilmiş SQL sorguları kullanabilir.

Sonuç olarak, ORM kütüphaneleri geliştirmenin hızını ve kolaylığını artırsa da, performans ve esneklik gerektiren projelerde dikkatli bir şekilde değerlendirilmelidir. ORM'leri kullanmak her zaman avantajlı olmayabilir; bu nedenle projenizin ihtiyaçlarına göre doğru aracı seçmek önemlidir.
