# Ders 7: Golang'de Clean Architecture ile Proje Yapısı

Golang'de Clean Architecture kullanarak bir proje yapısını en iyi şekilde organize etmek için dikkat edilmesi gereken birçok nokta var. Bu, hem projenin modüler, sürdürülebilir ve test edilebilir olmasını sağlar hem de uzun vadede kodun bakımını kolaylaştırır. Şimdi, önerdiğimiz klasör yapısını ve bunun nedenlerini detaylıca inceleyelim:

## 1. Clean Architecture Nedir?

**Clean Architecture**, Robert C. Martin (Uncle Bob) tarafından ortaya atılan bir yazılım mimarisi prensibidir. Bu mimari, yazılımın bağımsız, esnek, test edilebilir ve sürdürülebilir olmasını amaçlar. Clean Architecture'da temel olarak şu katmanlar bulunur:

- **Entities (Varlıklar)**: İş mantığını kapsayan saf veri modelleridir. Bu katman, uygulamanın kalbi olan iş kurallarını içerir.
- **Use Cases (Kullanım Senaryoları)**: İş mantığının uygulanmasını sağlayan katmandır. Bu katman, verilerin işlenmesini ve mantığın uygulanmasını sağlar.
- **Interface Adapters (Arayüz Adaptörleri)**: Verileri dış dünyadan alır ve iç katmanlara uygun hale getirir. Örneğin, bir veritabanından veri almak, bir HTTP isteğini işlemek gibi.
- **Frameworks & Drivers (Çerçeveler ve Sürücüler)**: Dış katmanlarda yer alır ve uygulamanın çalışması için gerekli olan altyapıyı sağlar. Veritabanı, web sunucusu gibi bileşenler bu katmanda yer alır.

## 2. Proje Klasör Yapısı ve Anlamı

```bash
myproject/
├── cmd/
│   ├── app/
│   │   └── main.go
│   └── bot/
│       └── main.go
├── internal/
│   ├── config/
│   │   └── config.go
│   ├── domain/
│   │   ├── entities.go
│   │   └── interfaces.go
│   ├── usecase/
│   │   ├── app_service.go
│   │   ├── bot_service.go
│   │   ├── app_service_test.go
│   │   └── bot_service_test.go
│   ├── adapter/
│   │   ├── db/
│   │   │   └── postgres.go
│   │   ├── http/
│   │   │   └── handler.go
│   │   └── repository/
│   │       └── user_repo.go
│   ├── infrastructure/
│   │   ├── logger.go
│   │   └── db.go
│   └── bot/
│       ├── bot_logic.go
│       └── bot_scheduler.go
├── pkg/
├── migrations/
├── go.mod
└── go.sum

```

## 3. Klasörlerin Açıklamaları

- **cmd**: Uygulamanın giriş noktalarını içerir. Her bir alt klasör, uygulamanın farklı bir giriş noktasını temsil eder. Örneğin, bir web uygulaması ve bir bot uygulaması gibi.
- **internal**: Uygulamanın iç katmanlarını içerir. Bu klasör, uygulamanın iş mantığını, veritabanı işlemlerini, HTTP isteklerini ve diğer işlemleri gerçekleştiren kodları içerir.
    - **config**: Uygulamanın konfigürasyonlarını içerir.
    - **domain**: Uygulamanın varlıklarını ve arayüzlerini içerir.
    - **usecase**: Uygulamanın kullanım senaryolarını içerir.
    - **adapter**: Uygulamanın dış dünyayla etkileşimini sağlayan adaptörleri içerir.
    - **infrastructure**: Uygulamanın alt yapısını sağlayan kodları içerir.
    - **bot**: Bot uygulamasının özel iş mantığını içerir. ** ismi değişebilir.**
- **pkg**: Harici paketlerin yer aldığı klasördür. Bu klasör, uygulamanın dış dünyayla etkileşimini sağlayan paketleri içerir.
- **migrations**: Veritabanı migrasyonları için kullanılan dosyaları içerir.

## 4. Nasıl Başlamalıyız?

Hikaye: Rider of Demand adında bir uygulama geliştirmek istiyoruz. Bu uygulama, kullanıcıların taleplerini karşılayan bir platform olacak. Kullanıcılar, uygulama üzerinden bir talep oluşturabilecek ve bu talepleri karşılayan sürücüler, talepleri alıp işleyecekler.

### Adım Adım İlerleyelim:
**1. Loglama ve Konfigürasyon Yapılarını Kurun:**
- İlk olarak, uygulamanın loglama ve konfigürasyon yapılarını oluşturun.
- Bu adım, uygulamanızın her aşamasında hata ayıklamayı ve performans izlemeyi kolaylaştırır.
- internal/infrastructure/logger.go gibi bir dosyada loglama mekanizmasını kurabilirsiniz.
- Konfigürasyon dosyalarını yüklemek ve uygulamanın yapılandırılmasını sağlamak için internal/config/config.go dosyasını oluşturun.

**2.	Varlıklar (Entities) Katmanını Oluşturun:**
- Uygulamanızın temel veri modellerini (örneğin, User, Request, Driver) tanımlayın.
- Bu varlıklar, uygulamanızın iş kurallarını ve veri yapısını temsil eder.
- internal/domain/entities.go dosyasını oluşturarak, tüm varlıkları burada tanımlayabilirsiniz.

**3.	Arayüzler (Interfaces) Katmanını Tanımlayın:**
- Veritabanı adaptörünüz ve diğer servisler için gerekli arayüzleri oluşturun.
- Bu arayüzler, uygulamanızın farklı bileşenleri arasındaki bağımlılıkları soyutlar ve bağımsız test edilebilirliği sağlar.
- Örneğin, UserRepository gibi bir arayüz tanımlayarak, veritabanı adaptörünün bu arayüzü uygulamasını sağlayın.

**4.	Kullanım Senaryoları (Use Cases) Katmanını Oluşturun:**
- İş mantığını uygulayan ve varlıklar üzerinde işlem yapan kullanım senaryolarını yazın.
- Bu katman, iş kurallarını ve veri işleme süreçlerini yönetir.
- internal/usecase içinde CreateUser, GetUserDetails gibi senaryoları oluşturabilirsiniz.

**5.	Veritabanı Adaptörünü (Database Adapter) Oluşturun:**
- Uygulamanızın veritabanıyla iletişim kurmasını sağlayacak bir adaptör oluşturun.
- Adaptörde veritabanı bağlantısını kurma ve CRUD (Create, Read, Update, Delete) işlemleri için gerekli fonksiyonları tanımlayın.
- internal/adapter/db/postgres.go gibi bir dosya oluşturabilirsiniz.

**6.	Authentication ve Authorization Mekanizmasını Kurun:**
- Uygulamanızın güvenlik altyapısını oluşturmak için bir authentication ve authorization mekanizması geliştirin.
- Kullanıcı oturumları, JWT veya OAuth gibi teknolojilerle güvence altına alınabilir.
- Bu adım, uygulamanızın güvenliğini sağlamak ve kullanıcı kimlik doğrulamasını yönetmek için kritiktir.

**7.	API Endpoint’lerini ve HTTP Handler’larını Oluşturun:**
- Uygulamanızın dış dünyaya açılacağı HTTP arayüzünü tasarlayın.
- RESTful API endpoint’lerini tanımlayarak, servislerinize erişimi sağlayın.
- HTTP isteklerini işleyen handler fonksiyonlarını oluşturun ve bu fonksiyonları kullanım senaryolarına bağlayın.
- Örneğin, internal/adapter/http/handler.go dosyasında handler fonksiyonlarını oluşturabilirsiniz.

**8.	Servisleri ve İş Mantığını (Service Layer) Oluşturun:**
- Kullanım senaryolarını kullanarak servis katmanını oluşturun.
- Bu katman, uygulamanızın iş mantığını dış dünyaya sunar ve API çağrılarını işleyebilir.
- Servis katmanı, kullanım senaryolarını çağırarak gerekli iş mantığını uygular ve sonuçları döner.

**9.	Uygulamanın Başlangıç Noktalarını (Entry Points) Tanımlayın:**
- cmd klasörü altında uygulamanızın farklı giriş noktalarını (örneğin, main.go dosyaları) oluşturun.
- Uygulamanızın başlatılacağı dosyada gerekli servisleri başlatın, veritabanı bağlantılarını kurun ve HTTP sunucusunu çalıştırın.

**10.	Integration Testleri Oluşturun:**

- Her katman için gerekli birim testlerini (unit tests) kontrol edin.
- Veritabanı işlemleri, HTTP istekleri ve diğer dış bağımlılıklar için entegrasyon testleri oluşturun.
- Testlerinizi çalıştırarak, uygulamanızın doğru çalıştığından emin olun.