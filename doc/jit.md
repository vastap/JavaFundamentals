# <a name="Home"></a> JIT Compiler

## Содержание:
- [JIT компилятор](#Overview)
- [Раннее и позднее связывание](#binding)
- [Дополнительно](#Additional)

## [↑](#Home) <a name="Overview"></a> JIT компилятор
Языки программирования можно разделить на два типа: Компилируемые и интерпритируемые.
Подробнее: "[Основные принципы программирования](https://tproger.ru/translations/programming-concepts-compilation-vs-interpretation)".
Java - интерпритируемый язык, потому что java классы при помощи компилятора java кода компилируются в некий промежуточный формат - байткод. Это не машинные команды для конкретной платформы, а некий абстрактный код для абстрактной машины. Эта машина называется Java Virtual Machine - JVM.
Но просто интерпретировать - долго. Поэтому, начиная с JDK 1.1 в Java появился JIT-компилятор.

**JIT-компиляция** (англ. Just-in-time compilation, компиляция «на лету»), динамическая компиляция — технология увеличения производительности программных систем, использующих байт-код, путём компиляции байт-кода в машинный код или в другой формат непосредственно во время работы программы. Таким образом достигается высокая скорость выполнения по сравнению с интерпретируемым байт-кодом (сравнимая с компилируемыми языками) за счёт увеличения потребления памяти (для хранения результатов компиляции) и затрат времени на компиляцию.

Про JIT компиляцию можно ознакомиться из следующих источников:
Статья: "[Введение в JIT компиляцию](http://www.polovko.me/blog/2012/10/04/vvedenie-v-jit-kompilyaciyu/)"
Видео: "[JIT-компилятор в JVM глазами Java-программиста](http://jeeconf.com/archive/jeeconf-2013/materials/jit-compiler/)"

JIT компилятор является частью JVM и запускается сразу после запуска Java программы.
Он анализирует исполняемый код, собирая так называемые "профили". Когда участок кода считается горячим (hotspot), то данный участок кода будет скомпилирован, а заодно будут выполнены оптимизации, которые JIT компилятор посчитает уместными.
Про примеры оптимизации можно прочитать тут: "[JIT-компилятор оптимизирует не круто, а очень круто](https://habrahabr.ru/post/305894/)".

Важно знать, что код компилируется в отдельную память, называемую CodeCache, которая относится к пространству **Non Heap**.
Подробнее: [Java Codecache](http://blog.andresteingress.com/2016/10/19/java-codecache).

Очень хороший обзор так же здесь: "[JVM Internals](http://blog.jamesdbloom.com/JVMInternals.html)".

## [↑](#Home) <a name="binding"></a> Раннее и позднее связывание
Благодаря механизмам JIT компиляции и интерпретации Java позволяет реализовывать механизмы как раннего связывания, так и позднего связывания.
- **Раннее связывание** - выполняется на этапе компиляции (Compile time).
- **Позднее связывание** - выполняется в момент выполнения (Runtime).

Благодаря позднему связыванию реализаван такой принцип ООП как **полиморфизм**.
На эту тему рекомендуется: статья "[Полиморфизм – позднее связывание](http://pr0java.blogspot.ru/2015/07/blog-post_66.html)".

Согласно [спецификации](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-6.html), у нас есть следующие типы вызывов:
- invokestatic
- invokevirtual
- invokespecial
- invokeinterface
- invokedynamic

Из них механизм раннего связывания обеспечивают метод invokespecial, т.е. вызов приватных методов.
```
Для всех методов Java используется механизм позднего связывания, если только метод не был объявлен как private. Вызов private метода компилируется в инструкцию байт-кода invokespecial, которая вызывает реализацию метода из конкретного класса, определенного в момент компиляции.
```
Так же есть invokestatic, который обеспечивает вызов статических методов. Что в свою очередь так же является механизмом раннего связывания.

Все остальные методы относят к механизму позднего связывания.
Дополнительный материал: "[Java Bytecode Fundamentals: Using Objects and Calling Methods](https://zeroturnaround.com/rebellabs/java-bytecode-fundamentals-using-objects-and-calling-methods/)".

Цитата с одного из ресурсов в сети:
```
All non-final non-private, not-static methods in Java are virtual
```

## [↑](#Home) <a name="Additional"></a> Дополнительно
Курс по байткоду от Hexlet: "[Байт-код Java](https://ru.hexlet.io/courses/bytecode)"