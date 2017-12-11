# <a name="Home"></a> Using operators and decision constructs

## Содержание:
- [Overview](#Overview)
- [Use Java operators](#Use)
    - [Assignment, Arithmetic, and Unary Operators](#operators1)
    - [Equality, Relational, and Conditional Operators](#operators2)
    - [Bitwise and Bit Shift Operators](#Shifts)
    - [Expressions, Statements, and Blocks](#ExprStatemens)
- [Equality between strings and other objects](#Equality)
- [Create and use if, if-else, and ternary constructs](#ifelse)
- [Use a switch statement](#switch)
- [InstanceOf operator](#InstanceOf)

## [↑](#Home) <a name="Overview"></a> Overview
Данный раздел относится к [OCA Java SE 8 Programmer I](http://education.oracle.com/pls/web_prod-plq-dad/db_pages.getpage?page_id=5001&get_params=p_exam_id:1Z0-808).
Разделы: "[Java SE 8 Programmer I Exam Topics](https://docs.oracle.com/javase/tutorial/extra/certification/javase-8-programmer1.html#operators)"
Подробнее: "[3. Using Operators and Decision Constructs](http://javacertification.wikidot.com/operators-and-decisions)"

**Операторы** в языке Java — это специальные символы, которые сообщают транслятору о том, что вы хотите выполнить операцию с некоторыми операндами (аргументами операции).
Некоторые операторы требуют одного операнда, их называют унарными. Одни операторы ставятся перед операндами и называются префиксными, другие — после, их называют постфиксными операторами.
Большинство же операторов ставят между двумя операндами, такие операторы называются инфиксными бинарными операторами.
Существует тернарный оператор, работающий с тремя операндами.
Хороший материал на тему базовых операций: [Основные операторы языка](http://proglang.su/java/operators).
Официальная документация: "[Operators](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html)".

## [↑](#Home) <a name="Use"></a> Use Java operators
### [↑](#Home) <a name="operators1"></a> Assignment, Arithmetic, and Unary Operators
Описаны в "[Assignment, Arithmetic, and Unary Operators](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op1.html)".

**Assignment Operators** - Операции присваивания
Операции присваивания. Помимо базовой операции при помощи ``=`` мы можем выполнить присваивание с какой-нибудь арифметической операцией:
```java
int a = 2;
a = a + 2;
a += 2;
```

**Unary Operators** - унарные операторы, требующие только 1 операнд.
Помимо логического отрицания ``!`` и указания знака ``+`` или ``-`` есть такие интересные унарные операторы, как операторы инкрементации:
- Инкремент: ``++``
- Декремент: ``--``

Их стоит выделить отдельно и вот почему:
```java
int a = 2;
System.out.println(a++); //2 выведет, 3 сохранит
System.out.println(++a); // сохранит 4, выведет 4
```
Важно то, в каком месте стоит знак инкремента/декремента.
Чтобы это запомнилось, стоит всегда читать такое так:
`` a += 2 `` - сначала выполни сложение, потом сохрани. Аргумент операции 2
`` a++ `` - сначала отдай значение a, а потом увелич его на 1
`` ++a `` - сначала увеличь, потом отдай значение a

Сложнее дела с такими выражениями:
```java
int a = 2;
System.out.println(a++ + --a);
```
Результат будет 4. Логика будет примерно следующая:
Возьмём число a, увеличим его на 1, но пока запомним это в уме (т.к. знаки после a). Отдадим другим операторам неизменённое значение. Ага, -- в начале. Значит, вычтем из a единицу и вернём значение. Итого: 4.

**ArithmeticOperators** - Арифметические операции.
Самыми базовыми операторами являются **Арифметические операторы**. Они выполняют математические действия, такие как: сложение, вычитание, умножение, деление.
Например: ```int a = 2 + 3;```
У деления есть ещё один вариант:
```java
int a = 5 / 3; //1
int b = 5 % 3; //2
```
Можно воспроизвести тут: [compile java online](https://www.tutorialspoint.com/compile_java_online.php).

Для выполнения более сложных арифметических операций используется пакет [java.lang.Math](https://docs.oracle.com/javase/8/docs/api/java/lang/Math.html).
Например:
```java
public static void main(String []args){
	System.out.printf("%d Squared equals %.0f \n", 3, Math.pow(3, 2));
	System.out.printf("2 to the power of %.0f", Math.sqrt(16));
}
```
Кроме того, на результат влияет сам тип операндов:
```java
System.out.println(2 + 3);      //5
System.out.println(2 + '3');    //53
System.out.println(2 + "3");    //"23"
System.out.println('2' + '3'); //101
```
например, несмотря на то, что char это символ, для Java это просто число, представляющее символ. Код символа `3` - 51 ([Таблица ascii](http://www.asciitable.com/)).

При помощи оператора ``+`` так же можно конкатенировать строки, т.е. складывать. Подробнее про сложение строк можно прочитать тут:
"[The Optimum Method to Concatenate Strings in Java](http://www.rationaljava.com/2015/02/the-optimum-method-to-concatenate.html?m=1)".

Конкатенация через + для строк является "синтаксическим сахаром" и будет превращена в выражение вида ``new StringBuilder().append()...toString()```.
Интересно, что для данного выражения действуют внутренние оптимизации JVM (они включены по умолчанию, управляются опцией JVM -XX:+OptimizeStringConcat).
Это называется **intrinsic** - переводится как "внутренний". Такие вещи JVM обрабатывает особенным образом, обрабатывая их как Native, только без дополнительных затрат на JNI.
Подробнее: "[Intrinsic Methods in HotSpot VM](https://www.slideshare.net/RednaxelaFX/green-teajug-hotspotintrinsics02232013)".

Так же стоит прочитать: "[Обработка строк в Java. Часть I: String, StringBuffer, StringBuilder](https://habrahabr.ru/post/260767/)".

### [↑](#Home) <a name="operators2"></a> Equality, Relational, and Conditional Operators
Описаны в "[Equality, Relational, and Conditional Operators](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op2.html)".

**Equality и relational** операторы - это операторы, определяющие, является ли один из операндов больше, меньше, равным или не равным другому.
**Conditional Operators** - условные операторы.
Подробнее см. "[Oracle Tutorial : Control Flow Statements](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/flow.html)".

Логические операторы бывают:
- [унарный логический оператор "!"](http://docs.oracle.com/javase/specs/jls/se7/html/jls-15.html#jls-15.15.6)
- [бинарные логические операторы "^", "|", "&"](http://docs.oracle.com/javase/specs/jls/se7/html/jls-15.html#jls-15.22.2)
- [сокращённый логический оператор "&&"](http://docs.oracle.com/javase/specs/jls/se7/html/jls-15.html#jls-15.23)
- [сокращённый логический оператор "||"](http://docs.oracle.com/javase/specs/jls/se7/html/jls-15.html#jls-15.24)

Используются в конструкциях:
- [if-then-else](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/if.html)
- [switch statement](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/switch.html)

Так же используются в тернарных операциях. Например:
```java
public static void main(String []args){
	System.out.println(minVal(4, 2));
}

public static int minVal(int a, int b) {
	return (a < b) ? a : b;
}
```
Подробнее см. "[The Java ternary operator](https://alvinalexander.com/java/edu/pj/pj010018)".

### [↑](#Home) <a name="Shifts"></a> Bitwise and Bit Shift Operators
Описаны в "[Bitwise and Bit Shift Operators](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op3.html)".

**Bitwise Operators** - Побитовые операции
Про побитовые операции можно прочитать тут: [О битовых операциях](https://tproger.ru/translations/bitwise-operations/).
И тут: [Побитовые операции](https://neerc.ifmo.ru/wiki/index.php?title=%D0%9F%D0%BE%D0%B1%D0%B8%D1%82%D0%BE%D0%B2%D1%8B%D0%B5_%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%86%D0%B8%D0%B8).

Хочется только оставить пример практической пользы:
```java
public static void main(String []args){
	System.out.println(isPowerOfTwo(0));
	System.out.println(isPowerOfTwo(4));
	System.out.println(isPowerOfTwo(3));
}

public static boolean isPowerOfTwo(int x) {
	return x > 0 && (x & (x - 1)) == 0;
}
```

**Shift Operators** - Операторы смещения (сдвигов)
Существует несколько видов сдвигов:
- **Сдвиг влево <<**
Например, 3 в двоичной системе: **011**
Сдвигаем на 1 влево и получаем: **110**
Таким образом будет получено число 6.
**Итого:** Сдвиг влево = умножению числа на 2 в степени, равной кол-ву сдвигов
Запомнить, что именно умножение легко: когда увеличивается двоичное число, то разряды добавляются слева. То есть мы растём (умножаемся) **справа налево**
```
3 << 2 = 3*(2^2) = 12, а 3 << 3 = 3*(2^3)= 24
```
- **Сдвиг вправо >>**
Например, 3 в двоичной системе: **011**
Сдвигаем на 1 вправо. 1 при сдвиге вправо становится нулём.
Поэтому в результате 3>>1 мы получим: 000, т.е. ноль
Возьмём интереснее нам число: **10101**, равное 21 в десятичной системе
Сместим вправо на 1, получим: **01010**, равное 10 в десятичной системе
**Итого:** Результат сдвига вправо = деление числа на 2 в степени, равной кол-ву сдвигов
```
10>>2 = 10/2^2 = 10/4 = 2
21>>1 = 21/2^1 = 21/2 = 10.5, int часть 10
```
Cдвиг вправо значения -1 всегда равен -1, поскольку дополнительные знаковые разряды добавляют новые единицы к старшим битам.
- **Побитовое XOR ^**
Результирующий бит, полученный в результате выполнения оператора ^, равен 1, если соответствующий бит только в одном из операндов равен 1. Во всех других случаях результирующий бит равен 0.

Подробнее про сдвиги: [Побитовые операторы](http://developer.alexanderklimov.ru/android/java/bitwise.php)

### [↑](#Home) <a name="ExprStatemens"></a> Expressions, Statements, and Blocks
Описаны в "[Expressions, Statements, and Blocks](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/expressions.html)".

Логически код можно разделить на:
- Expressions
Выражение - это конструкция, состоящая из переменнх, операторов и вызовов методов, которые получают/высчитывают какое-либо значение
- Statements
Утверждение - законченное действие, которое закончилось на ;
- Blocks
Группируют утверждения в блоки.

Блоки образуются между открывающими и закрывающими фигурными скобками.
Могут быть как частью утверждений, например [Control Flow Statements](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/flow.html) так и самостоятельной частью.
В случае самостоятельного блока это может быть блок инициализации: [Initializing Fields](https://docs.oracle.com/javase/tutorial/java/javaOO/initial.html).
Блоки можно использовать и как самостоятельную единицу внутри метода.

Выполнение statement'ов выполняется в определённом порядке:
[Operator Precedence in Java](https://introcs.cs.princeton.edu/java/11precedence/).


## [↑](#Home) <a name="Equality"></a> Equality between strings and other objects
Объекты в Java сравниваются оператором ``==`` по ссылке, а не по значению.
Для сравнения объектов по значению предусмотрен метод equals, который можно переопределять (т.к. любой класс наследуется от Object).
По умолчанию реализация метода equals:
```java
return (this == obj);
```
То есть без переопределения equals равносилен сравнению через ==.
Примером может служить String. У него переопределён метод equals, который вместо сравнения по ссылке сравнивает строки посимвольно.
В сравнение строк вносит некоторую изюминку String Pool.
Это пул строк, в котором хранятся объекты String, которые в коде указаны как литералы. Пример:
```java
public static void main(String[] args) {
		String s1 = "text";
		String s2 = "text";
		String s3 = new String("text");
		System.out.println(s1 == s2); //true
		System.out.println(s2 == s3); // false
		String s4 = s3.intern();
		System.out.println(s2 == s4); //true
	}
```
Последний вариант будет true, потому что метод intern добавляет значение в пул и возвращает ссылку уже на него. А так как в пуле уже есть text, то вернётся ссылка на него. Поэтому, s4 будет равен s2, т.к. в этих переменных лежат ссылки на один и тот же объект в String Pool.

Некоторые другие классы так же переопределяют метод Equals, например:
- ArrayList (итерируется в equals по всем элементам при помощи ListIterator)
- HashMap (итерируется по entry set)

Так как массивы являются объектами, то они тоже сравниваются через == как объекты, т.е. по ссылке, а не содержимому. Поэтому, сравнение массива выполняется через специальные утильные методы:
```java
int[] a = {1, 2, 3};
int[] b = {1, 3, 3};
System.out.println(Arrays.equals(a, b));
```

Сравнение ещё работает немного с изюминкой и для типов Integer:
```
int a = 500;
Integer b = 500;
Integer c = 500;
System.out.println(a == b); //true
System.out.println(b == c); //false
```
В первом случае будет true, т.к. выполнится unboxing и будут сравниваться два примитива.

Ещё при сравнении стоит учитывать, что некоторые вещи не являются чем-то, что можно сравнить, поэтому сравнение всегда выдаст false. Например:
```java
float val = Float.NaN;
float val2 = Float.NaN;
System.out.println(val == val2); //false
```
Несмотря на это, null можно сравнивать с null. Благодаря этому и работают "проверки на null".
Поэтому:
```java
System.out.println(null == null); // true
```

## [↑](#Home) <a name="ifelse"></a> Create and use if, if-else, and ternary constructs
Описано в разделе документации: [The if-then and if-then-else Statements](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/if.html) и [Equality, Relational, and Conditional Operators](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op2.html).

Нужно запомнить, что действия в if лучше всегда оформлять внутри блоков (т.е. внутри фигурных скобок), чтобы избежать подобных ошибок:
```java
int i = 42;
if(i == 42)
	System.out.println("That is the meaning of life");
	System.out.println("This is not inside the if!!!");
```
Здесь создаётся впечатление, что обе строчки должны выполниться. Но это не так. Т.к. блока нет - при условии i == 42 выполнится только одна, а другая будет выполняться всегда.

Так же стоит понимать, что if устроен так, что он считает выражение и получает boolean результат. Поэтому, нежелательно использовать долгие вычисления в самой проверке.

## [↑](#Home) <a name="switch"></a> Use a switch statement
Описано в разделе документации: "[The switch Statement](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/switch.html)".

Особенностью switch является то, что он может иметь несколлько веток выполнения, в отличии от if.
Доступные типы:
- primitives (byte, short, char, int)
- enumerated types
- String
- Character, Byte, Short, and Integer

Switch по строке появился в Java 7.
Пример:
```java
int num = 2;
//int num = 5; // will use default
switch(num) {
	case 1:
		System.out.println("2");
    case 2:
		System.out.println("2");
        //break;
	case 3:
		System.out.println("3");
		System.out.println("Either  1, 2 or 3");
        //break;
	case 4:
		System.out.println("3");
		break;  // break here
	default:
        System.out.println("default");
        break;
}
```
Особенностью switch является то, что он срабатывает на определённом case и начинает заходить выполнять каждую строку, пока не встретит слово **brake** или не закончится switch. Так же **switch-case** является блоком со всеми вытекающими.

Значение в **case** может быть только константным значением.
Блок **default** выполняется тогда, когда не сработал ни один case.

### [↑](#Home) <a name="InstanceOf"></a> InstanceOf
Ещё одним оператором, но который не попал ни в одну из групп, является оператор, выраженный ключевым словом instanceof.
Подробнее см. [The instanceof Keyword](http://www.java2s.com/Tutorial/Java/0060__Operators/TheinstanceofKeyword.htm).