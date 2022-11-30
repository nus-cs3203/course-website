<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#select-clause)Select Clause
===============================

The Select clause can now specify the following results to be returned:

1.  Single return values (discussed in [Basic SPA requirements](../../basic-spa-requirements/program-query-language/example-queries.html#queries-with-no-such-that-and-pattern-clause)).
2.  Multiple return values
3.  BOOLEAN value

For multiple return values, the query reports any results for which there exists a combination of synonym instances satisfying all the conditions specified in such-that, pattern and with clauses.

For BOOLEAN, the query reports `TRUE` if:

*   There are no constraints in the query, or
*   There exists a combination of synonym instances satisfying all the conditions specified in the query;

`FALSE` otherwise.

Queries should adhere to format of results introduced in [Basic SPA requirements](../../basic-spa-requirements/intended-behaviour-format-results.html#format-of-results) and [Advanced SPA requirements](../format-res.html).