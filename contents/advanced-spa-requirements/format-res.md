<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#format-of-results)Format of Results
=======================================

The format of the result returned by PQL queries is summarized below:

| Select        |                               Should return                              |
|---------------|:------------------------------------------------------------------------:|
| BOOLEAN       | In SPA implementation, return an array of a single string TRUE or FALSE. On paper, write either TRUE or FALSE. |
| Tuple (e.g. <a, p, s>) | In SPA implementation, arrange tuple elements separated by space in a string, and return an array of strings. E.g. Return a list of strings where the first element is 3 First 10, second element is 5 Second 4 etc.                                     |

Otherwise, format of results previously discussed in [Basic SPA requirements](../basic-spa-requirements/intended-behaviour-format-results.html#format-of-results) still holds.
