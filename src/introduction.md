"Casting Recursions Into Loops" is a book teaching how to convert recursive code
into loop code.

# Motivation

Recursive code is a beautiful and simple way of expressing repetition, and to
some people, for some problems, it is the most natural way of doing so. However,
in languages like Rust, where stack overflow is a possibility, recursion is
limited to inputs small enough to prevent stack overflow. One could solve this
problem by converting recursion code to loop code.

# Recursion

By recursion, we mean a function that calls itself in order to
implement repetition. In the following code, there is a classical example of
recursion, the factorial function:

```rust
fn factorial(n: u128) -> u128 {
    if n < 2 {
        1
    } else {
        factorial(n - 1) * n
    }
}
```

Mutual recursion is a subtype of recursion in which a group of functions call
each other in order to implement repetition. In the following code, there is a
naive but fun implementation of "is even" and "is odd" functions that is
mutually recursive:

```rust
fn is_even(n: u128) -> bool {
    if n == 0 {
        true
    } else {
        is_odd(n - 1)
    }
}

fn is_odd(n: u128) -> bool {
    if n == 0 {
        false
    } else {
        is_even(n - 1)
    }
}
```

# Loops

I'll assume most of you are comfortable with loops, but I will do a quick
recapitulation. Loops work by repeting a block of code until a given condition
is met. Rust has three types of loops: the one with a `loop` keyword, the one
with the `while` keyword, and the one with the `for` keyword. The `loop` loop
does not have a condition but can be stopped with a `break` keyword; the `while`
loop has a fixed condition; the `for` loop is a more dynamic loop, controlled by
an iterator object.
