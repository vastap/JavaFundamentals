# <a name="Home"></a> Using operators and decision constructs

## Содержание:
- [Обзор](#Overview)
- [Арифметические операции](#ArithmeticOperators)
- [Операции присваивания](#AssignmentOperators)
- [Инкрементация](#Incrementation)
- [Побитовые операции](#BitwiseOperators)
- [Логические операции](#LogicalOperators)
- [Сдвиги](#Shifts)
- [InstanceOf](#InstanceOf)
- [Дополнительные материалы](#Resources)

## [↑](#Home) <a name="Overview"></a> Обзор
**Операторы** в языке Java — это специальные символы, которые сообщают транслятору о том, что вы хотите выполнить операцию с некоторыми операндами (аргументами операции).
Некоторые операторы требуют одного операнда, их называют унарными. Одни операторы ставятся перед операндами и называются префиксными, другие — после, их называют постфиксными операторами.
Большинство же операторов ставят между двумя операндами, такие операторы называются инфиксными бинарными операторами.
Существует тернарный оператор, работающий с тремя операндами.
Хороший материал на тему базовых операций: [Основные операторы языка](http://proglang.su/java/operators).

## [↑](#Home) <a name="ArithmeticOperators"></a> Арифметические операции
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

## [↑](#Home) <a name="AssignmentOperators"></a> Операции присваивания
Операции присваивания. Помимо базовой операции при помощи ``=`` мы можем выполнить присваивание с какой-нибудь арифметической операцией:
```java
int a = 2;
a = a + 2;
a += 2;
```

## [↑](#Home) <a name="Incrementation"></a> Инкрементация
В арифметических операциях есть два важных оператора:
- Инкремент: ++
- Декремент: --

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

### [↑](#Home) <a name="BitwiseOperators"></a> Побитовые операции
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

### [↑](#Home) <a name="LogicalOperators"></a> Логические операции
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

### [↑](#Home) <a name="Shifts"></a> Сдвиги
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

### [↑](#Home) <a name="InstanceOf"></a> InstanceOf
Ещё одним оператором, но который не попал ни в одну из групп, является оператор, выраженный ключевым словом instanceof.
Подробнее см. [The instanceof Keyword](http://www.java2s.com/Tutorial/Java/0060__Operators/TheinstanceofKeyword.htm).

## [↑](#Home) <a name="Resources"></a> Дополнительные материалы
Официальные tutorials:
- [Equality, Relational, and Conditional Operators](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op2.html)
- [Operators](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html)