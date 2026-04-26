#elm #syntax
# Comments
```
-- a single line comment

{- a multiline comment
	{- can be nested -}
-}
```

Вот полезный приём, который должен знать каждый Elm-программист:

```
{--}
add x y = x + y
--}
```

Просто добавьте или удалите символ `}`в первой строке, и вы сможете переключаться между комментированным и некомментированным вариантом!


# Literals
```
True : Bool
False : Bool

42 : number -- Int or Float depending on usage
3.14 : Float

'a' : Char
"abc" : String

-- multi-line String
"""
Useful for holding JSON or other content that has "quotation marks".
"""
```
# Lists
```
[1,2,3,4]
1 :: [2,3,4]
1 :: 2 :: 3 :: 4 :: []
```
[[Functional Programming - Operators]]

# Conditionals
```
if powerLevel > 9000 then "abs" else "meh"
```
```
if key == 40 then
	n + 1
else if key == 38 then
	n -n
else
	n
```
Также можно задать условное поведение в зависимости от структуры пользовательских типов и литералов:
```
case maybeList of
	Just xs -> xs
	Nothing -> []
	
case xs of
	[] -> 
		Nothing
	first :: rest ->
		Just (first, rest)

case n of 
	0 -> 1
	1 -> 1
	_ -> fib (n-1) + fib (n-2)
```
Каждый элемент дизайна чувствителен к отступам, а это значит, что все элементы необходимо выровнять.
# Records
Для более подробного объяснения системы учета Elm см. [этот обзор](https://elm-lang.org/docs/records "Записи в Элме") , [первоначальное объявление](https://elm-lang.org/news/0.7 "Elm версия 0.7") или [эту научную статью](https://research.microsoft.com/pubs/65409/scopedlabels.pdf "Расширяемые записи с метками, имеющими область видимости.") .
```
-- create records
origin = { x = 0, y = 0 }
point = { x = 3, y = 4 }

-- access fields
origin.x == 0
point.x == 3

-- field access function
List.map .x [ origin, point ] = [ 0, 3 ]

-- update a field
{ point | x = 6 } == { x = 6, y = 4 }

-- update many fields
{ point | x = point.x + 1, y = point.y + 1 }
```

## Explanation
**Record** — это структура данных с именованными полями (как объект в JS).
```
origin = { x = 0, y = 0 }  
point  = { x = 3, y = 4 }
```

👉 аналог в JS:

```
{ x: 0, y: 0 }
```
### 🔹 Доступ к полям

origin.x == 0  
point.x == 3

 📌 Как читается:
- `origin.x` → “возьми поле `x` из `origin`”
- `== 0` → “и проверь, равно ли это 0”

👉 полностью:

> взять `x` у `origin` и проверить, что это 0

### 🔹 Функция доступа к полю

```
List.map .x [ origin, point ] == [ 0, 3 ]
```

📌 `.x` — это **функция**, которая:

```
.x record = record.x
```

👉 значит:

- берём список записей
- достаём поле `x` из каждой


 📌 Как читается

- `List.map` → “пройтись по списку”
- `.x` → “взять поле x”
- `[ origin, point ]` → список из двух записей

👉 полностью:

> пройтись по списку и взять x из каждого элемента

### 🔹 Обновление поля (immutability)

```
{ point | x = 6 } == { x = 6, y = 4 }
```

⚠️ важно:

- **оригинальный `point` не меняется**
- создаётся **новая запись**

👉 было:

```
{ x = 3, y = 4 }
```

👉 стало:

```
{ x = 6, y = 4 }
```

### Как читается

- `{ point | x = 6 }` → “создай новую запись на основе `point`, но поменяй `x` на 6”
- `==` → сравнение

👉 полностью:

> взять point, изменить x на 6 и проверить, что получилось { x = 6, y = 4 }

### ⚡ Ключевые идеи

- Records = **структурированные данные**
- доступ: `record.field`
- `.field` = функция
- обновление через `{ record | ... }`
- **всегда immutable (без мутаций)**
### 🚀 Практика (где используется)

- модели состояния (Model в Elm Architecture)
- данные API
- UI state
# Functions
```
square n =
	n^2

hypotenuse a b =
	sqrt (square a + square b)
	
distance (a,b) (x,y) =
	hypotenuse (a - x) (b - y)
```
**Anonymous functions**
```
square =
	\n -> n^2

squares =
	List.map (\n -> n^2) (List.range 1 100)
```
## Explanation
### 🔹 1.

```
square =  
  \n -> n^2
```

#### 📌 Как читается

> square — это функция, которая берёт `n` и возвращает `n^2`

👉 это **анонимная функция**, присвоенная имени

То же самое можно записать проще:
```

square n =  
  n^2
```

---

### 🔹 2.
```

squares =  
  List.map (\n -> n^2) (List.range 1 100)
```

#### 📌 Как читается

- `List.range 1 100` → список от 1 до 100
- `\n -> n^2` → функция “возвести в квадрат”
- `List.map` → применить функцию к каждому элементу

👉 полностью:

> взять числа от 1 до 100 и возвести каждое в квадрат

---

### ⚙️ Что происходит
```

List.range 1 5  
-- [1,2,3,4,5]  
  
List.map (\n -> n^2) ...  
-- [1,4,9,16,25]
```
# Operators
В дополнение к обычным математическим операциям сложения и вычитания, у нас есть операторы [`(<|)`](https://package.elm-lang.org/packages/elm/core/latest/Basics#%3C|) и [`(|>)`](https://package.elm-lang.org/packages/elm/core/latest/Basics#|). Они являются псевдонимами для применения функций, что позволяет использовать меньше скобок.
```
viewNames1 names = 
	String.join ", " (List.sort names) 
	
viewNames2 names = 
	names 
		|> List.sort 
		|> String.join ", " 

-- (arg |> func) is the same as (func arg) 
-- Just keep repeating that transformation!
```
Историческая справка: это заимствовано из F#, вдохновлено конвейерами Unix.

В связи с этим, [`(<<)`](https://package.elm-lang.org/packages/elm/core/latest/Basics#%3C%3C) и [`(>>)`](https://package.elm-lang.org/packages/elm/core/latest/Basics#%3E%3E) являются операторами композиции функций.
## Explanation
### 🔹 1.
```
viewNames1 names =   
    String.join ", " (List.sort names)
```

#### 📌 Как читается

> отсортировать `names`, затем соединить их в строку через `", "`

---

#### ⚙️ По шагам

```
List.sort names
```

👉 сортирует список строк

```
String.join ", " ...
```

👉 соединяет список в одну строку

---

#### 🔄 Пример

```
names = ["Bob", "Alice", "Charlie"]
```

👉 шаги:

```
List.sort names  
-- ["Alice","Bob","Charlie"]  
  
String.join ", " ...  
-- "Alice, Bob, Charlie"
```

### 🔹 2.

viewNames2 names =   
    names   
        |> List.sort   
        |> String.join ", " 

#### 📌 Как читается

> взять `names`, передать в `List.sort`, потом результат передать в `String.join`

---

#### 🔧 Оператор `|>`

```
a |> f
```

👉 это:

```
f a
```

---

#### ⚙️ Перепишем вручную

```
names   
|> List.sort   
|> String.join ", "

```
👉 превращается в:

```
String.join ", " (List.sort names)
```

---

### ⚡ Разница

#### ❌ Первый вариант

```
String.join ", " (List.sort names)
```

- вложенность
- читается “изнутри наружу”

---

#### ✅ Второй вариант

```
names   
|> List.sort   
|> String.join ", "
```

- читается сверху вниз
- как pipeline (конвейер)

---

#### 🧠 Интуиция

```
данные → обработка → результат
```


#### 🔥 Почему второй вариант важен

В реальных проектах:

```
data  
  |> step1  
  |> step2  
  |> step3  
  |> step4
```

👉 вместо:

```
step4 (step3 (step2 (step1 data)))
```

---

#### ⚡ Вывод

- оба варианта одинаковы
- `|>` делает код:
    - читаемее
    - линейным
    - ближе к “потоку данных”
---
# Let Expressions

`let`Эти значения определяются `in`данным конкретным выражением.

```
  let
    twentyFour =
      3 * 8

    sixteen =
      4 ^ 2
  in
  twentyFour + sixteen
```

Это полезно, когда выражение становится большим. Вы можете разбить `let`его на более мелкие определения и объединить их все `in`в более короткое выражение.

В выражениях let также можно определять функции и использовать «деструктурирующее присваивание».

```
  let
    ( three, four ) =
      ( 3, 4 )

    hypotenuse a b =
      sqrt (a^2 + b^2)
  in
  hypotenuse three four
```

Выражения типа Let чувствительны к отступам, поэтому каждое определение должно выравниваться по отношению к предыдущему.

Наконец, вы можете добавлять аннотации типов в let-выражения.

```
  let
    name : String
    name =
      "Hermann"

    increment : Int -> Int
    increment n =
      n + 1
  in
  increment 10
```

Лучше всего делать это только с _конкретными_ типами. Разбивайте обобщенные функции на отдельные определения верхнего уровня.

## Explanation
`let ... in` — это способ **создать временные переменные (или функции)** внутри выражения.

---

### 📌 Общий вид

```
let  
    имя = значение  
in  
    результат
```

👉 читается:

> пусть `имя = значение`, и используй это в `результате`

---

### 🔧 Простой пример

```
sum =  
    let  
        a = 5  
        b = 3  
    in  
    a + b
```

👉 результат:

```
8
```

---

### ⚙️ Что происходит

1. создаются локальные переменные:

```
a = 5  
b = 3
```

2. они используются:

```
a + b
```

---

### 🔹 Пример ближе к реальности

```
view names =  
    let  
        sorted = List.sort names  
        result = String.join ", " sorted  
    in  
    result
```

👉 читается:

> отсортируй → сохрани → склей → верни

---

### 🔄 Без `let`

```
view names =  
    String.join ", " (List.sort names)
```

👉 короче, но:

- хуже читается
- нет промежуточных имён

---

### ⚡ Когда использовать `let`

Используй, если:

- есть **несколько шагов**
- хочешь **понятные имена**
- повторно используешь значение

---

### 🧠 Интуиция

```
let = "сначала подготовь данные"  
in  = "потом используй"
```

### 🔥 Важные свойства

#### 1. Локальная область видимости

Переменные живут только внутри `let`

---

#### 2. Можно объявлять функции

```
let  
    square n = n * n  
in  
square 5
```

---

#### 3. Это выражение (а не блок как в JS)

👉 всегда возвращает значение

---

### ⚡ Главное отличие от JS

В TypeScript:

```
const a = 5;  
const b = 3;  
return a + b;
```

👉 это **инструкции**

---

В Elm:

```
let  
  a = 5  
  b = 3  
in  
a + b
```

👉 это **одно выражение**

---

### 🚀 Вывод

`let ... in` — это:

- способ разбить сложное выражение
- дать имена промежуточным шагам
- сделать код читаемым
# Applying Functions

```
-- alias for appending lists and two lists
append xs ys = xs ++ ys
xs = [1,2,3]
ys = [4,5,6]

-- All of the following expressions are equivalent:
a1 = append xs ys
a2 = xs ++ ys

b2 = (++) xs ys

c1 = (append xs) ys
c2 = ((++) xs) ys
```

Основные арифметические инфиксные операторы автоматически определяют свой тип.

```
23 + 19   : number
2.0 + 1   : Float

6 * 7     : number
10 * 4.2  : Float

100 // 2  : Int
1 / 2     : Float
```
## Explanation

**в Elm операторы — это обычные функции**, и функции можно вызывать по частям.

---

### 🔹 База

```
append xs ys = xs ++ ys
```

👉 ты создал функцию `append`, которая просто использует оператор `++` (склейка списков)

```
xs = [1,2,3]  
ys = [4,5,6]
```

---

### 🔹 1. Обычный вызов

```
a1 = append xs ys  
a2 = xs ++ ys
```

#### 📌 Как читается

- `append xs ys` → вызвать функцию с двумя аргументами
- `xs ++ ys` → тот же результат, но через оператор

👉 это **абсолютно одно и то же**

---

### 🔹 2. Оператор как функция

```
b2 = (++) xs ys
```

#### 📌 Как читается

> взять функцию `++` и передать ей `xs` и `ys`

👉 важно:

```
(++)
```

— это **сама функция**, а не оператор

---

#### ⚙️ Это то же самое, что:

```
append xs ys
```

---

### 🔹 3. Частичное применение (partial application)

```
c1 = (append xs) ys  
c2 = ((++) xs) ys
```

#### 📌 Как читается

#### шаг 1:

```
append xs
```

👉 это НЕ результат, а **новая функция**

```
append xs = \ys -> xs ++ ys
```

---

#### шаг 2:

```
(append xs) ys
```

👉 применяем вторую часть

---

### ⚙️ То же самое для `++`

```
(++) xs = \ys -> xs ++ ys
```

---

### 🧠 Главное понимание

В Elm:

```
любая функция = функция одного аргумента,  
которая возвращает функцию
```

---

### 🔥 В реальности

```
append xs ys
```

на самом деле:

```
((append xs) ys)
```

---


# Modules
```
module MyModule exposing (..)

-- qualified imports
import List                            -- List.map, List.foldl
import List as L                       -- L.map, L.foldl

-- open imports
import List exposing (..)              -- map, foldl, concat, ...
import List exposing ( map, foldl )    -- map, foldl

import Maybe exposing ( Maybe )        -- Maybe
import Maybe exposing ( Maybe(..) )    -- Maybe, Just, Nothing
```

Предпочтительнее использовать корректный импорт. Названия модулей должны совпадать с именами файлов, поэтому модуль `Parser.Utils`должен находиться в файле `Parser/Utils.elm`.

# Type Annotations
```
answer : Int
answer =
  42

factorial : Int -> Int
factorial n =
  List.product (List.range 1 n)

distance : { x : Float, y : Float } -> Float
distance {x,y} =
  sqrt (x^2 + y^2)
```

Узнайте, как читать типы и использовать аннотации типов [здесь](https://guide.elm-lang.org/types/reading_types.html) .
# Type Aliases
```
type alias Name = String
type alias Age = Int

info : (Name,Age)
info =
  ("Steve", 28)

type alias Point = { x:Float, y:Float }

origin : Point
origin =
  { x = 0, y = 0 }
```

Подробнее об псевдонимах типов можно узнать [здесь](https://guide.elm-lang.org/types/type_aliases.html) .
# Custom Types
```
type User
  = Regular String Int
  | Visitor String
```

Не уверены, что это значит? Прочитайте [это](https://guide.elm-lang.org/types/custom_types.html) !
# JavaScript Interop
```
-- incoming values
port prices : (Float -> msg) -> Sub msg

-- outgoing values
port time : Float -> Cmd msg
```

Из JavaScript вы взаимодействуете с этими портами следующим образом:

```
var app = Elm.Example.init();

app.ports.prices.send(42);
app.ports.prices.send(13);

app.ports.time.subscribe(callback);
app.ports.time.unsubscribe(callback);
```

Подробнее о взаимодействии с JavaScript можно прочитать [здесь](https://guide.elm-lang.org/interop/) .



# Resources
- [Elm Syntax](https://elm-lang.org/docs/syntax)
# Связи
- [[Elm - Resources]]
- [[Elm - The Elm Architecture]]
- [[Elm - Components]]
- [[Elm - Installation]]
- [[Elm - Overview]]
- [[Elm - Operators]]
- [[Functional Programming - Operators]]
- [[Functional Programming - Overview]]