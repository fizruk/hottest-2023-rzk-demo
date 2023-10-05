# Exercises

This is a literate Rzk file:

```rzk
#lang rzk-1
```

## Exercise 1

Show that `modus-ponens` is equal to a specialised `identity`.

??? abstract "Solution to Exercise 1"

    ```rzk title="Exercise 1"
    #define eq-modus-ponens-identity
      (A B : U)
      : modus-ponens A B = identity (A → B)
      := refl
    ```

## Exercise 2

Define composition of (non-dependent) functions. That is, given `f : A → B`
and `g : B → C`, provide a function of type `A → C`.

??? abstract "Solution to Exercise 2"

    ```rzk
    #define compose
      (A B C : U)
      (f : A → B)
      (g : B → C)
      : A → C
      := \ x → g (f x)
    ```

## Exercise 3

Consider the following definitions:

```rzk
#define Σ-arrow
  (A : U)
  : U
  := Σ (x : A), Σ (y : A), hom A x y

#define 2-to-Σ
  (A : U)
  : (2 → A) → Σ-arrow A
  := \ d → (d 0₂, (d 1₂, d))

#define Σ-to-2
  (A : U)
  : (Σ-arrow A) → (2 → A)
  := \ (x, (y, f)) → \ t →
    recOR
      ( t ≡ 0₂ ↦ x
      , t ≡ 1₂ ↦ y
      , TOP ↦ f t )
```

Show that the composition of `2-to-Σ` and `Σ-to-2` either way
is equal to `identity`.

??? abstract "Solution to Exercise 3"

    ```rzk
    #define eq-Σ-to-2-to-Σ
      (funext : FunExt)
      (A : U)
      (d : 2 → A)
      : compose
          (2 → A) (Σ-arrow A) (2 → A)
          (2-to-Σ A) (Σ-to-2 A)
        = identity (2 → A)
      := funext (2 → A) (2 → A)
            (compose (2 → A) (Σ-arrow A) (2 → A) (2-to-Σ A) (Σ-to-2 A))
            (identity (2 → A))
            (\_ → refl)

    #define eq-2-to-Σ-to-2
      (funext : FunExt)
      (A : U)
      (xyf : Σ-arrow A)
      : compose
          (Σ-arrow A) (2 → A) (Σ-arrow A)
          (Σ-to-2 A) (2-to-Σ A)
        = identity (Σ-arrow A)
      := funext (Σ-arrow A) (Σ-arrow A)
          (compose (Σ-arrow A) (2 → A) (Σ-arrow A) (Σ-to-2 A) (2-to-Σ A))
          (identity (Σ-arrow A))
          (\_ → refl)
    ```

## Exercise 4

Solve Exercises 3 using `#!rzk cofibration-composition` (hard).
