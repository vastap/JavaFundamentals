# <a name="Home"></a> Creating and using arrays

## Содержание:
- [Overview](#Overview)
- [Основное](#Main)
- [Создание](#Creating)
- [Копирование](#Copying)
- [Сортировка](#Sorting)
- [Другое](#Other)
- [Материалы](#Resources)

## [↑](#Home) <a name="Overview"></a> Overview
Данный раздел относится к [OCA Java SE 8 Programmer I](http://education.oracle.com/pls/web_prod-plq-dad/db_pages.getpage?page_id=5001&get_params=p_exam_id:1Z0-808).
Разделы: "[Java SE 8 Programmer I Exam Topics](https://docs.oracle.com/javase/tutorial/extra/certification/javase-8-programmer1.html#arrays)"
Подробнее: "[4. Creating and Using Arrays](http://javacertification.wikidot.com/arrays)"

Одна из саммых распространённых структур данных в Java является массив. Он везде. Даже строки являются на самом деле надстройкой над массивом символов, т.е. [Char](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html). Так же широко используется в буферах (массивы байт) и в **Java Collection Framework**.

Официальный tutorial по массивам: "[Arrays](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/arrays.html)".

## [↑](#Home) <a name="Main"></a> Основное
Массивы представляют из себя структуру данных в виде набора элементов, расположенных в памяти последовательно, т.е. непосредственно друг за другом. Обращение к элементам происходит по их индексу за константное время. У Oracle указано, что:
``An array is a container object that holds a fixed number of values of a single type.``
Т.е. массив - это прежде всего **ОБЪЕКТ**, который является контейнером для объектов **одного** и того же типа.
Так же благодаря тому, что массив - последовательная структура, то заранее известно, как найти элемент по индексу. Поэтому, массив является структурой данных с произвольным доступом ([Random Access](https://en.wikipedia.org/wiki/Random_access)).

Массив обозначается квадратными скобками:
```java
public static void main(String[] args){
	String[] lines = new String[3];
	lines[0] = "Hello";
	lines[1] = ", World!";
	System.out.println(lines[0] + lines[1]);
}
```
Можно опробовать тут: [compile java online](https://www.tutorialspoint.com/compile_java_online.php).

Размерность у массива может быть разная, т.е. они бывают не только одномерными.
На самом деле многомерный массив - это просто массив массивов. Поэтому, при объявлении многомерного массива обязательно указывать размерность только в первых скобках, т.е. только размерность "строк":
```java
public static void main(String[] args){
	String[][] lines = new String[3][];
	lines[0] = new String[3];
	lines[0][1] = "Hello";
	System.out.println(lines[0][1]);
}
```

Размерность массива может быть указана как у типа массива, так и у названия переменной. Но размерность массива является частью описания типа переменной, поэтому правильным считается указывать размерность у типа, а не у названия переменной.
Подробнее см. [Oracle Tutorials - Arrays](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/arrays.html)

Размерность может быть указана даже так:
```java
int[][] a = new int[1][];
int[] b[] = new int[1][];
```

**Сколько же элементов может хранить массив?**
Это ограничение равносильно предельному значению Integer, т.е. значение Integer ограничено 4 байтами (32 бита), а это 2 147 483 647 (два миллиарда сто сорок семь миллионов). Связано это как раз с тем, что массив - это объект. А у каждого Java объекта есть свой заголовок с метаинформацией. 

На эту тему можно прочитать [What is in java object header
](https://stackoverflow.com/questions/26357186/what-is-in-java-object-header) и [Размер Java объектов](https://m.habrahabr.ru/post/134102/) и на слайдах [JavaOne 2013: Memory Efficient Java](https://www.slideshare.net/cnbailey/memory-efficient-java).
Чтобы узнать длинну массива, необходимо использовать ``<массив>.length``.

Массивы имеют фиксированный размер, указываемый при созднании.
Это естественным образом вытекает из того, что массив - это единый участок памяти. И этот участок в памяти выделяется в области памяти, называемый кучей (Heap).
Простой эксперимент для подтверждения:
```java
public class Main {
	private static long last;

	public static void main(String[] args) {
		measure();
		int[] a = new int[100000];
		measure();
	}

	private static void measure() {
		long heapFreeSize = Runtime.getRuntime().freeMemory();
		if (last!=0) System.out.println(last - heapFreeSize);
		last = heapFreeSize;
	}
}
```
В примере видно, что мы создаём массив int'ов. Размер 1 примитивного integer равен 4 байтам. Итого ожидаем 100000 * 4. С небольшой погрешностью получим ожидаемый результат (погрешность из-за того, что используется не только объявление массива, но и различные вспомогательные операции для вычисления используемого объёма хипа). Мы можем такое увидеть потому что массив инициализирует каждый свой элемент значением по умолчанию. Для объектов это null, для примитивов это нулевое значение.

## [↑](#Home) <a name="Creating"></a> Создание
Создать массив можно несколькими способами:
```java
public static void main(String[] args){
	String[] empty = new String[2];
	String[] withValue = new String[]{"one", "two"};
	String[] fromValues = {"one", "two", "three"};
	System.out.println(fromValues.length);
}
```
Причём третий вариант возможен только при создании. Присвоить его на отдельной строке не выйдет:
```java
public static void main(String[] args){
	String[] okString;
	okString = new String[]{"one", "two"};
	String[] errString;
	errString = {"one", "two", "three"};
	System.out.println(errString.length);
}
```

## [↑](#Home) <a name="Copying"></a> Копирование
Единственным способом получить увеличенный массив - создать его копию.
Для этого существует несколько вариантов:
- Arrays.copyOf
```java
int[] original = new int[6];
int[] extended = Arrays.copyOf(original, original.length*2);
```
- System.arraycopy
```java
int[] original = new int[6];
int[] target = new int[original.length*2];
System.arraycopy(original, 0, target, 0, original.length);
```
Разницы между ними нет в реализации, т.к. Arrays.copyOf вызывает System.arraycopy. Единственная разница в том, что Arrays.copyOf создаёт массив, а System.arraycopy копирует в уже созданный ранее массив.

## [↑](#Home) <a name="Sorting"></a> Сортировка
При необходимости, массив можно отсортировать уже готовым средством.
Утильный класс **java.util.Arrays** предоставляет метод **Arrays.sort**:
Подробнее про сортировку тут:
"[Arrays.sort() in Java with examples](http://www.geeksforgeeks.org/arrays-sort-in-java-with-examples/)"

А так же здесь: "[Интерфейсы Comparable и Comparator](https://metanit.com/java/tutorial/5.6.php)".

## [↑](#Home) <a name="Other"></a> Другое
Для выполнения различных действий над массивами существует утильный/утилитный (Util) класс **java.util.Arrays**. При помощи него можно:
- Arrays.fill
- Arrays.binarySearch
Для двоичного (поиска делением) поиска необходимо, чтобы массив был отсортирован ascending, т.е. по восходящей.

Сдвиг элементов массива уже реализован и его можно подсмотреть в ArrayList.remove. Выполняется он при помощи копирования:
``System.arraycopy(<массив>, index+1, <массив>, index, numMoved)``.
Количество сдвигаемых элементов, numMoved вычисляется как ``size - index - 1``. Последний элемент выставляется при этом в null.

Более подробно см. [Arrays class in Java](http://www.geeksforgeeks.org/array-class-in-java).

## [↑](#Home) <a name="Resources"></a> Материалы
kostin.ws - Статья: "[Массивы в Java](http://kostin.ws/java/java-arrays.html)"
alexanderklimov - Статья: "[Массив](http://developer.alexanderklimov.ru/android/java/array.php)"
Спецификация: [jls JSE7](http://docs.oracle.com/javase/specs/jls/se7/html/jls-10.html#jls-10.4)
Задачи: [CodingBat](http://codingbat.com/java)
Лекция: [Princaton: 1.4   Arrays](https://introcs.cs.princeton.edu/java/14array/)