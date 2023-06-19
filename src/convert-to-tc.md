# Converting non-Tail Call Recursion to Tail Call Recursion

Non-tail call recursions can sometimes be converted to tail call recursion. One
case is when the recursion performs a commutative operation, such as addition
and multiplication. Recall that a commutative operation is an operation whose
operands can be swapped and the result is not changed.

## Recursion Performing Commutative Operations

In this case, we are adding an additional argument storing the intermediate
result. As a studycase, consider the factorial function written in the following
way:

```rust
fn factorial(n: u128) -> u128 {
    if n <= 1 {
        1
    } else {
        n * factorial(n - 1)
    }
}
```

It can be rewritten as:

```rust
fn factorial_impl(n: u128, acc: u128) -> u128 {
    if n <= 1 {
        acc
    } else {
        factorial_impl(n - 1, n * acc)
    }
}

fn factorial(n: u128) -> u128 {
    factorial_impl(n, 1)
}
```

Note:

1. The additional accumulator argument.
2. The accumulator is returned in the base-case, and is propagated by recursive
    callees, since the calls are tail calls.
3. And so, the actual computation happens in the arguments.
4. The multiplication order is reversed, but it does not matter, because
    multiplication is commutative. Multiplication is reversed because what
    happens is `((4 * 3) * 2) * 1` in the tail call version, while in the
    non-tail call, what happens is `4 * (3 * (2 * 1))`.

Another example we could explore is a function that finds the maximum element
of an array slice. Consider the following non-tail call recursion:

```rust
fn max(slice: &[u64]) -> u64 {
    match slice.split_first() {
        None => 0,
        Some((x, xs)) => u64::max(*x, max(xs)),
    }
}
```

It is not tail recursive function because after the recursive call, there is a
call to `u64::max`. Despite that, it is possible to make it tail recursive,
look at the following implementation:

```rust
fn max_impl(slice: &[u64], acc: u64) -> u64 {
    match slice.split_first() {
        None => acc,
        Some((x, xs)) => max(xs, u64::max(*x, acc)),
    }
}

fn max(slice: &[u64]) -> u64 {
    max_impl(slice, 0)
}
```

## Thinking of Parameters as "Variables" in a Loop

A more difficult case is this Fibonacci function implementation:

```rust
fn fibonacci(n: u16) -> u128 {
    if n <= 1 {
        1
    } else {
        fibonacci(n - 1) + fibonacci(n - 2)
    }
}
```

The difficulty here is that this function has two recursive calls. However,
we can fix this by having two accumulators that interact with each other. Look
at this reimplementation:

```rust
fn fibonacci_impl(n: u16, curr: u128, next: u128) -> u128 {
    if n == 0 {
        curr
    } else {
        fibonacci_impl(n - 1, next, curr + next)
    }
}

fn fibonacci(n: u16) -> u128 {
    fibonacci_impl(n, 1, 1)
}
```
