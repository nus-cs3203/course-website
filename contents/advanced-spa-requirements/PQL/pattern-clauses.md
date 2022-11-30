<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#pattern-clauses)Pattern Clauses
===================================

There are now three types of pattern clauses:

1.  Assign pattern clauses (discussed in [Basic SPA requirements](../../basic-spa-requirements/program-query-language/example-queries.html#queries-with-one-pattern-clause))
2.  While pattern clauses
3.  If pattern clauses

In while and if patterns, we are looking at matching control variables. A control variable is a variable that is used in the conditional expression of a container statement.

While pattern and if pattern has 2 arguments and 3 arguments within the parenthesis respectively.

*   Similar to assign pattern, the first argument within the parenthesis can only be:
    *   variable synonym
    *   wildcard `_`
    *   variable name in quotes
*   The second (and third) argument can only be a wildcard `_`.

**Example:**

##### [](#code-13)Code 13

    procedure p {
    1.  if (1 == 2) then {
    2.      while (3 == 4) {
    3.          x = 1; } }
        else {
    4.      if (x == 1) then {
    5.          while (y == x) {
    6.              z = 2; } }
            else {
    7.          y = 3; } } } }
    

The following queries will yield these answers:

`if ifs; while w;`

*   `Select ifs pattern ifs(_,_,_)` will return `4`, as only stmt `4` is a if statement uses variable(s) in conditional expression
*   `Select w pattern w(_,_)` will return `5` as only stmt 5 is a while statement uses variable(s) in conditional expression
*   `Select <ifs, v> pattern ifs(v,_,_)` will return `4 x`
*   `Select <w, v> pattern w(v,_)` will return `5 x, 5 y`