<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#general-requirements)General Requirements
=============================================

You are to revise your SPA prototype implemented in Milestone 1, and extend some features documented in the SPA requirements.

Your team will **complete the Basic SPA requirements**, and decide on a **subset of the Advanced SPA requirement** to implement.

*   The sections [Source Processor](#source-processor), [PKB](#pkb), [Query Processing Subsystem](#query-processing-subsystem) are just some of the recommendations that your team can take.
*   The scope for this milestone can be adjusted, so please use the weekly consultations to discuss your plans with your consultation.

General requirements stated in [Milestone 1 Scope](../project-requirements-1/scope.html) still holds.

[](#constraints-on-program-design-abstractions)Constraints on Program Design Abstractions
=========================================================================================

You are **not allowed** to pre-compute the program design abstractions `Next*` and `Affects` during parsing time, i.e. these relationship must be calculated **on the fly** during query evaluation.

*   Note that you can cache computed results during the evaluation of a **single query**, i.e. computed result can be used **between clauses** of the same query. The cache(s) should be **cleared after each query**.

[](#source-processor)Source Processor
=====================================

The Source Processor should be able to parse a SIMPLE source program with the specification stated in the [SPA Requirements](../basic-spa-requirements/simple-programming.html#concrete-syntax-grammar-csg).

[](#pkb)PKB
===========

The PKB should store information on design entities.

The PKB should also store information on the following design abstractions:

*   `Follows/Follows*`
*   `Parent/Parent*`
*   `Modifies` for statements and procedures
*   `Uses` for statements and procedures
*   `Calls/Calls*`
*   `Next`

[](#query-processing-subsystem)Query Processing Subsystem
=========================================================

The Query Processing Subsystem should be able to validate and evaluate the queries with the following specification.

In particular, the specification below does not consider such-that clauses with `Affects` relationship, and `not` operator.

The Query Processing Subsystem is also not expected to consider optimizations by means of changing the evaluation order of clauses.

**Lexical tokens:**

    LETTER: A-Z | a-z                     - capital or small letter
    DIGIT: 0-9
    NZDIGIT: 1-9
    IDENT : LETTER ( LETTER | DIGIT )*
    NAME : LETTER ( LETTER | DIGIT )*
    INTEGER : 0 | NZDIGIT ( DIGIT )*      - Constants are sequence of digits with no leading zero

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
            ModifiesP | Calls | CallsT | Next | NextT

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
