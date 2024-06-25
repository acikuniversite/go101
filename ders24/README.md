# Ders 24: Go ile WebSocket Kullanımı

Bu derste, Go dilinde WebSocket bağlantılarını nasıl kurabileceğinizi ve kullanabileceğinizi öğreneceksiniz. WebSocket ile gerçek zamanlı veri iletişimini nasıl gerçekleştirebileceğinizi göreceksiniz.

## İçindekiler

1. [Giriş](#giriş)
2. [WebSocket Nedir?](#websocket-nedir)
3. [Go'da WebSocket Kurulumu](#goda-websocket-kurulumu)
4. [WebSocket Sunucusu Oluşturma](#websocket-sunucusu-oluşturma)
5. [WebSocket İstemcisi Oluşturma](#websocket-istemcisi-oluşturma)
6. [Gerçek Zamanlı Veri İletişimi](#gerçek-zamanlı-veri-iletişimi)
7. [Örnek Proje](#örnek-proje)
8. [Sonuç](#sonuç)

## Giriş

Bu dersin amacı, Go dilinde WebSocket kullanarak gerçek zamanlı veri iletişimi yapmayı öğrenmektir. WebSocket, HTTP protokolüne alternatif olarak, çift yönlü ve düşük gecikmeli iletişim sağlar.

## WebSocket Nedir?

WebSocket, web tarayıcıları ve sunucular arasında çift yönlü iletişim sağlayan bir protokoldür. HTTP'den farklı olarak, WebSocket bağlantısı bir kez kurulduktan sonra sürekli açık kalır ve her iki taraf da veri gönderebilir ve alabilir.

## Go'da WebSocket Kurulumu

Go dilinde WebSocket kullanmak için `gorilla/websocket` paketini kullanacağız. Bu paketi kurmak için aşağıdaki komutu kullanabilirsiniz:

```sh
go get github.com/gorilla/websocket
```

## WebSocket Sunucusu Oluşturma

Aşağıda basit bir WebSocket sunucusu örneği bulunmaktadır:

```go
package main

import (
    "fmt"
    "net/http"
    "github.com/gorilla/websocket"
)

var upgrader = websocket.Upgrader{
    ReadBufferSize:  1024,
    WriteBufferSize: 1024,
}

func handler(w http.ResponseWriter, r *http.Request) {
    conn, err := upgrader.Upgrade(w, r, nil)
    if err != nil {
        fmt.Println(err)
        return
    }
    for {
        messageType, p, err := conn.ReadMessage()
        if err != nil {
            fmt.Println(err)
            return
        }
        if err := conn.WriteMessage(messageType, p); err != nil {
            fmt.Println(err)
            return
        }
    }
}

func main() {
    http.HandleFunc("/ws", handler)
    http.ListenAndServe(":8080", nil)
}
```

## WebSocket İstemcisi Oluşturma

Aşağıda basit bir WebSocket istemcisi örneği bulunmaktadır:

```html
<!DOCTYPE html>
<html>
<head>
    <title>WebSocket İstemcisi</title>
</head>
<body>
    <script type="text/javascript">
        var ws = new WebSocket("ws://localhost:8080/ws");
        ws.onmessage = function(event) {
            var messages = document.getElementById('messages');
            messages.innerHTML += '<br>' + event.data;
        };
        ws.onopen = function(event) {
            ws.send("Merhaba Sunucu!");
        };
    </script>
    <div id="messages"></div>
</body>
</html>
```

## Gerçek Zamanlı Veri İletişimi

WebSocket kullanarak gerçek zamanlı veri iletimi yapmak oldukça basittir. Sunucu ve istemci arasında sürekli açık bir bağlantı olduğu için, her iki taraf da istediği zaman veri gönderebilir ve alabilir.

## Örnek Proje

Bu bölümde, yukarıdaki kodları kullanarak basit bir sohbet uygulaması oluşturacağız. Sunucu tarafında, gelen mesajları tüm bağlı istemcilere yayınlayacağız.

## Sonuç

Bu derste, Go dilinde WebSocket kullanarak nasıl gerçek zamanlı veri iletişimi yapabileceğinizi öğrendiniz. WebSocket, özellikle gerçek zamanlı uygulamalar için oldukça kullanışlı bir protokoldür ve Go dilinde kullanımı oldukça basittir.

### Sonraki Ders

[# Ders 25: Go ile Şifreleme ve Güvenlik](../ders25/README.md)