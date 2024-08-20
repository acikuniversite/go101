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
