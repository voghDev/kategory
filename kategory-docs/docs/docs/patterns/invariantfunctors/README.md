---
layout: docs
title: Invariant functors
permalink: /docs/patterns/invariant_functors/
---

## Invariant and contravariant functors

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

```
@typeclass trait InvariantFunctor[F[_]] {
    def xmap[A, B](fa: F[A], f: A => B, g: B => A): F[B]
    ...
}

@typeclass trait Functor[F[_]] extends InvariantFunctor[F] {
    def map[A, B](fa: F[A])(f: A => B): F[B]
    def xmap[A, B](fa: F[A], f: A => B, g: B => A): F[B] = map(fa)(f)
    ...
}

@typeclass trait Contravariant[F[_]] extends InvariantFunctor[F] {
    def contramap[A, B](fa: F[A])(f: B => A): F[B]
    def xmap[A, B](fa: F[A], f: A => B, g: B => A): F[B] = contramap(fa)(f)
    ...
}
```

`Functor` implements `xmap` with its method `map` and ignores the function from B to A (called `g` in this case).
`Contravariant` implements `xmap` with its method `contramap` and ignores the function from A to B.
