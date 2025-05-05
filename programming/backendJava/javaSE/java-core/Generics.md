---
title: Generics
tags:
  - JavaSE
related_topics: 
created: 2024-08-31 13:39
modified: 2025-02-24T11:34:31+03:00
difficulty: 
questions: 
notes: 
links:
  - https://javarush.com/quests/lectures/jru.questcollections.level05.lecture09
---

# Generics

![[Pasted image 20250210173213.png]]

## Raw type

Проблема связанная до появления Generics представлена в коде ниже, невозможно гарантировать, что в список добавят элементы, которые принадлежат одному типу

```java
ArrayList stringList = new ArrayList();
stringList.add("abc"); //добавляем строку в список
stringList.add("abc"); //добавляем строку в список
stringList.add( 1 ); //добавляем число в список

for(Object o: stringList)
{
 String s = (String) o; //тут будет ClassCastException,
// когда дойдем до элемента-числа
}

```

Для решения данной проблемы приходилось проверять на принадлежность к типу:

```java
for (Object o : stringList) {
            if(o instanceof String){
                String s = ((String) o).toUpperCase();
            }
            else if(o instanceof Integer){
                String s = String.valueOf(o).toUpperCase();
            }
        }
```

Как решают данную проблему Generics, типизируя список, мы разрешаем класть в него в данном случае только экземпляры класса String:

```java
ArrayList<String> stringList = new ArrayList<String>();
stringList.add("abc"); //добавляем строку в список
stringList.add("abc"); //добавляем строку в список

stringList.add( 1 );**//тут будет ошибка компиляции**

for(Object o: stringList) {
 String s = (String) o;
}

Что происходит под капотом:

List strings = new ArrayList();

strings.add((String)"abc");
strings.add((String)"abc");

strings.add((String) 1); //ошибка компиляции

for(String s: strings)
{
 System.out.println(s);
}
```

## Стирание типов

Внутри класса-generic’а не хранится никакой информации о его типе-параметре:

```java
class Zoo<T>
{
 ArrayList<T> pets = new ArrayList<T>();

 public T createAnimal()
 {
  T animal = new T();
  pets.add(animal)
  return animal;
 }
}
```

```java
class Zoo
{
 ArrayList pets = new ArrayList();

 public Object createAnimal()
 {
  Object animal = new ???();
  pets.add(animal)
  return animal;
 }
}
```

При компиляции, все параметры типов (в данном случае `T`) стираются и заменяются на тип `Object`. Это означает, что, <mark class="hltr-yellow">несмотря на то, что в исходном коде мы работаем с параметризованными типами, в скомпилированном коде мы работаем с обычными объектами </mark>`Object`. <mark class="hltr-red">Этот процесс называется стиранием типов.</mark>

Таким образом, стирание типов позволяет Java сохранять обратную совместимость с более старыми версиями языка, где Generics не были поддерживаемы. Однако, это ограничивает возможности работы с параметризованными типами на уровне компиляции и требует использования различных механизмов для обхода ограничений стирания типов, таких как wildcard-аргументы, reflection

## WildCards

<mark class="hltr-red">Wildcards</mark> — э<mark class="hltr-yellow">то специальный механизм языка Java Generics, который позволяет определить параметры типов нестрого и динамически.</mark> Они используются для обозначения типов, которые неизвестны в момент написания кода, но могут быть определены в момент выполнения.

<mark class="hltr-purple">Существует три разновидности Wildcards:</mark>

- `? extends T`: <mark class="hltr-yellow">ограничивает тип сверху</mark> (<mark class="hltr-cyan">upper bounded wildcard</mark>). Этот wildcard принимает все типы, которые являются подтипами типа T. Например, если у нас есть класс `Animal`, а класс `Dog` является его наследником, то мы можем использовать `? extends Animal` как тип для параметра метода, который принимает все объекты, которые являются экземплярами класса `Animal` или его наследников.

```java
public static void printAnimals(List<? extends Animal> animals) {
    for (Animal a : animals) {
        System.out.println(a.getName());
    }
}
```

- `? super T`: <mark class="hltr-yellow">ограничивает тип снизу</mark> (<mark class="hltr-cyan">lower bounded wildcard</mark>). Этот wildcard принимает все типы, которые являются супертипами типа T. Например, если у нас есть класс `Animal`, а класс `Dog` является его наследником, то мы можем использовать `? super Dog` как тип для параметра метода, который принимает все объекты, которые являются экземплярами класса `Dog` или его родительских классов. Наследники класса Dog передавать в список нельзя.

```java
public static void addDog(List<? super Dog> animals) {
    animals.add(new Dog("Buddy"));
}
```

- `?`: неограниченный wildcard (<mark class="hltr-cyan">unbounded wildcard</mark>). Этот wildcard может принимать любой тип.

```java
public static void addTotListAndPrint(List<?> list) {
list.add(new String("Hello)";

list.add(new Boolean(true);

    for (Object o : list) {
        System.out.println(o);
    }
}
```

### Проблема, которую решает extends

В данном примере нельзя передать List\<Archer> или List\<Magician>\в метод, который принимает List\<Warrior>\, хотя эти классы наследники Warrior.

Потому что List\<Archer> не наследник List\<Warrior>.

```java
public  abstract  class Warrior {
}

public class Magician extends Warrior{}

public class Archer extends Warrior{}
```

```java
public static void main(String[] args) {

List<Archer> archers = new ArrayList<>();
archers.add(new Archer());
List<Magician> magicians = new ArrayList<>();
magicians.add(new Magician());

boolean isFirstTeamCooler = teamFight(archers,magicians);
}  //ошибка компиляции

public static boolean teamFight(List<Warrior> firstTeam,
 List<Warrior> secondTeam) {
if (firstTeam.size() > secondTeam.size()) return true;
else return false;
}
}
```

Решается с помощью extends:

```java
public static boolean teamFight(List<? extends Warrior> firstTeam,
 List<? extends Warrior> secondTeam) {
        if (firstTeam.size() > secondTeam.size()) return true;
        else return false;
    }
```

### Проблема, которую решает super

```java
public void do(List<?extends Myclass list){
for(Myclass object : list){
sout(object.getState());  // тут все работает
list.add(new Myclass); // тут сломается
}
```

Потому что в качестве аргумента в <mark class="hltr-yellow">функцию может прилететь не</mark> List\<MyClass>, <mark class="hltr-yellow">a List наследник MyClass, и в него уже передать родителя нельзя.</mark>

Чтобы решить эту проблему <mark class="hltr-yellow">необходимо использовать super</mark>, так как тогда в функцию можно передать либо List\<MyClass> либо список родителя MyClass.

```java
public void do(List<?super Myclass list){
for(Myclass object : list){
sout(object.getState());  // тут все работает
list.add(new Myclass); // тут сломается
}
```

Но сломается этот код:

```java
public void do(List<?super Myclass list){
for(Myclass object : list){ // тут сломается

list.add(new Myclass); // тут работает
}
```

<mark class="hltr-yellow">Потому что в качестве аргумента в функцию может поступить класс Object, и перебор по MyClass невозможен.</mark>




![[images/Untitled 159.png|Untitled 159.png]]

![[images/Untitled 1 20.png|Untitled 1 20.png]]

![[images/Untitled 2 18.png|Untitled 2 18.png]]

![[images/Untitled 3 17.png|Untitled 3 17.png]]

### Наследование и расширители обобщений 

  
![[images/Untitled 4 17.png|Untitled 4 17.png]]

- Параметризация на уровне метода. (Такая же как у класса).
- Какой дженерик у `Hero<World>` значит можно передать людей `Class extends Herro`, но чтобы у этого класса был `<World>`
- Параметризация на уровне метода. \<T> решает эту проблему

### Wildcard

![[images/Untitled 5 17.png|Untitled 5 17.png]]

- Не так удобно оперировать, как с параметризацией на уровне метода
- Consumer -super / Produser - extends

  

  

![[images/Untitled 6 16.png|Untitled 6 16.png]]

![[images/Untitled 7 15.png|Untitled 7 15.png]]

Вопрос про 3 тип

![[images/Untitled 8 14.png|Untitled 8 14.png]]

![[images/Untitled 9 14.png|Untitled 9 14.png]]

![[images/Untitled 10 12.png|Untitled 10 12.png]]

![[images/Untitled 11 12.png|Untitled 11 12.png]]

![[images/Untitled 12 12.png|Untitled 12 12.png]]

  

![[images/Untitled 13 12.png|Untitled 13 12.png]]



------

<mark class="hltr-yellow">Существует такое понятие, связанное с generics</mark>, как необработанные типы (в литературе, интернете еще можно встретить такое название как "<mark class="hltr-red">сырые типы</mark>"). Обозначаются они также как и generics в скобках <>, в которых проставляются заглавные латинские символы, зарезервированные специально для этих целей символы - полный список можно найти по ссылке:
[https://docs.oracle.com/javase/tutorial/java/generics/types.html](https://docs.oracle.com/javase/tutorial/java/generics/types.html)

Для того чтобы создать класс общего типа достаточно в его объявлении в <> указать перечень общих типов, которые будут использоваться для реализации класса (типов может быть несколько):
```java
package ru.job4j.generics;

public class GenericsClass<K, V> {
    private K key;

    private V value;

    public GenericsClass(K key, V value) {
        this.key = key;
        this.value = value;
    }

    @Override
    public String toString() {
        return "GenericsClass{"
                + "key=" + key
                + ", value=" + value
                + '}';
    }
}
```

Когда мы будем использовать этот класс нам будет необходимо указать точный параметр, который будет использоваться вместо K и V. Добавим метод main() выведем в консоль создаваемые объекты с разными параметрами:

```java
public static void main(String[] args) {
    GenericsClass<String, String> first = new GenericsClass<>("First key", "First value");
    System.out.println("Вывод в консоль: " + first);

    GenericsClass<Integer, String> second = new GenericsClass<>(12345, "Second value");
    System.out.println("Вывод в консоль: " + second);
}
```

<mark class="hltr-green2">Подобным же образом generics может использоваться и с интерфейсами</mark>

-------

<mark class="hltr-red">!!!!!!!!!!!!!!</mark>
Рассмотрим следующий универсальный класс, который представляет собой узел в односвязном списке:
```java
package ru.job4j.generics;

public class Node<T> {
    private T data;
    private Node<T> next;

    public Node(T data, Node<T> next) {
        this.data = data;
        this.next = next;
    }

    public T getData() {
        return data;
    }
}
```

![[Pasted image 20250219135155.png]]


------

### !!!!!!!!!!!!!!!!!!!!!!!!!!!!!
Осталось разобраться с одним вопросом - <mark class="hltr-yellow">мы можем задавать тип generics, а можем ли мы его прочитать и узнать?</mark> Такая возможность есть - даже несмотря на то, что это используется довольно редко, но об этом знать необходимо.<mark class="hltr-red"> Существует только одна ситуация, когда универсальный тип доступен во время выполнения - это когда универсальный тип является частью сигнатуры класса подобным образом:</mark>

```java
package ru.job4j.generics;

import java.util.ArrayList;

public class FloatList extends ArrayList<Float> {
   
}
```

<mark class="hltr-green2">теперь мы можем узнать что класс ArrayList (а, соответственно, и класс FloatList) был параметризован классом Float следующим образом:</mark>

```java
public static void main(String[] args) {
    ArrayList<Float> listOfNumbers = new FloatList();
    
    Class actual = listOfNumbers.getClass();
    ParameterizedType type = (ParameterizedType) actual.getGenericSuperclass();
    System.out.println(type);
    Class parameter = (Class) type.getActualTypeArguments()[0];
    System.out.println(parameter);
}
```

![[Pasted image 20250219135426.png]]

<mark class="hltr-orange">Таким образом, теперь мы можем узнать актуальный параметр generic-класса, если этот параметр был задан явным образом (т.е. параметр определен внутри секции extends одного из наследников).</mark>

-----------------

![[Pasted image 20250219135639.png]]

------

![[Pasted image 20250219151925.png]]


## Ковариантность, контравариантность и инвариантность
------
 <mark class="hltr-red">Ковариантность</mark> — это <mark class="hltr-green2">сохранение иерархии наследования исходных типов в производных типах в том же порядке.</mark> Например, если _Кошка_ — это подтип _Животные_, то _Множество<Кошки>_ — это подтип _Множество<Животные>_. <mark class="hltr-orange">Следовательно, с учетом принципа подстановки можно выполнить такое присваивание:  </mark>
  
_Множество<Животные>  = Множество<Кошки>_  

**Массивы в Java ковариантны**. Тип `S[]` является подтипом `T[]`, если `S` — подтип `T`. Пример присваивания:
```java
String[] strings = new String[] {"a", "b", "c"};
Object[] arr = strings;
```

Мы присвоили ссылку на массив строк переменной `arr`, тип которой – `«массив объектов»`. Если бы массивы не были ковариантными, нам бы это сделать не удалось. Java позволяет это сделать, программа скомпилируется и выполнится без ошибок.
```java
arr[0] = 42; // ArrayStoreException. Проблема обнаружилась на этапе выполнения программы
```

Но если мы попытаемся изменить содержимое массива через переменную `arr` и запишем туда число 42, то получим `ArrayStoreException` на этапе выполнения программы, поскольку<mark class="hltr-yellow"> 42 является не строкой, а числом. </mark>В этом недостаток ковариантности массивов Java: мы не можем выполнить проверки на этапе компиляции, и что-то может сломаться уже в рантайме.

-----
<mark class="hltr-red">Контравариантность</mark> — это <mark class="hltr-green2">обращение иерархии исходных типов на противоположную в производных типах</mark>. Например, если _Кошка_ — это подтип `Животные`, то _Множество<Животные>_ — это подтип _Множество<Кошки>_. <mark class="hltr-orange">Следовательно,  с учетом принципа подстановки можно выполнить такое присваивание: </mark> 
  
_Множество<Кошки> = Множество<Животные>_  

-------  
<mark class="hltr-red">Инвариантность</mark> — <mark class="hltr-green2">отсутствие наследования между производными типами</mark>. Если _Кошка_ — это подтип _Животные_, то _Множество<Кошки>_ <mark class="hltr-orange">не является подтипом</mark> _Множество<Животные>_ и _Множество<Животные>_ <mark class="hltr-orange">не является подтипо</mark>м _Множество<Кошки>_.

**«Дженерики» инвариантны.** Приведем пример:
```java
List<Integer> ints = Arrays.asList(1,2,3);
List<Number> nums = ints; // compile-time error. Проблема обнаружилась на этапе компиляции
nums.set(2, 3.14);
assert ints.toString().equals("[1, 2, 3.14]");
```

Если взять список целых чисел, то он не будет являться ни подтипом типа `Number`, ни каким-либо другим подтипом. Он является только подтипом самого себя. То есть `List <Integer>` — это `List<Integer>` и ничего больше. Компилятор позаботится о том, чтобы переменная `ints`, объявленная как список объектов класса _Integer_, содержала только объекты класса `Integer` и ничего кроме них. <mark class="hltr-orange">На этапе компиляции производится проверка, и у нас в рантайме уже ничего не упадет.</mark>

--------

**Всегда ли Generics инварианты? Нет. Приведу примеры:**

Это <mark class="hltr-orange">ковариантность</mark>. `List<Integer>` — подтип `List<? extends Number>`
```java
List<Integer> ints = new ArrayList<Integer>();
List<? extends Number> nums = ints;
```

Это <mark class="hltr-orange">контравариантность</mark>. `List<Number>` является подтипом `List<? super Integer>`.
```java
List<Number> nums = new ArrayList<Number>();
List<? super Integer> ints = nums;
```

----
### The Get and Put Principle или PECS (Producer Extends Consumer Super)

Особенность wildcard с верхней и нижней границей дает дополнительные фичи, связанные с безопасным использованием типов.<mark class="hltr-red"> Из одного типа переменных можно только читать, в другой — только вписывать (исключением является возможность записать </mark>`null` для `extends` и прочитать `Object` для `super`). Чтобы было легче запомнить, когда какой wildcard использовать, существует принцип PECS — Producer Extends Consumer Super.  

- Если мы объявили _wildcard с extends_, то это _producer_. Он только «продюсирует», предоставляет элемент из контейнера, а сам ничего не принимает.  
    
- Если же мы объявили _wildcard с super_ — то это _consumer_. Он только принимает, а предоставить ничего не может.

-----

![[Pasted image 20250219154232.png]]

![[Pasted image 20250219154245.png]]


------

![[Pasted image 20250219154822.png]]


-----


![[Pasted image 20250219154846.png]]
![[Pasted image 20250219154901.png]]

------
https://habr.com/ru/companies/sberbank/articles/416413/
https://habr.com/ru/articles/207360/#pecs
----

🚀 **Разница между методами:**

- **Метод с `<T>`:**
    
    - Объявляется обобщённый тип `T` для метода.
    - Это позволяет использовать тип `T` в дальнейших вычислениях или возвращать объекты типа `T`, если потребуется.
    - В данном случае, так как мы просто выводим элементы, этот функционал не используется.
    - 🔍 **Плюс:** Гибкость для расширения логики в будущем, если понадобится работать именно с типом элемента.
- **Метод с `<?>` (wildcard):**
    
    - Используется универсальный wildcard, что позволяет принимать список с любым типом элементов.
    - Код получается короче и проще, если не требуется работать с конкретным типом.
    - 🔍 **Плюс:** Читаемость и лаконичность, особенно если метод только выводит элементы.

🔥 **Какой лучше написать?**  
Если задача — просто вывести элементы списка, то метод с `List<?>` будет предпочтительнее, так как он проще и не перегружен лишними обобщениями. Это также часто приветствуется на собеседованиях за чистоту и понятность кода.

✅ **Итог:**  
Для вывода элементов списка, независимо от типа, оптимально использовать метод с `List<?>`.

-----

![[Pasted image 20250224113112.png]]

-------------

**Reifiable (реифицируемый)** — тип, о котором **полностью** доступна информация в runtime. Java спецификация называет reifiable те типы, которым **не нужны** дополнительные проверки или информация о параметрах типа во время выполнения.

К ним относятся:

1. Примитивы (например, `int`, `double`)
2. Непараметризованные классы или интерфейсы (например, `String`, `Integer`)
3. Параметризованные типы, где **все** параметры — unbounded wildcard (`?`)
    - Пример: `List<?>`, `Map<?, ?>`
4. Raw-типы (например, `List`, `Map`)
5. Массивы из reifiable-типов (например, `String[]`, `List[]`)

### 2. `List<?>` против `List<T>`

- **`List<?>`**:
    
    - Синтаксически говорит компилятору: «Это список, чьи элементы могут быть **любого** типа».
    - При этом во время выполнения `List<?>` выглядит как raw-тип `List`, **но** это допустимо, потому что «?» не требует от JVM «хранить» какой-то конкретный тип для проверки.
    - Спецификация Java разрешает такой параметр как reifiable, ведь там не остаётся никакой информации, которую нужно было бы проверить или хранить.
- **`List<T>`** (где `T` — это **type variable**, переменная типа)
    
    - Во время компиляции `T` может быть заменён на `String`, `Integer` и т.д.
    - Но в **байткоде** всё равно стирается до `List`.
    - Для JVM `T` — это «что-то конкретное», но **неизвестное**, и чтобы корректно проверять такой объект на этапе runtime, пришлось бы хранить «T» (чего Java не делает).
    - Следовательно, **параметризованный тип с настоящим `<T>`** не может считаться reifiable, потому что JVM **не знает** (и не хранит) реальный параметр, а значит, **не может** его проверить.

> **Почему `List<?>` считается reifiable, хотя «?» тоже стирается?**  
> Потому что «?» в Java — это просто «любой тип» (unbounded wildcard). Компилятору **не** нужно (и нельзя) вставлять туда проверки для runtime, ведь «?» уже говорит: «мне всё подходит». Нет никаких «ограничений» или «дополнительной информации», которую нужно сохранить.

Иными словами:

- `List<?>` → «Список чего угодно» → **никаких** проверок на этапе выполнения **не требуется** → reifiable.
- `List<T>` (с настоящим `T`) → «Список чего-то конкретного, но неизвестного» → чтобы корректно проверить, что объект действительно «список чего-то конкретного», нужна бы дополнительная информация о «T» → **стирается**, значит, JVM не сможет проверить → **не** reifiable.

----------------
