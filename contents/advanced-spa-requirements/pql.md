<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#advanced-pql)Advanced PQL
=============================

Queries now contain:

*   **Declaration of synonyms** to be used in the query
    *   E.g. `procedure p; variable v;` where `p` is a procedure entity and `v` is a variable entity
*   **Select clause** that specifies either:
    *   a single return value
    *   **\[NEW\]** multiple return values (tuples)
    *   **\[NEW\]** BOOLEAN value
*   Any number of **such-that clauses** that constrains the results in terms of relationships
*   Any number of **pattern clauses** that constrains results in terms of syntactic patterns
*   **\[NEW\]** Any number of **with clauses** that constrains results in terms of [attribute values](..\basic-spa-requirements\simple-programming.html#abstract-syntax-grammar-asg)

There is an implicit `and` operator between clauses. Query results must make sure that they satisfy all clauses.

[](#advanced-pql-grammar)Advanced PQL Grammar
=============================================

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
    INTEGER : 0 | NZDIGIT ( DIGIT )*      - no leading zero [Updated: 27th Feb]
    
    synonym : IDENT
    stmtRef : synonym | '_' | INTEGER
    entRef : synonym | '_' | '"' IDENT '"'
    
    elem : synonym | attrRef
    attrName : 'procName'| 'varName' | 'value' | 'stmt#'
    design-entity : 'stmt' | 'read' | 'print' | 'call' | 'while' | 'if' | 'assign' |
                    'variable' | 'constant' | 'procedure'
    

**Grammar rules:**

    select-cl : declaration* 'Select' result-cl ( suchthat-cl | pattern-cl | with-cl)*
    declaration : design-entity synonym (',' synonym)* ';'
    
    result-cl : tuple | 'BOOLEAN'
    tuple : elem | '<' elem ( ',' elem )* '>'
    
    
    suchthat-cl : 'such' 'that' relCond
    pattern-cl : 'pattern' patternCond
    with-cl : 'with' attrCond
    
    relCond : relRef ( 'and' relRef )*
    relRef: Follows | FollowsT | Parent | ParentT | UsesS | UsesP | ModifiesS |
            ModifiesP | Calls | CallsT | Next | NextT | Affects | AffectsT
    
    Follows : 'Follows' '(' stmtRef ',' stmtRef ')'
    FollowsT : 'Follows*' '(' stmtRef ',' stmtRef ')'
    
    Parent : 'Parent' '(' stmtRef ',' stmtRef ')'
    ParentT : 'Parent*' '(' stmtRef ',' stmtRef ')'
    
    UsesS : 'Uses' '(' stmtRef ',' entRef ')'
    UsesP : 'Uses' '(' entRef ',' entRef ')'
    
    ModifiesS : 'Modifies' '(' stmtRef ',' entRef ')'
    ModifiesP : 'Modifies' '(' entRef ',' entRef ')'
    
    Calls : 'Calls' '(' entRef ',' entRef ')'
    CallsT : 'Calls*' '(' entRef ',' entRef ')'
    
    Next : 'Next' '(' stmtRef ',' stmtRef ')'
    NextT : 'Next*' '(' stmtRef ',' stmtRef ')'
    
    Affects : 'Affects' '(' stmtRef ',' stmtRef ')'
    AffectsT : 'Affects*' '(' stmtRef ',' stmtRef ')'
    
    
    patternCond : pattern ( 'and' pattern )*
    pattern : assign | while | if
    
    assign: syn-assign '(' entRef ',' expression-spec ')'
    expression-spec :  '"' expr'"' | '_' '"' expr '"' '_' | '_'
    
    expr: expr '+' term | expr '-' term | term
    term: term '*' factor | term '/' factor | term '%' factor | factor
    factor: var_name | const_value | '(' expr ')'
    
    var_name: NAME
    const_value : INTEGER
    
    while : syn-while '(' entRef ',' '_' ')'
    if : syn-if '(' entRef ',' '_' ',' '_' ')'
    
    
    attrCond : attrCompare ( 'and' attrCompare )*
    attrCompare : ref '=' ref
    ref : '"' IDENT '"' | INTEGER | attrRef
    attrRef : synonym '.' attrName
    

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

1.  Rules stated in [Basic SPA requirements](..\basic-spa-requirements\program-query-language\introduction.html#other-rules) still holds.
2.  `syn-while` must be declared as a synonym of an while-statement (design entity `while`).
3.  `syn-if` must be declared as a synonym of an if-statement (design entity `if`).
4.  For `attrCompare`, the two `ref` comparison must be of the same type (both `NAME`, or both `INTEGER`)

In addition, if `BOOLEAN` is declared as a synonym in a PQL query, this declaration takes _precedence_, i.e. we can no longer get a boolean query result for that PQL query.

*   E.g. `stmt BOOLEAN; Select BOOLEAN` gives the list of statements in the SIMPLE source.