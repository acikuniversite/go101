# Mikroservisler ve Klasör Yapısı

## Giriş

Bu derste, mikroservisler kavramını ve proje klasörlerinizi etkili bir şekilde nasıl yapılandıracağınızı inceleyeceğiz. Mikroservis mimarisi, uygulamanızı daha küçük, yönetilebilir hizmetlere ayırmanıza olanak tanır ve bu hizmetler bağımsız olarak geliştirilebilir, dağıtılabilir ve ölçeklendirilebilir.

## Mikroservisler Nedir?

Mikroservisler, bir uygulamayı gevşek bağlı hizmetler koleksiyonu olarak düzenleyen bir yazılım geliştirme tekniğidir. Mikroservis mimarisinde, hizmetler ince tanelidir ve protokoller hafiftir.

### Mikroservislerin Temel Özellikleri

- **Bağımsız Dağıtılabilirlik**: Her hizmet bağımsız olarak geliştirilebilir, dağıtılabilir ve ölçeklendirilebilir.
- **Merkezi Olmayan Veri Yönetimi**: Her hizmet kendi veritabanını yönetir.
- **Çok Dilli Programlama**: Farklı hizmetler farklı programlama dillerinde yazılabilir.
- **Hata İzolasyonu**: Bir hizmetteki hata diğerlerini etkilemez.

## Klasör Yapısı

İyi organize edilmiş bir klasör yapısı, ölçeklenebilir ve yönetilebilir bir kod tabanını korumak için çok önemlidir. Aşağıda, bir mikroservis projesi için tipik bir klasör yapısının örneği verilmiştir:

```
proje-kök/
├── hizmet1/
│   ├── src/
│   │   ├── main.py
│   │   ├── utils.py
│   ├── tests/
│   │   ├── test_main.py
│   ├── Dockerfile
│   ├── requirements.txt
├── hizmet2/
│   ├── src/
│   │   ├── main.py
│   │   ├── utils.py
│   ├── tests/
│   │   ├── test_main.py
│   ├── Dockerfile
│   ├── requirements.txt
├── geçit/
│   ├── src/
│   │   ├── main.py
│   │   ├── utils.py
│   ├── tests/
│   │   ├── test_main.py
│   ├── Dockerfile
│   ├── requirements.txt
├── docker-compose.yml
└── README.md
```

### Klasör Yapısının Açıklaması

- **hizmet1, hizmet2, ...**: Her klasör bir mikroservisi temsil eder.
  - **src/**: Hizmetin kaynak kodunu içerir.
    - **main.py**: Hizmetin giriş noktası.
    - **utils.py**: Hizmet tarafından kullanılan yardımcı fonksiyonlar.
  - **tests/**: Hizmet için test vakalarını içerir.
    - **test_main.py**: main.py dosyası için test vakaları.
  - **Dockerfile**: Hizmet için bir Docker görüntüsü oluşturma talimatları.
  - **requirements.txt**: Hizmetin bağımlılıklarının listesi.

- **geçit/**: İstekleri uygun mikroservise yönlendiren bir API geçidi olarak görev yapar.
  - **src/**: Geçidin kaynak kodunu içerir.
    - **main.py**: Geçidin giriş noktası.
    - **utils.py**: Geçit tarafından kullanılan yardımcı fonksiyonlar.
  - **tests/**: Geçit için test vakalarını içerir.
    - **test_main.py**: main.py dosyası için test vakaları.
  - **Dockerfile**: Geçit için bir Docker görüntüsü oluşturma talimatları.
  - **requirements.txt**: Geçidin bağımlılıklarının listesi.

- **docker-compose.yml**: Tüm hizmetleri birlikte çalıştırmak için bir Docker Compose dosyası.
- **README.md**: Proje için dokümantasyon.

## Örnek Kod

İşte Flask kullanarak Python'da yazılmış basit bir mikroservis örneği:

### main.py

```python
from flask import Flask, jsonify

app = Flask(__name__)

@app.route('/health', methods=['GET'])
def health_check():
    return jsonify({"status": "UP"}), 200

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

### Dockerfile

```dockerfile
FROM python:3.8-slim

WORKDIR /app

COPY src/requirements.txt requirements.txt
RUN pip install -r requirements.txt

COPY src/ /app

CMD ["python", "main.py"]
```

### requirements.txt

```
Flask==2.0.1
```

## Sonuç

Mikroservis mimarisi, bağımsız dağıtım, hata izolasyonu ve farklı hizmetler için farklı teknolojiler kullanma yeteneği gibi birçok fayda sunar. İyi yapılandırılmış bir klasör organizasyonu, mikroservis projenizin ölçeklenebilirliğini ve yönetilebilirliğini korumak için gereklidir.

