# Ders 16 - Golang ile Veritabanı İşlemleri (Database Operations)

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

## 4. Hata Yönetimi

Veritabanı işlemleri sırasında oluşabilecek hataları yönetmek önemlidir. Hataları loglamak ve uygun şekilde ele almak, uygulamanızın güvenilirliğini artırır.

```go
if err != nil {
    log.Fatalf("An error occurred: %v", err)
}
```

Bu dersin sonunda, Golang ile temel veritabanı işlemlerini gerçekleştirebilecek bilgiye sahip olacaksınız.
