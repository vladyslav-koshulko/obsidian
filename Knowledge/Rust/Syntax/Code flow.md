#rust #syntax #code-flow
### If statments

```
if x == 0 {...} else if x < 100 {...} else {...}
```
```
if x == 0 {
	...
} else if x < 100 {
	...
} else {
	...
}
```
```
let size = if x < 20 {...} else {...};
```

**Оскільки `if` є виразом і повинен мати певний тип, обидва його блоки розгалужень повинні мати той самий тип.**


### Cycles
У Rust є три ключові слова циклу: `while`, `loop` і `for`:

#### while
```
while x > 10 {
	...
}
```
#### for
```
for x in 1..5 {...} # from 1 to 4 included
for x in 1..=5 {...} # from 1 to 5 included
for x in [1,2,3,4,5] # from 1 to 5 included
```

Під капотом циклів `for` використовується концепція, яка називається “ітератори”, для обробки ітерацій над різними типами діапазонів/колекцій.
Зверніть увагу, що перший цикл `for` виконує ітерацію тільки до `4`. Cинтаксис `1..=5` для включеного діапазону.

#### loop
Оператор [`loop`](https://doc.rust-lang.org/std/keyword.loop.html) просто повторюється до нескінченності, поки не трапиться `break`.
```
let mut i = 0;
loop {
	...
	if i > 100 {
		break;
	}
}
```
The `loop` statement works like a `while true` loop. Use it for things like servers which will serve connections forever.

#### Мітки
І `continue`, і `break` можуть додатково приймати аргумент мітки, який використовується для виходу з вкладених циклів:
```
`outer: for i in 0..=2 {
	...
	if i == 2 {
		break `outer;
	}
}
```
Позначене переривання також працює на довільних блоках, наприклад
```
'label: { 
	break 'label;
	println!("Цей рядок пропускається"); 
}
```