<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#design-abstractions)Design Abstractions
===========================================

In this section, we will use [Code 6](#code-6) to explain more design abstractions.

For simplicity, procedures `First` and `Third` are excluded from statement numbering.

##### [](#code-6)Code 6

          procedure First {
          read x;
          read z;
          call Second; }
    
          procedure Second {
    01        x = 0;
    02        i = 5;
    03        while (i!=0) {
    04            x = x + 2*y;
    05            call Third;
    06            i = i - 1; }
    07        if (x==1) then {
    08            x = x+1; }
              else {
    09            z = 1; }
    10        z = z + x + i;
    11        y = z + 2;
    12        x = x * y + z; }
    
          procedure Third {
              z = 5;
              v = z;
              print v; }
    

[](#advanced-design-abstractions)Advanced Design Abstractions
=============================================================

The table shows a summary of the design abstractions discussed in this section.

Design Abstraction
|                         Design Abstraction                         | Relationship between |
|:------------------------------------------------------------------:|:--------------------:|
| <a href="#calls--calls">Calls / Calls*</a>                         | Procedures           |
| <a href="#next--next">Next / Next*</a>                             | Statements           |
| <a href="#affects">Affects</a> / <a href="#affects-1">Affects*</a> | Assignments          |

[](#calls--calls)Calls / Calls\*
================================

**Definition:**

    For any procedures p and q:
    
    Calls(p, q) holds if procedure p directly calls q.
    
    Calls*(p, q) holds if procedure p directly or indirectly calls q. That is:
        - Calls (p, q) or
        - Calls (p, p1) and Calls* (p1, q) for some procedure p1
    

`Calls*` is the transitive closure of `Calls`.

**Examples:**

In [Code 6](#code-6), the following relationships hold (are true):

*   `Calls ("First", "Second")`
*   `Calls ("Second", "Third")`
*   `Calls* ("First", "Second")`
*   `Calls* ("First", "Third")`

In contrast, the following relationships are false (do not hold):

*   `Calls ("First", "Third")`
*   `Calls ("Second", "First")`
*   `Calls* ("Second", "First")`

[](#next--next)Next / Next\*
============================

**Definition:**

    For two statements s1 and s2 in the same procedure:
    
    Next(s1, s2) holds if s2 can be executed immediately after n1 in some execution sequence.
    Next*(n1, n2) holds if n2 can be executed after n1 in some execution sequence.
    

`Next*` is the transitive closure of `Next`.

**Examples:**

In [Code 6](#code-6), the following relationships hold (are true):

*   `Next (2, 3)`
*   `Next (3, 4)`
*   `Next (3, 7)`
*   `Next (5, 6)`
*   `Next (7, 9)`
*   `Next (8, 10)`
*   `Next* (1, 2)`
*   `Next* (1, 3)`
*   `Next* (2, 5)`
*   `Next* (4, 3)`
*   `Next* (5, 5)`
*   `Next* (5, 8)`
*   `Next* (5, 12)`

The following relationships are false (do not hold):

*   `Next (6, 4)`
*   `Next (7, 10)`
*   `Next (8, 9)`
*   `Next* (8, 9)`
*   `Next* (5, 2)`

**Notes:**

*   The `Next/*` relationship is determined by the [control flow](Advanced-SPA-Control-Flow-Graph), while `Follows/*` is determined by the [structural location](Basic-SPA-Abstract-Syntax-Tree) of a statement.
    
*   When discussing `Next/*` for container statements, the statement number refers to the evaluation of the conditional expression, rather than the whole body when discussing `Follows/*`.
    
    *   E.g. In [Code 6](#code-6), statement 3 refers to `while (i!=0)` and statement 7 refers to `if (x==1)`.

[](#affects)Affects
===================

**Definition:**

    For two statements s1 and s2 in the same procedure:
    
    Affects(s1, s2) holds if:
        - s1 and s2 are assignment statements,
        - s1 modifies a variable v which is used in s2 and
        - there is a control flow path from s1 to s2 on such that v is not modified
          in any assignment, read, or procedure call statement on that path
    

**Examples:**

In [Code 6](#code-6), the following relationships hold (are true):

*   `Affects (2, 6)`
*   `Affects (4, 8)`
*   `Affects (4, 10)`
*   `Affects (6, 6)`
*   `Affects (1, 4)`
*   `Affects (1, 8)`
*   `Affects (1, 10)`
*   `Affects (1, 12)`
*   `Affects (2, 10)`
*   `Affects (9, 10)`

In particular, `Affects (1, 12)` holds because:

*   Variable `x` is modified in statement `1`,
*   Variable `x` is used in statement `12`, and
*   There is a path in the CFG (`1 > 2 > 3 > 7 > 9 > 10 > 11 > 12`) on which variable `x` is not modified by any assignment, read, or procedure call.

The following relationships are false (do not hold):

*   `Affects (9, 11)`
*   `Affects (9, 12)`
*   `Affects (2, 3)`
*   `Affects (9, 6)`

In particular, `Affects (9, 12)` does not hold because:

*   Variable `z` is modified by assignment `10` that is on any control flow path between assignments `9` and `12`.

**Notes:**

*   The `Affects` relationship models data flows in a program.
    
*   The `Affects` relationship is defined only among assignment statements and involves a variable.
    
    *   Informally, the relationship `Affects (a1, a2)` holds if the value of `v` as computed at `a1` may be used at `a2`.

**More examples:**

##### [](#code-7)Code 7

    procedure alpha {
    1.    x = 1;
    2.    if ( i != 2 ) {
    3.        x = a + 1; }
          else {
    4.        a = b; }
    5.    a = x; }
    

In [Code 7](#code-7), `Affects (1, 5)` is true because variable `x` is modified in statement `1` and used in statement `5`, and there is a path in the CFG (`1 > 2 > 4 > 5`) on which `x` is not modified in any assignment, read, or procedure call.

##### [](#code-8)Code 8

    procedure p {
    1.    x = a;
    2.    call q;
    3.    v = x; }
    

In [Code 8](#code-8), if `Modifies ("q", "x")` holds, then `Affects (1, 3)` does not hold.

If `Modifies ("q", "x")` does not hold, then `Affects (1, 3)` holds.

##### [](#code-9)Code 9

    procedure p {
    1.    x = 1;
    2.    y = 2;
    3.    z = y;
    4.    call q;
    5.    z = x + y + z; }
    
    procedure q {
    6.    x = 5;
    7.    t = 4;
    8.    if ( z > 0 ) then {
    9.        t = x + 1; }
           else {
    10.       y = z + x; }
    11.  x = t + 1; }
    

In [Code 9](#code-9),

*   `Affects (1, 5)` and `Affects (2, 5)` do not hold as both variable `x` and `y` are modified in procedure `q`.
    
    *   Even though modification of variable `y` in procedure `q` is only conditional, we will still use the definition of `Modifies` for procedures. That is, `Modifies(4, "q")` holds.
*   `Affects (3, 10)` do not hold because assignments `3` and `10` are in different procedures.
    

##### [](#code-10)Code 10

    procedure alpha {
    1.    x = 1;
    2.    call beta;
    3.    a = x; }
    
    procedure beta {
    4.    if ( i != 2 ) {
    5.        x = a + 1; }
          else {
    6.        a = b; } }
    

In [Code 10](#code-10), `Affects (1, 3)` is false because variable `x` is modified in statement `1` and used in statement `3`, but the only paths from 1 to 3 (1 > 2 > 3) contains a call statement `2` that modifies `x` according to the definition of `Modifies` for procedure calls.

##### [](#code-11)Code 11

    procedure p {
    1.    x = a;
    2.    read x;
    3.    v = x; }
    

In [Code 11](#code-11), `Affects (1, 3)` does not hold because read statement `2` modifies variable `x` on the paths from statement `1` to `3`.

[](#affects-1)Affects\*
=======================

**Definition:**

    For two statements s1 and s2 in the same procedure:
    
    Affects*(s1, s2) holds if s1 and s2 are assignment statements; and
      - Affects(s1, s2) or
      - Affects(s1, s) and Affects*(s, s2) for some statement s
    

`Affects*` is the transitive closure of `Affects`.

**Examples:**

In [Code 6](#code-6), the following relationships hold (are true):

*   `Affects* (1, 4)`
*   `Affects* (1, 10)`
*   `Affects* (1, 11)`
*   `Affects* (1, 12)`

**More examples:**

##### [](#code-12)Code 12

    procedure p {
    1. x = a;
    2. v = x;
    3. z = v; }
    

In [Code 12](#code-12),

*   Modification of variable `x` in statement `1` affects variable `v` in statement `2`,
*   Modification of variable `v` in statement `2` affects use of variable `v` in statement `3`.

Hence the following holds: `Affects (1,2)`, `Affects (2,3)` and `Affects*(1,3)`.