# <a name="Home"></a> Java Basics

## Содержание:
- [Overview](#Overview)
- [The structure of a Java class](#Structure)
- [Java packages](#Packages)
- [The scope of variables](#Scopes)
- [Executable Java applications with a main method](#Executable)
- [The features and components of Java](#Features)
Дополнительно:
- [Байткод](#Bytecode)
- [JRE, JDK, JLS](#JDKJREJLS)
- [Распространение программ](#Distribution)

## [↑](#Home) Overview <a name="Overview"></a>
Данный раздел относится к [OCA Java SE 8 Programmer I](http://education.oracle.com/pls/web_prod-plq-dad/db_pages.getpage?page_id=5001&get_params=p_exam_id:1Z0-808).
Разделы: [Java SE 8 Programmer I Exam Topics](https://docs.oracle.com/javase/tutorial/extra/certification/javase-8-programmer1.html#basics)
Подробнее: "[1. Java Basics](http://javacertification.wikidot.com/basics)"

Примеры из разделов, описанных здесь, можно воспроизвести в блокноте. Но если хочется эстетики, то лучше использовать [Sublime Text](https://www.sublimetext.com/3).

## [↑](#Home) The structure of a Java class <a name="Structure"></a>
Java - является объектно ориентированным языком, поэтому почти всё с чем мы работаем - есть объект. Но что такое объект? Какой он? Как он должен себя вести? Чтобы об этом рассказать мы используем такое понятие, как [Class](https://docs.oracle.com/javase/tutorial/java/javaOO/classes.html).
Как сказано на официальном сайте:
```A class is a blueprint or prototype from which objects are created```

Создание класса очень декларативно и описывает следующие аспекты:
- scope класса (пакет, в котором находится)
- импортируемые классы из других пакетов
- декларация самого класса ("[Class Declaration](https://docs.oracle.com/javase/tutorial/java/javaOO/classdecl.html)")
- декларация членов класса ([поля](https://docs.oracle.com/javase/tutorial/java/javaOO/variables.html), [методы](https://docs.oracle.com/javase/tutorial/java/javaOO/methods.html), [конструкторы](https://docs.oracle.com/javase/tutorial/java/javaOO/constructors.html))

Для примера, откроем Sublime Text 3, откроем новое окно ("File" → "New Window").
Допустим, мы хотим описать что-то, что будет выдавать сообщение. Назовём это Messenger.
Определимся так же, кому он будет доступным. Области видимости будут разобраны ниже, поэтому пока решим, что нам нужен общедоступный, публичный класс. Поэтому он будет у нас **public** класс.
Объявим наш класс ("[Class Declaration](https://docs.oracle.com/javase/tutorial/java/javaOO/classdecl.html)"):
```java
package test;

import java.util.Date;

public class Messenger {

	public void print() {
		System.out.println(new Date());
	}

}

```
Сохраняем класс. Главное помнить, что:
- 1 класс = 1 файл
- Название класса = название файла
- Классы по [Code convention](http://www.oracle.com/technetwork/java/codeconventions-150003.pdf) называются в большой буквы
- Название пакета должно совпадать со структурой каталогов (в данном случае, каталог test)

Описания методов, **Сигнатуры методов**, состоят из название метода и его параметров. Именно таким образом один метод отличается от другого. В одном классе сигнатуры методов не могут быть одинаковыми.

## [↑](#Home) Java packages <a name="Packages"></a>
Тема пакетов раскрвывается в официальных tutorials:
- [Creating and Using Packages](https://docs.oracle.com/javase/tutorial/java/package/packages.html)
- [Using Package Members](https://docs.oracle.com/javase/tutorial/java/package/usepkgs.html)

**Пакет** - это группа, в которую типы объединены по какому либо признаку. Пакет предоставляет защиту доступа и управление прастранствами имён.

Все классы в Java относятся к какому либо пакету. Если пакет не указан - класс относится к **"пакету по умолчанию"**.
Это позволяет избежать конфликтов и неоднозначностей при импорте. Например, класс Date описывает слишком часто используемую сущность - дату. Это может быть тип данных в базе данных, это может быть дата на компьютере, это может быть ещё какая либо дата. Чтобы JVM точно знала, какой класс использовать, ей нужно знать, из какого пакета доставать класс.

Есть пакеты, которые импортируются по умолчанию. Классы из таких пакетов не надо импортировать. Подробнее: [Which Java packages are imported by default?](http://cs-fundamentals.com/tech-interview/java/which-java-package-is-imported-by-default.php).

Классы из пакета ``java.lang`` импортируются по умолчанию.
Классы из пакета "по умолчанию" недоступны другим пакетам и не могут быть импортированы начиная с JSE1.4: [How to access java-classes in the default-package?
](https://stackoverflow.com/questions/283816/how-to-access-java-classes-in-the-default-package).
Классы из пакета можно импортировать как "поштучно", так и пакетом целиком:
```java
import com.test.mypackage.MyClass;
import com.test.mypackage.*;
```
Второе, правда, нежелательно.

Небольшой пример:
```java
import java.util.*; // Импорт всех классов
// import java.sql.*; - Ошибка wildcard import'а,
// т.к. Date class, as ambiguous, ведь он уже есть в java.utils
import java.sql.Date;
import java.util.ArrayList;
import static java.lang.Math.sqrt;

public class Lesson {

    public static void main(String[] args) {

        /*
        HashMap hashMap = new HashMap(); - Без импорта нельзя
        Но можно использовать полное имя, тогда импорт не нужен
         */
        java.util.HashMap hashMap = new java.util.HashMap<>();
        // А для ArrayList мы выполнили импорт
        ArrayList arrayList = new ArrayList();
         /*
            Опасность импорта по wilcard в том, что
            более точный импорт java.sql.Date приоритетнее,
            чем ранее указанный импорт java.util.*
         */
        java.util.Date date = new java.util.Date();
        System.out.println(date.getClass().getName()); //java.util.Date
        Date date2 = new Date(2000);
        System.out.println(date2.getClass().getName()); // java.sql.Date
        System.out.println(sqrt(4)); // method from static import
    }
}
```

Кроме того, не все пакеты "одинаково полезны" и доступны. Например, можно встретить проблемы при импорте внутренних классов JRE (JRE internal). Например: com.sun.xml.internal. Поэтому, такого следует избегать.

## [↑](#Home) The scope of variables <a name="Scopes"></a>
У переменных есть свой Scope - то есть область, которая определяет время жизни переменной.
Описано это в tutorial: "[Java Oracle Tutorials: Variables](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/variables.html)".

Не следует путать scope с уровнем доступа из "[Controlling Access to Members of a Class](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html)".

Scope переменной может быть задан следующим образом:
- block
- method
- instance
- class

Как сказано в tutorial, переменные бывают:
- Instance Variable (Non-Static Fields)
Объекты хранят своё состояние в полях, объявленных без ключевого слова static.
Значения в этих полях уникальны для каждого instance класса (для каждого объекта данного класса).
- Class Variables (Static Fields)
Поля класса, указанные с ключевым словом static. Данное ключевое слово говорит компилятору, что будет существовать всего 1 копия поля на все объекты, вне зависимости от того, сколько будет создано объектов.
- Local Variables
Подобно тому, как объекты хранят своё состояние в полях, методы часто сохраняют промежуточные результаты или промежуточные состояния в локальные переменные
- Parameters
Параметры используются методами и передаются в метод. Классифицируются как "variables"

Один из самых простых способов определить область видимости - ориентироваться на фигурные скобки (**brackets**).
Пример:
```java
public class Lesson {

    public static void main(String[] args) {
        {
            int x = 3;
            System.out.println(x);
        }
        int x = 2;
        System.out.println(x);
    }
}
```
Причём, если переместить код под скобками наверх скобок - будет ошибка компиляции, т.к. тогда int x = 2 начнёт действовать и на внутренний блок. А значения не перекрываются, а дополняются.

## [↑](#Home) Executable Java applications <a name="Executable"></a>
**Executable Java applications** - это такое приложение, которое содержит **Executable Java class**, класс который будет обработан/обслужен JVM, который начнёт своё выполнение со специальной точки - **main method**, специального объявленного метода в классе.
Подробнее можно прочитать здесь: "[A Beginner's Guide to Executable Java Applications](https://dzone.com/articles/executable-java-applications)".

Main method называется точкой входа, или [Entry point](https://docs.oracle.com/javase/tutorial/deployment/jar/appman.html). Подробнее про особенности: [Why is the Java main method static?](https://stackoverflow.com/questions/146576/why-is-the-java-main-method-static/151666#151666).

При исполнении Java программ следует понимать, что такое [ClassPath](https://docs.oracle.com/javase/tutorial/essential/environment/paths.html).

Отличный материал по теме: "[Работа с Java в командной строке](https://habrahabr.ru/post/125210/)".

Простейшие пример:
- Откроем командуню строку (на Windows можно Win+R, cmd).
- Создадим каталог для нашего простого проекта.
Например: ``mkdir C:\simpleproject``
- Далее создадим пакет: ``mkdir C:\simpleproject\messenger``
- Далее создадим java файл: ``echo "" >C:\simpleproject\messenger\Messenger.java``
- Откроем его.
Например, на Windows, это просто ``notepad C:\simpleproject\messenger\Messenger.java``.
- Сохраним его со следующим содержанием:

```java
package messenger;

public class Messenger {
	public static void main(String[] args) {
		System.out.println("Message");
	}
}
```
Теперь, необходимо его скопилировать.
Для удобства перейдём в корневой каталог проекта: ``cd C:\simpleproject``
Самый простейший способ: javac <путь до java файла>
Например: ``javac messenger/Messenger.java``
Для запуска: ``java messenger.Messenger``
Вроде всё просто.
Подробнее: [What's the default classpath when not specifying classpath?](https://stackoverflow.com/questions/8227682/whats-the-default-classpath-when-not-specifying-classpath)

Но давайте усложним наш класс. Добавим импорт: ```import org.joda.time.DateTime;```
И изменим наш метод:
```java
public static void main(String[] args) {
	DateTime dt = new DateTime();
	System.out.println(dt + ": Message");
}
```
Теперь нам в проект нужна библиотека: [joda-time](https://mvnrepository.com/artifact/joda-time/joda-time/2.9.9).
Создаём каталог (например, C:\simpleproject\lib) и помещаем туда jar библиотеки.
Теперь, чтобы скомпилировать, нам нужно указать, где лежат библиотеки:
``javac -cp lib/* messenger/Messenger.java``
Параметр **-cp** сокращение от classpath
Соответственно, для запуска так же нужны эти библиотеки:
``java -cp ".;lib/*" messenger.Messenger``

Теперь пришлось в classpath добавить как текущий каталог, так и каталог lib.
В текущем каталоге будет найден class с точкой входа, а в lib будут найдены библиотеки.
Важно помнить, что формат classpath является OS-dependent: [Classpath does not work under linux](https://stackoverflow.com/questions/4528438/classpath-does-not-work-under-linux). В Unix системах вместо ";" используется ":"

Так же стоит помнить, что считается правильным поддерживать следующую структуру:
Исходный код в каталоге src, выходные файлы в отдельном каталоге, например, out.
Тогда:
```
javac -d ./out -cp lib/* -sourcepath ./src ./src/messenger/Messenger.java
```
И теперь мы запускаем так же с указанием в classpath библиотек:
```
java -cp "out;lib/*" messenger.Messenger
```

Так же может быть сделан непосредственно jar архив: "[Creating a JAR File](https://docs.oracle.com/javase/tutorial/deployment/jar/build.html)".
Не забываем, что нам по прежнему нужен Entry Point: [Setting an Application's Entry Point](https://docs.oracle.com/javase/tutorial/deployment/jar/appman.html).

The syntax is jar cfe [jar-name] [entry-point] [files to include]. In your case: jar cfe Hello.jar HelloViewer *

Выполняем сборку: ```jar cvfe test.jar messenger.Messenger lib -C out .```
После чего в архиве находим META-INF\MANIFEST.MF и добавляем туда:
``Class-Path: lib/joda-time-2.9.9.jar``
Второй вариант собрать по готовому MANIFEST.MF:
```jar cvfm test.jar Manifest.mf lib -C out .```
Содержимое файла:
```
Manifest-Version: 1.0
Class-Path: lib/joda-time-2.9.9.jar
Main-Class: messenger.Messenger

```
**ВАЖНО!!:** Должна быть 1 пустая строка, иначе может не отработать!!

Теперь можно выполнить: ``java -jar test.jar``

**ВАЖНО** помнить, что в Class-path используется или путь к конкретной библиотеке или путь к class файлам: [How to load a whole directory using Class-Path: jar manifest header?](https://stackoverflow.com/questions/25665867/how-to-load-a-whole-directory-using-class-path-jar-manifest-header).

## [↑](#Home) The features and components of Java <a name="Features"></a>
Про возможности, которые обеспечивает Java:
- [About the Java Technology](https://docs.oracle.com/javase/tutorial/getStarted/intro/definition.html)
- [Lesson: Object-Oriented Programming Concepts](https://docs.oracle.com/javase/tutorial/java/concepts/index.html)

Основные особенности:
- Объектно-ориентированный язык программирования
- Платформенно-независимый код (засчёт использования JVM)
- Безопасный (т.к. выполняется внутри JVM и контролируется код ей же)
- Оптимизация производительности (благодаря оптимизациям JVM и JIT)
- Простой
- Надёжный
Про данные особенности см. "[Features-Java-Buzzwords](http://java.meritcampus.com/core-java-topics/Java-8-Features-Java-Buzzwords)".

## [↑](#Home) Байткод <a name="Bytecode"></a>
Чтобы увидеть, как выглядит скомпилированный файл в байткоде можно попросить дизассемблер джава класс файлов [javap](http://docs.oracle.com/javase/8/docs/technotes/tools/windows/javap.html).

Хороший материал, чтобы примерно представлять, как это работает: "[Java Bytecode Fundamentals](https://habrahabr.ru/post/111456/)" и "[Структура байт-кода виртуальной машины Java](https://habrahabr.ru/post/69797/)". А так же пример можно увидеть здесь: [A Java Programmer’s Guide to Byte Code](https://www.beyondjava.net/blog/java-programmers-guide-java-byte-code/) и даже посмотреть доклад: [Байткод для любознательных](https://www.youtube.com/watch?v=YtFT9vJG2lw).

## [↑](#Home) JRE, JDK, JLS <a name="JDKJREJLS"></a>
Java может поставляться как JRE, так и JDK.
- **JRE** - Java Runtime Environment.
Из названия понятно, что это среда выполнения. Содержит набор стандартных библиотек, виртуальную машину Java (Java Virtual Machine, JVM).
http://www.oracle.com/technetwork/java/javase/jre-8-readme-2095710.html
- **JDK** - Java Developer Kit
Набор разработчика Java. Соответственно, сюда входят исходный код (src.zip), документация, компилятор (javac), декомпилятор (javap), различные вспомогательные утилиты (например, архиватор jar). И сама JRE.
Более подробно про состав: "[Contents of the JDK](http://www.oracle.com/technetwork/java/javase/jdk-8-readme-2095712.html)".
- **JLS** - Java Language Specification
Описывает то, каким образом внутри всё работает. Различные технические нюансы и хитрости. Подробнее: [Java8 JLS](http://docs.oracle.com/javase/specs/jls/se8/html/index.html)

## [↑](#Home) Распространение программ <a name="Distribution"></a>
Распространение написанного Java приложения зависит от его типа.
JavaSE приложениt: "[Распространение настольных приложений Java](https://netbeans.org/kb/docs/java/javase-deploy_ru.html)".
Java WebStart: [Java WebStart](https://www.java.com/ru/download/faq/java_webstart.xml)
JavaFX приложение: [Native pack](http://docs.oracle.com/javafx/2/deployment/self-contained-packaging.htm).
JavaEE приложения: [Types of J2EE Archive Files](https://docs.oracle.com/cd/E19830-01/819-4712/ablgz/index.html)