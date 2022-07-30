# Collections

* Stored on the heap so fill your boots
* Read about other collections https://doc.rust-lang.org/std/collections/index.html

## `Vec<T>` "Vector" (list)

* Stored contiguously
* Generic: can hold any type, but only one type
  * [Can get around this in some ways by using Enum](https://doc.rust-lang.org/book/ch08-01-vectors.html#using-an-enum-to-store-multiple-types)
* Contents cleaned up when dropped from scope
* Borrow checker enforces references to individual elements and the entire vector at the same time
   * Pushing to a vector will invalidate any references to borrowed elements

```rust
// create
let v: Vec<i32> = Vec::new(); // type declaration needed for empty vector
let v = vec![1,2,3]; // compiler can infer type from initial values
```

```rust
// append
v.push(1);
```

```rust
// get by index access, panics on out of bounds
let third: &i32 = &v[2];
```

```rust
// get by index method, returns Option<&T>
match v.get(2) {
  Some(x) => println!("The element is {}", x),
  None => println!("No element!"),
}
```

```rust
// iterate and mutate a vector
for i in &mut v {
  // deref i with the deference operator *
  *i += 1;
  println!("{}", i);
}
```

* `v.contains(x)` return `bool`


## `String` ..."String" (str)

* https://doc.rust-lang.org/std/string/struct.String.html
* Defined in the standard library, not to be confused with the primitive `str` (string slice) type defined in the Rust core (e.g. reference string slices for string literals) ...or indeed with other string types (OsString, OsStr, CString, CStr) 
* Triumvirate of complexity because Rust likes errors, strings are hard, and UTF-8
  * [Why is capitalizing the first letter of a string so convoluted in Rust?
](https://stackoverflow.com/questions/38406793/why-is-capitalizing-the-first-letter-of-a-string-so-convoluted-in-rust)
* Strings are implemented as `Vec<u8>`
* Do not support indexing by integer (specifically, `Index<{integer}>` is not implemented on `String`!)
* `s.len()` counts bytes not characters, use `s.chars().count()`

```rust
// making strings
let mut s: String::new();
let s = "string literals to string mate".to_string();
let s = String::from("string from a string literal mate");
```

```rust
// append a string slice
let s2 = "hoot";
s.push_str(s2); // s does not take any ownership of s2
s.push("!"); // or push single chars
```

```rust
// cat strings
let s3 = s + &s2;
// add takes ownership of s, appends s2 and returns ownership of the result to s3
// s is invalid from this point on
// can't add two strings so we use a reference to append s2 (which gets co-erced to &str)
```

```rust
// formatting without catting
let s = format!("{}-{}-{}", s4, s5, s6);
// no ownership fiddling so s4, s5, s6 stil valid here
```

```rust
// slicing strings as a means to index substrings
let substr = &s[0..4]; // take the first four bytes of s
// will panic if slice does not start or end on a character boundary
```

```rust
// iterating strings
for c in s.chars() {
  ...
}
// iterating bytes
for c in s.bytes() {
  ...
} // recall unicode scalars can be larger than a byte
```

```rust
// get char n if you really have to
s.chars().nth(n);
// O(N) perf
```

```rust
// string length
s.chars().count()
// O(N) perf as string must be walked, still doesn't count graphemes
```

* `s.pop()` removes last **char** from `s` and returns `Option<char>`
* `s.remove(idx)` removes char starting at byte `idx` from s and returns `char` (can panic)
* `s.starts_with(&str)` returns `bool`
* `s.trim()` returns `&str` with leading and trailing whitespace removed (also `trim_start`, `trim_end`)
* `s.to_lowercase()` returns new `String` (as UTF-8 chars can expand or contract with case changes!)
* `s.split_whitespace()` returns iterator with string slices

## `HashMap<K,V>` "Hash map" (dict)

* Keys of type K, values of type V
* Not brought into scope by the prelude
  * Apparently not as well supported as some of the other collections (eg. no macros)
  * `use std::collections::HashMap;`
* Types with `Copy` trait are copied into the map
* Owned types (eg. `String`) are moved into the map, map takes ownership
* Can insert references into map (they won't be moved) but needs to be valid so long as the map is valid
* Uses `SipHash` by default (same as Py>=3.4): slightly less performant but is more robust to collision attacks

```rust
// make map
use std::collections::HashMap;
let mut map = HashMap::new();
```

```rust
// insert with ...insert
map.insert(String::from("hoot"), 1);
```

```rust
// insert with collect
let keys = vec!["a", "b", "c"];
let values = vec![1, 2, 3];
let mut map: HashMap<_,_> = keys.into_iter().zip(values.into_iter()).collect();
// hint how collect should return with <_,_> and let Rust infer the types
// keys and values are invalid here after into_iter
```

```rust
// get a map Entry or insert
map.entry(String::from("hoot")).or_insert(1);
// or_insert returns mutable ref to value (existing or new)
```

```rust
// get things out again (returns Option<&V> of course!)
let key = "a";
map.get(&key);
```

```rust
// iterate map
for (key, value) in &map {
  println!("{}: {}", key, value);
}
```
