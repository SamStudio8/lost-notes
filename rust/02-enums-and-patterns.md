# Enumerations and Patterns

So you want to enumerate some variants?

## Intro

* Not your average enums
* Variants are namespaced under an identifier, delimited with `::`

```
enum HooterSize {
    Lorge,
    Smol,
}
let small_hooter = HooterSize::Smol;

fn get_hooters_by_size(size: HooterSize) { ... }
get_hooters_by_size(HooterSize::Lorge);
```
