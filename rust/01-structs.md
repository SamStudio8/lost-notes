# Rust `struct`

So you want a collection of named data?

## Intro

* Data members are called fields
* Instances are created by providing concrete values for all of the fields
* Fields accessed with dot notation
* Just like regular variables `mut` is required to mutate the `struct`
  * Mutability is applied to the instance, not the field
* Creating an instance is an expression, a function can implicitly return one if it is the last thing the function does
* Structs can store references to data (ie. data owned by something else) but requires lifetimes to be defined

```rust
struct Hooter {
    active: bool,
    name: String,
    email: String,
    speed: u32,
}

let mut hoot1 = Hooter {
    active: true,
    name: String::from("Hoot McDooter"),
    email: String::from("hoot@mcdoot.example.org"),
    speed: 8,
};

hoot1.active = false;
```

## Field init shorthand

If a function's parameter names match the struct's field names, there is a convinient shorthand for initialising a new instance:

```rust
fn build_hooter(name: String, email: String, speed: u32) -> Hooter {
    Hooter {
        name,
        email,
        speed,
        active: true,
    }
}
```

## Struct update shorthand

Updating a new instance from an existing one has a convinient shorthand.
Note for fields that do not implement `Copy`, this will *move* the data from `hoot1` to `hoot2`; potentially invalidating fields in the `hoot1` struct.

```rust
let hoot2 = Hooter {
    speed: 10,
    ..hoot1
};
```

## Methods

* Methods are functions defined within the context of a struct (or enum, or trait)
* First parameter must be `self` which represents the instance the method has been called on
    * Functions without self can be defined (eg. for constructors) but are not referred to as methods, called with `::` syntax (`namespace::associated_function`)
* For structs, defined in an `impl` (implementation block), these are called "associated functions"
    * structs are allowed to have multiple implementation blocks
* Methods called on the instance with "method syntax" (dot)
    * Rust does not distinguish between calling a method on an instance or a reference (ie. there is no `.` / `->` confusion)
    * Rust acheives this with "automatic referencing and deferencing", adding `&`, `&mut` or `*`; which works because all methods know to receive `&self`
* `&self` is shorthand for `self: &Self`
* `&mut self` required to borrow the reference mutably
* Methods can have the same name as a struct field; the compiler can work out if you're accessing the field or calling a method
    * Getters are not created automatically so the convention is to name a getter with the field name 

```rust
impl Hooter {
    fn distance_in_time(&self, time: u32) -> u32 {
        self.speed * time
    }
}
```


## Other structs

### Tuple structs

* A named tuple, but fields are not named
* Useful when you need to define a type but field names would be verbose or redundant
* Otherwise behave like regular tuples (supports destructuring, 0-based dot notation)

```rust
struct Point(i32, u32);
let origin = Point(0, 0);
```

### Unit-like structs

* Useful for implementing traits without data

```rust
struct MyUnitLikeStruct;
let thing = MyUnitLikeStruct;
```

## Misc
### Printing

* `{:?}` requires `Debug` trait, which can be derived automatically by adding the "outer attribute" `#[derive(Debug)]` to your struct
  * [Docs for other derivable traits](https://doc.rust-lang.org/book/appendix-03-derivable-traits.html)
