<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#simple-programming-language)SIMPLE Programming Language
===========================================================

The static analysis is done on a programming language called SIMPLE. It is designed for the purpose of experimenting with SPA techniques and allows students to complete the project in one semester. It is not a language for solving real programming problems. However, it contains all the basic constructs of a programming language for writing meaningful programs.

Several sample programs that can be written in SIMPLE are shown below:

##### [](#code-1-compute-average-of-three-integer-numbers)Code 1: Compute average of three integer numbers

    procedure computeAverage {
        read num1;
        read num2;
        read num3;

        sum = num1 + num2 + num3;
        ave = sum / 3;

        print ave;
    }


##### [](#code-2-print-numbers-in-ascending-order)Code 2: Print numbers in ascending order

    procedure printAscending {
        read num1;
        read num2;
        noSwap = 0;

        if (num1 > num2) then {
          temp = num1;
          num1 = num2;
          num2 = temp;
        } else {
          noSwap = 1;
        }

        print num1;
        print num2;
        print noSwap;
    }


##### [](#code-3-compute-the-sum-of-the-digits-of-an-integer)Code 3: Compute the sum of the digits of an integer

    procedure sumDigits {
        read number;
        sum = 0;

        while (number > 0) {
            digit = number % 10;
            sum = sum + digit;
            number = number / 10;
        }

        print sum;
    }


##### [](#code-4-program-with-multiple-procedures)Code 4: Program with multiple procedures

        procedure main {
    01      flag = 0;
    02      call computeCentroid;
    03      call printResults;
        }
        procedure readPoint {
    04      read x;
    05      read y;
        }
        procedure printResults {
    06      print flag;
    07      print cenX;
    08      print cenY;
    09      print normSq;
        }
        procedure computeCentroid {
    10      count = 0;
    11      cenX = 0;
    12      cenY = 0;
    13      call readPoint;
    14      while ((x != 0) && (y != 0)) {
    15          count = count + 1;
    16          cenX = cenX + x;
    17          cenY = cenY + y;
    18          call readPoint;
            }
    19      if (count == 0) then {
    20          flag = 1;
            } else {
    21          cenX = cenX / count;
    22          cenY = cenY / count;
            }
    23      normSq = cenX * cenX + cenY * cenY;
        }


[](#general-information)General Information
===========================================

The following are some general information:

*   **Program**: Made up of one or more procedures
*   **Procedure**: Non-empty list of statements with no parameters, nesting nor recursion
*   **Variable**: Unique names, global scope, integer type, and does not have declarations
*   **Statement**: Assignments, while loops, if-then-else statements, call statements, print statements or read statements
*   **Condition**: Boolean expressions containing operators
*   **Operator**: `+, -, *, /, %, <, >`, etc.
*   No arrays, no pointers

Program statements are of the following types:

*   Call statements
    *   Invokes a procedure by its name.
    *   E.g. `call readPoint;`
*   Assignment statement
    *   Assigns the results of the expression on the right to the variable on the left.
    *   E.g. `x = a + 2 * b;`
*   While statement
    *   Executes statements in the body until the condition becomes false.
    *   E.g. `while (i > 0) { ... }`
*   If statement
    *   Executes statements in the `then` branch if condition is true; otherwise executes statements in the `else` branch.
    *   The `else` branch is mandatory.
    *   E.g. `if (i > 0) then { ... } else { ... }`
*   Read statement
    *   Reads the value from standard input (console) and assigns it to the variable.
    *   It is similar to `scanf` and `cin` in C/C++, and does not read the value from the variable.
    *   E.g. `read x;`
*   Print output
    *   Outputs the value of the variable to standard output (console).
    *   E.g. `print x;`

[](#concrete-syntax-grammar-csg)Concrete Syntax Grammar (CSG)
=============================================================

A source program written in SIMPLE is syntactically valid if it follows all the defined language rules. The rules are defined as a **Concrete Syntax Grammar (CSG)**.

CSG contains rules (or grammar productions) that are written using terminals (keywords) and non-terminals.

*   Terminals (keywords) are between apostrophes
    *   E.g. `'procedure'`, `')'`, `'+'`, `'while'`, `'if'`, etc.
*   Non-terminals are in lower-casing
    *   E.g. `var_name`, `term`, `expr`, `stmt_list`, etc.

For example, the grammar for SIMPLE starts with a non-terminal `program`. A program contains one or more procedures. Each procedure is defined using the terminal keyword `'procedure'`, followed by a procedure name (non-terminal `proc_name`), followed by the keyword `'{'`, followed by a statement list (non-terminal `stmtLst`), followed by `'}'`. Non-terminal `stmtLst` contains one or more statements (`stmt`), and `stmt` can be a read, print, call, while, if or assign statement.

**Meta symbols:**

    a*        - repetition 0 or more times of a
    a+        - repetition 1 or more times of a
    a | b     - a or b
    Brackets ( and ) are used for grouping


**Lexical tokens:**

    LETTER: A-Z | a-z                - capital or small letter
    DIGIT: 0-9
    NZDIGIT: 1-9                          - non-zero digit
    NAME: LETTER (LETTER | DIGIT)*   - procedure names and variables are strings of
                                       letters, and digits, starting with a letter
    INTEGER : 0 | NZDIGIT ( DIGIT )*      - Constants are sequence of digits with no leading zero


**Grammar rules:**

    program: procedure+
    procedure: 'procedure' proc_name '{' stmtLst '}'
    stmtLst: stmt+
    stmt: read | print | call | while | if | assign

    read: 'read' var_name';'
    print: 'print' var_name';'
    call: 'call' proc_name';'
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


**Note:**

1.  SIMPLE is case-sensitive. The grammar shows the accepted casing for the keywords of the language. Due to case-sensitivity, variables "abc" and "Abc" are two different variables.
2.  Whitespaces (including multiple spaces, tabs, or no spaces) can be used freely in SIMPLE. For example, tokenizer should recognize three tokens `x`, `+` and `y` in any of the following character streams:
    *   `x+y`
    *   `x + y`
    *   `x +y`
3.  Procedure names, variable names and terminals can all be the same.
    *   E.g. `read read; read = read + 1;`
4.  Logical expressions may only appear as the conditions for while / if statements and they are fully bracketed.
5.  There are no boolean values (`true` / `false`).
6.  Expressions are left-associative due to the following grammar rules:

    expr: expr '+' term | expr '-' term | term
    term: term '*' factor | term '/' factor | term '%' factor | factor
    factor: var_name | const_value | '(' expr ')'


[](#other-rules)Other Rules
===========================

A syntactically valid source program written in SIMPLE is semantically invalid if it violates rules that cannot be captured by the CSG.

**The following are rules that cannot be captured by CSG:**

1.  A program cannot have two procedures with the same name.
2.  A procedure cannot call a non-existing procedure.
3.  Recursive and cyclic calls are not allowed.
    *   E.g. Procedure A calls procedure B, procedure B calls procedure C, and procedure C calls procedure A.
    *   E.g. Procedure A calls procedure A.

[](#statement-numbers)Statement Numbers
=======================================

The statements in a SIMPLE program are indexed for easy referencing and are not part of the syntax. The first statement in the program located in the first procedure of the program is statement number 1 (stmt #1). The numbering continues from one procedure to the next.

The procedure definition does not receive an index. Furthermore, empty lines, `else` keywords on a line, curly brackets on a line do not receive an index.

An example of how statements are numbered is shown in [Code 4](#code-4-program-with-multiple-procedures).

[](#abstract-syntax-grammar-asg)Abstract Syntax Grammar (ASG)
=============================================================

A CSG defines the language and its vocabulary. However, you may need to use abstractions to define a language. In that case, the ASG defines the relationships among different abstractions in a language, without defining the exact vocabulary.

For example, an expression (non-terminal `expr`) in the ASG of SIMPLE may be a `plus`, `minus`, `times`, `div`, or `mod` expression or a reference (non-terminal `ref`).

There can be multiple CSGs for the same ASG.

**Meta symbols:**

    a+        - repetition 1 or more times of a
    a | b     - a or b


**Lexical tokens:**

    LETTER: A-Z | a-z               - capital or small letter
    DIGIT: 0-9
    NZDIGIT: 1-9
    NAME: LETTER (LETTER | DIGIT)*
    INTEGER: 0 | NZDIGIT ( DIGIT )*


**Grammar rules:**

    program: procedure+
    procedure: stmtLst
    stmtLst: stmt+
    stmt: read | print | call | while | if | assign

    read, print: variable
    while: cond_expr stmtLst
    if: cond_expr  stmtLst stmtLst
    assign: variable expr

    cond_expr: rel_expr | not | and | or
    not: cond_expr
    and, or: cond_expr cond_expr

    rel_expr: gt | gte | lt | lte | eq | neq
    gt, gte, lt, lte, eq, neq: rel_factor rel_factor
    rel_factor: variable | constant | expr

    expr: plus | minus | times | div | mod | ref
    plus, minus, times, div, mod: expr expr

    ref: variable | constant


**Attributes and value types:**

    procedure.procName, call.procName, variable.varName, read.varName, print.varName: NAME
    constant.value: INTEGER
    stmt.stmt#, read.stmt#, print.stmt#, call.stmt#, while.stmt#, if.stmt#, assign.stmt#: INTEGER
