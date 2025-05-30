---
title: Classloaders
tags:
  - JavaSE
related_topics: 
created: 2024-12-12 16:58
modified: 2025-03-31T14:25:27+03:00
questions: 
notes: 
links: 
---



![[Pasted image 20241212170036.png]]

![[Pasted image 20241212170226.png]]



---

![[Pasted image 20241212170434.png]]

<mark class="hltr-yellow">В Java 9 и более поздних версиях введены изменения в механизме загрузки классов, в результате которых Extension Class Loader больше не является отдельным загрузчиком классов. </mark> 
  
В предыдущих версиях Java, начиная с Java 1.2, Extension Class Loader (также известный как Extension Classpath Loader) использовался для загрузки классов из директории расширений (extension directory), которая содержит JAR-файлы, расширяющие функциональность JDK.  
  
Однако, в<mark class="hltr-red"> Java 9+ введена новая модульная система, известная как Java Platform Module System (JPMS), которая вводит понятие модулей и модульной зависимости. В контексте JPMS, Extension Class Loader больше не является отдельным загрузчиком классов, а модульная система управляет загрузкой классов и модулей.  </mark>
  
Вместо использования Extension Class Loader в Java 9+ рекомендуется использовать модульную систему JPMS для управления зависимостями и загрузкой классов из модулей.  
  
Таким образом, Extension Class Loader больше не применяется в Java 9 и выше, и его функции по загрузке классов из директории расширений были перенесены в модульную систему JPMS

![[Pasted image 20241212170458.png]]
![[Pasted image 20241213135110.png]]
![[Pasted image 20241212170746.png]]
![[Pasted image 20241212170924.png]]
![[Pasted image 20241212171002.png]]
![[Pasted image 20241212171146.png]]
![[Pasted image 20241212171300.png]]
![[Pasted image 20241212171407.png]]

![[Pasted image 20241212171434.png]]
![[Pasted image 20241212171604.png]]
![[Pasted image 20241212171714.png]]
![[Pasted image 20241212171803.png]]



### 1. **Загрузка байт-кода и создание экземпляра `Class`**

Этот этап — про то, как JVM находит и загружает байт-код. Вот что происходит здесь:

1. **Поиск и загрузка байт-кода.** JVM ищет файл `.class` по указанному пути (например, в файловой системе, архиве JAR или через сетевой запрос). После нахождения файла, JVM загружает его в память как массив байт.
    
2. **Верификация байт-кода.** На этом этапе JVM проверяет базовую корректность структуры файла `.class`. Например:
    
    - Проверяет, что файл не повреждён.
    - Убеждается, что магическое число в начале файла соответствует стандарту Java (`0xCAFEBABE`).
    - Проверяет наличие корректной информации о полях, методах, иерархии классов и т.д.
    
    **Важно:** здесь проверяется _структура байт-кода_, а не его логическая корректность.
    
3. **Создание объекта `Class`.** После успешной загрузки и проверки байт-кода JVM создаёт объект `Class`, который представляет загруженный класс. Этот объект позволяет работать с метаданными класса в runtime.
    
4. **Загрузка родительских классов и интерфейсов.** Если класс имеет родителя или реализует интерфейсы, JVM должна загрузить их перед завершением этого этапа. Если какой-то из родительских классов не удаётся загрузить, текущий класс не считается загруженным.
    

---

### 2. **Связывание (или линковка)**

Связывание происходит после успешной загрузки класса. Оно состоит из трёх подэтапов:

#### a) **Verification** (проверка корректности)

На этом подэтапе JVM проверяет байт-код более глубоко, чем на этапе загрузки:

- Убеждается, что байт-код соответствует спецификации JVM.
- Проверяет, что инструкции байт-кода корректны:
    - Типы данных совпадают.
    - Нет операций, которые могут вызвать ошибки во время выполнения.
    - Переменные корректно используются.
- Убеждается, что коды доступа (private, public и т.д.) не нарушаются.

Это защита JVM от некорректного или вредоносного байт-кода. Например, некорректный байт-код может быть результатом ручной модификации `.class` файла.



---

#### b) **Preparation** (подготовка)

На этом этапе:

- JVM выделяет память для статических полей класса.
- Статическим полям присваиваются значения по умолчанию:
    - Для чисел — `0`.
    - Для `boolean` — `false`.
    - Для ссылок — `null`.

**Пример:**

```java
static int x = 10;
```

На этом этапе `x` будет равно `0`. Явное присваивание значения (`10`) произойдёт позже, на этапе инициализации.

---
#### Линковка

Линковка (Linking) - это процесс подготовки и проверки типа объекта класса или интерфейса и его прямых родителей (superclass, superinterfaces). Линковка состоит из трёх шагов: верификации, подготовки и опционально resolving.

Верификация (Verifying) это процесс подтверждения того, что класс или интерфейс структурно корректны и подчиняются семантическим требованиям языка программирования Java и JVM, например выполняются следующие проверки:

1. Последовательная и корректно отформатированная таблица символов
    
2. Final методы / классы не переопределяются
    
3. Соблюдается корректность ключевых слов управления доступом (прим. private, protected, public)
    
4. Методы имеют корректное число и типы параметров
    
5. Байт-код корректно работает со стеком
    
6. Переменные инициализированы перед чтением
    
7. Переменные имеют значения соответствующего типа
    

<mark class="hltr-purple">Проведение этих проверок в течении фазы верификации означает, что нет необходимости проводить их во время выполнения (runtime). Верификация во время линковки замедляет загрузку класса, однако позволяет избежать множественных проверок при выполнении байт-кода.</mark>

Подготовка (Preparing) включает выделение памяти (allocation) для статического хранилища и других структур данных, используемых JVM, таких как таблица методов. Статические поля создаются и инициализируются значениями по-умолчанию, однако на этом этапе не выполняется код и инициализации, так это это происходит в рамках самой инициализации.

Resolving это необязательный этап, который состоит из проверки символических ссылок путём загрузки классов или интерфейсов, на которые ссылаются и проверки корректности этих ссылок. Этот этап может быть отложен до момента их использования в байт-коде.

-----
#### c) **Resolution** (разрешение символьных ссылок)

На этапе загрузки и верификации JVM хранит ссылки на классы, поля и методы в виде символов (строк). На этапе разрешения JVM преобразует эти символические ссылки в прямые указатели на объекты в памяти.

**Пример:** Если метод класса `MyClass` вызывает метод `println` у объекта `System.out`, JVM должна найти этот метод в памяти и связать вызов с его адресом.

---

### 3. **Инициализация**

Это завершающий этап. Здесь JVM выполняет статический инициализационный блок (если он есть) и присваивает явные значения статическим полям. Также выполняется конструктор, если создаётся объект.

**Пример:**

 
```java
class MyClass {     
	static int x = 10;      
	static {        
		 System.out.println("Инициализация статического блока");       
		   x = 20;    
	} 
}
```


На этапе инициализации:

1. JVM выполнит статический блок, напечатает сообщение.
2. Значение `x` изменится на `20`.

---

### Почему в двух пунктах упоминается верификация?

1. На этапе загрузки проверяется базовая корректность структуры байт-кода.
2. На этапе связывания (Verification) проверяется корректность самого байт-кода и его безопасности.

Это два разных уровня проверки:

- **Первая верификация**: проверяет структуру файла.
- **Вторая верификация**: проверяет корректность логики выполнения байт-кода.



----

![[Pasted image 20241212185703.png]]

![[Pasted image 20241212185755.png]]
![[Pasted image 20241212185810.png]]
![[Pasted image 20241212185848.png]]
 
![[Pasted image 20241212190800.png]]

![[Pasted image 20241212190833.png]]
![[Pasted image 20241212190846.png]]

---

![[Pasted image 20241212191217.png]]


----

Статические инициализаторы позволяют выполнить сложную логику инициализации, которая может включать обращение к внешним ресурсам, вызов других методов или чтение конфигурационных файлов. Они выполняются только один раз, при первой загрузке класса или при первом обращении к статическим элементам класса.

<mark class="hltr-yellow">Инициализация класса является потокобезопасной операцией в JVM. Механизм синхронизации в JVM гарантирует, что инициализация будет производиться только одним потоком, даже в многопоточной среде. Это обеспечивает корректность и безопасность инициализации класса.</mark>

<mark class="hltr-red">Инициализация класса может также вызывать исключения. Если во время инициализации происходит исключение, JVM обрабатывает его и может отметить класс как неполностью инициализированный. Дальнейший доступ к классу может вызывать исключение ExceptionInInitializerError, указывающее на ошибку в процессе инициализации.</mark>

Пример 
```java
public class Example {
    // Статическое поле
    static int value;

    // Статический блок инициализации
    static {
        System.out.println("Инициализация класса Example");
        // Искусственно вызываем исключение
        value = 10 / 0; // Деление на ноль
    }

    public static void main(String[] args) {
        try {
            // Попытка обратиться к классу Example
            System.out.println(Example.value);
        } catch (ExceptionInInitializerError e) {
            System.err.println("Ошибка: " + e);
            System.err.println("Первопричина: " + e.getCause());
        }
    }
}

```
![[Pasted image 20241217172449.png]]

### Объяснение:

1. **Что происходит в коде?**
    
    - Когда JVM пытается загрузить и инициализировать класс `Example`, она выполняет статический блок инициализации.
    - В блоке инициализации возникает деление на ноль (`10 / 0`), что вызывает **ArithmeticException**.
2. **Как обрабатывается исключение JVM?**
    
    - JVM фиксирует, что класс не был инициализирован корректно.
    - JVM выбрасывает **ExceptionInInitializerError**, чтобы сигнализировать, что возникла ошибка в процессе инициализации класса.
3. **Результат выполнения кода:**
Инициализация класса Example
Ошибка: java.lang.ExceptionInInitializerError
Первопричина: java.lang.ArithmeticException: / by zero

### Важные моменты:

- **ExceptionInInitializerError** <mark class="hltr-yellow">возникает только для ошибок, связанных со статической инициализацией (включая статические блоки или статические поля).</mark>
- После выброса этого исключения класс считается "неполностью инициализированным", и дальнейшие обращения к классу также будут приводить к тому же исключению.

Это демонстрирует, как исключения во время инициализации могут повлиять на поведение класса в JVM.


![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXda9OyebK-vyJGe08_FlKEw4DYFGWFkjBuunPX-XedjXkyuHqL40FTUuE1PgzHU1iOxDY5MjF19nfdlK5WhFAVK23zCBed-TpR7GAavxBKEqcbJAC8aMI57ltRiW3MWEswVgi-I_3AGLmntme00_6By34-MBa_TsGqMz_QRKXRlq7eblwHYe10?key=ckipu91rvPgHMFice7GhIA)



------

**Таблица методов (method table)** – это внутренняя структура данных, которую использует JVM для динамического связывания вызовов методов. Она хранит указатели (ссылки) на реализацию методов класса.

### Что это даёт:

- **Динамический полиморфизм:**  
    Когда вызывается виртуальный метод на объекте, JVM обращается к таблице методов, чтобы определить, какая именно реализация метода должна быть выполнена, учитывая переопределения в подклассах.
    
- **Быстрый вызов методов:**  
    Таблица методов позволяет оптимизировать вызов методов, так как поиск нужного метода сводится к обращению к фиксированному массиву указателей.
    

### Как это работает:

1. **При загрузке класса:**  
    JVM создаёт для каждого класса таблицу методов, в которой содержатся ссылки на все виртуальные (недекларированные как static, private или final) методы класса.
    
2. **При вызове метода:**  
    Если у объекта вызывается метод, JVM через таблицу методов определяет, какая реализация метода соответствует фактическому типу объекта, и вызывает её.
    

Таким образом, таблица методов является ключевым элементом для реализации переопределения методов и полиморфизма в Java. Она обеспечивает правильное и эффективное выполнение вызовов методов в иерархии классов

-----

🔹 **Этап "Подготовка" в класслоадере** отвечает за выделение памяти для всех структур, связанных с классом, в том числе статического хранилища (метод-таблицы, статические поля и т.д.). На этом этапе происходит следующее:

- **Выделение памяти:** JVM резервирует место для всех статических полей класса.
    
- **Инициализация значениями по умолчанию:** Все статические поля получают базовые значения (например, 0 для чисел, false для boolean, null для ссылок).
    

---

🔸 **Зачем это нужно?**

1. **Гарантия предсказуемости:**  
    До того, как будет выполнен пользовательский код (например, блоки инициализации или метод <clinit\>), статические поля уже существуют и имеют определённые значения. Это гарантирует, что если другой класс обратится к этим полям до их явной инициализации, он увидит стабильные значения по умолчанию.
    
2. **Безопасное связывание:**  
    Фаза подготовки обеспечивает корректное выделение памяти и установку базового состояния для всех классов, что необходимо для последующей фазы инициализации. Это снижает риск ошибок при запуске кода, который может обращаться к статическим полям.
    
3. **Разделение обязанностей:**  
    JVM разделяет загрузку, связывание (linking) и инициализацию класса на отдельные этапы:
    
    - **Подготовка (Preparation):** Выделяется память и устанавливаются значения по умолчанию.
        
    - **Инициализация (Initialization):** Выполняется пользовательский код (например, статические блоки инициализации, <clinit\>), который задаёт конечные значения для статических полей.
        
    
    Такое разделение гарантирует, что базовое состояние класса (все поля существуют) установлено до того, как любой код начнёт их изменять.


------