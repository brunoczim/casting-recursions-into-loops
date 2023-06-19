# What is a Tail Call?

A tail call is a function call that is the last thing the callee will execute.
In other words, if a function `F` calls a function `G`, such call is a tail call
if after calling `G`, `F` immediately returns; `return G()` is a tail call, but
`return G() * 3` is not a tail call, because after `G()` finishes, `F` still
needs to multiply the result by `3`.

## Example of a Tail Call Recursive Function 

The following function is a recursive function that has the recursive call in
tail position:

```rust
fn add(m: u64, n: u64) -> u64 {
    if m == 0 {
        n
    } else {
        add(m - 1, n + 1)
    }
}
```

Note that after performing the recursive call, the result is returned unmodified
and the function will do nothing but return it immediately.

## Counterexample: a Non-Tail Call Recursive Function

The following function is a recursive function that does **not** have the
recursive call in tail position:

```rust
fn add(m: u64, n: u64) -> u64 {
    if m == 0 {
        n
    } else {
        1 + add(m - 1, n)
    }
}
```

After performing the recursive call, the function needs to add `1` to the
result, therefore, it does not immediately return.
