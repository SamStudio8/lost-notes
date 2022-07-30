# Error handling

## `panic!` for unrecoverable errors

* After a panic, Rust "unwinds" the stack and cleans up after itself (unless `panic = 'abort'` in your Cargo.toml)
* Set `RUST_BACKTRACE=1` for backtrace (or to `full` for ...full backtrace)
* Rust convention to panic when a function's "contract" has been broken as it is unsafe to continue

```rust
fn main() {
  panic!("bang");
}
```

## Recovering from errors with `Result<T,E>`

* A magic `Enum` which has two variants: "Ok(T)" and "Err(E)"
  * Like `Option` the `Result` variants `Ok` and `Err` are brought into scope by the prelude
* Returned by functions that might fail (eg. `File::open` returns `Result<File, std::io::Error>`)
* Can end up with a lot of `match`, [closures can be used to mop this up](https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html#alternatives-to-using-match-with-resultt-e)

### `unwrap` and `expect`

* Sugar for "get T or panic!"
* Can be used with `Option` too!
* `expect` is basically `unwrap` with a custom panic message prepended to the error
* `unwrap` also useful when you have a `Result` but know that an `Err` variant cannot happen

```
use std::fs::File;

fn main() {
    // let f = File::open("hoot.txt").unwrap();
    let f = File::open("hoot.txt").expect("oh no we could not access the hoot data");
}
```

### `?`

* a `match` can `return Err(e)` to propagate the error to the caller
* any function call can propagate an error to the caller by whacking an `?` after its invocation
  * can be chained, first error will be passed up
  * the error goes via `from` to convert error type to the type returned by the current function to not break any type assumptions
    * can only be used in functions that return `Option` or `Result` (or implements `FromResidual`)
    * requires `From<OtherError>` to be implemented for the error type the current function is returning

```
use std::fs::File;

fn main() {
    // don't know if this works so call it like it's a question
    File::open("hoot.txt")?.do_something()?;
}
```

## Trivia

* `?` works on functions that return `Option` and `Result` but can't convert between them, use Option's `ok_or` and Result's `ok` methods
* `main` can actually return `Result<(), E>` with some magic: `fn main() -> Result< (), Box<dyn, Error> >` and `Ok( () )`
* Rust programs return integers to match the C convention (`Ok` returns 0, `Err` returns non-zero)
* In future, `main` will be able to return any type that implements `std::process::Termination`
