- [Введение](#org215ee90)
- [Особенности **Ocaml**](#orgfa998ac)
- [Загрузка и установка](#org52dbb7f)
- [Использование **REPL**](#org20cde9b)
- [Встроенные типы данных](#orgd95beeb)
  - [Кортежи](#org9adc33b)
  - [Списки](#org5c1892d)
  - [Массивы](#org7e94d04)
- [Конструкция **let**](#orgbbd2d35)
  - [Пример с проверкой числа на простоту](#orge40b26a)
- [Работа с source-файлами](#orgf758f0a)
- [Список источников](#org23fa391)
  - [Ссылки](#org0f66eb5)
  - [Книги](#orgddd8191)



<a id="org215ee90"></a>

# Введение

<span class="underline">Цель данного пособия</span> - познакомить с основами **OCaml**, который относится к разряду функциональных (аппликативных) языков программирования.

У функциональных языков, ведущих свою "родословную" от Лиспа, сложная и интересная судьба. Эти языки, любимые в университетах, научных институтах и центрах, почти незнакомы широким массам разработчиков.

Не будучи промышленными языками программирования, функциональные языки, тем не менее, являются носителями множества интересных концепций и возможностей, с которыми необходимо познакомиться любому профессионалу в области ИТ. В разные периоды времени, когда традиционные императивные языки выходят из моды, заменяются на более современные, функциональные языки демонстрируют удивительную актуальность в любые времена. Можно утверждать, что они намного опередили свое время, а может быть именно их время ещё не пришло.

**OCaml** достаточно широко используется на Западе для обучения языкам программирования, курсы по нему можно найти во многих университетах мира. На странице <https://ocaml.org/learn/teaching-ocaml.html> приведен внушительный список университетских курсов, изучающих **OCaml**.

Приходится выразить сожаление, что **OCaml** в России почти неизвестен и практически нет книг и пособий, посвящённых ему, за исключением перевода документации и книг&#x2026;


<a id="orgfa998ac"></a>

# Особенности **Ocaml**

-   Язык является *мультипарадигмальным*, то есть поддерживает несколько парадигм программирования. Среди них:
    -   функциональная
    -   императивная
    -   объектно-ориентированная.
-   Язык обладает высокой степенью *выразительности*, облегчающей его освоение и использование.
-   Пакет разработчика предоставляет возможность как *компиляции* кода программы в бай-код или в машинный код, а также работу с программой в режиме *интерпретатора*.
-   Управление памятью в программах полностью *автоматическое*.
-   Ocaml обеспечивает *статическую* и *строгую* типизацию, что обнаруживает ошибки ещё на этапе компиляции.


<a id="org52dbb7f"></a>

# Загрузка и установка

Основной ресурс **OCaml** расположен по адресу <https://ocaml.org>, там же можно найти ссылки на установочные пакеты для большого числа операционных систем. После загрузки и развёртывания пакета разработчика (можно произвести установку на обычный флеш-накопитель и использовать как portable-приложение) пользователю доступны следующие программы:

-   `ocaml` - интерпретатор;
-   `ocamlc` - компилятор в байт-код;
-   `ocamlopt` - компилятор в машинный (native) код;

Представляет интерес использование редактора **MS Visual Studio Code**, далее сокращенно **vscode** для разработки. Необходимо установить расширение редактора <https://github.com/hackwaly/vscode-ocaml> или <https://github.com/reasonml-editor/vscode-reasonml> для комфортной работы. В качестве интерпретатора можно использовать `ocaml` или (что еще лучше)- `utop`.

Менеджер пакетов **OCaml** называется `opam`. С помощью этой программы устанавливается большинство библиотечных пакетов.

Более подробная информация об установки и настройке приведена на странице <https://ocaml.org/docs/install.html>


<a id="org20cde9b"></a>

# Использование **REPL**

Для перехода в режим интерпретатора нужна запустить программу `ocaml`. Появится приглашение командной строки (#) и система переходит в режим **REPL**.

```
OCaml version 4.07.0

# 
```

**Read Eval Print Loop** является одним из самых популярных режимов работы с программами на **OCaml**. Вы просто набираете в интерпретаторе выражения, а потом видите результат их исполнения.

Рассмотрим примеры использования **OCaml** в качестве калькулятора:

```ocaml
# 3 + 5;;
```

Результат выполнения:

<div class="verbatim">
-   : int = 8

</div>

**OCaml** является строготипизированным языком, так что появление **int** в ответе не должно удивлять. Тип результата выражения должен совпадать с типами операндов. Попробуйте смешать в одном выражение целочисленную и вещественную константы и вы немедленно получите сообщение об ошибке:

```
# 3 + 5.7;;
Characters 2-5:
  3+5.7;;
    ^^^
Error: This  expression has type float 
but an expression was expected of type
int
```

На первый взгляд, такое поведение интерпретатора может показаться странным, но не будем забывать, что именно с неявным преобразованием типов в программах начинают появляться трудноуловимые ошибки.

Знаки операций для целых и вещественных чисел отличаются: `+` и `+.`

```ocaml
# 1.7 +. 7.1;;
- : float = 8.7999999999999989
```

Для преобразования значений из одного типа в другой существуют специальные функции **int\_of\_float** и **float\_of\_int**:

```ocaml
# int_of_float 1.7 + 4;;
- : int = 5
# 1.7 +. float_of_int 4;;
- : float = 5.7
```

Для выхода из интерпретатора в ОС нужно выполнить команду `exit 0;;`


<a id="orgd95beeb"></a>

# Встроенные типы данных

В качестве основных типов используются:

| Название     | Обозначение | Пример  |
|------------ |----------- |------- |
| Целый        | int         | 8       |
| Вещественный | float       | 3.1     |
| Логический   | bool        | true    |
| Строковый    | string      | "Hello" |
| Символьный   | char        | 'a'     |
|              |             |         |

Дополнительно к типам (множественным) относят:

-   кортежи
-   списки
-   массивы

Пример действий над строками (конкатенации):

```ocaml
# "Hello " ^ "world!";;
- : string = "Hello world!"
```

Пример действий с символьным типом:

```ocaml
# int_of_char('A');;
- : int = 65
```

Для работы с логическим типом имеются логические операции:

| Название   | Обозначение | Пример        |
| Отрицание  | not         | not true      |
| Конъюнкция | &&          | true && false |
| Дизъюнкция | ll          | true ll false |

Для сравнения значений между собой используют такие операции:

| Название              | Обозначение |
| Структурное равенство | =           |
| Физическое равенство  | ==          |
| Отрицание =           | <>          |
| Отрицание ==          | !=          |
|                       |             |

Чем отличаются структурное и физическое равенства?

**Структурное** равенство позволяет сравнить значение полей структуры (при множественных типах), в то время как **физическое** сравнивает адреса в памяти. Следует помнить, что вещественные числа и строки относятся к *структурным* типам.


<a id="org9adc33b"></a>

## Кортежи

**Кортеж** представляет собой набор величин разных типов, например:

```ocaml
# (1,4.5,"OK");;
- : int * float * string = (1, 4.5, "OK")
```

Здесь имеется три величины разных типов, которые **OCaml** определил как `int*float*string`

Для кортежей длины 2 (пар) можно использовать функции **fst** и **snd**:

```ocaml
# fst ("hello","world");;
- : string = "hello"
# snd ("hello","world");; 
- : string = "world"
```


<a id="org5c1892d"></a>

## Списки

**Списком** называется набор данных одного типа.

```ocaml
# [1;2;3];;
- : int list = [1; 2; 3]
```

Первой важной операцией для работы со списком является **::**, которая позволяет добавить в список очередной элемент, или построить список из нескольких элементов:

```ocaml
# 0::[1;2;3;4];;
- : int list = [0; 1; 2; 3; 4]
# 1::2::3::4::5::[];;
- : int list = [1; 2; 3; 4; 5]
```

Вторая операция **@** объединяет несколько списков в один:

```ocaml
# [1;2;3]@[4;5;6];;
- : int list = [1; 2; 3; 4; 5; 6]
```


<a id="org7e94d04"></a>

## Массивы

**Массивом** называется разновидность списка фиксированного размера

```ocaml
# [|1;2;3|];;
- : int array = [|1; 2; 3|]
```

Для массивов определён специальный синтаксис, позволяющий обращаться по индексу к его элементам:

```ocaml
# [|1;2;3|].(0);;
- : int = 1
```

Индексация, как и во многих других языках, производится, начиная с нуля.

Следует заметить, что строки в **OCaml** тоже представляют собой разновидность массивов (как в Си), так что допускается ображение к отдельным символам по индексам:

```ocaml
# "hello".[0];;
- : char = 'h'
```


<a id="orgbbd2d35"></a>

# Конструкция **let**

С помощью конструкции **let** можно задавать имена различным экземплярам данных, а также объявлять функции.

Создадим именованную константу:

```ocaml
# let pi=3.14;;
val pi : float = 3.14
```

В дальнейшем можно использовать имена для построения других выражений:

```ocaml
# let pi2=2. *. pi;;
val pi2 : float = 6.28
```

Приведённые примеры использования **let** задают **глобальные объявления**, которые будут действовать на всём протяжении программы.

Одной из форм использования **let** является группировка глобальных объявлений:

```ocaml
# let a=1 and b=2 and c=3;;
val a : int = 1
val b : int = 2
val c : int = 3
```

**Локальное объявление** создаёт связь между именем и значеним в определённой области кода:

```ocaml
# let a=2 in a*a;;
- : int = 4
```

Аналогично можно использовать группировку при локальных обхявлениях:

```ocaml
# let a=2 and b=3 in a*b;;
- : int = 6
```


<a id="orge40b26a"></a>

## Пример с проверкой числа на простоту

Рассмотрим пример функции, которая получает целое число на вход и проверяет, является ли переданное число простым (не имеет делителей, кроме себя и 1).

Первая реализация:

```ocaml
# let is_prime n =
    let n = abs n in
    let rec is_not_divisor d =
      d * d > n || (n mod d <> 0 && is_not_divisor (d+1)) in
    n <> 1 && is_not_divisor 2;;
```


<a id="orgf758f0a"></a>

# Работа с source-файлами

Режим **REPL** - не единственный режим работы **OCaml**. Можно подготовить файл с исходным кодом и загрузить его в интерпретатор, а можно скомпилировать, причём компиляция осуществляется либо в бай-код, либо в машинный код.

Рассмотрим, как можно загрузить файл в интерпретатор и выполнить его.

```ocaml
(* Содержимое hello.ml *)
Printf.printf "Hello, world!\n" 
```

Далее, мы загружаем подготовленный файл из интерпретатора:

```ocaml
# #use "путь/hello.ml";;
Hello, world!
- : unit = ()
```

При использовании команды **use** нужно указать либо абсолютный либо относительный путь к файлу с исходным кодом.


<a id="org23fa391"></a>

# Список источников


<a id="org0f66eb5"></a>

## Ссылки

-   Главный сайт: <https://ocaml.org>.
-   PLEAC: <http://pleac.sourceforge.net/pleac_ocaml/>
-   Try OCaml: <http://try.ocamlpro.com/>
-   Real world OCaml book: <http://dev.realworldocaml.org>
-   Awesome OCaml: <https://github.com/ocaml-community/awesome-ocaml>


<a id="orgddd8191"></a>

## Книги

-   *Hickey J.* Introduction to Objective Caml, 2008.
-   *Harrop J.* OCaml for scientists, 2005.
-   *Chailloux E., Manoury P., Pagano B.* Developing applications with Objective Caml, O'Reilly, 2000.
-   *Chailloux E., Manoury P., Pagano B.* Разработка программ с помощью Objective Caml, (русский перевод), 2007.
-   *Smith J.* Practical OCaml, APress, 2006.
-   *Downey A., Monje N.* Think OCaml, Green Tea Press, 2008.
-   *Whitington J.* OCaml from the Very Beginning, Coherent press. Cambridge, 2013.
-   *Whitington J.* More OCaml: Algorithms, Methods, and Diversions, Coherent press. Cambridge, 2014.
-   *Leroy X., Remy D.* Unix system programming in OCaml, 2013.
-   *Minsky Y., Madhavapeddy A., Hickey J.* Real world OCaml, O'Reilly, 2013.
-   *Didier Rémy* Using, Understanding, and Unraveling OCaml, 2001.