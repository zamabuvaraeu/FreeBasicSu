﻿# Хранение данных в программе: литералы, константы, переменные

Всё программирование сводится к тому, чтобы манипулировать какими‐нибудь данными. Данные хранятся не просто так «в воздухе», а физически размещаются в оперативной памяти компьютера. В исходном коде данные представлены литералами, константами и переменными.

## Константы и литералы

### Литералы

Литерал
 ~ Безымянная область памяти, которая не может изменяться. Это числа и строки, записанные в тексте программы как есть.

Число (числовой литерал) может быть записан в десятичной, шестнадцатеричной, восьмеричной и двоичной системах счисления, используя специальные префиксы.

| Система счисления    |  Префикс               |               Пример |
|----------------------|------------------------|----------------------|
| Десятеричная         | Отсутствует            |         234          |
| Шестнадцатеричная    | &h или &H              |   &hEA               |
| Двоичная             | &b или &B              |         &b11101010   |
| Восьмеричная         | &o или &O              |         &o352        |

### Константы

Константа
 ~ Именованная область памяти, которую нельзя изменять.

Константы объявляются в тексте программы через оператор `Const` по такой схеме:

```FreeBASIC
Const ИмяКонстанты [ As ТипДанных ] = Литерал
```

Константу необходимо объявлять вместе с инициализирующим значением, иначе компилятор сообщит об ошибке.

### Примеры

```FreeBASIC
' Объявляем константу количества месяцев в году
Const MonthsInYear = 12

' Объявляем константу числа пи
Const Pi = 3.141592653 
```

В данном примере <var>MonthsInYear</var> и <var>Pi</var> являются константами, а 12 и 3.141592356 — литералами.

Если попытаться присвоить константе какое‐нибудь значение, то это вызовет ошибку компиляции:

```FreeBASIC
' Объявляем константу количества секунд в минуте
Const SecondsInMinute = 60

' Пробуем изменить её
SecondsInMinute = 70
```

Результат:

<pre class="samp"><samp><span class="promt">C:\Programming\FreeBASIC Projects&gt;</span><kbd>fbc HelloWorld.bas</kbd>
HelloWorld.bas(5) error 119: Cannot modify a constant, before '=' in 'SecondsInMinute = 70

<span class="promt">C:\Programming\FreeBASIC Projects&gt;</span><span class="cursor">_</span>
</samp></pre>

Объявлять можно не только числовые константы, но и строковые:

```FreeBASIC
Const HelloWorld = "Привет, мир!"
```

<var>HelloWorld</var> — это константа, а строка `"Привет, мир!"` — литерал. Обрати внимание, что кавычки не являются частью данных, а лишь ограничивают их. Как же в таком случае поместить кавычку внутрь литерала? Достаточно её удвоить:

```FreeBASIC
Const QuotationLiteral = "Вот это "" кавычка внутри литерала"
```

Технически строковый литерал представляет собой массив символов, в конце которого компилятор добавляет символ с кодом 0, чтобы программе можно было определить конец этого массива (конец строки).

### Зачем нужны константы

Константы упрощают отладку и сопровождение программ:

* исчезает необходимость помнить конкретные числа — имена запоминаются легче;
* компилятор автоматически выявляют ошибки в именах;
* упрощается внесение изменений: значение константы задано в программе всего в одном месте.

## Переменные

Переменная
 ~ Именованная область памяти, которую можно изменять.

Любая переменная должна быть объявлена до её использования. Переменные объявляются через операторы `Dim` и `var`.

### Как допустимо называть переменные

У любой переменной должно быть имя. FreeBASIC разрешает использовать в имени переменной только английские буквы, цифры и символ подчёркивания `_`, причём имя переменной не может начинаться с цифры.

#### Примеры допустимых имён

| Имя переменной   | Пояснение |
|------------------|--------------|
| a1               | Присутствуют только английские буквы и цифра |
| \_board          | Присутствуют только английские буквы и подчёркивание |
| Super_Mega_Power | Присутствуют только английские буквы и подчёркивание |
| i                | Присутствуют только английские буквы |

#### Примеры недопустимых имён

Компилятор выдаст ошибку и откажется создавать программу при виде таких имён:

| Имя переменной   | Причина недопустимого имени |
|------------------|--------------|
| 1variable        | Имя начинается с цифры |
| Variable&        | Присутствуют недопустимые символы |
| Переменная       | Присутствуют неанглийские буквы |

У всех BASIC‐подобных языков есть особенность: большие и маленькие буквы в именах переменных, операторов и функций не различаются, поэтому компилятор будет считать <var>Variable</var> и <var>vAriaBLE</var> одним и тем же именем.

### Объявление переменных через оператор Dim

Переменные через оператор `Dim` объявляются по следующей схеме:

```FreeBASIC
Dim ИмяПеременной As ТипДанных [ = Выражение]
```

Выражением может быть любой литерал, константа, результат выполнения функции или другая переменная. Единственное здесь условие: типы данных переменной и выражения должны совпадать.

Примеры:

```FreeBASIC
' Объявляем целочисленную переменную
Dim x As Integer

' Также переменные можно объявлять сразу же вместе с начальным значением
Dim y As Integer = 35
```

В одной строке можно объявлять несколько переменных через запятую. В таком случае спецификатор типа следует указывать вначале:

```FreeBASIC
Dim As Integer x, y
```

Если для переменной не указывать инициализирующее значение, то FreeBASIC инициализирует её значением по умолчанию. Чисел и указатели инициализируются нулём, булёвые величины — `False`, строки — строкой нулевой длины.

### Объявление переменных через оператор var

Через оператор `var` переменные объявляют без указания типа данных, но сразу же с инициализирующим значением. Тип переменной автоматически определится из инициализирующего выражения.

```FreeBASIC
var ИмяПеременной = Выражение
```

Это удобно и немного сокращает код:

```FreeBASIC
' Объявим переменную, в которой будет храниться длина
var length = 5
```

Через оператор `var` можно объявить несколько переменных сразу через запятую:

```FreeBASIC
' Объявим несколько переменных в одной строке
var UserId = 251994, Rational = 2.54
```

Переменная <var>UserId</var> будет типа `Integer`, переменная <var>Rational</var> будет типа `Double`.


## Указатели

Данные хранятся в памяти компьютера. Память можно представить как последовательный набор пронумерованных коробок начиная от нуля, и размером в 1 байт. Обращаться к данным в этих коробках можно по их номеру, то есть адресу.

Указатель
 ~ Область памяти, которая хранит в себе «номер коробки» (адрес) где находятся данные.

Обычная переменная позволяет обращаться к данным по имени, указатель — по адресу.

### Объявление указателей

Указатель, как и любая переменная, должен быть объявлен. Указатели объявляют почти также, как и обычные переменные, но с добавлением ключевых слов `Ptr` или `Pointer`. Указатели можно объявлять через оператор `Dim` или `var` по таким схемам:

```FreeBASIC
' Первый вариант
Dim ИмяУказателя As ТипДанных Ptr [ = Адрес]

' Второй вариант, менее распространён
Dim ИмяУказателя As ТипДанных Pointer [ = Адрес]
```

Указатель хранит в себе номер коробки, где лежат данные определённого типа. Какого именно типа — определяется при объявлении указателя:

```FreeBASIC
' Объявим указатель на целочисленные данные
Dim pInt As Integer Ptr

' Объявим указатель на байт
Dim pByte As Byte Ptr

' Можно объявлять указатель, который будет указывать на другой адрес, где хранятся данные
Dim pIntInt As Integer Ptr Ptr
```

Тип указателя должен соответствовать типу переменной, на которую он указывает. При присваивании указателю адреса не на тот тип данных, фрибейсик при компиляции выдаст предупреждение.

### Получение адреса переменной

Каждая переменная в памяти имеет свой адрес — номер первой коробки, где она расположена, а также своё значение. Как же получить адрес данных, которые хранятся в переменной?

Для этого существует оператор `@` (`VarPtr`)— он возвращает адрес объекта.

```FreeBASIC
' Объявим целую переменную и запишем туда данные
Dim Days As Integer = 15

' Объявим указатель и получим адрес переменной
Dim pDays As Integer Ptr = @Days

' Распечатаем адрес
Print "pDays = "; pDays
```

Результат работы программы:

<pre class="samp"><samp><span class="promt">C:\Programming\FreeBASIC Projects&gt;</span><kbd>Pointers.exe</kbd>
pDays = 1375744

<span class="promt">C:\Programming\FreeBASIC Projects&gt;</span><span class="cursor">_</span>
</samp></pre>

Как видно, данные в переменной Days находятся по адресу 1375744. У тебя этот адрес наверняка будет другим, потому что программы могут загружаться в оперативную память по разным адресам.

Следует отметить, что операцию взятия адреса `@` неприменима к литералам и константам, потому что у них нет адреса.

### Получение данных по адресу

Для того, чтобы извлечь данные, на которые ссылается указатель, существует оператор `*` — он возвращает ссылку на значение, хранящееся по адресу. Такая операция называется «разыменование указателя».

Не следует путать операцию разыменования и умножение, которое тоже обозначается звёздочкой. Разыменование указателя — это унарная операция, то есть в ней участвует один операнд; умножение — бинарная операция, в ней участвуют два операнда.

```FreeBASIC
' Объявим целую переменную и запишем туда данные
Dim Days As Integer = 15

' Объявим указатель и получим адрес переменной
Dim pDays As Integer Ptr = @Days

' Получим данные, которые хранятся по адресу указателя
Dim OriginalDays As Integer = *pDays
Print "OriginalDays = "; OriginalDays

' Распечатаем данные, на которые ссылается указатель
Print "*pDays = "; *pDays

' Перезапишем данные по указателю
*pDays = 20

' Посмотрим на оригинальную переменную, её значение будет изменено
Print "Days = "; Days
```

Результат работы программы:

<pre class="samp"><samp><span class="promt">C:\Programming\FreeBASIC Projects&gt;</span><kbd>Pointers.exe</kbd>
OriginalDays =  15
*pDays =  15
Days =  20

<span class="promt">C:\Programming\FreeBASIC Projects&gt;</span><span class="cursor">_</span>
</samp></pre>

Перед разыменованием, необходимо убедиться, что значение указателя содержит действительный адрес памяти.


### Действительные и недействительные адреса

Действительный адрес
 ~ Такой адрес, который указывает на настоящие данные, переменную или функцию.

Недействительный адрес
 ~ Адрес, указывающий не на настоящие данные, переменную или функцию.

При объявлении численной переменной без инициализации компилятор автоматически инициализирует её и записывает в неё 0. Указатель также является целочисленной беззнаковой переменной, поэтому он тоже будет инициализирован нулём.

```FreeBASIC
' Объявим указатель без инициализации
Dim pDays As Integer Ptr

' В нём находится ноль
Print "pDays = "; pDays
```

Результат:

<pre class="samp"><samp><span class="promt">C:\Programming\FreeBASIC Projects&gt;</span><kbd>Pointers.exe</kbd>
pDays = 0

<span class="promt">C:\Programming\FreeBASIC Projects&gt;</span><span class="cursor">_</span>
</samp></pre>

Ящик с номером ноль — это специальное зарезервированное значение адреса, что указатель содержит недействительный адрес. При любой операции к такой памяти процессор генерирует специальное прерывание, операционная система перехватывает это прерывание и завершает вызвавший его процесс с ошибкой времени выполнения: «Программа выполнила недопустимую операцию и будет закрыта».

Разыменование указателя с недействительным адресом является операцией с неопределённым поведением. Это значит, что может произойти всё, что угодно: обращение к непредназначенной для использования данной программой памяти, запись в непринадлежащие программе данные, завершение процесса, зависание всей системы или что угодно другое:

```FreeBASIC
Dim p As Integer Ptr

' Указателю не присвоен действительный адрес, разыменовывать его нельзя
' Следующая строка приведёт к неопределённому поведению
Print *p
```


## Упражнения

1. Какой тип данных будет лучшим для хранения числа 196?
2. Какой тип данных будет лучшим для хранения числа 2.134?
3. Какой тип данных является наилучшим для общего использования в 32‐битных системах? А в 64‐битных системах?
4. В чём разница между знаковыми и беззнаковыми типами данных?
5. Какой префикс ты будешь использовать для обозначения двоичного числа?
6. Какой префикс ты будешь использовать для обозначения шестнадцатеричного числа?
7. Какие буквы допускаются в шестнадцатеричном числе?
8. Дано шестнадцатеричное число: 1AF. Переведи в десятичное число.