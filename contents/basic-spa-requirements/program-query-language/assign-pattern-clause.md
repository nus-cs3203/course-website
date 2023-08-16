<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

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

| PQL query                                       | Returns | Explanation                                                     |
|-------------------------------------------------|:-------:|-----------------------------------------------------------------|
| `Select a pattern a ( _ , "v + x * y + z * t")`   | 1       | Exact pattern match                                             |
| `Select a pattern a ( _ , "v")`                   | none    | stmt #1 contains other expression terms other than v            |
| `Select a pattern a ( _ , _"v"_)`                 | 1       | stmt #1 contains `v` as part of the expression on the RHS         |
| `Select a pattern a ( _ , _"x*y"_)`               | 1       | stmt #1 contains `x*y` as a term on the RHS                       |
| `Select a pattern a ( _ , _"v+x"_)`               | none    | stmt #1 does not contain `v+x` as a sub-expression on the RHS     |
| `Select a pattern a ( _ , _"v+x*y"_)`             | 1       | stmt #1 contains `v+x*y` as a sub-expression on the RHS           |
| `Select a pattern a ( _ , _"y+z*t"_)`             | none    | stmt #1 does not contain `y+z*t` as a sub-expression on the RHS   |
| `Select a pattern a ( _ , _"x * y + z * t"_)`      | none    | stmt #1 does not contain `x*y+z*t` as a sub-expression on the RHS |
| `Select a pattern a ( _ , _"v + x * y + z * t"_)` | 1       | stmt #1 contains `v+x*y+z*t` as a sub-expression on the RHS       |

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
