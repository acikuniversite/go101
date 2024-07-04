# Mikroservisler ve Klasör Yapısı Golang

Bu ders, mikroservis mimarisini ve Go dilinde mikroservislerin nasıl yapılandırılacağını ele alır. Mikroservislerin
avantajları, dezavantajları ve Go dilinde mikroservislerin nasıl oluşturulacağına dair örnekler sunar.

## Mikroservis Nedir?

Mikroservis mimarisi, büyük ve karmaşık uygulamaları daha küçük, bağımsız ve yönetilebilir servislere bölmeyi amaçlayan
bir yazılım geliştirme yaklaşımıdır. Her mikroservis, belirli bir işlevi yerine getirir ve diğer mikroservislerle
iletişim kurarak bütün bir sistemi oluşturur.

### Avantajları

- **Bağımsız Geliştirme:** Her mikroservis bağımsız olarak geliştirilebilir ve dağıtılabilir.
- **Ölçeklenebilirlik:** Mikroservisler, ihtiyaç duyulan bileşenlerin bağımsız olarak ölçeklenmesine olanak tanır.
- **Hata İzolasyonu:** Bir mikroservisteki hata, diğer mikroservisleri etkilemez.

### Dezavantajları

- **Dağıtık Sistem Karmaşıklığı:** Mikroservisler, dağıtık sistemlerin yönetiminde ek karmaşıklıklar getirir.
- **Ağ Gecikmeleri:** Mikroservisler arasındaki iletişim, ağ gecikmelerine neden olabilir.

## Go ile Mikroservis Oluşturma

Go, mikroservis geliştirme için ideal bir dildir. Basitliği, performansı ve güçlü standart kütüphanesi ile
mikroservislerin hızlı ve verimli bir şekilde geliştirilmesini sağlar.

### Proje Klasör Yapısı

Mikroservis projelerinde düzenli bir klasör yapısı, kodun okunabilirliğini ve yönetilebilirliğini artırır. Aşağıda,
tipik bir Go mikroservis projesi için önerilen klasör yapısı ve her klasörün işlevi açıklanmıştır:

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

#### /cmd

Bu klasör, uygulamanın giriş noktalarını içerir. Her mikroservis için ayrı bir alt klasör oluşturulur.
Örneğin, `service` alt klasörü, mikroservisin ana dosyasını (`main.go`) içerir.

#### /pkg

Bu klasör, diğer projeler tarafından kullanılabilecek genel kodları içerir. Örneğin, `api` alt klasörü, API ile ilgili
işlevleri içerir ve `service` alt klasörü, mikroservisin iş mantığını içerir.

#### /internal

Bu klasör, yalnızca bu proje tarafından kullanılacak kodları içerir. Diğer projeler bu klasördeki kodlara erişemez.
Örneğin, `config` alt klasörü, yapılandırma ile ilgili işlevleri içerir ve `database` alt klasörü, veritabanı
bağlantılarını yönetir.

#### go.mod ve go.sum

`go.mod` dosyası, projenin bağımlılıklarını ve modül bilgilerini içerir. `go.sum` dosyası ise, bağımlılıkların doğrulama
bilgilerini içerir.

### Örnek Mikroservis

Aşağıda basit bir mikroservis örneği verilmiştir. Bu örnek, `HelloHandler` ve `GoodbyeHandler` adında iki API işlevini
içerir. Farklı `main.go` dosyaları, bu iki işlevi farklı portlarda çalıştırır. bu iki servis dışında loglama servisi ve
veritabanı servisi de oluşturulabilir.

**Örnek Mikroservis Yapısı:**

	1.	Hello Service - Dış servis
	2.	Goodbye Service - Dış servis
	3.	Logging Service - İç servis
	4.	Database Service - İç servis
	5.	API Gateway - Dış servis, tüm kullanıcı isteklerini yönlendirir ve iç servislere iletir.

**1. Hello Service (hello/main.go)**

```go
package main

import (
	"fmt"
	"net/http"
	"my-microservice/pkg/api"
)

func HelloHandler(w http.ResponseWriter, r *http.Request) {
	db_client := api.DatabaseConnect()
	res := db_client.Get("SELECT * FROM users")
	fmt.Fprintf(w, "Hello, World! %v", res)
}

func main() {
	http.HandleFunc("/hello", HelloHandler)
	fmt.Println("Hello service is running on port 8081")
	http.ListenAndServe(":8081", nil)
}
```

**2. Goodbye Service (goodbye/main.go)**

```go
package main

import (
	"fmt"
	"net/http"
)

func GoodbyeHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Goodbye, World!")
}

func main() {
	http.HandleFunc("/goodbye", GoodbyeHandler)
	fmt.Println("Goodbye service is running on port 8082")
	http.ListenAndServe(":8082", nil)
}
```

**3. API Gateway (gateway/main.go)**

```go
package main

import (
	"fmt"
	"net/http"
	"my-microservice/pkg/api"
## SDK import edildi
)

func GatewayHandler(w http.ResponseWriter, r *http.Request) {
	if r.URL.Path == "/hello" {
		helloURL := "http://localhost:8081/hello"
		http.Redirect(w, r, helloURL, http.StatusFound)
	} else if r.URL.Path == "/goodbye" {
		goodbyeURL := "http://localhost:8082/goodbye"
		http.Redirect(w, r, goodbyeURL, http.StatusFound)
	} else {
		api.LogRequest(fmt.Errorf("Unknown path: %s", r.URL.Path)) ## SDK
		kullanıldı
		fmt.Fprintf(w, "API Gateway: Unknown path")
	}
}

func main() {
	http.HandleFunc("/", GatewayHandler)
	fmt.Println("API Gateway is running on port 8080")
	http.ListenAndServe(":8080", nil)
}
```

**4. Log Service (log/main.go)**

```go

package main

import (
	"fmt"
	"net/http"
)

func LogHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Println("Request received:", r.URL.Path)
	fmt.Fprintf(w, "Request received: %s", r.URL.Path)
}

func main() {
	http.HandleFunc("/", LogHandler)
	fmt.Println("Log service is running on port 8083")
	http.ListenAndServe(":8083", nil)
}
```

**5. Database Service (database/main.go)**

```go

package main

import (
	"fmt"
	"net/http"
)

func DatabaseHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Println("Database connection established")
	fmt.Fprintf(w, "Database connection established")
}

func main() {
	http.HandleFunc("/", DatabaseHandler)
	fmt.Println("Database service is running on port 8084")
	http.ListenAndServe(":8084", nil)
}
```

**6. SDK (pkg/api/api.go)**

```go

package api

import (
	"fmt"
	"net/http"
)

func LogRequest(err error) {
	body := fmt.Sprintf("Error: %v", err)
	_, err = http.Post("http://localhost:8083", "text/plain", strings.NewReader(body))
	if err != nil {
		fmt.Println("Error logging request:", err)
	}
}

func DatabaseConnect() {
	dbURL := "http://localhost:8084"
	_, err := http.Get(dbURL)
	if err != nil {
		fmt.Println("Error connecting to database:", err)
	}
}
```

### Sonraki Ders

[# Ders 15 - Golang ile Web Uygulamaları](../ders15/README.md)
