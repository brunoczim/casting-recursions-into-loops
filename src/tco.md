# Tail Call Optimization/Elimination

There is a compiler optimization called "Tail Call Optimization" or
"Tail Call Elimination" (TCO/TCE) which transforms tail call recursive functions
into loops (without changing the source code!). Some languages guarantee
TCO/TCE, even allowing potentially infinite recursions as if they were infinite
loops, but others, like Rust, do not guarantee the optimization and merely have
it as an... optimization. Let's recall the tail call factorial function:

```rust
fn factorial_impl(n: u128, acc: u128) -> u128 {
    if n <= 1 {
        acc
    } else {
        factorial_impl(n - 1, n * acc)
    }
}
```

In a fictious assembly language, this function could be compiled like this
without the optimization:

```
factorial_impl:
    mov %temp0, %arg0
    cmp %temp0, 1
    jle .base
    sub %arg0, %temp0, 1
    mul %arg1, %temp0, %arg1
    call factorial_impl
    ret
.base:
    mov %ret0, %arg1
    ret
```

With the optimization, the sequence `call; ret` would become a simple `jmp`:

```
factorial_impl:
    mov %temp0, %arg0
    cmp %temp0, 1
    jle .base
    sub %arg0, %temp0, 1
    mul %arg1, %temp0, %arg1
    jmp factorial_impl
.base:
    mov %ret0, %arg1
    ret
```

This is why transforming a recursive function into a tail call recursive
function matters! We can avoid stack overflow easily. Even though the
optimization is not guaranteed in Rust, having a tail call version of your
function can make it easier to understand how a loop version relate to
recursion, as you will see in the next subchapter.
