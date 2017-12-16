# <a name="Home"></a> Working with Inheritance

## Содержание:
- [Overview](#Overview)
- [Inheritance and its benefits](#InheritanceAndBenefits)
- [Develop code that makes use of polymorphism](#Polymorphizm)
- [Determine when casting is necessary](#Casting)
- [Super and this to access objects and constructors](#SuperAndThis)
- [Abstract classes and interfaces](#Abstract)
- [Interfaces](#Interfaces)
- [Ресурсы](#Resources)

## [↑](#Home) <a name="Overview"></a> Overview
Данный раздел относится к [OCA Java SE 8 Programmer I](http://education.oracle.com/pls/web_prod-plq-dad/db_pages.getpage?page_id=5001&get_params=p_exam_id:1Z0-808).
Разделы: "[Java SE 8 Programmer I Exam Topics](https://docs.oracle.com/javase/tutorial/extra/certification/javase-8-programmer1.html#inheritance)"
Подробнее: "[7. Working with Inheritance](http://javacertification.wikidot.com/inheritance)"

## [↑](#Home) <a name="InheritanceAndBenefits"></a> Inheritance and its benefits
Наследование - один из основополагающих принципов объектно-ориентированного программирования. Официальный tutorial от Oracle: [Inheritance](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)
Стоит помнить, что если методы и поля наследуются, то конструкторы - нет.

Другим важным моментом при наследовании является переопределение методов: [Overriding](https://docs.oracle.com/javase/tutorial/java/IandI/override.html).
При переопределении методов следует помнить, что в классе наследнике нельзя менять область видимости на более слабую/более закрытую. Если в родителе метод public, то мы не можем его сделать protected, например.

Так же при наследовании желательно использовать аннотацию **@Override**. Она указывает компилятору, что далее мы собираемся переопределять метод базового класса.
Если в базовом классе не окажется метода с аналогичной сигнатурой, то компилятор выдаст ошибку о том, что хотя мы и собирались что-то переопределить, по факту этого не произошло.

Так же стоит помнить, что все объекты неявным образом наследуются от **java.lang.Object**:
```java
String text = new String("Text");
System.out.println(text instanceof Object);
```
Альтернативный вариант:
```java
String text = new String("Text");
System.out.println(Object.class.isInstance(text));
System.out.println(Object.class.isAssignableFrom(text.getClass()));
```

## [↑](#Home) <a name="Polymorphizm"></a> Develop code that makes use of polymorphism
Наследование позволяет реализовать такой принцип ООП как [polymorphism](https://docs.oracle.com/javase/tutorial/java/IandI/polymorphism.html).
Если упростить идею, то с точки зрения использования классов полиморфизм - это возможность объявить переменную типом А, а присваивать ей объекты типа Б, когда тип Б является наследником типа А.
Так же класс Б может быть реализацией абстрактного типа А или реализовывать интерфейс класса А.
Простой пример:
```java
public static void printNumber(Number number) {
	System.out.println(number.toString());
}

public static void main(String[] args) {
	printNumber(new Integer(5));
	printNumber(new Long(2L));
	printNumber(new Float(3.0));
}
```
Как видно из примера, метод printNumber может принимать любого, кто является наследником Number. Допустим, целочисленные, т.к. ```Integer extends Number```.

## [↑](#Home) <a name="Casting"></a> Determine when casting is necessary
В Java наследование напрямую связано с таким понятием, как приведение типов.
Т.к. Java язык строго типизированный, то он следит, чтобы мы не сделали какую-нибудь глупость в коде и пытается оградить нас от ошибок.
На самом деле, необходимость в Casting очень логична.
Существует две категории приведения типов:
- Расширяющее (Widening)
- Сужающее (Narrowing)

Помним, что тип переменной определяет контракт, по которому мы работаем с объектом, который там лежит. Грубо говоря, если объект типа А, то мы можем вызывать любые методы, которые позволяет вызывать тип А.
Если есть класс Б, который наследуется от класса А, то он наследует всё, что есть в классе А. Тогда мы можем себе позволить:
```java
Number number = new Integer(5);
```
Мы теряем возможность использовать метод compareTo через ссылку number, но нам это сделать не дадут, т.к. через ссылку number мы не знаем про compareTo метод класса String, т.к. контракт от класса Number.
А вот в обратную сторону мы должны явно указать, какой класс ожидаем:
```java
Number number = new Integer(5);
Integer numberInt = (Integer) number;
```
Сделано это для того, чтобы защитить от таких ошибок:
```java
Number number = new Long(5L);
Integer numberInt = (Integer) number;
```

## [↑](#Home) <a name="SuperAndThis"></a> Super and this to access objects and constructors
Применение наследования тесно связано с такими ключевыми словами:
- [super](https://docs.oracle.com/javase/tutorial/java/IandI/super.html)
- [this](https://docs.oracle.com/javase/tutorial/java/javaOO/thiskey.html)

Про разницу между ними можно прочитать тут: [Разница между ключевыми словами this и super в Java](http://info.javarush.ru/grishin/2015/03/31/Разница-между-ключевыми-словами-this-и-super-в-Java.html).
Интересно, что при помощи данных ключевых слов можно не только обращаться к элементам классов, но и к конструкторам. Важно, чтобы обращение к конструкторам было первой строкой, иначе будет ошибка.
Вызов конструктора предка - является важным аспектом построения всей модели, т.к. благодаря конструкторам инициализируются поля. Точнее, инициализация полей является скрытой работой конструкторов (подробнее см. [Creation of New Class Instances](https://docs.oracle.com/javase/specs/jls/se7/html/jls-12.html#jls-12.5)).
Поэтому, даже если первой строчкой в констркторе нет отсылки к конструктору родителя - за нас её подставят. Причём всегда на default конструктор.
Например, в IntelliJ Idea есть специальное средство для этого: View -> Show Bytecode. Мы увидим, что будет вызов конструктора родителя.
Например: ```INVOKESPECIAL ru/test/package1/ClassA.<init> ()V```

## [↑](#Home) <a name="Abstract"></a> Abstract classes
Абстрактные классы - классы, instance которых нельзя создать и являются каркасом для классов, которые будут созданы на основе Abstract классов.
Могут содержать абстрактные методы, т.е. методы, тело которых не определено. Такие методы обязаны быть определены в не абстрактных наследниках.

Подробнее: "[Абстрактные классы и методы](http://kostin.ws/java/java-abstract-and-interfaces.html)".
Oracle предоставляет tutorial: "[Abstract Methods and Classes](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)".

В абстрактных классах можно определять статические методы, но нельзя объявлять статический абстрактный метод. И это логично, т.к. метод вызывается у класса, а не у объекта.

Абстрактные классы не могут иметь абстрактные конструкторы. Но объявленный в асбтрактном предке необстрактный конструктор должен быть вызван из неабстрактного предка.

Интересный вопрос для понимания: "[Почему абстрактные классы имеют конструктор?](https://stackoverflow.com/questions/2170500/why-do-abstract-classes-in-java-have-constructors)".
Ещё один вопрос про коснтруктор: "[Can abstract class have Constructor in Java - Interview Question](http://www.java67.com/2013/02/can-abstract-class-have-constructor-in-java.html)".

## [↑](#Home) <a name="Interfaces"></a> Interfaces
Важной частью языка Java так же являются интерфейсы.
Их использование описано в официальных tutorial:
- [Defining an Interface](https://docs.oracle.com/javase/tutorial/java/IandI/interfaceDef.html)
- [Implementing an Interface](https://docs.oracle.com/javase/tutorial/java/IandI/usinginterface.html)

Интерфейс - представляет описание контракта класса, который реализует (implements, имплементирует) данный интерфейс.
Материал: "[Интерфейсы](https://metanit.com/java/tutorial/3.7.php)"

Интерфейсы в Java 8 изменились.
Более подробно: "[Java 8 Interface](http://www.journaldev.com/2752/java-8-interface-changes-static-method-default-method)".
Появились default методы: "[Java 8 explained: Default Methods](https://zeroturnaround.com/rebellabs/java-8-explained-default-methods/)".
Важно, что если есть 2 default метода, которые конфликтуют, то ни один из них не будет вызван и нужно указать свою реализацию, о чём написано в статье выше.

Важно помнить, что все поля в интерфейсах неявно static и final: "[Why are all fields in an interface implicitly static and final?](https://stackoverflow.com/questions/1513520/why-are-all-fields-in-an-interface-implicitly-static-and-final)".

## [↑](#Home) <a name="Resources"></a> Ресурсы
Карточки для запоминания: [inheritance flash cards](https://quizlet.com/30047053/oca-java-se-7-inheritance-flash-cards/)