<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#general-requirements)General Requirements
=============================================

You are to develop an SPA prototype which should allow you to enter a SIMPLE source program and PQL queries. The idea of the prototype is to let you see the main SPA components in simplified form.

1.  Your SPA architecture should be similar to the one described in the course materials (i.e., lecture notes and handouts).

2.  Your source files and directories should be organized and correspond to the SPA architecture.

3.  Your solution should use naming conventions to make correspondences visible.

    *   SPA components should be implemented in separate C++ source files and directories.
    *   Each design abstractions should be implemented in separate source files.
    *   Concrete APIs (public interfaces) should be defined in the corresponding header files.
    *   Communications between SPA components should be done via their concrete APIs.
    *   Symbolic names for data types in abstract APIs and corresponding implementation classes should be the same.
        *   `typedef` can easily assign specific C++ data type to symbolic names.
4.  Integrate Autotester with your program. Use Autotester to reuse test cases and automate testing.


You are **not** allowed to use parser generators or additional libraries (e.g. Bison/Flex/Boost) for parsing SIMPLE source program.

[](#source-processor)Source Processor
=====================================

The Source Processor should be able to parse a SIMPLE source program with the following specification.

**Lexical tokens:**

    LETTER: A-Z | a-z                     - capital or small letter
    DIGIT: 0-9
    NZDIGIT: 1-9                          - non-zero digit
    NAME: LETTER (LETTER | DIGIT)*        - procedure names and variables are strings of
                                            letters, and digits, starting with a letter
    INTEGER : 0 | NZDIGIT ( DIGIT )*      - Constants are sequence of digits with no leading zero


**Grammar rules:**

    program: procedure
    procedure: 'procedure' proc_name '{' stmtLst '}'
    stmtLst: stmt+
    stmt: read | print | while | if | assign

    read: 'read' var_name';'
    print: 'print' var_name';'
    while: 'while' '(' cond_expr ')' '{' stmtLst '}'
    if: 'if' '(' cond_expr ')' 'then' '{' stmtLst '}' 'else' '{' stmtLst '}'
    assign: var_name '=' expr ';'

    cond_expr: rel_expr | '!' '(' cond_expr ')' |
               '(' cond_expr ')' '&&' '(' cond_expr ')' |
               '(' cond_expr ')' '||' '(' cond_expr ')'

    rel_expr: rel_factor '>' rel_factor | rel_factor '>=' rel_factor |
              rel_factor '<' rel_factor | rel_factor '<=' rel_factor |
              rel_factor '==' rel_factor | rel_factor '!=' rel_factor

    rel_factor: var_name | const_value | expr
    expr: expr '+' term | expr '-' term | term
    term: term '*' factor | term '/' factor | term '%' factor | factor
    factor: var_name | const_value | '(' expr ')'

    var_name, proc_name: NAME
    const_value: INTEGER


In particular, a program will only have one procedure, and there are no call statements.

[](#pkb)PKB
===========

The PKB should store information on design entities.

The PKB should also store information on the following design abstractions:

*   `Follows/Follows*`
*   `Parent/Parent*`
*   `Modifies` for statements
*   `Uses` for statements

You may use any data structures that you find useful in implementing SPA. Implementations that do not contain an AST are acceptable, as long as all design abstractions and pattern matching are stored and computed correctly.

[](#query-processing-subsystem)Query Processing Subsystem
=========================================================

The Query Processing Subsystem should be able to validate and evaluate the queries with the following specification.

**Lexical tokens:**

    LETTER: A-Z | a-z                     - capital or small letter
    DIGIT: 0-9
    NZDIGIT: 1-9                          - non-zero digit
    IDENT : LETTER ( LETTER | DIGIT )*
    NAME : LETTER ( LETTER | DIGIT )*
    INTEGER : 0 | NZDIGIT ( DIGIT )*      - Constants are sequence of digits with no leading zero

    synonym : IDENT
    stmtRef : synonym | '_' | INTEGER
    entRef : synonym | '_' | '"' IDENT '"'


**Grammar rules:**

    select-cl : declaration* 'Select' synonym [ suchthat-cl ] [ pattern-cl ]
    declaration : design-entity synonym (',' synonym)* ';'
    design-entity : 'stmt' | 'read' | 'print' | 'while' | 'if' | 'assign' |
                    'variable' | 'constant' | 'procedure'

    suchthat-cl : 'such' 'that' relRef
    relRef : Follows | FollowsT | Parent | ParentT | UsesS | ModifiesS

    Follows : 'Follows' '(' stmtRef ',' stmtRef ')'
    FollowsT : 'Follows*' '(' stmtRef ',' stmtRef ')'

    Parent : 'Parent' '(' stmtRef ',' stmtRef ')'
    ParentT : 'Parent*' '(' stmtRef ',' stmtRef ')'

    UsesS : 'Uses' '(' stmtRef ',' entRef ')'

    ModifiesS : 'Modifies' '(' stmtRef ',' entRef ')'

    pattern-cl : 'pattern' syn-assign '(' entRef ',' expression-spec ')'
    syn-assign : IDENT
    expression-spec : '_' '"' factor '"' '_' | '_'

    factor: var_name | const_value

    var_name: NAME
    const_value : INTEGER


In particular, a query will only have at most one such-that clause and one pattern clause in sequence.

You do not need to implement query evaluation for queries with:

1.  synonyms of procedure calls
2.  `Modifies` and `Uses` for procedures
3.  Expression matching with operators in pattern clause
    *   You only need to handle constant matching and variable matching
    *   E.g. You do not need to handle `pattern a(_, _"x + 1"_)`
    *   E.g. You have to handle `pattern a(_, _"x"_)` and `pattern a(_, _"1"_)`
4.  Exact matching in pattern clause
    *   You only need to handle partial matching or wildcard
    *   E.g. You do not need to handle `pattern a(_, "x")`
    *   E.g. You need to handle `pattern a(_, _"x"_)` and `pattern a(_, _)`
5.  More than one such-that clauses
6.  More than one pattern clauses
