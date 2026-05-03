#rust #core #syntax #immutability #mutability #variable-declaration

# Declaration

**Base declaration (Immutable)**
```
let x = 5;
```
- неизменяемый
- тип выводиться автоматически

```
let x: i32 = 10;
```
- явное указание типа (полезно при неочевидности)

**Mutable var**
```
let mut x = 10;
x = 20;
```
- mut - — единственный способ разрешить изменение, без него компилятор запретит изменение и выдаст ошибку.

Пример:
```
fn main() { 
	let x = 5; 
	println!("The value of x is: {x}"); 
	x = 6; 
	println!("The value of x is: {x}"); 
}
```

```
$ cargo run 
	Compiling variables v0.1.0 (file:///projects/variables) 
error[E0384]: cannot assign twice to immutable variable `x`
 --> src/main.rs:4:5
  |
2 | let x = 5;
  |     - first assignment to `x`
3 | println!("The value of x is: {x}");
4 | x = 6;
  | ^^^^^ cannot assign twice to immutable variable
  | 
help: consider making this binding mutable
  |
2 | let mut x = 5; 
  |     +++

For more information about this error, try `rustc --explain E0384`. 
error: could not compile `variables` (bin "variables") due to 1 previous error
```

# Next
- [[Constants]]

# Связи
- [[Data types]]
- [[Functions]]