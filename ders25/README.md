# Ders 25: Go ile Şifreleme ve Güvenlik

Bu derste, Go dilinde şifreleme ve güvenlik konularını öğreneceksiniz. Verilerinizi nasıl şifreleyebileceğinizi, şifre çözme işlemlerini ve güvenli veri iletimi için en iyi uygulamaları göreceksiniz.

## İçindekiler

1. [Giriş](#giriş)
2. [Temel Şifreleme Kavramları](#temel-şifreleme-kavramları)
3. [AES Şifreleme](#aes-şifreleme)
4. [RSA Şifreleme](#rsa-şifreleme)
5. [Hash Fonksiyonları](#hash-fonksiyonları)
6. [Uygulama Örnekleri](#uygulama-örnekleri)
7. [Sonuç](#sonuç)

## Giriş

Şifreleme, verilerinizi yetkisiz erişimden korumanın en etkili yollarından biridir. Bu derste, Go dilinde şifreleme ve güvenlik konularını ele alacağız.

## Temel Şifreleme Kavramları

Şifreleme, verilerinizi okunamaz bir forma dönüştürme işlemidir. Şifre çözme ise bu verileri tekrar okunabilir hale getirme işlemidir. İki ana şifreleme türü vardır: simetrik ve asimetrik şifreleme.

## AES Şifreleme

AES (Advanced Encryption Standard), simetrik bir şifreleme algoritmasıdır. Aynı anahtar hem şifreleme hem de şifre çözme işlemleri için kullanılır.

```go
package main

import (
    "crypto/aes"
    "crypto/cipher"
    "crypto/rand"
    "encoding/hex"
    "fmt"
    "io"
)

func encryptAES(key, text []byte) (string, error) {
    block, err := aes.NewCipher(key)
    if err != nil {
        return "", err
    }

    ciphertext := make([]byte, aes.BlockSize+len(text))
    iv := ciphertext[:aes.BlockSize]
    if _, err := io.ReadFull(rand.Reader, iv); err != nil {
        return "", err
    }

    stream := cipher.NewCFBEncrypter(block, iv)
    stream.XORKeyStream(ciphertext[aes.BlockSize:], text)

    return hex.EncodeToString(ciphertext), nil
}

func main() {
    key := []byte("myverystrongpasswordo32bitlength")
    text := []byte("Hello, World!")

    encrypted, err := encryptAES(key, text)
    if err != nil {
        fmt.Println("Error encrypting:", err)
        return
    }

    fmt.Println("Encrypted text:", encrypted)
}
```

## RSA Şifreleme

RSA, asimetrik bir şifreleme algoritmasıdır. Bir anahtar çifti (özel ve açık anahtar) kullanır. Açık anahtar şifreleme için, özel anahtar ise şifre çözme için kullanılır.

```go
package main

import (
    "crypto/rand"
    "crypto/rsa"
    "crypto/sha256"
    "fmt"
)

func encryptRSA(publicKey *rsa.PublicKey, text []byte) ([]byte, error) {
    return rsa.EncryptOAEP(sha256.New(), rand.Reader, publicKey, text, nil)
}

func main() {
    privateKey, err := rsa.GenerateKey(rand.Reader, 2048)
    if err != nil {
        fmt.Println("Error generating key:", err)
        return
    }

    publicKey := &privateKey.PublicKey
    text := []byte("Hello, World!")

    encrypted, err := encryptRSA(publicKey, text)
    if err != nil {
        fmt.Println("Error encrypting:", err)
        return
    }

    fmt.Println("Encrypted text:", encrypted)
}
```

## Hash Fonksiyonları

Hash fonksiyonları, verilerinizi sabit uzunlukta bir değere dönüştürür. Bu değer, verilerinizi temsil eder ve genellikle veri bütünlüğünü sağlamak için kullanılır.

```go
package main

import (
    "crypto/sha256"
    "fmt"
)

func main() {
    text := []byte("Hello, World!")
    hash := sha256.Sum256(text)

    fmt.Printf("Hash: %x\n", hash)
}
```

## Uygulama Örnekleri

Bu bölümde, yukarıda öğrendiğiniz şifreleme tekniklerini kullanarak basit uygulamalar geliştireceksiniz. Örneğin, bir dosya şifreleme aracı veya güvenli bir mesajlaşma uygulaması yapabilirsiniz.

## Sonuç

Bu derste, Go dilinde şifreleme ve güvenlik konularını ele aldık. AES ve RSA şifreleme algoritmalarını, hash fonksiyonlarını ve bu teknikleri nasıl uygulayacağınızı öğrendiniz. Şimdi, bu bilgileri kullanarak kendi güvenli uygulamalarınızı geliştirebilirsiniz.

### Sonraki Ders

[# Ders 26: Go ile Unit Test Yazma](../ders26/README.md)