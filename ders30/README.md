# Ders 30: Go ile Serverless

Bu derste, Go dilinde serverless uygulamaları nasıl geliştireceğinizi öğreneceksiniz. Serverless kavramını ve avantajlarını öğrenecek, Go dilinde serverless uygulamaları nasıl geliştirebileceğinizi göreceksiniz.

## İçindekiler

1. [Serverless Nedir?](#serverless-nedir)
2. [Serverless Mimarisi](#serverless-mimarisi)
3. [AWS Lambda ile Go](#aws-lambda-ile-go)
4. [AWS Lambda Fonksiyonu Oluşturma](#aws-lambda-fonksiyonu-oluşturma)
5. [AWS Lambda Fonksiyonunu Test Etme](#aws-lambda-fonksiyonunu-test-etme)
6. [Sonuç](#sonuç)

## Serverless Nedir?

Serverless, sunucusuz anlamına gelir ve uygulama geliştiricilerinin sunucu yönetimi ile uğraşmadan kodlarını çalıştırmalarını sağlar. Bu modelde, bulut sağlayıcıları sunucu yönetimini üstlenir ve geliştiriciler sadece kodlarını yazarak uygulamalarını çalıştırabilirler.

## Serverless Mimarisi

Serverless mimarisi, uygulamaların küçük, bağımsız fonksiyonlar olarak çalıştırılmasını sağlar. Bu fonksiyonlar, belirli olaylar tetiklendiğinde çalışır ve sadece çalıştıkları süre boyunca ücretlendirilirler. Bu sayede, maliyetler düşer ve ölçeklenebilirlik artar.

## AWS Lambda ile Go

AWS Lambda, Amazon Web Services (AWS) tarafından sunulan bir serverless hizmetidir. AWS Lambda ile Go dilinde fonksiyonlar yazarak serverless uygulamalar geliştirebilirsiniz.

### AWS Lambda Fonksiyonu Oluşturma

1. AWS Management Console'a gidin ve Lambda hizmetini seçin.
2. "Create function" butonuna tıklayın.
3. "Author from scratch" seçeneğini seçin.
4. Fonksiyon adını girin ve runtime olarak "Go 1.x" seçin.
5. "Create function" butonuna tıklayın.

### Örnek Go Lambda Fonksiyonu

```go
package main

import (
    "context"
    "github.com/aws/aws-lambda-go/lambda"
)

type MyEvent struct {
    Name string `json:"name"`
}

type MyResponse struct {
    Message string `json:"message"`
}

func HandleRequest(ctx context.Context, event MyEvent) (MyResponse, error) {
    return MyResponse{Message: "Hello " + event.Name}, nil
}

func main() {
    lambda.Start(HandleRequest)
}
```

### AWS Lambda Fonksiyonunu Test Etme

1. Fonksiyon oluşturulduktan sonra, "Test" butonuna tıklayın.
2. Test için bir event JSON oluşturun:
    ```json
    {
        "name": "Dünya"
    }
    ```
3. "Create" butonuna tıklayın ve ardından "Test" butonuna tekrar tıklayın.
4. Fonksiyonun çıktısını göreceksiniz:
    ```json
    {
        "message": "Hello Dünya"
    }
    ```

## Sonuç

Bu derste, Go dilinde AWS Lambda kullanarak serverless bir fonksiyon oluşturmayı ve test etmeyi öğrendiniz. Serverless mimarisi ile uygulamalarınızı daha ölçeklenebilir ve maliyet etkin bir şekilde geliştirebilirsiniz.
