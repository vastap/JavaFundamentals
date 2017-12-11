# <a name="Home"></a> Recursion

## Содержание:
- [Обзор](#Overview)
- [Стэк](#Stack)
- [Примеры](#Examples)
- [Материалы](#Resources)

## [↑](#Home) <a name="Overview"></a> Обзор
**Рекурсия** — это процесс определения чего-либо в терминах самого себя.
Применительно к программированию на Java рекурсия — это способ получения результата при помощи вызова метода самого себя.
Частным случаем рекурсии является [Хвостовая рекурсия](https://dic.academic.ru/dic.nsf/ruwiki/596290).

Рекурсии на хабрахабре посвящена отличная статья: [Рекурсия. Тренировочные задачи](https://habrahabr.ru/post/275813/).
Так же можно прочитать тут: [Классы. Часть 5 – рекурсивные методы.](http://pr0java.blogspot.com.by/2015/06/5_23.html).

Одним из базовых примеров рекурсии - вычисление [чисел Фибоначчи](https://ru.wikipedia.org/wiki/Числа_Фибоначчи).
В тему можно посмотреть тут видео: [3.8 Практика на Java: Числа Фибоначчи](https://stepik.org/lesson/15831/step/1).

Для проб нам подойдёт обычный онлайн компилятор, например: [от tutorialspoint](https://www.tutorialspoint.com/compile_java_online.php).
Вот самый простой пример:
```java
public static void main(String[] args){
	System.out.println(fibonachi(6));
}

static int fibonachi(int n){
	//Условие выхода
	if (n == 0 || n == 1){
		return n;
	}
	// Рекурсивный вызов
	return fibonachi(n - 1) + fibonachi(n - 2);
}
```

Но, с рекурсией не так всё хорошо.
Первая сложность - стэк вызовов методов. См. следующий раздел.

Пример правильного вычисления чисел Фибоначчи:
```java
public static void main(String[] args){
	//Sequence: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610
	System.out.println(fibonachi(6));
}

public static int fibonachi(int n) {
	int first = 0;
	int second = 1;
	int sum = 0;
    for(int i = 1; i < n; i++){
		sum = first + second;
		first = second;
		second = sum;
	}
	return sum;
}
```
Как видно из примера, рекурсию можно заменить на цикл. В данном случае нет избыточных вычислений и не будет переполнения стэка.

## [↑](#Home) <a name="Stack"></a> Стэк
На том же [online compiler](https://www.tutorialspoint.com/compile_java_online.php)'е можем выполнить следующее:
```java
public static void main(String[] args){
	try {
		visitMethod();
	} catch (Error e) {
		e.printStackTrace();
	}
}

public static void visitMethod() {
	visitMethod();
}
```
Данный код при выполнении бросит Error, а не Exception: **java.lang.StackOverflowError**. Связано это с тем, что происходит переполнение стэка вызовов.
Подробнее про стэк см. "[Java Memory Model. Что такое Heap и Stack](http://begoml.by/java-memory-model/)" и "[Стек-трейс Java](http://info.javarush.ru/IvanDurov/2013/12/23/Стек-трейс-Java.html)".

Про фреймы, из которых состоит стэк, можно прочитать в спецификации языка: "[2.6. Frames](https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-2.html#jvms-2.6)".

Ну и обсуждения того, чем плох стэк: "[Почему рекурсия так плоха](https://ru.stackoverflow.com/questions/620933/java-Почему-рекурсия-так-плоха)".

Простым примером того, что рекурсия не подходит для Фибоначчи является хотя бы то, что пример, указанный выше, даже на не очень больших числах будет работать оочень долго.

## [↑](#Home) <a name="Examples"></a> Примеры
Задачи для решения рекурсией:
- [CodingBat: Recursion-1](http://codingbat.com/java/Recursion-1)
- [CodingBat: Recursion-2](http://codingbat.com/java/Recursion-2)

Примеры решения задач через рекурсию:
[Top 10 Coding Interview Questions using Recursion](http://www.topjavatutorial.com/java/java-programs/top-programming-interview-questions-using-recursion-in-java/)

## [↑](#Home) <a name="Resources"></a> Материалы
- [Introduction to Programming in Java : recursion](https://introcs.cs.princeton.edu/java/23recursion/)
- [Рекурсия.тренировочные задачи](https://habrahabr.ru/post/275813/)
- [5 способов вычисления чисел Фибоначчи: реализация и сравнение](https://habrahabr.ru/post/261159/)
- [Подборка материалов по рекурсии](http://www.pvsm.ru/cat/rekursiya)