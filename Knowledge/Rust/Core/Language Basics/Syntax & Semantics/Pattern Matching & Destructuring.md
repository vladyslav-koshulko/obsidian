
# match
```
match VALUE {
	PATTERN => EXPRESSION,
	PATTERN => EXPRESSION,
	PATTERN => EXPRESSION,
}
```

```
match x {
	None => None,
	Some(i) => Some(i + 1),
}
```

```
match x {
	Some(i) => Some(i * 2)
	_ => None # For other variants
}
```

- должны быть учтены все возможные выражения `match`
- `_` - шабон охвата всех вариантов

# let
```
let PATTERN = EXPRESSION;
```

```
let (x, y, z) = (1,2,3);
```

Если количество элементов в шаблоне не совпадает с количеством элементов в кортеже, общий тип не будет совпадать, и мы получим ошибку компилятора.

```
let (x, y) = (1,2,3);
```

```
$ cargo run
   Compiling patterns v0.1.0 (file:///projects/patterns)
error[E0308]: mismatched types
 --> src/main.rs:2:9
  |
2 |     let (x, y) = (1, 2, 3);
  |         ^^^^^^   --------- this expression has type `({integer}, {integer}, {integer})`
  |         |
  |         expected a tuple with 3 elements, found one with 2 elements
  |
  = note: expected tuple `({integer}, {integer}, {integer})`
             found tuple `(_, _)`

For more information about this error, try `rustc --explain E0308`.
error: could not compile `patterns` (bin "patterns") due to 1 previous error

```

Чтобы исправить ошибку, мы можем игнорировать одно или несколько значений в кортеже, используя `_`или `..`, как вы увидите в разделе [«Игнорирование значений в шаблоне»](https://doc.rust-lang.org/book/ch19-03-pattern-syntax.html#ignoring-values-in-a-pattern) .

# if let 

```
fn main() {
    let favorite_color: Option<&str> = None;
    let is_tuesday = false;
    let age: Result<u8, _> = "34".parse();

    if let Some(color) = favorite_color {
        println!("Using your favorite color, {color}, as the background");
    } else if is_tuesday {
        println!("Tuesday is green day!");
    } else if let Ok(age) = age {
        if age > 30 {
            println!("Using purple as the background color");
        } else {
            println!("Using orange as the background color");
        }
    } else {
        println!("Using blue as the background color");
    }
}
```

**Недостаток использования `if let`выражений заключается в том, что компилятор не проверяет исчерпывающий список вариантов, в отличие от `match`выражений. Если бы мы пропустили последний `else`блок и, следовательно, упустили обработку некоторых случаев, компилятор не предупредил бы нас о возможной логической ошибке.**

# while let
Подобно условному циклу `if let`, `while let`условный цикл позволяет `while`циклу выполняться до тех пор, пока шаблон продолжает соответствовать заданному значению.

```
    let (tx, rx) = std::sync::mpsc::channel();
    std::thread::spawn(move || {
        for val in [1, 2, 3] {
            tx.send(val).unwrap();
        }
    });

    while let Ok(value) = rx.recv() {
        println!("{value}");
    }

```


# for
В `for`цикле значение, непосредственно следующее за ключевым словом, `for`представляет собой шаблон. Например, в `for x in y`цикле `x`шаблоном является значение.
```
    let v = vec!['a', 'b', 'c'];

    for (index, value) in v.iter().enumerate() {
        println!("{value} is at index {index}");
    }
```
Мы модифицируем итератор, используя этот `enumerate`метод, так чтобы он выдавал значение и индекс этого значения, помещенные в кортеж. Первое полученное значение — это кортеж `(0, 'a')`. Когда это значение совпадает с шаблоном `(index, value)`, индекс будет равен `0`, а значение будет равно `'a'`, что выводит первую строку результата.

# Function Parameters
Как и в случае с `let`, мы можем сопоставить кортеж в аргументах функции с этим шаблоном.
```
fn print_coordinates(&(x, y): &(i32, i32)) {
    println!("Current location: ({x}, {y})");
}

fn main() {
    let point = (3, 5);
    print_coordinates(&point);
}
```

# Next
- [[Enums]]

# Связи
- [[Variables]]
- [[Data types]]
- [[Control Flow & Constructs]]
- [[Blocks & Scopes]]
- [[Functions & Method Syntax]]