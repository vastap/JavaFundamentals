# <a name="Home"></a> Using Loop Constructs

## Содержание:
- [Overview](#Overview)
- [Create and use while and do/while loops](#dowhile)
- [Create and use for loops](#for)
    - [Enhanced for loop: for-each](#foreach)
    - [For-each in Java 8](#foreachjava8)
- [Compare loop constructs](#compare)
- [Use break and continue](#break)
- [Материалы](#Resources)

## [↑](#Home) <a name="Overview"></a> Overview
Данный раздел относится к [OCA Java SE 8 Programmer I](http://education.oracle.com/pls/web_prod-plq-dad/db_pages.getpage?page_id=5001&get_params=p_exam_id:1Z0-808).
Разделы: "[Java SE 8 Programmer I Exam Topics](https://docs.oracle.com/javase/tutorial/extra/certification/javase-8-programmer1.html#loopConstructs)"
Подробнее: "[5. Using Loop Constructs](http://javacertification.wikidot.com/loops)"

## [↑](#Home) <a name="dowhile"></a> Create and use while and do/while loops
Одним из простых циклов является цикл **while**.
У данного цикла есть условие, на основе которого решается, выполнять ли блок кода, который относится к данному условию.
Если условие не истинно, то код может быть не вызван вообще ни одного раза.

Официальный tutorial: [The while and do-while Statements](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/while.html)

Так же есть его изменённая версия: **do-while**.
Отличается она тем, что сначала выполняется блок кода, а потом решается, следует ли выполнять эти действия ещё раз.

Выглядят как:
```java
while (expression) {
     statement(s)
}
```
и
```java
do {
     statement(s)
} while (expression);
```

## [↑](#Home) <a name="for"></a> Create and use for loops
Как в большей части языков программирования, если не во всех, в Java есть циклы.
Официальный tutorial: "[The for Statement](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/for.html)".
Есть даже интерактивные уроки по циклам: [Loops](http://www.learnjavaonline.org/en/Loops).

В общем случае можно сказать, что выражение for состоит из трёх частей:
- initialization: инициализация счётчика
- termination: условие выхода (когда станет false цикл прекратится)
- increment: правило изменения счётчика (увеличения или уменьшения)

```java
public static void main(String[] args){
	for (int i = 0; i < 5; i++) {
		System.out.println(i);
	}
}
```
Соответственно, шаг 2 будет:
```java
for (int i = 0; i < 5; i+=2) {
	System.out.println(i);
}
```
Причём данные части не являются обязательными. Например:
```java
public static void main(String[] args){
	int i = 0;
	for (; i < 5; i+=2) {
		System.out.println(i);
	}
}
```
В этом кроются опасности. Например, тут будет бесконечный цикл:
```java
for(int i =0; i< 10; ){
	i= i++;
	System.out.print("Hello world");
}
```
А всё потому, что в i кладётся значение до инкрементации )

Так же в Java при помощи меток можно выйти внутри внутреннего цикла сразу из внешнего:
```java
loop:
for (int i = 0; i < 10; i++) {
	for (int z = 0; z < 10; z++){
		if (z == 5) {
			break loop;
		}
	}
}
```

### [↑](#Home) <a name="#foreach"></a> Enhanced for loop: for-each
Начиная с Java 5 появился For Each Loop.
Подробнее можно прочитать в официальном tutorial: "[The For-Each Loop](https://docs.oracle.com/javase/1.5.0/docs/guide/language/foreach.html)".
В общем виде выглядит как:
```java
public static void main(String[] args){
	String[] elements = new String[]{"a", "b", "c"};
	for (String element : elements) {
		System.out.println(element);
	}
}
```
То есть ```for (Элемент множества : Множество)```

Встаёт вопрос: А для чего мы можем использовать такой цикл?
Ответ прост: Для массивов и для любых объектов, реализующих интерфейс **Iterable**.
Это опять же связано с тем, что for-each loop на самом деле просто синтаксический сахар над итератором.

For-each loop использует метод **iterator()**.
Поочерёдно вызывая методы **hasNext()** и **next()** при помощи итератора обходится всё множество. Соответственно, на такой цикл накладываются все те же ограничения, что и на итератор, т.к. это одно и то же.

Хотелось бы отметить, что for-each loop по разному ведёт себя для объектов и для массивов:
```
The compiler knows if you are using the for-each loop statement for a collection or for an array.
If used for collection, the compiler translates the for-each loop to the equivalent for loop using an Iterator.
If used for an array, the compiler translates the for-each loop to the equivalent for loop using an index variable.
```

## [↑](#Home) <a name="#foreachjava8"></a> For each в Java 8
В Java 8 к коллекциям добавлен **метод forEach**.
Итераторы разделяются на два вида: внутренние (internal) и внешние (external).
Используя External итераторы мы управляем обходом сами. То есть говорим что надо делать при итерировании и как выполнять итерирование.
Используя Internal мы говорим только то, что нужно делать при итерации.
Добавленный метод forEach является именно тем самым Internal итератором.
Пример:
```java
List<String> list = new Vector<>();
list.add("1");
list.add("2");
list.forEach(new Consumer<String>() {
	@Override
	public void accept(String s) {
		System.out.println(s);
	}
});
```
Подробнее см. в статье: "[Guide to the Java 8 forEach](http://www.baeldung.com/foreach-java)"

## [↑](#Home) <a name="compare"></a> Compare loop constructs
Официальный tutorial: "[Summary of Control Flow Statements](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/flowsummary.html)".
```
Summary of Control Flow Statements

The if-then statement is the most basic of all the control flow statements. It tells your program to execute a certain section of code only if a particular test evaluates to true. The if-then-else statement provides a secondary path of execution when an "if" clause evaluates to false. Unlike if-then and if-then-else, the switch statement allows for any number of possible execution paths. The while and do-while statements continually execute a block of statements while a particular condition is true. The difference between do-while and while is that do-while evaluates its expression at the bottom of the loop instead of the top. Therefore, the statements within the do block are always executed at least once. The for statement provides a compact way to iterate over a range of values. It has two forms, one of which was designed for looping through collections and arrays.
```

Про эффективность: "[Which is more efficient, a for-each loop, or an iterator?](https://stackoverflow.com/questions/2113216/which-is-more-efficient-a-for-each-loop-or-an-iterator)"

## [↑](#Home) <a name="break"></a> Use break and continue
Официальный tutorial: [Branching Statements](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/branch.html)
Смысл в том, что:
```
break statement has two forms: labeled and unlabeled.
```
Это позволяет выполнять break и continue не только для текущего блока, но и выходить за него.
Пример см. в приведённом tutorial про Branching Statements.

## [↑](#Home) <a name="Resources"></a> Материалы
[Циклы в Java](http://kostin.ws/java/java-loops.html)

[For loop in Java with example](https://beginnersbook.com/2015/03/for-loop-in-java-with-example/)

[Java for Loops](http://tutorials.jenkov.com/java/for.html)