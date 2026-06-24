# hypertypes-ast

AST, unification, and type inference utilities for
[`hypertypes`](https://github.com/lamdu/hypertypes).

For the core idea behind hypertypes, the field-constructor pattern, witnesses,
plain representations, and comparison with DTALC, `multirec`, HKD, and
`recursion-schemes`, see the
[`hypertypes` README](https://github.com/lamdu/hypertypes#readme).

This package contains the compiler-oriented pieces built on top of that core:

* `Hyper.Syntax`: reusable AST terms such as variables, lambdas, lets,
  applications, type signatures, and function types.
* `Hyper.Unify`: unification for mutually recursive hypertypes.
* `Hyper.Infer`: Hindley-Milner type inference over annotated ASTs.

## Reusable Syntax Terms

The syntax modules provide small terms that can be embedded in your own ASTs:

```Haskell
data RExpr h
    = RVar (Var Text RExpr h)
    | RApp (App RExpr h)
    | RLam (TypedLam Text Typ RExpr h)
    deriving (Generic, Generic1, HNodes, HFunctor, HFoldable, HTraversable, ZipMatch)
```

This keeps the AST constructors explicit while reusing the instances and logic
for common language constructs.

See `test/ReadMeExamples.hs` for a checked example that combines these terms
with `hypertypes` plain representations and structural diffing.

## Unification and Inference

`Hyper.Unify` provides the unification machinery used by the syntax terms and
type inference.

`Hyper.Infer` performs Hindley-Milner inference and annotates each node with its
inferred type:

```Haskell
infer ::
    Infer m t =>
    Ann a # t ->
    m (Ann (a :*: InferResult (UVarOf m)) # t)
```

For complete language examples, see the `test/Lang*.hs` modules.
