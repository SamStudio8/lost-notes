# Rust `struct`

So you want a collection of named data?

## Intro

* Three types of struct:
  * struct
  * tuple struct
  * unit-like struct
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
}

let mut hoot1 = Hooter {
    active: true,
    name: String::from("Hoot McDooter"),
    email: String::from("hoot@mcdoot.example.org"),
};

hoot1.active = false;
```

## Field init shorthand

If a function's parameter names match the struct's field names, there is a convinient shorthand for initialising a new instance:

```rust
fn build_hooter(name: String, email: String) -> Hooter {
    Hooter {
        name,
        email,
        active: true,
    }
}
```

## Struct update shorthand

Updating a new instance from an existing one has a convinient shorthand.
Note for fields that do not implement `Copy`, this will *move* the data from `hoot1` to `hoot2`; potentially invalidating fields in the `hoot1` struct.

```rust
let hoot2 = Hooter {
    email: String::from("hoot.mcdooter.two@example.org"),
    ..hoot1
};
```

## Tuple structs

* A named tuple, but fields are not named
* Useful when you need to define a type but field names would be verbose or redundant
* Otherwise behave like regular tuples (supports destructuring, 0-based dot notation)

```rust
struct Point(i32, u32);
let origin = Point(0, 0);
```

## Unit-like structs

* Useful for implementing traits without data

```rust
struct MyUnitLikeStruct;
let thing = MyUnitLikeStruct;
```

## Printing

* `{:?}` requires `Debug` trait, which can be derived automatically by adding the "outer attribute" `#[derive(Debug)]` to your struct
  * [Docs for other derivable traits](https://doc.rust-lang.org/book/appendix-03-derivable-traits.html)
