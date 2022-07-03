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

## Option
