# <a name="Home"></a> Working with Methods and Encapsulation

## Содержание:
- [Overview](#Overview)
- [Create methods with arguments and return values](#createmethods)
- [Apply the static keyword](#static)
- [Create an overloaded method](#overload)
- [Apply access modifiers](#access)
- [Apply encapsulation principles to a class](#encapsulate)
- [Effect upon object](#effect)
- [Ресурсы](#Resources)

## [↑](#Home) <a name="Overview"></a> Overview
Данный раздел относится к [OCA Java SE 8 Programmer I](http://education.oracle.com/pls/web_prod-plq-dad/db_pages.getpage?page_id=5001&get_params=p_exam_id:1Z0-808).
Разделы: "[Java SE 8 Programmer I Exam Topics](https://docs.oracle.com/javase/tutorial/extra/certification/javase-8-programmer1.html#encapsulation)"
Подробнее: "[6. Working with Methods and Encapsulation](http://javacertification.wikidot.com/methods-and-encapsulation)"

## [↑](#Home) <a name="createmethods"></a> Create methods with arguments and return values
Определение метода описано в tutorial: [Defining Methods](https://docs.oracle.com/javase/tutorial/java/javaOO/methods.html).

**Важное определение:**
```
Definition: Two of the components of a method declaration comprise the method signature—the method's name and the parameter types.
```
Метод определяется его сигнатурой, т.е. названием и используемыми входными параметрами. Не может существовать 2 метода с одинаковыми сигнатурами внутри одного класса.

Иногда нам может потребоваться использование одного названия метода, но с разными типами и разным количеством параметров. Такой подход называется "перегрузкой" или **overloading**. А метод будет называться перегруженным, **overloaded**.

Так же существует интересная особенность: **varargs**
Подробнее: "[Varargs](https://docs.oracle.com/javase/1.5.0/docs/guide/language/varargs.html)".

Важно, что ```Varargs can be used only in the final argument position.```
И важно понимать, что Varargs всего лишь "синтаксический сахар" над массивами, никакой магии:
```java
public static void main(String []args){
	test("Hello", ", ", "World", "!");
}

public static void test(String... varargs) {
	System.out.println(varargs instanceof String[]); //true
}
```

## [↑](#Home) <a name="static"></a> Apply the static keyword to methods and fields
В официальных tutorial ключевое слово **static** описывается в разделе "[Understanding Class Members](https://docs.oracle.com/javase/tutorial/java/javaOO/classvars.html)".

При помощи ключевого слова **static** обозначает поля и методы, которые принадлежат не instance (не экземпляру класса), а самому классу. То есть они будут едины для всех экземпляров класса.

Так же конструкцией **static final** указываются константы:
```java
static final double PI = 3.141592653589793;
```

Статические члены классов хранятся в области памяти **Permanent Generation** (с Java8 **MetaSpace**). Что логично, т.к. статические члены классов являются частью метаданных классов. Это также предотвращает сборку данных полей Garbage Collector'ом.
На эту тему есть отличный ответ: "[Как в java хранятся статические поля?](https://ru.stackoverflow.com/questions/466504/Как-в-java-хранятся-статические-поля)".
Статические поля не требуют создания Instance класса.
Интересно, что внутренние **Enum** всегда являются static: "[JLS - Enum](http://docs.oracle.com/javase/specs/jls/se7/html/jls-8.html#jls-8.9)".
Так как поле является метаданными класса, то важно учитывать тип, через который происходит обращение к public static полям.

Подробнее можно почитать статью: "[10 заметок о модификаторе Static в Java](http://info.javarush.ru/translation/2014/04/15/10-заметок-о-модификаторе-Static-в-Java)".

Так же есть статические блоки: "[Что такое статический блок и статическая инициализация в Java?](http://javaway.info/chto-takoe-staticheskij-blok-i-staticheskaya-initsializatsiya-v-java/)".

Важной особенностью работы со static полями является **сокрытие (hiding)**.
```java
public static void main(String []args){
	new A().print();
	new B().print();
	A a = new B();
	B b = new B();
	a.print();
	b.print();
}

public static class A {
	public static void print() {
		System.out.println("A");
	}
}

public static class B extends A{
	public static void print() {
		System.out.println("B");
	}
}
```
Подробнее см. tutorial: "[Overriding and Hiding Methods](https://docs.oracle.com/javase/tutorial/java/IandI/override.html)".

Такое же применимо и для полей. Но не рекомендуется. см. "[Hiding Fields](https://docs.oracle.com/javase/tutorial/java/IandI/hidevariables.html)".

## [↑](#Home) <a name="overload"></a> Create an overloaded method
Создание перегруженного метода указано в "[Overriding and Hiding Methods](https://docs.oracle.com/javase/tutorial/java/IandI/override.html)".

Констркутор класса можно считать тоже методом, просто с особой сигнатурой, благодаря которой JVM знает, что перед нами не просто метод, а конструктор.
Следовательно, конструкторы тоже можно перегружать.

Использование констркторов описано в "[Providing Constructors for Your Classes](https://docs.oracle.com/javase/tutorial/java/javaOO/constructors.html)".
Существует два вида конструкторов:
- default (конструктор без параметров)
- user defined

Default констркуктор будет предоставлен автоматически до тех пор, пока программист в классе не объявит первый конструктор. Как только программист объявил конструктор - будут доступны только они.

Один из "заковыристых" вопросов - порядок вызова конструкторов:
- "[Java order of Initialization and Instantiation](#https://stackoverflow.com/questions/23093470/java-order-of-initialization-and-instantiation)"
- "[Initialization blocks, constructors and their order of execution](http://www.thejavageek.com/2013/07/21/initialization-blocks-constructors-and-their-order-of-execution/)"

Конструктор текущего класса может быть вызван как this(), конструктор родителя может быть вызван через ключевое слово super().
При этому super() должно быть первой командой в методе.
В той же документации сказано, что:
```
This default constructor will call the no-argument constructor of the superclass.
```
То есть default конструктор и no-argument constructor - это несколько разные вещи.
Про абстрактные классы разговор будет позже, но в тему конструкторов полезно прочитать следующие ссылки:
- "[Почему абстрактные классы имеют конструктор?](https://stackoverflow.com/questions/2170500/why-do-abstract-classes-in-java-have-constructors)".
- "[Can abstract class have Constructor in Java - Interview Question](http://www.java67.com/2013/02/can-abstract-class-have-constructor-in-java.html)".

## [↑](#Home) <a name="access"></a> Apply access modifiers
Одна из важнейших тем - это модификаторы доступа.
Ссылка на официальнуый tutorial: [Controlling Access to Members of a Class](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html).
Модификаторы доступа описаны много где, например: "[Модификаторы доступа и инкапсуляция](https://metanit.com/java/tutorial/3.3.php)".
Модификаторы доступа:
- **public**: Самый доступный модификатор - public. Классы и поля с таким модификатором доступа видны всем.
- **protected**: Защищённый элемент доступен из того же пакета + наследникам. Т.е. мы хотим предоставить свободу и ответственность тем, кто реализует что-то на основе нашего элемента + т.к. пакет мы пишем - то зачем нам что-то от себя скрывать?
- **private**: Приватные элементы доступны только в том же классе, в котором объявлены.
- **по умолчанию (package private)**: Если модификатор доступа не указать, то это означает, что будет использован уровень доступа по умолчанию, т.е. доступность в рамках одного пакета.

В некоторых обстоятельствах не все модификаторы доступа применимы но всё подчиняется логике. Например:
- Абстрактные методы не могут быть private. Что логично, т.к. их кто-то должен переопределить. А как их переопределить или реализовать, если эти методы недоступны?
- Enum не может быть private или protected, если он не является вложенным классом

## [↑](#Home) <a name="encapsulate"></a> Apply encapsulation principles to a class
Принцип инкапсуляции - сокрытия деталей реализации.
Одним из принципов является [Inheritance - наследование](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html).

В Java не разрешено множественное наследование. Подробнее см. [Multiple Inheritance of State, Implementation, and Type](https://docs.oracle.com/javase/tutorial/java/IandI/multipleinheritance.html).
Так же см. "[Множественное наследование в Java. Композиция в сравнении с Наследованием](http://info.javarush.ru/translation/2013/10/22/Множественное-наследование-в-Java-Композиция-в-сравнении-с-Наследованием.html)" и "[Java 101: Inheritance in Java, Part 1](https://www.javaworld.com/article/2987426/core-java/java-101-inheritance-in-java-part-1.html)".

Так же к инкапсуляции можно отнести внутренние классы, т.е. [Inner Classes](https://docs.oracle.com/javase/tutorial/java/javaOO/innerclasses.html) и вложенные классы, они же [Nested Classes](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html).

Так же к инкапсуляции можно отнести технологию или прицнип: [Java Beans](http://www.javable.com/tutorials/fesunov/lesson25/).

## [↑](#Home) <a name="effect"></a> Effect upon object
Данный раздел освещается в разделе oracle tutorial: [Passing Information to a Method or a Constructor](https://docs.oracle.com/javase/tutorial/java/javaOO/arguments.html).

Главная цель:
```
Determine the effect upon object references and primitive values when they are passed into methods that change the values.
```
Главное, что нужно запомнить, что в Java мы ```'Pass by copy' in Java```. То есть значения в параметры метода передаются как копия значения. И тут вспоминаем, какое значение хранит примитивные переменные, а какие ссылочные.
Это означает, что передав в метод ссылку на объект, метод не может поменять эту ссылку, но он может взаимодействовать с объектом и его менять. Он будет работать уже с новой ссылкой, но эта ссылка будет вести на тот же объект, ссылка на который была указана как аргумент метода.

## [↑](#Home) <a name="Resources"></a> Ресурсы
- [OCA Java SE 7: Methods & Encapsulation](https://quizlet.com/29934219/oca-java-se-7-methods-encapsulation-flash-cards/)
- [Chapter 4. Methods and Encapsulation](http://apprize.info/javascript/oca/6.html)
- [Chapter 6Working with Methods and Encapsulation](https://www.safaribooksonline.com/library/view/oca-ocp/9781119363392/c06.xhtml)