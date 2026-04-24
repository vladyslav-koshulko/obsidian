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
# Let Expressions
# Applying Functions
# Modules
# Type Annotations
# Type Aliases
# Custom Types
# JavaScript Interop




# Resources
- [Elm Syntax](https://elm-lang.org/docs/syntax)
# Связи
- [[Elm - Resources]]
- [[Elm - The Elm Architecture]]
- [[Elm - Components]]
- [[Elm - Installation]]
- [[Elm - Overview]]
- [[Functional Programming - Operators]]
- [[Functional Programming - Overview]]