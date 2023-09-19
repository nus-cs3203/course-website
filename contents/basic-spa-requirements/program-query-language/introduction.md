<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#program-query-language-pql)Program Query Language (PQL)
===========================================================

PQL queries are expressed in terms of program design models (entities and abstractions).

PQL queries references the following:

*   Design entities
    *   E.g. procedure, variable, assign, etc.
*   Attributes
    *   E.g. `procedure.procName` or `variable.varName`
*   Relationships
    *   E.g. `Modifies (procedure, variable)`
*   Syntactic patterns
    *   E.g. `assign (variable, expr)`

Evaluation of a query yields a list of program elements that match a query. Program elements are specific instances of design entities (e.g. procedure named `main`, statement number `35`, or variable named `x`).

[](#basic-pql)Basic PQL
=======================

Basic queries contain:

*   **Declaration of synonyms** to be used in the query
    *   E.g. `procedure p; variable v;` where `p` is a procedure entity and `v` is a variable entity
*   **Select clause** that specifies a single return value for query result
*   At most one **such-that clause** that constrains the results in terms of relationships
*   At most one **pattern clause** that constrains results in terms of syntactic patterns

There is an implicit `and` operator between clauses. Query results must make sure that they satisfy all clauses.

[](#basic-pql-grammar)Basic PQL Grammar
=======================================

A query written in PQL is syntactically valid if it follows all the defined language rules.

**Meta symbols:**

    a*        - repetition 0 or more times of a
    a+        - repetition 1 or more times of a
    [ a ]     - repetition 0 or one occurrence of 'a'
    a | b     - a or b


**Lexical tokens:**

    LETTER: A-Z | a-z                     - capital or small letter
    DIGIT: 0-9
    NZDIGIT: 1-9                          - non-zero digit
    IDENT : LETTER ( LETTER | DIGIT )*
    NAME : LETTER ( LETTER | DIGIT )*
    INTEGER : 0 | NZDIGIT ( DIGIT )*      - no leading zero

    synonym : IDENT
    stmtRef : synonym | '_' | INTEGER
    entRef : synonym | '_' | '"' IDENT '"'


**Grammar rules:**

    select-cl : declaration* 'Select' synonym [ suchthat-cl ] [ pattern-cl ]
    declaration : design-entity synonym (',' synonym)* ';'
    design-entity : 'stmt' | 'read' | 'print' | 'call' | 'while' |
                    'if' | 'assign' | 'variable' | 'constant' | 'procedure'

    suchthat-cl : 'such' 'that' relRef
    relRef : Follows | FollowsT | Parent | ParentT | UsesS | UsesP | ModifiesS | ModifiesP

    Follows : 'Follows' '(' stmtRef ',' stmtRef ')'
    FollowsT : 'Follows*' '(' stmtRef ',' stmtRef ')'

    Parent : 'Parent' '(' stmtRef ',' stmtRef ')'
    ParentT : 'Parent*' '(' stmtRef ',' stmtRef ')'

    UsesS : 'Uses' '(' stmtRef ',' entRef ')'
    UsesP : 'Uses' '(' entRef ',' entRef ')'

    ModifiesS : 'Modifies' '(' stmtRef ',' entRef ')'
    ModifiesP : 'Modifies' '(' entRef ',' entRef ')'

    pattern-cl : 'pattern' syn-assign '(' entRef ',' expression-spec ')'
    expression-spec :  '"' expr'"' | '_' '"' expr '"' '_' | '_'

    expr: expr '+' term | expr '-' term | term
    term: term '*' factor | term '/' factor | term '%' factor | factor
    factor: var_name | const_value | '(' expr ')'

    syn-assign : IDENT

    var_name: NAME
    const_value : INTEGER


**Notes:**

1.  PQL is case-sensitive. The grammar shows the accepted casing for the keywords of the language. Due to case-sensitivity, synonyms "abc" and "Abc" are two different synonyms.
2.  Whitespaces (including multiple spaces, tabs, or no spaces) can be used freely in PQL. For example, tokenizer should recognize three tokens `x`, `+` and `y` in any of the following character streams:
    *   `x+y`
    *   `x + y`
    *   `x +y`
3.  Synonym names and terminals can all be the same.
    *   E.g. `assign pattern; variable Select, assign; Select Select pattern pattern(assign, _)`

[](#other-rules)Other Rules
===========================

A syntactically valid query is semantically invalid if it violates rules that cannot be captured by the language rules.

**The following are rules that are not captured by the grammar:**

1.  A synonym name can only be declared once.
2.  All the synonyms used in clauses must be declared exactly once.
3.  `syn-assign` must be declared as a synonym of an assignment (design entity `assign`).
4.  The first argument for `Modifies` and `Uses` cannot be `_`, as it is unclear whether `_` refers to a statement or procedure.
5.  Synonyms of design entities can appear as relationship arguments, and should match the design entity defined for the relationship.
    *   E.g. For `Parent(arg1, arg2)`, if `arg1` is a synonym, then `arg1` must be a statement synonym, or a subtype of a statement synonym (read, print, assign, if, while, call).
    *   E.g. For `Modifies(arg1, arg2)`, if `arg2` is a synonym, then `arg2` must be a variable synonym.
6.  Similarly, synonyms of design entities can appear as arguments in pattern clauses, and should match the design entity defined for pattern clauses.
    *   E.g. For clause `pattern a (arg1, _)`, if `arg1` is a synonym, then `arg1` must be a variable synonym.
