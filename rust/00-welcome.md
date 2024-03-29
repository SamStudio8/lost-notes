# Welcome
So you've finally decided to learn Rust?
Here are some things I found out from [THE BOOK](https://doc.rust-lang.org/book/) that I need to remember:

## Rust

* Rust is about empowerment
* Rust aims for memory safety and fearless concurrency
* The unofficial mascot of Rust is a crab, their name is Ferris and [there is a gallery for cute art of them](https://rustacean.net/)
* I installed it on both Windows and Linux with `rustup`
  * [Instructions for installing Rust and getting it working with VS Code](https://code.visualstudio.com/docs/languages/rust)
  * Windows debugging support needs the [MS C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)
* [Rust by Example](https://doc.rust-lang.org/rust-by-example/index.html) is like THE BOOK but without words
* `rustc --version`
* Memory is managed not by running garbage collection (overhead), nor with manual allocation (risking early or double frees, or memory leaks) but instead a set of "ownership" rules ensure that memory is "dropped" (deallocated) when values fall out of scope
    * This is one of the most fundamental and unique components of Rust and takes a bit of getting used to
    * Rust will never deep copy your data without explicit permission, so you can safely assume "copying" is inexpensive
* There is no null type (but `Option::None` is used for explicitly representing the absence of something)
* There are no exceptions: Rust forces you to handle the possibility of recoverable errors explicitly (eg. with `Option` and `Result`) or `panic!` where unrecoverable
* `match`ing an enum, binding variables to the data inside the enum, and executing an expression is an incredible common Rust idiom

## Forget me not

* `let x = ...` not `x = ...`
* `;` ends a statement
* immutable borrow: `do_something(&x)`; mutable borrow: `do_something(&mut x)`
* `fn my_func(param: type) { ... }`
* `my_func(&self, ...` not `my_func(self, ...`
* `// i am a comment`
* `true` not `True`

## Fixed size Data Types

### Overview

* The four scalars
  * integers
    * i/u8, 16, 32, 64, 128, i/usize (arch dependent)
    * allows `100_000` visual sep
    * dec, `0x` hex, `0o` oct, `0b` bin, b'A' byte
    * `i32` is default, `isize` useful for collection iterators
    * integer overflow with `--release` will just wrap
  * IEEE-754 float
    * f32, f64 (default)
  * bools
    * true, false
    * 1 byte
  * char
    * "unicode scalar value" 
    * 4 byte
* Two "compound" types
  * tuple
  * array

### Tuples

```rust
let my_tuple = (1, 2.2, '3', "four");
println!("This is a tuple: {:?}", my_tuple);
println!("This is a tuple member: {}", my_tuple.0);
```

Tuples can be destructured like Python:

```rust
let point = (1, 2);
let (x, y) = point; // "destructuring"
let point = (x, y);
```

#### The unit tuple

The default value for an expression that does not otherwise evaluate to something. Somewhat Rust's answer to Python's `None`?

```rust
let unit_tuple = ();
```

### Arrays

* Go on the stack (no Pythonic magic private heap nonsense here)
* Index out of bounds caught at run time and prevents invalid memory access

#### Typed

`let name: [type; n] = ...`
```rust
let my_typed_array: [i32; 5] = [1, 2, 3, 4, 5];
```

#### Default values

`let name = [value; n]`
```rust
let my_zeroed_array = [0; 10];
```

## Variables

* Variable assignment is a special case of pattern matching! `let PATTERN = EXPRESSION;`

### Immutable by default

Need to `let mut` if you plan to change the value of `x`:
```rust
let mut x = 5;
x = 6
```

### Variable shadowing

Variables can be shadowed. This is useful for changing the value of a variable during a scope block, or for changing its type (eg. a String to numeric conversion) without specifying a new variable.
```rust
let x = 5;
let x = "outer hoot";
{
    let x = "inner hoot";
}
```

### Constants

Cannot be made mutable (for some reason) and require an explicit type.
Compiler can [evaluate expressions for const](https://doc.rust-lang.org/reference/const_eval.html) which permits specifiying expressions in a human readable fashion.

```rust
const HOOT_MULTIPLIER: i32 = 8;
```

## Cooler types

### String

* `ptr`, `len` (used memory), `capacity` (allocated memory) stored on the stack
* contents stored on the heap

#### String literals vs. `String`

String literals can go on the stack because the size is known at compile time. Distinguished from a `String` which goes on the heap as the size is only known at run time. For the same reason, string literals cannot be manipulated but `String` can be mutated.

```rust
let string_literal = "hoot";
let heap_string = String::from(string_literal);
heap_string.push_str("hoot!");
```
String literals are actually slices (specifically `&str`, a type of reference) that point to a specific point of the compiled binary!

## Expressions

* Critical to understand the difference between *statements* and *expressions*
* Statements cannot return a value, expressions evaluate to a value
* Expressions do not end in `;`
* Assignments are statements, not expressions (cannot `let x = (let y = 6)`)
* Function calls, macro calls and scope blocks are expressions

```rust
let my_scope_goes_to_11: u32 = {
    let x = 10;
    x + 1
}
```

## Functions

* Returns the value of the final expression in the function block (no `return` keyword)
* Returns the unit tuple `()` if an ending expression has not been provided
* Parameters must have a type, return value must be typed
* Arguments are often referred to as "concrete values" to reduce confusion with people who interchange params and args
* Optional parameters are not a thing (but there is an RFC)
* Polymorphism is not implemented by just defining an alternate function definition

```rust
fn take_five() -> i32 {
    5
}

fn add_one(x: u32) -> u32 {
    x + 1  // ; notably absent
}
```

## Branching

### if

* An expression starting with the `if` keyword (eg: `if number < 5 { ... }`)
  * If the expression in any arm is not `()`, all arms must return the same type
* Must evaluate to a `bool` (no magic implicit `if variable` nonsense)
* Blocks may be referred to as "arms"
* `if`, `else if` and `else`
* The `match` construct is nicer than a lot of `else if`

```rust
let my_var = if number % 2 == 0 { "hoot" } else { "noot" };
```

## Looping

* Three forms of loop:
  * `loop`
    * on the surface just a `while true` but importantly, is an expression!
    * `break` can return a value!
  * `while`
    * shorthand for `loop` with an `if`, `break` 
    * `while number < 10 { ... }`
  * `for`
    * `for element in collection { ... }`
* `continue` works as one would expect

A `loop` is an expression that can `break` with a value!

```rust
let x = loop {
    [...]
    break 2
}
```

All loops can be named to help disambiguate nested loops.
Note the syntax is specific -- a leading single quote `'`, a name and `:` -- this is not a String!

```rust
'my_named_loop: while number < 10 {
    loop {
        break 'my_named_loop
    }
}
```

## Conventions

* The top level scope is called the "prelude" and includes the most useful symbols, macros (eg. `println!`) and enums (`Option`)
* Surrounding braces are not used in a `match` if the matching expression is short
* Blocks inside an `if` and especially `match` are referred to as "arms"

### Naming stuff

```rust
const UPPERCASE_WITH_UNDERSCORES: type = "hello";
let variable_with_snakecase = "hoot";
```

## Basics

### Printing stuff

* `println!` is a macro
* `{}` requires `Display` trait
* `{:?}` requires `Debug` trait

```rust
println!("Hello world!");
println!("Number is {number}");
println!("Number is {}", number);
println!("Debug print is {:?}", my_tuple);
```

### Debug macro

* `dbg!` macro takes ownership of an expression, prints the location and value of the macro and returns the value of the expression

### Iterate a string as bytes

```rust
let s = String::from("hoot");
let bytes = s.as_bytes();
for (i, &item) in bytes.iter().enumerate() { ... }
```

### Cargo

```
cargo new my_package
cargo build
cargo run # build and run
cargo update # update deps
cargo doc --open # generate and provide a link to open doc for crate in browser
```
