---
layout: docs
title: Invariant functor
permalink: /docs/datatypes/invariant_functor/
---

## Invariant functor

InvariantFunctor, also known as the exponential functor is a `Functor` typeclass that has a method called `xmap`, which must meet the following:

given two functions `f` and `g`, where

```
f: A => B
```

and

```
g: B => A
```

`xmap` must convert F[A] to F[B]

### Functor

`Functor` is a short name for what should be `covariant functor`. Since `Functor` is so popular, it gets the nickname.
Likewise, `Contravariant` should really be `contravariant functor`

```
Usually called        Is equivalent to
-------------------------------------------
Functor               Covariant functor
Contravariant         Contravariant functor
```


Let's see an example through a simple hierarchy:

```kotlin
interface InvariantFunctor<F> {
    fun <A, B> xmap(fa: () -> F, f: () -> B, g: () -> A) : B
    ...
}

interface Functor<F> : InvariantFunctor<F> {
    fun <A, B> map(fa: () -> F, f: () -> B, g: () -> A): B
    override fun <A, B> xmap(fa: () -> F, f: () -> B): B = map(fa, f, _)
}

interface ContravariantFunctor<F> : InvariantFunctor<F> {
    fun <A, B> contramap(fa: () -> F, f: () -> B, g: () -> A): B
    override fun <A, B> xmap(fa: () -> F, g: () -> A): B = contramap(fa, _, g)
}

```

`Functor` implements `xmap` with its method `map` and ignores the function from B to A (called `g` in this case).
`Contravariant` implements `xmap` with its method `contramap` and ignores the function from A to B.
