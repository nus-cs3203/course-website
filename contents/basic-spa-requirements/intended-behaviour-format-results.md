<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#spa-behaviour)SPA Behaviour
===============================

It is crucial to take note that SPA should **terminate**, and not answer any queries if SPA detects an invalid source program (both syntactically and semantically).

Otherwise, queries should be answered according to the [required format of results](#format-of-results).

[](#format-of-results)Format of Results
=======================================

The format of the result returned by PQL queries is summarized below:

| Select                                            |                                           Should return                                           |
|---------------------------------------------------|:-------------------------------------------------------------------------------------------------:|
| Statement  (stmt/read/print/call/while/if/assign) | In SPA implementation, return an array of statement numbers in string format (no need to use ""). |
| Variable                                          | In SPA implementation, return an array of variable names in string format (no need to use "").    |
| Procedure                                         | In SPA implementation, return an array of procedure names in string format (no need to use "").   |
| Constant                                          | In SPA implementation, return an array of constant values in string format (no need to use "").   |

Array should not be populated with any string elements if there are no answers to the query (not even `none`).

Results should not contain duplicates.

For **syntactically invalid queries**, populate the list of results with one value `SyntaxError`. On paper, state `SyntaxError` as your answer, together with the reason why.

Examples of syntactically invalid queries are as follows:

    variable v;
    Select v;
    

    variable v;
    select v
    

For **semantically invalid queries**, populate the list of results with one value `SemanticError`. On paper, state `SemanticError` as your answer, together with the reason why.

Examples of semantically invalid queries are as follows:

    variable v;
    Select v such that Uses(_, v)
    

    stmt s;
    Select s pattern s(_, _)
    

Note that a semantically invalid query must be syntactically valid.