---
title: Реализация собственного Stack
tags:
  - Algorithm
related_topics: 
created: 2024-12-18 19:41
modified: 2024-12-27T15:26:26+03:00
questions: 
notes: 
links: 
---

# Массив int[]
```java
public class Stack {
    private int maxSize;
    private int top;
    private int[] stackArray;

    public Stack(int size) {
        maxSize = size;
        stackArray = new int[size];
        top = -1;  // Стек пуст, когда top == -1
    }

    public boolean isEmpty() {
        return top == -1;
    }

    public boolean isFull() {
        return top == maxSize - 1;
    }

    public void push(int value) {
        if (isFull()) {
            System.out.println("Стек переполнен");
        } else {
            stackArray[++top] = value;
        }
    }

    public int pop() {
        if (isEmpty()) {
            System.out.println("Стек пуст");
            return -1;
        } else {
            return stackArray[top--];
        }
    }

    public int peek() {
        if (isEmpty()) {
            System.out.println("Стек пуст");
            return -1;
        } else {
            return stackArray[top];
        }
    }
}

```




# Через LinkedList
```java
public class Stack {

	private LinckedList<String> stack;

	public Stack() {
		this.stack = new LinkedList<>();
	}

	public boolean isEmpty() {
		stack.isEmpty();
	}

	public String pop() {
		if (isEmpty()) { 
		System.out.println("Стек пуст"); 
		return -1; // Возвращаем -1, если стек пуст } else { return stack.pop(); // Удаляем и возвращаем элемент с вершины стека 
		}
	}

	public String peek() {
		if (isEmpty()) { 
		System.out.println("Стек пуст"); 
		return -1; // Возвращаем -1, если стек пуст } else { return stack.peek(); // Получаем элемент с вершины стека без удаления
		 }
	}

	public void push(String str) {
		stack.push(str);
	}

}
```

![[Pasted image 20241227151723.png]]
Методы `pop()` и `poll()` в классе `LinkedList` имеют схожее назначение — извлечение первого элемента из списка, но между ними есть несколько важных различий, касающихся поведения при пустом списке.

### Различия между `pop()` и `poll()`:

1. **Обработка пустого списка**:
    
    - **`pop()`**:
        
        - Если список пуст, метод вызывает исключение `NoSuchElementException`. Это гарантирует, что операция извлечения элемента не может завершиться "тихо" (то есть без какого-либо уведомления о проблеме), если список пуст.
        - В документации указано, что этот метод эквивалентен методу `removeFirst()`.
    - **`poll()`**:
        
        - Если список пуст, метод **возвращает `null`**, а не выбрасывает исключение. Это означает, что метод может быть использован в ситуациях, когда не хочется обрабатывать исключения.
2. **Предназначение и контекст**:
    
    - **`pop()`** используется в контексте **стека** и чаще применяется, когда мы уверены, что стек не пустой и всегда будем извлекать элементы.
    - **`poll()`** используется в контексте **очереди** или в случаях, когда мы хотим безопасно извлекать элементы, даже если список пуст, не вызвав исключение.

# Через Node 
