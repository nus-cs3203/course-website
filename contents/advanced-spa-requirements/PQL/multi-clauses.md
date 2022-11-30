<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#multi-clause-queries)Multi-clause Queries
=============================================

Note that a query can now contain any number of such-that, pattern and with clauses.

*   Clauses can be swapped without changing the meaning of the query.
    
*   `and` can be used to connect clauses of the same type.
    

The following queries are the same: `assign a; while w;`

*   `Select a such that Modifies (a, "x") and Parent* (w, a) and Next* (1, a)`
*   `Select a such that Parent* (w, a) and Modifies (a, "x") such that Next* (1, a)`
*   `Select a such that Next* (1, a) such that Parent* (w, a) and Modifies (a, "x")`

The following queries are the same: `assign a; while w;`

*   `Select a pattern a ("x", _) such that Parent* (w, a) and Next* (1, a)`
*   `Select a such that Parent* (w, a) and Next* (1, a) pattern a ("x", _)`
*   `Select a such that Next* (1, a) such that Parent* (w, a) pattern a ("x", _)`

However, the following queries are syntactically incorrect. `and` should not be used to connect or introduce clauses of different types. E.g.: `assign a; while w;`

*   `Select a such that Parent* (w, a) and Modifies (a, "x") and such that Next* (1, a)`
    *   There is usage of `and` and `such that` together before `Next* (1, a)`.
*   `Select a such that Parent* (w, a) and pattern a ("x", _) such that Next* (1, a)`
    *   There is usage of `and` and `pattern` together before `a ("x", _)`.
*   `Select a such that Parent* (w, a) pattern a ("x", _) and Next* (1, a)`
    *   There is usage of `and` to connect a pattern clause `a ("x", _)` and such-that clause `Next* (1, a)`.