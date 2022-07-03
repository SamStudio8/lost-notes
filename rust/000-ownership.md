## Ownership

* Recall pushing and popping from the stack (often kept in CPU cache) is faster than allocating (finding space) and accessing (following pointers) from the heap
* Ownership rules cover the book keeping of data on the heap

### The rules of ownership club

* Don't talk about ownership club
* A value in Rust has an owner
* A value can only have one owner at a time
* A value whose owner is out of scope is dropped

### Three memory allocation scenarios

#### Copying (stack only copy)

```rust
let x = 8;
let y = x;
```

* `y` will have its own independent copy of the value `8`
* These ownership rules do not apply to simple data types that live on the stack (because there is no heap data to manage)
    * More specifically any type that implements the `Copy` trait will be trivially copied automatically
    * `Copy` trait is mutually exclusive and cannot be implemented on any type that implements (or has a part that implements) the `Drop` trait (which manages how heap allocations are released)
* Tuples will `Copy` iff it only contains types that also implment `Copy` 

#### Moving (not quite a shallow copy)

```rust
let s1 = String::from("hoot");
let s2 = s1;
```

* This will copy the `String` structure to another `String` on the stack -- pointing at the same contents on the heap...
* On the surface this looks like a shallow copy, but it is a move!
* `s1` becomes an invalidated reference to avoid a double free when both `s1` and `s2` go out of scope!
* Why? Because the data on the heap can only have one owner at a time
* Efficient and safe to assume this will never lead to a deep copy of data

#### Cloning (deep copy)

```rust
let s1 = String::from("hoot");
let s2 = s1.clone();
```

* An explicit true copy of both the stack and heap data
* `s1` and `s2` own their own copy of the heap data respectively, and drop will be called independently as each falls out of scope

## Borrowing and references

* Passing a value to a function will "move" or "copy" using the assignment semantics described in the ownership section 
    * That is, by default passing a variable to a function that does not return it again means the value is dropped as soon as the called function scope ends!
* Borrowing allows a function to take a reference to a value, to get around having to return anything you want to keep in scope back to the caller's scope
    * `&s1` references `s1` and is said to "refer" to the value of s1 but does not have ownership of it
    * The compiler guarantees that references will never be dangling references and will also point to a valid value (a value cannot go out of scope before any reference to it does)
    * A value can be referred to by multiple immutable references
* Like variables, references are immutable by default
    * The underlying value, the reference and the function signature must be changed to mutable to allow a reference to be used to mutate the value it is referring to 
    * If a value has been borrowed as mutable, it cannot be borrowed mutably or immutably by any other reference (preventing data races)

```rust
let mut s = String::from("hoot");
let r = &mut s;
more_hoots(r);

fn more_hoots(s: &mut String) {
    s.push_str("hoot");
}
```

* Although scope blocks can be used to break apart usage of mutable references, a reference scope terminates at the last time the reference is used
    * If you create a mutable reference, use it, and create another (and never refer to the former), this will compile OK as the usage scopes do not overlap
    * The compiler uses NLL (non-lexical lifetimes) to figure this out

## Slices

* A type, referencing (ie. no ownership) a contiguous sequence of elements in a collection
* A nice home cooked slice just like your Python used to make, `end_index` is one-off just like back home
* Form is `[i..j]`
    * If `i=0` you can use `[..j]`
    * If `j=s.len()` you can use `[i..]`
    * Yes, a whole slice is `[..]`
* For `String` the slice type is `&str`, for an array of i32 numbers the slice type is `&[i32]`

```rust
let s = String::from("hello world!");
let hello = &s[..5];
```

* Note, if slicing a String with multibyte characters, slices must occur at character boundaries
* Slices help keep the semantics of using a collection valid by applying the rules of borrowing to subsequences of the collection
    * eg. You cannot take a slice of a String, clear the String and try and use the slice later -- the reference to the slice is invalid when the source is destroyed
* Functions can take a slice (eg. `&str`) instead of a reference to the underlying type (eg. `&String`) without any loss of function
    * `String` implements the `Deref` trait such that "deref coercion" can convert `&String` to `&str`
        * Reduces the amount of explicit referencing and deferencing needed

```rust
fn my_func(s: &str) -> &str { ... }
```
