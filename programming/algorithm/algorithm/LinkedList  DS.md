---
title: LinkedList  DS
tags:
  - Algorithm
related_topics: 
created: 2024-12-28 12:48
modified: 2025-01-13T10:42:55+03:00
questions: 
notes: 
links: 
---


**`LinkedList`** — это структура данных, состоящая из узлов (nodes). Каждый узел содержит:

- **Данные (E data)** — значение, которое хранится в узле.
- **Ссылку на следующий узел (next)**.
- (В двусвязном списке) **Ссылку на предыдущий узел (prev)**.

---

### 2. Как реализовать `LinkedList` самому?

#### Основные операции:

- `add(E e)` – добавить элемент в конец списка.
- `addFirst(E e)` – добавить элемент в начало.
- `remove(E e)` – удалить элемент.
- `get(int index)` – получить элемент по индексу.
- `contains(E e)` – проверить, есть ли элемент в списке.


```java
public class MyLinkedList<E> {
    private Node<E> head;
    private int size = 0;

    private static class Node<E> {
        E data;
        Node<E> next;

        Node(E data) {
            this.data = data;
            this.next = null;
        }
    }

    public void add(E data) {
        Node<E> newNode = new Node<>(data);
        if (head == null) {
            head = newNode;
        } else {
            Node<E> current = head;
            while (current.next != null) {
                current = current.next;
            }
            current.next = newNode;
        }
        size++;
    }

    public void addFirst(E data) {
        Node<E> newNode = new Node<>(data);
        newNode.next = head;
        head = newNode;
        size++;
    }

    public E get(int index) {
        if (index < 0 || index >= size) throw new IndexOutOfBoundsException();
        Node<E> current = head;
        for (int i = 0; i < index; i++) {
            current = current.next;
        }
        return current.data;
    }

    public boolean remove(E data) {
        if (head == null) return false;

        if (head.data.equals(data)) {
            head = head.next;
            size--;
            return true;
        }

        Node<E> current = head;
        while (current.next != null) {
            if (current.next.data.equals(data)) {
                current.next = current.next.next;
                size--;
                return true;
            }
            current = current.next;
        }
        return false;
    }

    public int size() {
        return size;
    }

    public boolean contains(E data) {
        Node<E> current = head;
        while (current != null) {
            if (current.data.equals(data)) {
                return true;
            }
            current = current.next;
        }
        return false;
    }
}

```


### `ListIterator` и его роль в `LinkedList`

**`ListIterator`** — это итератор, который позволяет:

- **Итерироваться в обоих направлениях (вперёд и назад)**.
- **Добавлять элементы во время итерации**.
- **Удалять элементы безопасно (без `ConcurrentModificationException`)**.

```java
LinkedList<String> list = new LinkedList<>();
list.add("One");
list.add("Two");
list.add("Three");

ListIterator<String> iterator = list.listIterator();
while (iterator.hasNext()) {
    System.out.println(iterator.next());
    iterator.add("Inserted");
}
System.out.println(list);  // [One, Inserted, Two, Inserted, Three, Inserted]

```

- **`next()`** — двигает вперёд.
- **`previous()`** — двигает назад.
- **`add()`** — вставляет элемент на текущей позиции.
- **`remove()`** — удаляет последний элемент, возвращённый `next()` или `previous()`.
- 