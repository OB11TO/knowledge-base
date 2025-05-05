---
title: Принципы Kiss, Dry и Yagni подробнее
tags:
  - Pattern
  - Architecture
related_topics: 
created: 2025-02-13 14:17
modified: 2025-02-13T14:52:34+03:00
questions: 
notes: 
links: 
---


## 1. KISS (Keep It Simple, Stupid) 🤓

**Суть:**  
Будь проще! Код должен быть понятным и не перегружаться лишней сложностью. Если задача решается простым способом – не придумывай изящные, но запутанные решения.

**Пример на Java:**
```java
public class Calculator {
    // Простое сложение двух чисел 😊
    public int add(int a, int b) {
        return a + b;
    }
    
    // Простое умножение двух чисел 🔢
    public int multiply(int a, int b) {
        return a * b;
    }
    
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        System.out.println("Sum: " + calc.add(3, 5)); // Sum: 8
        System.out.println("Product: " + calc.multiply(3, 5)); // Product: 15
    }
}

```

**Возможные вопросы на собеседовании:**

- «Как вы применяете принцип KISS в своих проектах? 🤔»
- «Приведите пример, когда излишняя сложность привела к проблемам в поддержке или тестировании кода. 🔍»

-----

## 2. DRY (Don't Repeat Yourself) 🔄

**Суть:**  
Не дублируйте код! Если логика повторяется, вынесите её в отдельный метод или класс. Это помогает избежать ошибок и облегчает поддержку.
```java
public class Invoice {
    private double amount;
    private double taxRate;

    public Invoice(double amount, double taxRate) {
        this.amount = amount;
        this.taxRate = taxRate;
    }

    // Метод для расчёта налога – используется единожды, что предотвращает дублирование кода 📊
    private double calculateTax() {
        return amount * taxRate;
    }

    // Метод для расчёта итоговой суммы
    public double calculateTotal() {
        return amount + calculateTax();
    }
    
    public static void main(String[] args) {
        Invoice invoice = new Invoice(100, 0.2);
        System.out.println("Total: " + invoice.calculateTotal()); // Total: 120.0
    }
}

```

**Возможные вопросы на собеседовании:**

- «Как вы определяете, что код дублируется, и как решаете эту проблему? 🔍»
- «Расскажите, как применение DRY помогло вам избежать ошибок в проекте. 👍»

****

## 3. YAGNI (You Aren’t Gonna Need It) ⏳

**Суть:**  
Не реализовывайте функциональность, которая может понадобиться в будущем, если сейчас она не требуется. Избыточный код только усложняет систему.

**Пример на Java:**

```java
public class OrderProcessor {
    // Реализуем только необходимый функционал для обработки заказа
    public void processOrder(Order order) {
        if (order.isValid()) {
            System.out.println("Processing order #" + order.getId());
            // Логика обработки заказа здесь 💼
        }
    }
    
    // Не добавляем метод reserveItem, если в данный момент он не требуется 🚫
}

```


----

## 4. BDUF (Big Design Up Front) 📝

**Суть:**  
Перед началом реализации подробно спланируйте архитектуру системы. Хорошее проектирование помогает избежать масштабных изменений на поздних стадиях разработки.

**Пример на Java:**

```java
// Интерфейс для работы с книгами 📚
public interface BookRepository {
    void addBook(Book book);
    Book findBookByTitle(String title);
}

// Конкретная реализация репозитория (например, в памяти)
public class InMemoryBookRepository implements BookRepository {
    private List<Book> books = new ArrayList<>();

    @Override
    public void addBook(Book book) {
        books.add(book);
    }

    @Override
    public Book findBookByTitle(String title) {
        return books.stream()
                    .filter(book -> book.getTitle().equals(title))
                    .findFirst()
                    .orElse(null);
    }
}

// Сервис для управления библиотекой, где четко разделена бизнес-логика
public class LibraryService {
    private BookRepository repository;

    public LibraryService(BookRepository repository) {
        this.repository = repository;
    }

    public void addNewBook(Book book) {
        repository.addBook(book);
    }

    public void lendBook(String title) {
        Book book = repository.findBookByTitle(title);
        if (book != null) {
            System.out.println("Lending book: " + book.getTitle());
            // Логика выдачи книги
        }
    }
}

```


----

## 5. SOLID 🏗️

**Суть:**  
Набор из пяти принципов ООП, позволяющих создавать гибкий и легко расширяемый код.

### 5.1 Single Responsibility Principle (SRP) 🎯

**Идея:** Каждый класс отвечает только за одну задачу.
```java
// Класс, отвечающий только за хранение данных отчёта
public class Report {
    private String content;

    public Report(String content) {
        this.content = content;
    }

    public String getContent() {
        return content;
    }
}

// Класс, отвечающий только за вывод отчёта
public class ReportPrinter {
    public void printReport(Report report) {
        System.out.println(report.getContent());
    }
}

```

### 5.2 Open/Closed Principle (OCP) 🔓

**Идея:** Модули открыты для расширения, но закрыты для изменений.

**Пример:**
```java
public interface DiscountStrategy {
    double applyDiscount(double amount);
}

public class SeasonalDiscount implements DiscountStrategy {
    @Override
    public double applyDiscount(double amount) {
        return amount * 0.9; // 10% скидка 🎁
    }
}

public class Order {
    private double amount;
    private DiscountStrategy discountStrategy;

    public Order(double amount, DiscountStrategy discountStrategy) {
        this.amount = amount;
        this.discountStrategy = discountStrategy;
    }

    public double getFinalAmount() {
        return discountStrategy.applyDiscount(amount);
    }
}

```

### 5.3 Liskov Substitution Principle (LSP) 🔄

**Идея:** Подклассы должны быть заменяемыми базовыми классами без нарушения работы программы.
```java
public abstract class Shape {
    public abstract double area();
}

public class Rectangle extends Shape {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double area() {
        return width * height;
    }
}

public class Square extends Shape {
    private double side;

    public Square(double side) {
        this.side = side;
    }

    @Override
    public double area() {
        return side * side;
    }
}

```

### 5.4 Interface Segregation Principle (ISP) 🔌

**Идея:** Разделяйте интерфейсы на небольшие и специализированные, чтобы класс не реализовывал ненужные методы.
```java
public interface Printer {
    void print();
}

public interface Scanner {
    void scan();
}

public class MultiFunctionDevice implements Printer, Scanner {
    @Override
    public void print() {
        System.out.println("Printing document... 🖨️");
    }

    @Override
    public void scan() {
        System.out.println("Scanning document... 📠");
    }
}

```

### 5.5 Dependency Inversion Principle (DIP) 🔄

**Идея:** Модули высокого уровня не должны зависеть от модулей низкого уровня – они должны зависеть от абстракций.
```java
public interface Repository {
    void save(Object object);
}

public class UserRepository implements Repository {
    @Override
    public void save(Object object) {
        System.out.println("User saved! ✅");
    }
}

public class UserService {
    private Repository repository;

    public UserService(Repository repository) {
        this.repository = repository;
    }

    public void createUser(Object user) {
        repository.save(user);
    }
}

```

----
## Итоговый вывод 💡

Применяя принципы:

- **KISS** – сохраняем простоту и понятность кода 😊
- **DRY** – избегаем дублирования, создавая переиспользуемые решения 🔄
- **YAGNI** – пишем только необходимое сейчас, не загружая систему лишним функционалом ⏳
- **BDUF** – планируем архитектуру заранее, чтобы избежать дорогостоящих изменений 📝
- **SOLID** – создаём гибкие и легко поддерживаемые системы с помощью набора ООП-принципов 🏗️
- **APO** – оптимизируем только по факту, не жертвуя читаемостью кода ⏱️
- **Бритва Оккама** – выбираем самое простое решение, которое решает задачу без лишних абстракций ✂️