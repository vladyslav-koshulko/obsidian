Функциональное программирование (ФП) — эт==о парадигма, рассматривающая вычисления как вычисление математических функций, избегая изменения состояния и изменяемых данных==. Основной фокус на использовании «чистых» функций, которые всегда возвращают одинаковый результат для одних и тех же аргументов, обеспечивая предсказуемость и легкое тестирование кода.

# **Основные принципы ФП:**

- **[Чистые функции](https://www.google.com/search?q=%D0%A7%D0%B8%D1%81%D1%82%D1%8B%D0%B5+%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8&sca_esv=aad7451a74ba4d56&biw=925&bih=932&sxsrf=ANbL-n6Wbitnmm-_BCp0jY-A0-DtC-BhtA%3A1777027608248&ei=GErraY_nDoeji-gPq4f3qAY&oq=%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%BE%D0%BD%D0%B0%D0%BB%D1%8C%D0%BD%D0%BE%D0%B5+&gs_lp=Egxnd3Mtd2l6LXNlcnAiHdGE0YPQvdC60YbQuNC-0L3QsNC70YzQvdC-0LUgKgIIADIFEAAYgAQyBRAAGIAEMgUQABiABDIFEAAYgAQyBRAAGIAEMgUQABiABDIFEAAYgAQyBRAAGIAEMgUQABiABDIFEAAYgARIlS1QvQhY0yFwA3gBkAEAmAGmBaABkRmqAQoxLjExLjEuNS0yuAEDyAEA-AEBmAISoALcGcICChAAGEcY1gQYsAPCAgsQLhiABBjHARjRA8ICBRAuGIAEwgIKECMYgAQYigUYJ8ICBBAjGCfCAgoQABiABBiKBRhDmAMAiAYBkAYEkgcMNC4xMC4yLjAuMS4xoAeofrIHDDEuMTAuMi4wLjEuMbgH0RnCBwYwLjExLjfIBzeACAE&sclient=gws-wiz-serp&ved=2ahUKEwiv1tGdqoaUAxXwQ_EDHeSrExkQgK4QegYIAQgAEAk) (Pure Functions):** Отсутствие побочных эффектов (side effects). Функция не меняет переменные вне своей области видимости, а только возвращает значение.
- **[Неизменяемость](https://www.google.com/search?q=%D0%9D%D0%B5%D0%B8%D0%B7%D0%BC%D0%B5%D0%BD%D1%8F%D0%B5%D0%BC%D0%BE%D1%81%D1%82%D1%8C&sca_esv=aad7451a74ba4d56&biw=925&bih=932&sxsrf=ANbL-n6Wbitnmm-_BCp0jY-A0-DtC-BhtA%3A1777027608248&ei=GErraY_nDoeji-gPq4f3qAY&oq=%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%BE%D0%BD%D0%B0%D0%BB%D1%8C%D0%BD%D0%BE%D0%B5+&gs_lp=Egxnd3Mtd2l6LXNlcnAiHdGE0YPQvdC60YbQuNC-0L3QsNC70YzQvdC-0LUgKgIIADIFEAAYgAQyBRAAGIAEMgUQABiABDIFEAAYgAQyBRAAGIAEMgUQABiABDIFEAAYgAQyBRAAGIAEMgUQABiABDIFEAAYgARIlS1QvQhY0yFwA3gBkAEAmAGmBaABkRmqAQoxLjExLjEuNS0yuAEDyAEA-AEBmAISoALcGcICChAAGEcY1gQYsAPCAgsQLhiABBjHARjRA8ICBRAuGIAEwgIKECMYgAQYigUYJ8ICBBAjGCfCAgoQABiABBiKBRhDmAMAiAYBkAYEkgcMNC4xMC4yLjAuMS4xoAeofrIHDDEuMTAuMi4wLjEuMbgH0RnCBwYwLjExLjfIBzeACAE&sclient=gws-wiz-serp&ved=2ahUKEwiv1tGdqoaUAxXwQ_EDHeSrExkQgK4QegYIAQgAEAs) (Immutability):** Данные не меняются после создания. Вместо изменения объекта создается новый с обновленными данными.
- **Функции высшего порядка (Higher-Order Functions):** Функции, принимающие или возвращающие другие функции.
- **Декларативный стиль:** Описание того, _что_ нужно сделать, а не _как_ (в отличие от императивного подхода).
- **Ссылочная прозрачность:** Вызов функции можно заменить её значением без изменения работы программы.

# Концепции
## Каррирование (Currying)
Трансформация функции от нескольких аргументов в цепочку функций, каждая из который принимает ровно один аргумент.

**Как работает:** Вместо f(a, b, c) мы вызываем f(a) (b) (c).
**Зачем нужно:** Позволяет делать "Частичное применение". Например можно создать функцию add(x, y), зафиксировать первый аргумент как 5 и получить новую специальную функцию add_five(y).

## Монады (Monads)
В программировании монанды это просто контейнер для значения, который умеет применять к этому значению функции, соблюдая определенные правила.

Монада это как "умная коробка":
1. Есть значение в коробке (например, число или результат запроса к базе).
2. Вы не вынимаете значение вручную. Вместо этого вы передаете коробке функцию: "Эй, примени это к тому что внутри".
3. Коробка сама обрабатывает логику (например если внутри пусто или ошибка, она просто не применет функцию и не "упадет").
**Зачем монады нужны:** Монады позволяет обрабатывать побочные эффекты (ошибки, ввод-вывод, состояние), оставаясь в рамках "чистых" функций. Самый известный пример - монада Maybe (или Optional), которая избавляет от проверок на null на каждом шагу.

## Функции высшего порядка (High-Order Function)
Эти функции, которые принимают другие функции в качестве аргументов или возвращяют их. Это основа для композиции программ.
### Реализации (JavaScript)
```
const multiply = (factor) => (num) => num * factor;
const double = multiply(2); // Возвращяет новую функцию

console.log(double(10)) // 20
```

## Рекурсии
В чистом ФП нет циклов (for, while). Вместо них используеться рекурсия - когда функция вызывает саму себя. Что бы не переполнять стек, используеться хвостовая рекурсия.
### Реализация (Haskell / Псевдокод):
```
factorial n = if n == 0 then 1 else n * factorial (n - 1)
```

## Композиция функций (Function Composition)
Процесс обьеденения двух и более фукнций для создания новой. Результат одной функции становиться входным данным для другой.
### Реализация (JavaScript)
```
const compose = (f, g) => (x) => f(g(x));

const toUpper = s => s.toUpperCase();
const exclaim = s => s + '!';

const shout = compose(exclaim, toUpper);
console.log(shout("hello")); // "Hello!"
```

## Ссылочная прозрачность (Referential Transparency)
Свойство при котором вызов функции можно заменить ее резальтутом без измениния поведения программы. Это возможно только при отсутствии побочный эффектов.
### Пример (Python)
```
# Эта функция ссылочно прозрачна:
def add(a, b): return a + b

# Вызов add(2, 3) всегда можно заменить числом 5 во всем коде.
```

## Ленивые вычисления (Lazy Evaluation)
Вычисления откладываються до тех пор пока их результат действительно не понадобиться. Это позволяет работать с бесконечными структурами данных.

### Реализация (Python - генераторы)
```
def infinite_sequence():
	num = 0;
	while True:
		yield num
		num += 1

gen infinite_sequence()
print(next(gen)) # 0
print(next(gen)) # 1
```

## Сопоствление с образцом (Pattern Matching)
Это продвинутый switch/case, который позволяет проверять структуру данных и сразу извлекать из нее значения.

### Реализация (Rust / Elixir style)
```
match message {
	Quit => println!("Exit"),
	Move {x, y} => println("Movement in {}, {}", x, y),
	Write(text) => println!("Text: {}", text),
}
```
# **Преимущества ФП:**

1. **Легкость отладки:** Отсутствие побочных эффектов позволяет тестировать функции изолированно.
2. **Параллелизация:** Неизменяемые данные исключают конфликты при одновременном доступе (thread-safe).
3. **Читаемость:** Декларативный стиль делает код более понятным.

# Минусы
- Порог входа: Требует перестроки мышления и понимания математических концепций.
- Производительность: Постоянное копирование данных и работа сборщика мусора могут замедлять роботу.
- Ресурсоемкость: Потребляет больше оперативной памяти из-за неизменяемости структур.

# **Популярные языки:** 
**Чисто функциональные:**
- Haskell, Elm
**C сильным влиянием:**
- F#
- Scala
- Clojure
- Erlang
- Elixir
**Мультипарадигменные:**
- JS
- Python
- Java
- [[Rust - Overview]]

# Примеры использования

## Python
### Императивный стиль
Создаем пусой список и пошагово меняем его состояние в цикле.
```
numbers = [1,2,3,4,5]
result = []
for n in numbers:
	if n % 2 == 0:
		result.append(n ** 2)
```
### Функциональный стиль
Комбинируем функции не меняя исходные данные.
```
numbers = [1,2,3,4,5]
result = list(map(lambda x: x**2, filter(lambda x: x % 2 == 0, numbers
)))
```

# Связи
- [[Elm - Overview]]
- [[Rust - Overview]]