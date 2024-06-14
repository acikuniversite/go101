# Ders 16 - Veritabanı İşlemleri Glang

Bu dersin içeriği burada yer alacak.

## İçindekiler

1. [Giriş](#giriş)
2. [Veritabanı Bağlantısı](#veritabanı-bağlantısı)
3. [CRUD İşlemleri](#crud-işlemleri)
    - [Create (Oluşturma)](#create-oluşturma)
    - [Read (Okuma)](#read-okuma)
    - [Update (Güncelleme)](#update-güncelleme)
    - [Delete (Silme)](#delete-silme)
4. [Örnek Proje](#örnek-proje)
5. [Sonuç](#sonuç)

## Giriş

Bu derste, Glang programlama dilinde veritabanı işlemlerini nasıl gerçekleştireceğimizi öğreneceğiz. Veritabanı bağlantısı kurma, CRUD (Create, Read, Update, Delete) işlemleri ve örnek bir proje ile konuyu pekiştireceğiz.

## Veritabanı Bağlantısı

Veritabanı bağlantısı kurmak için öncelikle gerekli kütüphaneleri projeye dahil etmemiz gerekmektedir. Aşağıdaki kod parçası, bir veritabanı bağlantısının nasıl kurulacağını göstermektedir:

```glang
import "database/sql"
import _ "github.com/lib/pq"

func connectDB() (*sql.DB, error) {
    connStr := "user=username dbname=mydb sslmode=disable"
    db, err := sql.Open("postgres", connStr)
    if err != nil {
        return nil, err
    }
    return db, nil
}
```

## CRUD İşlemleri

### Create (Oluşturma)

Veritabanına yeni bir kayıt eklemek için aşağıdaki fonksiyonu kullanabiliriz:

```glang
func createRecord(db *sql.DB, name string, age int) error {
    query := `INSERT INTO users (name, age) VALUES ($1, $2)`
    _, err := db.Exec(query, name, age)
    return err
}
```

### Read (Okuma)

Veritabanından kayıtları okumak için aşağıdaki fonksiyonu kullanabiliriz:

```glang
func readRecords(db *sql.DB) ([]User, error) {
    query := `SELECT id, name, age FROM users`
    rows, err := db.Query(query)
    if err != nil {
        return nil, err
    }
    defer rows.Close()

    var users []User
    for rows.Next() {
        var user User
        if err := rows.Scan(&user.ID, &user.Name, &user.Age); err != nil {
            return nil, err
        }
        users = append(users, user)
    }
    return users, nil
}
```

### Update (Güncelleme)

Veritabanındaki bir kaydı güncellemek için aşağıdaki fonksiyonu kullanabiliriz:

```glang
func updateRecord(db *sql.DB, id int, name string, age int) error {
    query := `UPDATE users SET name = $1, age = $2 WHERE id = $3`
    _, err := db.Exec(query, name, age, id)
    return err
}
```

### Delete (Silme)

Veritabanından bir kaydı silmek için aşağıdaki fonksiyonu kullanabiliriz:

```glang
func deleteRecord(db *sql.DB, id int) error {
    query := `DELETE FROM users WHERE id = $1`
    _, err := db.Exec(query, id)
    return err
}
```

## Örnek Proje

Bu bölümde, yukarıda öğrendiğimiz veritabanı işlemlerini kullanarak basit bir kullanıcı yönetim sistemi oluşturacağız. Proje dosya yapısı şu şekilde olacaktır:

```
project/
│
├── main.glang
├── db.glang
└── models.glang
```

### main.glang

```glang
package main

import (
    "fmt"
    "log"
)

func main() {
    db, err := connectDB()
    if err != nil {
        log.Fatal(err)
    }
    defer db.Close()

    // Create a new record
    err = createRecord(db, "John Doe", 30)
    if err != nil {
        log.Fatal(err)
    }

    // Read records
    users, err := readRecords(db)
    if err != nil {
        log.Fatal(err)
    }
    for _, user := range users {
        fmt.Printf("ID: %d, Name: %s, Age: %d\n", user.ID, user.Name, user.Age)
    }

    // Update a record
    err = updateRecord(db, 1, "Jane Doe", 25)
    if err != nil {
        log.Fatal(err)
    }

    // Delete a record
    err = deleteRecord(db, 1)
    if err != nil {
        log.Fatal(err)
    }
}
```

### db.glang

```glang
package main

import (
    "database/sql"
    _ "github.com/lib/pq"
)

func connectDB() (*sql.DB, error) {
    connStr := "user=username dbname=mydb sslmode=disable"
    db, err := sql.Open("postgres", connStr)
    if err != nil {
        return nil, err
    }
    return db, nil
}
```

### models.glang

```glang
package main

type User struct {
    ID   int
    Name string
    Age  int
}
```

## Sonuç

Bu derste, Glang programlama dilinde veritabanı işlemlerini nasıl gerçekleştireceğimizi öğrendik. Veritabanı bağlantısı kurma, CRUD işlemleri ve örnek bir proje ile konuyu pekiştirdik. Bir sonraki derste daha ileri seviye veritabanı işlemlerini ele alacağız.
