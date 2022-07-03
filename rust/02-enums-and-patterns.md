# Enumerations and Patterns

So you want to enumerate some variants?

## Intro

* Variants are namespaced under an identifier, delimited with `::`
* Not your average enums, we can store data!
    * Not just limited to basic data types, can use your own structs, and other enums
* Useful for grouping what would otherwise be a set of related but distinct small structs
* Can have functions defined in an implementation block (`impl`)
 
```rust
enum Message {
    Quit,                       // Unit-like struct enum
    Move { x: i32, u: i32 },    // Struct-like enum
    Write(String),              // Tuple-like enum (single value)
    Change(i32, i32, i32),      // Tuple-like enum (multiple values)
}

impl Message {
    fn call(&self) {
        ...
    }
}

let m = Message::Write(String::from("hoot"));
m.call();
```

## Useful std library enums

### Option

* Encodes the concept of having something or nothing
* Rust has no null value, so this enum (and its variants `Some<T>` and `None`) are so useful they are included in the prelude
* `<T>` is the generic type parameter and effectively means `Some<T>` can hold a piece of data of any type
* `Option<T>` is not the same type as `T`, forcing programmers to explicitly handle scenarios where a value may be null
   * The side effect is you can always trust a value of type `T` cannot be null 

```rust
let string_or_not = Some("hoot");      // type is Option<&str>
let number_or_not: Option<i32> = None; // type is explicit as compiler cannot infer a type from Option::None
```

## Match construct

* Compare a value against a series of value patterns, and execute code based on the matched pattern
* Unlike an if which requires a boolean, the condition expression can return any type
* `match` is broken into "arms"
   * An arm is made up of a pattern, and the expression that is executed for that pattern (separated by `=>`) 
   * The matched expression is the return value of the `match` expression
* Patterns can bind to values, and be used to extract values from enums
   * Useful for `Option<T>`
* The first matching arm is returned
* Matching must be exhaustive
   * The compiler will ensure all possible cases are handled


```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
   match x {
      None => None,
      Some(i) => Some(i + 1),
   }
}
```

* An arbitrary variable name will act as a catch-all
   * `_` is a special catch-all pattern that instructs the compiler to match and throw away the value
   * If you want nothing to happen for a pattern, you can use the unit tuple `()`

```rust {
match y {
   ...
   something => do(something),
```

```rust
match z {
   ...
   _ => (),
}
```

* `if let` offers a shorthand to cover scenarios where you want `match` to do something for one pattern and ignore all other patterns

```rust
if let Some(val) = z {
   println!("z is {}", val);
}
```
