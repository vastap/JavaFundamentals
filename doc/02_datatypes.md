# <a name="Home"></a> Working with Java data types

## Содержание:
- [Overview](#Overview)
- [Declare and initialize variables](#DeclareAndInit)
    - [Casting](#Casting)
	- [Number Systems](#NumberSystems)
- [Object Reference and Primitives](#ObjReference)
    - [Integer Primitives](#IntegerPrimitives)
	- [Floating Primitives](#Floating)
	- [Boolean and Char](#BooleanAndChar)
	- [Difference](#Difference)
- [Read and Write to object fields](#ReadWriteToObjFields)
- [Object lifecycle](#ObjLifecycle)
    - [Garbage Collector](#GC)
- [Wrapper classes (such as Boolean, Double, and Integer)](#Wrappers)
Дополнительно:
- [Формат литералов](#Literal)
- [Дополнительные материалы](#Resources)

## [↑](#Home) <a name="Overview"></a> Overview
Данный раздел относится к [OCA Java SE 8 Programmer I](http://education.oracle.com/pls/web_prod-plq-dad/db_pages.getpage?page_id=5001&get_params=p_exam_id:1Z0-808).
Разделы: "[Java SE 8 Programmer I Exam Topics](https://docs.oracle.com/javase/tutorial/extra/certification/javase-8-programmer1.html#dataTypes)"
Подробнее: "[2. Working With Java Data Types](http://javacertification.wikidot.com/data-types)"

## [↑](#Home) <a name="DeclareAndInit"></a> Declare and initialize variables
**Объявление:** Как и в других языках программирования, Java использует переменные для хранения промежуточных результатов. См. официальные Tutorials: "[Variables](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/variables.html)".

Объявление переменной состоит из трёх частей:
- Модификатор доступа (не указан = package private)
- Тип поля (переменной)
- Назвние поля (переменной)
Например: ``private final String message;``

**Правила именования:**
- чувствительны к регистру
- начинаются с маленькой буквы, новые части названия с большой
- могут начинаться на: _, $, or letter
- другие символы: _, $, letters, numbers
- константы (static final) в верхнем регистре, части разделяются подчёркиваниями

**Инициализация:** После объявления переменные могут быть инициализированы: "[Initializing Fields](https://docs.oracle.com/javase/tutorial/java/javaOO/initial.html)".
Инициализация поля - заполнение поля значением. Именно в этот момент будет выделено место в памяти, а не в момент объявления.

Могут быть инициализированы в одну строку:
```java
int x = 1, y = 2, z = x + y;
```
или даже
```java
int i, j, k;
i = j = k = 9;
```
Но такой стиль написания считается некорректным, т.к. трудно понять, что и чем заполняется. 
Например, в ``int one, two = 0;`` заполнится только two. Но код внешне не очевиден

Если значения для полей не указаны, они будут инициализированы значениями по умолчанию:
- Для объектных типов значение - null
- Для примитивов: для числовых - 0, для логических - false

Кроме того, начиная с Java 7 стала допустимой запись вида:
```java
double legal = 1_00_0.0_0;
```
Главное, чтобы число с подчёркивания не начиналось, не заканчивалось, и чтобы подчёркивание не стояло у точки.

### [↑](#Home) <a name="Casting"></a> Casting
Приведение типов описано в главе "[Chapter 5. Conversions and Contexts](https://docs.oracle.com/javase/specs/jls/se8/html/jls-5.html)" спецификации языка Java.

**Приведение типов бывает:**
- сужающим (Narrowing conversions)
например, из int в byte
Требует явного указания типа, к которому приводится.
Например: `` byte b = (byte)a; ``
- расширяющим (Widening conversions)
например, из byte в int
Не требует явного указания типа, является неявным.

Про преобразование типов хорошо описано тут:
- "[Преобразования базовых типов данных](https://metanit.com/java/tutorial/2.2.php)"
- "[Преобразование типов в Java](https://vertex-academy.com/tutorials/ru/prividenie-tipov-v-java/)"
- "[Преобразование примитивных типов в Java](http://pr0java.blogspot.ru/2015/12/java.html)"

Интересная особенность при вычислениях: [Сложение 2 чисел типа short в Java](https://ru.stackoverflow.com/questions/608863/%D0%A1%D0%BB%D0%BE%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5-2-%D1%87%D0%B8%D1%81%D0%B5%D0%BB-%D1%82%D0%B8%D0%BF%D0%B0-short-%D0%B2-java).

### [↑](#Home) <a name="NumberSystems"></a> Number Systems
В Java есть возможность использовать разные системы счисления:
```java
public static void numberSystems() {
	int decimal = 15;
	int binary = 0b00001111;    // or 0B
	int octal = 017;            // only from 0-7, no 8 or 9
	int hex = 0xF;             // or 0XEF
}
```
Значения указываются литералами.
**Литерал** - явно заданное значение. Могут быть указаны следующим образом:
- Десятеричная система: ``10;``
- Шестнадцатеричная система: ``0x1F4``, начинается с ``0x``
```
--> Перевод числа в шестнадцатеричную систему:
500/16: 31 при умножении на 16 даёт 496, 500-496, даёт остаток 4
31/16: можем 16 только умножить на 1, 31-16 даёт остаток 15
Дошли до 1, дальше не продолжаем. 15 это F.
На конце 1. Переворачиваем 4F1 и получаем ответ: 1F4
--> Обратное действует так же:
1F4 состоит из трёх чисел, поэтому:
1*16^2 + 15*16^1 + 4*16^0 = 256 + 240 + 4 = 496 + 4 = 500
```
- Восьмеричная система: ``010``, начинается с нуля.
```
По аналогии с вышеуказанной:
010 = 3 знака
0*8^2 + 1*8^1 + 0*8^0 = 0+8+0 = 8
```
- Двоичная система (начиная с Java7): ``0b101`` ,[начинается с 0b](http://docs.oracle.com/javase/7/docs/technotes/guides/language/binary-literals.html)
```
1*2^2 + 0*2^1 + 1^2^0 = 1*4 + 1 = 5
```
Пример переводов можно также посмотреть в книгах, вроде этой: [Assembler: Учеб. для вузов](https://goo.gl/VNs9xr).


## [↑](#Home) <a name="ObjReference"></a> Differentiate between object reference variables and primitive variables
Данный раздел описывается в "[Primitive Data Types](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)".

Java - язык со строгой типизацией. Это означает, что у каждого значения есть свой тип. В Java используемые типы данных можно разделить на две категории:
- Примитивные
- Ссылочные (объектные)

![](../img/JavaTypes.png)
Существует **8** примитивных типов, как количество битов в байте.
4 из них являются целочисленными (**Integer**), два из них являются числами с плавающей точкой (**Floating**), логический **boolean** и символьный **char**.

Минимальной единицей информации является **бит**. Он может принимать значение **1** или **0**. По сути, вся информация из себя и представляет набор единиц и нулей, то есть бит. Так как мы работаем с двоичной системой, то значение вычисляется при помощи возведения **2** в определённую степень.

Для вычисления можно следовать следующей логике: 2 в степени 2 (т.е. в квадрате) = 4. Дальше каждая степень является результатом умножения предыдущего числа на 2. Поэтому, получаем следующую цепочку:
``4`` -> ``8`` -> ``16`` -> ``32`` -> ``64`` -> ``128`` -> ``256`` -> ``512`` -> ...

Подробнее можно прочитать в статье "[Примитивные типы в Java](https://sohabr.net/habr/post/261315/)".

### [↑](#Home) <a name="IntegerPrimitives"></a> Integer Primitives (Целочисленные типы)
#### byte
8 бит объединяются в 1 байт и образуют самый маленький примитивный тип в Java, называемый **byte**.
Принимает значения от -128 до 127. Казалось бы, почему не до 128? А всё потому, что 1 бит "расходуется" на определение того, положительное число или нет.
Подробнее см. [Why is the range of bytes -128 to 127 in Java?](https://stackoverflow.com/a/42623517).
Значение можно запомнить как -2 в степени на единицу меньшей, чем количество бит для нижней границы, а для верхней границы тоже самое но минус 1 (т.к. -1 бит на знак).
++Минимальное значение++: -2^7. Как мы видели ранее, это -128.
++Максимальное значение++: 2^7 - 1 = 128-1 = 127

#### short
Следующий тип - короче чем integer. Поэтому называется **short**.
Размер примитивных типов возрастает в 2 раза. Его размер больше чем байт в 2 раза, т.е. 8*2=16, но меньше чем integer в 2 раза.
++Минимальное значение++: -2^15
++Максимальное значение++: 2^15-1

#### integer
Следующий примитивный тип - **integer**. Он в 2 раза больше, чем short и равен 32 битам.
++Минимальное значение++: -2^31
++Максимальное значение++: 2^31-1
Это почти 2 с девятью нулями, даже больше.
В интернете можно найти сведения, например в статье "[Примитивные типы в Java](https://sohabr.net/habr/post/261315/)", что при выполнении действий с short и byte они будут приведены к integer (так называемое **расширяющее преобразование**).

#### long
Следующий тип - длиннее чем integer. Поэтому называется **long**.
Он больше чем integer в 2 раза и равен 64 битам.
Соответственно, его значения от -2^63 до 2^63-1.

### [↑](#Home) <a name="Floating"></a> Floating Primitives (C плавающей точкой)
Реализованы в Java в виде двух примитивных типов:
- **float** (32 бита)
- **double** (64 бита)
Примером может служить число pi из матерматики:
```java
public class FloatAndDouble {
 	public static void main(String[] args) {
        float piValue = (float)Math.PI;
        double piValueExt = Math.PI;
        System.out.println("Float value: "+piValue );
        System.out.println("Double value: "+piValueExt );
    }
}
```
Можно увидеть разницу, скопировав код сюда: [compile java online](https://www.tutorialspoint.com/compile_java_online.php).
Результат:
```
Float value: 3.1415927
Double value: 3.141592653589793
```
Лишний раз вспомним, что Math - часть пакета java.lang, подключаемый по умолчанию.
Так же стоит не забывать и про разницу в точности. Подробнее см. "[Потеря точности из Double во Float или «Куда пропадали копейки?»](https://habrahabr.ru/post/201066/)
Здесь же можно упоминуть и про модификатор strictfp. Подробнее см. "[Модификатор strictfp](https://ru.stackoverflow.com/questions/617822/%D0%9C%D0%BE%D0%B4%D0%B8%D1%84%D0%B8%D0%BA%D0%B0%D1%82%D0%BE%D1%80-strictfp)".

###  [↑](#Home) <a name="BooleanAndChar"></a> Boolean и Char
#### char
Представляет из себя Unicode символ и занимает 16 бит (половину от того, что занимает integer).
Его значение по умолчанию равно ‘\u0000’.
Как сказано в [Java Api: Character](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html), "A char value, therefore, represents Basic Multilingual Plane (BMP) code points" - т.е. это ["Основная многоязычная плоскость"](https://goo.gl/9WVdQ4).

#### boolean
Хранят логическое значение true или false.
Несмотря на то, что казалось бы, может иметь значение 0 или 1 занимаемый объём памяти зависит от конкретной реализации JVM.
Подробнее см. [Why is Java's boolean primitive size not defined?](https://stackoverflow.com/questions/1907318/why-is-javas-boolean-primitive-size-not-defined).

### [↑](#Home) <a name="Difference"></a> Difference
Разница между примитивными и ссылочными типами заключается в способе их хранения в памяти.
Примитивы хранятся в области памяти под названием Stack: "[2.5.2. Java Virtual Machine Stacks](https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-2.html#jvms-2.5.2)".
Ссылочные типы хранятся в области памяти под названием Heap: "[2.5.3 Heap](https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-2.html#jvms-2.5.3)".

Первое, что важно, это оператор сравнения ``==``:
```java
Integer a = 300;
Integer b = 300;
System.out.println(a == b);
```
Вернёт false, т.к. **a** и **b** в данном случае это разные объекты, а == сравнивает ссылочные типы по ссылке, а не по значению.
Тут важно вот что: "[5.1.7. Boxing Conversion](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html#jls-5.1.7)". То есть в пределах указанных значений, а для Integer это Integer Pool, сравнение чисел от -128 до 127 по == равносильно сравнению по значению. Поэтому при уменьшении с 300 до 127 значение изменится с false на true.

Следующая важная особенность - передача примитивов и объектов в параметре метода.
Подробнее: "[Передача параметров в Java](http://info.javarush.ru/translation/2014/06/30/Передача-параметров-в-Java-перевод-.html)".
Важно это потому, что передача параметров в java всегда по значению. Важно, что значение примитивного типа совпадает с его значением, т.е. если int numb = 2, то numb равен двум. А вот значение ссылочного типа = ссылке на объект. Поэтому когда Integer numb = 2, то numb будет равен не двум, а ссылке на объект, через которую мы можем пройти к объекту, содержащий значение 2.

Объекты должны сравниваться через специальный метод Object'а - **equals**.

## [↑](#Home) <a name="ReadWriteToObjFields"></a> Read or write to object fields
Объекты могут содержать поля: "[Declaring Member Variables](https://docs.oracle.com/javase/tutorial/java/javaOO/variables.html)".
```java
public class HelloWorld{

     public static void main(String []args){
        Example example = new Example();
        example.printAndSet(2);
        example.printAndSet(3);
     }

    public static class Example {
        private int number = 0;

        public void printAndSet(int number) {
            System.out.println(this.number);
            this.number = number;
        }
    }
}
```
Чтобы получить ссылку на объект внутри этого объекта используется ключевое слово **this**.

Чтобы обратиться к полю родительского класса, нам необходимо получить на него ссылку при помощи ключевого слова **super**.

При использовании внутренних классов есть ещё один нюанс. Допустим, есть класс:
```java
public class Outer {
    private String test = "test";

    public class Inner {
        public void print() {
            System.out.println(Outer.this.test);
        }
    }
}
```
Тогда можем:
```java
public static void main(String[] args) {
	Outer outer = new Outer();
	Outer.Inner inner = outer.new Inner();
	inner.print();
}
```

Однако, такого прямого доступа стоит избегать и использовать Getter'ы и Setter'ы:
[Java Getter and Setter Tutorial - from Basics to Best Practices](http://www.codejava.net/coding/java-getter-and-setter-tutorial-from-basics-to-best-practices). Это и является одной из главных частей инкапсуляции.

Для облегчения этого существуют готовые библиотеки. Например, [Lambok](https://projectlombok.org/features/GetterSetter).

## [↑](#Home) <a name="ObjLifecycle"></a> Object's Lifecycle
Каждый созданный в Java объект неявно всегда наследуется от java.lang.Object.
У объектов есть свой жизненный цикл: "[OCAJP 7 Object Lifecycle in Java](https://dzone.com/articles/ocajp-7-object-lifecycle-java)".

При создании объекта под него выделяется место в области памяти под названием **"Heap"** (куча).
Для создания объектов используется ключевое слово **new**.

Пример создания объекта (Instantiating a Class):
```Point originOne = new Point(23, 94);```
Т.е. создаётся Instance класса.

При создании класса вызывается метод, называемый конструктором. Они не имеет возвращаемого типа (даже void) и его название совпадает с названием класса. То есть в отличии от остальных методов он начинается с заглавной буквы, как и название класса.

Существует конструктор по-умолчанию - это конструктор без параметров. Пока не будет объявлен конструктор явно - он будет предоставлен автоматически. Как только у класса объявлен хотя бы один конструктор - конструктор по умолчанию необходимо объявлять самостоятельно, если требуется наличие конструктора без параметров.

За жизнью объектов следит Garbage Collector.
Как только на объект не остаётся доступных ссылок - объект подвергается очистке.
Как сказано в документации:
```
An object is eligible for garbage collection when there are no more references to that object. References that are held in a variable are usually dropped when the variable goes out of scope. Or, you can explicitly drop an object reference by setting the variable to the special value null. Remember that a program can have multiple references to the same object; all references to an object must be dropped before the object is eligible for garbage collection.

The Java runtime environment has a garbage collector that periodically frees the memory used by objects that are no longer referenced. The garbage collector does its job automatically when it determines that the time is right.
```

**Heap** - это основной сегмент памяти, где хранятся объекты. Он делится на два подсегмента: **Old Generation** и **New Generation**.
**New Generation** делится на подсегменты: **Eden** и два сегмента **Survivor**.
При запуске Java приложения **JVM** загружает необходимые классы (например, базовые классы из **rt.jar**, которые входят в JRE) в хип. Про загрузку классов можно прочитать в статье на хабре: [Загрузка классов в Java. Теория](https://habrahabr.ru/post/103830/).
Новые объекты попадают сначала в New Generation в подраздел Eden, а потом по мере работы сборщик мусора попадают в Survivor. Позже, если объект является долгоживущим, то он попадает в Old Generation (он же Tenured Generation).

## [↑](#Home) <a name="GC"></a> **Garbage Collector**
**Garbage Collector** является одной из главной частей JVM.
По теме сборщика мусора есть хорошие материалы:

Цикл статей [Дюк, вынеси мусор!](https://habrahabr.ru/post/269621)
Обзор [Garbage Collection наглядно](https://habrahabr.ru/post/112676)
Видео-лекция [Сборка мусора в Java](https://www.youtube.com/watch?v=FEraejGeul8)
Видео-лекция [Garbage First](https://www.youtube.com/watch?v=D9LDIp9FuYI)

## [↑](#Home) <a name="Wrappers"></a> Wrapper classes (such as Boolean, Double, and Integer)
Существуют классы - обёртки над примитивными типами, Wrapper classes.
Часто, когда требуется преобразование из ссылочного типа в примитивный и наоборот, компилятор языка Java сам выполняет эту работу.
Это называется: [Autoboxing and Unboxing](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html).

Для использования с числами существуют [The Numbers Classes](https://docs.oracle.com/javase/tutorial/java/data/numberclasses.html).
Эти класы являются наследниками абстрактного класса **java.lang**. Это позволяет использовать единый подход для работы с ними на основе API класса Number.

Начиная с JDK 5 введены понятия Autoboxing и Unboxing.
Автобоксинг - это упаковка примитивного типа в его объектную обёртку.
Например: ``Integer i = 12;``

И тут стоит не забывать про интересную особенность. Есть некий **Constant pool**, в котором кэшируются значения Integer. Диапазон кэшируемых значений равен значению минимального примитива, т.е. byte: -128 до 127. Так как это относится к автобоксингу, то верхняя граница кэшируемых значений может быть настроена при помощи указания параметра JVM: "**-XX:AutoBoxCacheMax**".

```java
public class IntCache {
 	public static void main(String[] args) {
       Integer first = 127;
       Integer second = 127;
       System.out.println(first==second); //True
       first = new Integer(127);
       second = new Integer(127);
       System.out.println(first==second); //False
       first = Integer.valueOf(127);
       second = Integer.valueOf(127);
       System.out.println(first==second); //True
    }
}
```
При использовании параметризированного конструктора будет результат false потому, что мы явно просим создать для нас новый объект. В других вариантах в любом случае будет вызван **valueOf**, который кэширует используемые integer при помощи **java.lang.Integer.IntegerCache**.

Более подробно в статье: "[Autoboxing и unboxing в Java](http://habrahabr.net/habr/329498/)"

## [↑](#Home) <a name="Resources"></a> Дополнительные материалы
- [Beginning Java: Data types, Variables, and Arrays](https://www.sitepoint.com/beginning-java-data-types-variables-and-arrays/)
- [Primitive Data Types](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)
- [The Numbers Classes](https://docs.oracle.com/javase/tutorial/java/data/numberclasses.html)