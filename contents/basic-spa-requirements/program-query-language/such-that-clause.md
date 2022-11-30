<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#relationships-in-such-that-clauses)Relationships in Such-that Clauses
=========================================================================

There are [relationships (design abstractions)](../design-abstractions.html) to be considered.

Within each relationship used in such-that clauses, there are a few arguments that are allowed in each relationship.

*   Synonyms of [program design entities](../design-entities.html)
*   Wildcard `_`
*   Character strings / Integers

Synonyms of design entities can appear as relationship arguments, and should match the design entity defined for the relationship.

*   E.g. For `Parent(arg1, arg2)`, if `arg1` is a synonym, then `arg1` must be a statement synonym, or a subtype of a statement synonym (read, print, assign, if, while, call).
*   E.g. For `Modifies(arg1, arg2)`, if `arg2` is a synonym, then `arg2` must be a variable synonym.

Assign, read, print, if, while, call synonyms can appear in place of statement synonyms, and vice-versa, and the query will still be semantically valid. However, there may not be results for some relationships. Let us use `Parent` as illustration:

*   E.g. For `Parent(w, _)`, where `w` is a while synonym: Semantically valid, and may have answers.
*   E.g. For `Parent(a, _)`, where `a` is a assign synonym: Semantically valid, but will not have any answers (since assignment is not a container statement).
*   E.g. For `Parent(proc, _)`, where `proc` is a procedure synonym: Semantically invalid because procedure synonym cannot be substituted as statement synonym.

In addition, wildcard `_` can appear as relationship arguments.

*   The only exception would be for `Modifies(arg1, arg2)` and `Uses(arg1, arg2)`, where `arg1` cannot be `_`, as it leads to ambiguity whether `_` refers to a statement or procedure.

A character string in quotes (e.g., `"xyz"`) can appear as relationship arguments if an instance of the design entity can be identified by that string.

*   E.g. For `Modifies(arg1, arg2)`, if `arg1` is `"xyz"`, then it will be interpreted as a procedure.
*   E.g. Similarly, for `Modifies(arg1, arg2)`, if `arg2` is `"xyz"`, then it will be interpreted as a variable.

An integer (e.g. `23`) can also appear as relationship arguments if an instance of the design entity can be identified by the integer.

*   E.g. For `Modifies(arg1, arg2)`, if `arg1` is `23`, then it will be interpreted as a statement.

Do refer to [example queries with one such-that clause](example-queries.html#queries-with-one-such-that-clause).


[](#assign-pattern-clauses)Assign Pattern Clauses
=================================================

The assign pattern clause comprises of an assign synonym, followed by 2 arguments.

The following are allowed as the first argument:

*   Variable synonyms
*   Wildcard `_`
*   Character strings

The following are allowed as the second argument:

*   Wildcard `_`
*   Expression for exact match (e.g. `"x*y"`)
*   Expression for partial match (e.g. `_"x*y"_`)

Assuming your SIMPLE source code contains only one procedure with only one assignment (stmt# 1):

`1. x = v + x * y + z * t`

The evaluation of the following queries would result in:

`assign a;`
|                    PQL query                    | Returns | Explanation                                                     |
|:-----------------------------------------------:|:-------:|-----------------------------------------------------------------|
| Select a pattern a ( _ , "v + x * y + z * t")   | 1       | Exact pattern match                                             |
| Select a pattern a ( _ , "v")                   | none    | stmt #1 contains other expression terms other than v            |
| Select a pattern a ( _ , _"v"_)                 | 1       | stmt #1 contains v as part of the expression on the RHS         |
| Select a pattern a ( _ , _"x*y"_)               | 1       | stmt #1 contains x*y as a term on the RHS                       |
| Select a pattern a ( _ , _"v+x"_)               | none    | stmt #1 does not contain v+x as a sub-expression on the RHS     |
| Select a pattern a ( _ , _"v+x*y"_)             | 1       | stmt #1 contains v+x*y as a sub-expression on the RHS           |
| Select a pattern a ( _ , _"y+z*t"_)             | none    | stmt #1 does not contain y+z*t as a sub-expression on the RHS   |
| Select a pattern a ( _, _"x * y + z * t"_)      | none    | stmt #1 does not contain x*y+z*t as a sub-expression on the RHS |
| Select a pattern a ( _ , _"v + x * y + z * t"_) | 1       | stmt #1 contains v+x*y+z*t as a sub-expression on the RHS       |

All sub-expressions in pattern matching an expression are sub-trees in the AST.

As such, all subexpressions for stmt# 1 are:

*   `x`, `y`, `z`, `t`, `v`
*   `v`, `x * y`, `z * t`,
*   `v + x * y`,
*   `v + x * y + z * t`

Another way to identify a sub-expression is to use brackets, and every bracket is a sub-expression:

`1. x = (((v) + ((x) * (y))) + ((z) * (t)))`

Do not forget that expression in SIMPLE are left-associative.

Do refer to [example queries with one pattern clause](example-queries.html#queries-with-one-pattern-clause).