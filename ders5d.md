# Ders 5: Veri Yapıları (Stack, Queue, Linked List)

## Veri Yapıları Nedir?

Veri yapıları, bilgisayar biliminde verileri depolamak ve organize etmek için kullanılan yapılar olarak tanımlanabilir. Veri yapıları, verileri depolamak ve işlemek için kullanılan yapılar olarak tanımlanabilir ve verileri depolamak ve işlemek için farklı veri yapıları kullanılabilir.

## Örnek Veri Yapıları

- **Stack**: Verilerin LIFO (Last In, First Out) mantığına göre depolandığı veri yapısıdır.
- **Queue**: Verilerin FIFO (First In, First Out) mantığına göre depolandığı veri yapısıdır.
- **Linked List**: Verilerin bağlı listeler şeklinde depolandığı veri yapısıdır.
- **Binary Tree**: Verilerin ağaç yapısında depolandığı veri yapısıdır.
- **Graph**: Verilerin düğüm ve kenarlarla depolandığı veri yapısıdır.
- **Hash Table**: Verilerin anahtar-değer çiftleri şeklinde depolandığı veri yapısıdır.
- **Heap**: Verilerin belirli bir sıraya göre depolandığı veri yapısıdır.


## Stack Veri Yapısı

Stack, verilerin LIFO (Last In, First Out) mantığına göre depolandığı veri yapısıdır. Stack veri yapısında, veriler yığıt (stack) şeklinde depolanır ve en son eklenen veri, en üstte (top) yer alır.

### Stack Nerelerde Kullanılır?

- **Fonksiyon Çağrıları**: Fonksiyon çağrıları, stack veri yapısı kullanılarak yönetilir.
- **Tarayıcı Geçmişi**: Tarayıcı geçmişi, stack veri yapısı kullanılarak yönetilir.
- **Geri Alma İşlemleri**: Geri alma işlemleri, stack veri yapısı kullanılarak yönetilir.
- (Gerçek hayatta) Tabaklar: Tabaklar, stack veri yapısı kullanılarak yönetilir.

### Stack Operasyonları

Stack veri yapısında aşağıdaki operasyonlar gerçekleştirilebilir:

- **Push**: Stack'e yeni bir eleman ekler.
- **Pop**: Stack'ten en üstteki elemanı çıkarır.
- **Peek**: Stack'teki en üstteki elemanı döndürür.
- **isEmpty**: Stack'in boş olup olmadığını kontrol eder.
- **isFull**: Stack'in dolu olup olmadığını kontrol eder.

### Stack Veri Yapısı Uygulaması

Aşağıda, Stack veri yapısının Go dilinde nasıl uygulanabileceği gösterilmiştir:

```go
package main

import "fmt"

type Item struct {
    value int
	value2 string
	extra float64
	err error
}

type Stack struct {
    items []Item
}

func (s *Stack) Push(value int) {
    s.items = append(s.items, Item{value: value})
}

func (s *Stack) Pop() int {
    if len(s.items) == 0 {
        return -1
    }
    item := s.items[len(s.items)-1]
    s.items = s.items[:len(s.items)-1]
    return item.value
}

func (s *Stack) Peek() int {
    if len(s.items) == 0 {
        return -1
    }
    return s.items[len(s.items)-1].value
}

func (s *Stack) IsEmpty() bool {
    return len(s.items) == 0
}

func (s *Stack) IsFull() bool {
    return false
}

func main() {
    var s Stack
    s.Push(1)
    s.Push(2)
    s.Push(3)
    fmt.Println(s.Pop()) // 3
    fmt.Println(s.Pop()) // 2
    fmt.Println(s.Pop()) // 1
}
```

Yukarıdaki örnekte, Stack veri yapısı `Stack` adında bir struct ile tanımlanmıştır. Stack struct'ı, `items` adında bir tamsayı dizisi içerir. Stack struct'ı, `Push`, `Pop`, `Peek`, `IsEmpty` ve `IsFull` metodları ile Stack operasyonlarını gerçekleştirir.

## Queue Veri Yapısı

Queue, verilerin FIFO (First In, First Out) mantığına göre depolandığı veri yapısıdır. Queue veri yapısında, veriler kuyruk (queue) şeklinde depolanır ve en son eklenen veri, en sona (rear) eklenir.

### Queue Nerelerde Kullanılır?

- **Yazıcı Kuyruğu**: Yazıcı kuyruğu, yazdırılacak belgelerin sırasını belirlemek için kullanılır.
- **İşlemci Kuyruğu**: İşlemci kuyruğu, işlemciye gönderilecek işlemlerin sırasını belirlemek için kullanılır.
- (Gerçek hayatta) Banka Kuyruğu: Banka kuyruğu, banka işlemlerinin sırasını belirlemek için kullanılır.

### Queue Operasyonları

Queue veri yapısında aşağıdaki operasyonlar gerçekleştirilebilir:

- **Enqueue**: Queue'ye yeni bir eleman ekler.
- **Dequeue**: Queue'den en önemli elemanı çıkarır.
- **Front**: Queue'deki en önemli elemanı döndürür.
- **Rear**: Queue'deki en son elemanı döndürür.

### Queue Veri Yapısı Uygulaması

Aşağıda, Queue veri yapısının Go dilinde nasıl uygulanabileceği gösterilmiştir:

```go
package main

import "fmt"

type Item struct{
    value int
    next *Item
}

type Queue struct {
    front *Item
    rear *Item
}

func (q *Queue) Enqueue(value int) {
    item := &Item{value: value}
    if q.front == nil {
        q.front = item
        q.rear = item
    } else {
        q.rear.next = item
        q.rear = item
    }
}

func (q *Queue) Dequeue() int {
    if q.front == nil {
        return -1
    }
    item := q.front
    q.front = q.front.next
    if q.front == nil {
        q.rear = nil
    }
    return item.value
}

func (q *Queue) Front() int {
    if q.front == nil {
        return -1
    }
    return q.front.value
}

func (q *Queue) Rear() int {
    if q.rear == nil {
        return -1
    }
    return q.rear.value
}

func main() {
    var q Queue
    q.Enqueue(1)
    q.Enqueue(2)
    q.Enqueue(3)
    fmt.Println(q.Dequeue()) // 1
    fmt.Println(q.Dequeue()) // 2
    fmt.Println(q.Dequeue()) // 3
}
```


## Linked List Veri Yapısı

Linked List, verilerin bağlı listeler şeklinde depolandığı veri yapısıdır. Linked List veri yapısında, her eleman bir sonraki elemana işaret eden bir referansa sahiptir.

Linked List'ler aşağıdaki tiplerde olabilir:

- **Singly Linked List**: Her eleman sadece bir sonraki elemana işaret eder.
- **Doubly Linked List**: Her eleman hem bir önceki hem de bir sonraki elemana işaret eder.
- **Circular Linked List**: Son elemanın ilk elemana işaret ettiği bir Linked List türü.
- **Doubly Circular Linked List**: Son elemanın ilk elemana, ilk elemanın da son elemana işaret ettiği bir Linked List türü.
- **Skip List**: İleri seviye bir Linked List türüdür ve arama işlemlerini hızlandırmak için kullanılır, mantığı birden fazla Linked List'in bir araya gelmesi ile oluşur.

### Linked List Nerelerde Kullanılır?

- **Dosya Sistemleri**: Dosya sistemleri, dosyaların bağlı listeler şeklinde depolandığı veri yapısıdır.
- **Tarayıcı Geçmişi**: Tarayıcı geçmişi, ziyaret edilen sayfaların bağlı listeler şeklinde depolandığı veri yapısıdır.
- **Otomatik Tamamlama**: Otomatik tamamlama, kelimelerin bağlı listeler şeklinde depolandığı veri yapısıdır.
- **(Gerçek hayatta) Tren Vagonları**: Tren vagonları, bağlı listeler şeklinde depolandığı veri yapısıdır.

### Linked List Operasyonları

Linked List veri yapısında aşağıdaki operasyonlar gerçekleştirilebilir:

- **Insert**: Linked List'e yeni bir eleman ekler.
- **Delete**: Linked List'ten bir elemanı çıkarır.
- **Search**: Linked List'te bir elemanı arar.

### Linked List Veri Yapısı Uygulaması

Aşağıda, Singly Linked List veri yapısının Go dilinde nasıl uygulanabileceği gösterilmiştir:

```go
package main

import "fmt"

type Node struct {
    value int
    next *Node
}

type LinkedList struct {
    head *Node
}

func (l *LinkedList) Insert(value int) {
    node := &Node{value: value}
    if l.head == nil {
        l.head = node
    } else {
        current := l.head
        for current.next != nil {
            current = current.next
        }
        current.next = node
    }
}

func (l *LinkedList) Delete(value int) {
    if l.head == nil {
        return
    }
    if l.head.value == value {
        l.head = l.head.next
        return
    }
    current := l.head
    for current.next != nil {
        if current.next.value == value {
            current.next = current.next.next
            return
        }
        current = current.next
    }
}

func (l *LinkedList) Search(value int) bool {
    current := l.head
    for current != nil {
        if current.value == value {
            return true
        }
        current = current.next
    }
    return false
}

func main() {
    var l LinkedList
    l.Insert(1)
    l.Insert(2)
    l.Insert(3)
    fmt.Println(l.Search(2)) // true
    l.Delete(2)
    fmt.Println(l.Search(2)) // false
}
```

Yukarıdaki örnekte, Singly Linked List veri yapısı `LinkedList` adında bir struct ile tanımlanmıştır. Linked List struct'ı, `head` adında bir Node referansı içerir. Node struct'ı, `value` adında bir tamsayı ve `next` adında bir Node referansı içerir. Linked List struct'ı, `Insert`, `Delete` ve `Search` metodları ile Linked List operasyonlarını gerçekleştirir.

## Binary Tree Veri Yapısı

Binary Tree, verilerin ağaç yapısında depolandığı veri yapısıdır. Binary Tree veri yapısında, her eleman en fazla iki çocuğa sahip olabilir.

Binary Tree'ler aşağıdaki tiplerde olabilir:

- **Binary Search Tree (BST)**: Her elemanın sol çocuğu kendisinden küçük, sağ çocuğu kendisinden büyük olan bir Binary Tree türü.
```
     24
    / \
   12  36
  / \  / \
 6  18 30 42
 

```
- **Balanced Binary Tree**: Her elemanın sol ve sağ alt ağaçları arasındaki yükseklik farkı en fazla 1 olan bir Binary Tree türü.
- **Complete Binary Tree**: Her seviyede sol taraftan sağa doğru elemanlarla doldurulmuş bir Binary Tree türü.

### Binary Tree Nerelerde Kullanılır?

- **Arama İşlemleri**: Binary Search Tree (BST) veri yapısı, arama işlemlerini hızlandırmak için kullanılır.
- **Sıralama İşlemleri**: Inorder traversal algoritması ile Binary Tree'deki elemanlar sıralı bir şekilde listelenebilir.
- **(Gerçek hayatta) Aile Ağacı**: Aile ağacı, aile üyelerinin ağaç yapısında depolandığı veri yapısıdır.

### Binary Tree Operasyonları

Binary Tree veri yapısında aşağıdaki operasyonlar gerçekleştirilebilir:,

- **Insert**: Binary Tree'ye yeni bir eleman ekler.
- **Delete**: Binary Tree'den bir elemanı çıkarır.
- **Search**: Binary Tree'de bir elemanı arar.

### Binary Tree Veri Yapısı Uygulaması

Aşağıda, Binary Tree veri yapısının Go dilinde nasıl uygulanabileceği gösterilmiştir:

```go

package main

import "fmt"

type Node struct {
    value int
    left *Node
    right *Node
}

type BinaryTree struct {
    root *Node
}

func (t *BinaryTree) Insert(value int) {
    node := &Node{value: value}
    if t.root == nil {
        t.root = node
    } else {
        current := t.root
        for {
            if value < current.value {
                if current.left == nil {
                    current.left = node
                    break
                }
                current = current.left
            } else {
                if current.right == nil {
                    current.right = node
                    break
                }
                current = current.right
            }
        }
    }
}

func (t *BinaryTree) Delete(value int) {
    var parent *Node
    current := t.root
    for current != nil {
        if value < current.value {
            parent = current
            current = current.left
        } else if value > current.value {
            parent = current
            current = current.right
        } else {
            if current.left == nil && current.right == nil {
                if parent == nil {
                    t.root = nil
                } else if parent.left == current {
                    parent.left = nil
                } else {
                    parent.right = nil
                }
            } else if current.left == nil {
                if parent == nil {
                    t.root = current.right
                } else if parent.left == current {
                    parent.left = current.right
                } else {
                    parent.right = current.right
                }
            } else if current.right == nil {
                if parent == nil {
                    t.root = current.left
                } else if parent.left == current {
                    parent.left = current.left
                } else {
                    parent.right = current.left
                }
            } else {
                successor := current.right
                for successor.left != nil {
                    successor = successor.left
                }
                current.value = successor.value
                t.Delete(successor.value)
            }
            break
        }
    }
}

func (t *BinaryTree) Search(value int) bool {
    current := t.root
    for current != nil {
        if value < current.value {
            current = current.left
        } else if value > current.value {
            current = current.right
        } else {
            return true
        }
    }
    return false
}

func main() {
    var t BinaryTree
    t.Insert(24)
    t.Insert(12)
    t.Insert(36)
    t.Insert(6)
    t.Insert(18)
    t.Insert(30)
    t.Insert(42)
    fmt.Println(t.Search(30)) // true
    t.Delete(30)
    fmt.Println(t.Search(30)) // false
}
```

## Graph Veri Yapısı

Graph, verilerin düğüm ve kenarlarla depolandığı veri yapısıdır. Graph veri yapısında, her düğüm bir veriyi temsil eder ve her kenar iki düğüm arasındaki ilişkiyi temsil eder.

Graph'lar aşağıdaki tiplerde olabilir:

- **Directed Graph**: Kenarlar bir yönde hareket eder.
- **Undirected Graph**: Kenarlar iki yönde hareket eder.

### Graph Nerelerde Kullanılır?

- **Sosyal Ağlar**: Sosyal ağlar, kullanıcıların arkadaşlık ilişkilerini düğüm ve kenarlarla depolandığı veri yapısıdır.
- **Haritalar**: Haritalar, şehirler arasındaki yolculukları düğüm ve kenarlarla depolandığı veri yapısıdır.
- **(Gerçek hayatta) Elektrik Şebekeleri**: Elektrik şebekeleri, elektrik hatlarını düğüm ve kenarlarla depolandığı veri yapısıdır.

### Graph Operasyonları

Graph veri yapısında aşağıdaki operasyonlar gerçekleştirilebilir:

- **AddNode**: Graph'a yeni bir düğüm ekler.
- **AddEdge**: Graph'a yeni bir kenar ekler.
- **RemoveNode**: Graph'tan bir düğümü çıkarır.
- **RemoveEdge**: Graph'tan bir kenarı çıkarır.
- **HasNode**: Graph'ta bir düğüm olup olmadığını kontrol eder.
- **HasEdge**: Graph'ta bir kenar olup olmadığını kontrol eder.

### Graph Veri Yapısı Uygulaması

Aşağıda, Graph veri yapısının Go dilinde nasıl uygulanabileceği gösterilmiştir:

```go

package main

import "fmt"

type Graph struct {
    nodes map[int]map[int]bool
}

func (g *Graph) AddNode(node int) {
    if g.nodes == nil {
        g.nodes = make(map[int]map[int]bool)
    }
    g.nodes[node] = make(map[int]bool)
}

func (g *Graph) AddEdge(node1, node2 int) {
    if _, ok := g.nodes[node1]; !ok {
        g.AddNode(node1)
    }
    if _, ok := g.nodes[node2]; !ok {
        g.AddNode(node2)
    }
    g.nodes[node1][node2] = true
    g.nodes[node2][node1] = true
}

func (g *Graph) RemoveNode(node int) {
    delete(g.nodes, node)
    for _, neighbors := range g.nodes {
        delete(neighbors, node)
    }
}

func (g *Graph) RemoveEdge(node1, node2 int) {
    delete(g.nodes[node1], node2)
    delete(g.nodes[node2], node1)
}

func (g *Graph) HasNode(node int) bool {
    _, ok := g.nodes[node]
    return ok
}

func (g *Graph) HasEdge(node1, node2 int) bool {
    _, ok := g.nodes[node1][node2]
    return ok
}

func main() {
    var g Graph
    g.AddNode(1)
    g.AddNode(2)
    g.AddNode(3)
    g.AddEdge(1, 2)
    g.AddEdge(2, 3)
    fmt.Println(g.HasNode(2)) // true
    fmt.Println(g.HasEdge(1, 2)) // true
    g.RemoveEdge(1, 2)
    fmt.Println(g.HasEdge(1, 2)) // false
}
```

## Hash Table Veri Yapısı

Hash Table, verilerin anahtar-değer çiftleri şeklinde depolandığı veri yapısıdır. Hash Table veri yapısında, her anahtar bir değeri temsil eder ve her anahtarın bir hash değeri vardır.

Hash Table'lar aşağıdaki tiplerde olabilir:

- **Open Addressing**: Çakışmaları çözmek için boş bir yere taşınır.
- **Separate Chaining**: Çakışmaları çözmek için bağlı listeler kullanılır.
- **Linear Probing**: Çakışmaları çözmek için bir sonraki boş yere taşınır.

### Hash Table Nerelerde Kullanılır?

- **Veritabanı İşlemleri**: Veritabanı işlemleri, anahtar-değer çiftleri şeklinde depolandığı veri yapısıdır.
- **Cache**: Cache, anahtar-değer çiftleri şeklinde depolandığı veri yapısıdır.
- **(Gerçek hayatta) Telefon Rehberi**: Telefon rehberi, kişilerin isim ve telefon numaralarının anahtar-değer çiftleri şeklinde depolandığı veri yapısıdır.
- **(Gerçek hayatta) DNS Çözümleme**: DNS çözümleme, alan adlarının IP adreslerine çevrildiği anahtar-değer çiftleri şeklinde depolandığı veri yapısıdır.

### Hash Table Operasyonları

Hash Table veri yapısında aşağıdaki operasyonlar gerçekleştirilebilir:

- **Put**: Hash Table'a yeni bir anahtar-değer çifti ekler.
- **Get**: Hash Table'dan bir anahtarın değerini döndürür.
- **Remove**: Hash Table'dan bir anahtarı çıkarır.
- **Contains**: Hash Table'da bir anahtarın olup olmadığını kontrol eder.

### Hash Table Veri Yapısı Uygulaması

Aşağıda, Hash Table veri yapısının Go dilinde nasıl uygulanabileceği gösterilmiştir:

```go
package main

import "fmt"

type Konutlar struct {
    Adres string
    Kisi string
}

type Sehir struct {
    sokaklar map[string][]Item
}

func (c *Sehir) Ekle(sokak string, konut Item) {
    if c.sokaklar == nil {
        c.sokaklar = make(map[string][]Item)
    }
    c.sokaklar[sokak] = append(c.sokaklar[sokak], konut)
}

func (c *Sehir) Sil(sokak string, konut Item) {
    if _, ok := c.sokaklar[sokak]; !ok {
        return
    }
    for i, item := range c.sokaklar[sokak] {
        if item == konut {
            c.sokaklar[sokak] = append(c.sokaklar[sokak][:i], c.sokaklar[sokak][i+1:]...)
            return
        }
    }
}

func (c *Sehir) Getir(sokak string) []Item {
    return c.sokaklar[sokak]
}

func main() {
    var c Sehir
    c.Ekle("Ataturk", Item{Adres: "Ataturk Cad. 1", Kisi: "Ali"})
    c.Ekle("Ataturk", Item{Adres: "Ataturk Cad. 2", Kisi: "Veli"})

    fmt.Println(c.Getir("Ataturk")) // [{Ataturk Cad. 1 Ali} {Ataturk Cad. 2 Veli}]
    c.Sil("Ataturk", Item{Adres: "Ataturk Cad. 1", Kisi: "Ali"})
    fmt.Println(c.Getir("Ataturk")) // [{Ataturk Cad. 2 Veli}]

}

```

## Heap Veri Yapısı

Heap, Tree  benzeri bir veri yapısıdır ve verilerin belirli bir sıraya göre depolandığı veri yapısıdır. Heap veri yapısında, her düğüm, kendisinden daha küçük veya daha büyük olan bir veya daha fazla çocuğa sahiptir.

Heap'lar aşağıdaki tiplerde olabilir:

- **Min Heap**: Her düğüm, kendisinden daha küçük olan çocuklara sahiptir.
- **Max Heap**: Her düğüm, kendisinden daha büyük olan çocuklara sahiptir.

### Heap Nerelerde Kullanılır?

- **Öncelik Kuyruğu**: Öncelik kuyruğu, elemanların belirli bir öncelik sırasına göre depolandığı veri yapısıdır.
- **Sıralama İşlemleri**: Heap sort algoritması, elemanları belirli bir sıraya göre sıralamak için kullanılır.
- **(Gerçek hayatta) Acil Durumlar**: Acil durumlar, hastaların aciliyet sırasına göre depolandığı veri yapısıdır.

### Heap Yapısı 
Min Heap ve Max Heap yapısı aşağıdaki gibidir:

```
     1
    / \
   2   3
  / \ / \
 4  5 6  7
```
      
Max Heap yapısı aşağıdaki gibidir:
```
     7
    / \
   6   3
  / \ / \
 4  5 2  1
```


### Heap Operasyonları

Heap veri yapısında aşağıdaki operasyonlar gerçekleştirilebilir:

- **Insert**: Heap'e yeni bir eleman ekler.
- **Delete**: Heap'ten bir elemanı çıkarır.
- **Peek**: Heap'teki en üstteki elemanı döndürür.
- **Heapify**: Heap veri yapısını yeniden düzenler.
- **Extract**: Heap'teki en üstteki elemanı çıkarır.
- **HeapSort**: Heap veri yapısını sıralar.
- **Merge**: İki Heap veri yapısını birleştirir.
- **DecreaseKey**: Heap'teki bir elemanın değerini azaltır.
- **IncreaseKey**: Heap'teki bir elemanın değerini artırır.

### Heap Veri Yapısı Uygulaması

Aşağıda, Min Heap veri yapısının Go dilinde nasıl uygulanabileceği gösterilmiştir:

```go
package main

import "fmt"

type Heap struct {
    items []int
}

func (h *Heap) Insert(value int) {
    h.items = append(h.items, value)
    h.heapifyUp()
}

func (h *Heap) Delete() int {
    if len(h.items) == 0 {
        return -1
    }
    item := h.items[0]
    h.items[0] = h.items[len(h.items)-1]
    h.items = h.items[:len(h.items)-1]
    h.heapifyDown()
    return item
}

func (h *Heap) Peek() int {
    if len(h.items) == 0 {
        return -1
    }
    return h.items[0]
}

func (h *Heap) heapifyUp() {
    index := len(h.items) - 1
    for h.items[index] < h.items[(index-1)/2] {
        h.items[index], h.items[(index-1)/2] = h.items[(index-1)/2], h.items[index]
        index = (index - 1) / 2
    }
}

func (h *Heap) heapifyDown() {
    index := 0
    for {
        left := 2*index + 1
        right := 2*index + 2
        smallest := index
        if left < len(h.items) && h.items[left] < h.items[smallest] {
            smallest = left
        }
        if right < len(h.items) && h.items[right] < h.items[smallest] {
            smallest = right
        }
        if smallest == index {
            break
        }
        h.items[index], h.items[smallest] = h.items[smallest], h.items[index]
        index = smallest
    }
}

func main() {
    var h Heap
    h.Insert(3)
    h.Insert(2)
    h.Insert(1)
    fmt.Println(h.Peek()) // 1
    fmt.Println(h.Delete()) // 1
    fmt.Println(h.Peek()) // 2
}
```