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
