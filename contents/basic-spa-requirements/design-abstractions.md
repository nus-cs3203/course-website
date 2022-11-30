<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>


[](#design-abstractions)Design Abstractions
===========================================

Program design abstractions are relationships among [program design entities](design-entities.html).

In this section, we will use [Code 5](#code-5) to explain the design abstractions. For simplicity, procedures `main`, `readPoint` and `printResults` are excluded from statement numbering.

##### [](#code-5)Code 5

        procedure main {
            flag = 0;
            call computeCentroid;
            call printResults;
        }
        procedure readPoint {
            read x;
            read y;
        }
        procedure printResults {
            print flag;
            print cenX;
            print cenY;
            print normSq;
        }
        procedure computeCentroid {
    01      count = 0;
    02      cenX = 0;
    03      cenY = 0;
    04      call readPoint;
    05      while ((x != 0) && (y != 0)) {
    06          count = count + 1;
    07          cenX = cenX + x;
    08          cenY = cenY + y;
    09          call readPoint;
            }
    10      if (count == 0) then {
    11          flag = 1;
            } else {
    12          cenX = cenX / count;
    13          cenY = cenY / count;
            }
    14      normSq = cenX * cenX + cenY * cenY;
        }
    

[](#basic-design-abstractions)Basic Design Abstractions
=======================================================

The table shows a summary of the design abstractions discussed in this section.
|                 Design Abstraction                 |       Relationship between       |
|:--------------------------------------------------:|:--------------------------------:|
| <a href="#follows--follows">Follows / Follows*</a> | Statements                       |
| <a href="#parent--parent">Parent / Parent*</a>     | Statements                       |
| <a href="#uses">Uses</a>                           | Statement/Procedure and Variable |
| <a href="#modifies">Modifies</a>                   | Statement/Procedure and Variable |

[](#follows--follows)Follows / Follows\*
========================================

**Definition:**

    For any statements s1 and s2:
    
    Follows(s1, s2) holds if they are at the same nesting level, in the same
    statement list (stmtLst), and s2 appears in the program text immediately after
    s1.
    
    Follows*(s1, s2) holds if
        - Follows(s1, s2) or
        - Follows(s1, s) and Follows*(s, s2) for some statement s
    

`Follows*` is the transitive closure of `Follows`.

**Examples:**

In procedure `computeCentroid` ([Code 5](#code-5)), the following relationships hold (are true):

*   `Follows (1, 2)`
*   `Follows (4, 5)`
*   `Follows (5, 10)`
*   `Follows* (3,10)`
*   `Follows* (1, 14)`

In contrast, the following relationships are false (do not hold):

*   `Follows (5, 6)`
*   `Follows (9, 10)`
*   `Follows (11, 12)`
*   `Follows* (12, 14)`

**Notes:**

*   For `Follows (4, 5)`, statement number `5` refers to the whole while-statement, including lines 5-9, rather than to the program line `while ((x != 0) && (y != 0)) {` only.
*   Similarly, statement number `10` refers to the whole if-statement including lines 10-13.

It is useful to check which statement list directly contains the statement mentioned in a relationship.

[](#parent--parent)Parent / Parent\*
====================================

**Definition:**

    For any statements s1 and s2:
    
    Parent(s1, s2) holds if s2 is directly nested in s1.
    
    Parent*(s1, s2) holds if
        - Parent(s1, s2) or
        - Parent(s1, s) and Parent*(s, s2) for some statement s
    

`Parent*` is the transitive closure of `Parent`.

**Examples:**

In procedure `computeCentroid` ([Code 5](#code-5)), the following relationships hold (are true):

*   `Parent (5, 7)`
*   `Parent (5, 8)`
*   `Parent (10, 11)`
*   `Parent (10, 13)`
*   `Parent* (5, 7)`
*   `Parent* (10, 13)`

The following relationships are false (do not hold):

*   `Parent (2, 3)`
*   `Parent (4, 7)`
*   `Parent (9, 5)`
*   `Parent* (10, 14)`

**Notes:**

*   As in the case of `Follows`, statement number `5` refers to the whole while-statement, and statement number `10` refers to the whole if-statement. The first statement should be a container statement, while the second statement should be nested inside the first statement.)

[](#uses)Uses
=============

**Definition:**

    For any variable v,
            assignment a,
            print stmt pn,
            container stmt (if or while) s,
            procedure call c,
            procedure p:
    
    Uses(a, v) holds if v appears on the right hand side of a.
    
    Uses(pn, v) holds if variable v appears in pn.
    
    Uses(s, v) holds if v appears in the condition of s, or
               there is a statement s1 in the container such that Uses(s1, v) holds.
    
    Uses(p, v) holds if there is a statement s in p or in a procedure called
               (directly or indirectly) from p such that Uses(s, v) holds.
    
    Uses(c, v) is defined in the same way as Uses(p, v).
    

**Examples:**

In the program ([Code 5](#code-5)), the following relationships hold (are true):

*   `Uses (7, "x")`
*   `Uses (10, "count")`
*   `Uses (10, "cenX")`
*   `Uses ("main", "cenX")`
*   `Uses ("main", "flag")`
*   `Uses ("computeCentroid", "x")`

The following relationships are false (do not hold):

*   `Uses (3, "count")`
*   `Uses (10, "flag")`
*   `Uses (9, "y")`

**Notes:**

If a number refers to statement `s` that is a procedure call, then `Uses (s, v)` holds for any variable `v` used in the called procedure (or in any procedure called directly or indirectly from that procedure).

*   E.g. `flag` is used in the print statement of procedure `printResults`; hence the call statement to `printResults` in procedure `main` uses `flag`; therefore `main` uses `flag`.

If a number refers to a container statement `s` (while or if statement), then `Uses (s, v)` holds for any variable used by any statement in the container `s` (including call statements), or used in the condition of `s`.

*   E.g. Statement 10 contains statement 12 that uses `cenX`; hence 10 uses `cenX`.

[](#modifies)Modifies
=====================

**Definition:**

    For any variable v,
            assignment a,
            read stmt re,
            container stmt (if or while) s,
            procedure call c,
            procedure p:
    
    Modifies(a, v) holds if variable v appears on the left hand side of a.
    
    Modifies(re, v) holds if variable v appears in re.
    
    Modifies(s, v) holds if there is a statement s1 in the container
                   such that Modifies(s1, v) holds.
    
    Modifies(p, v) holds if there is a statement s in p or in a procedure called
                   (directly  or indirectly) from p such that Modifies(s, v) holds.
    
    Modifies(c, v) is defined in the same way as Modifies(p, v).
    

**Examples:**

In the program ([Code 5](#code-5)), the following relationships hold (are true):

*   `Modifies (1, "count")`
*   `Modifies (7, "cenX")`
*   `Modifies (9, "x")`
*   `Modifies (10, "flag")`
*   `Modifies (5, "x")`
*   `Modifies ("main", "y")`

The following relationships are false (do not hold):

*   `Modifies (5, "flag")`
*   `Modifies ("printResults", "normSq")`

**Notes:**

If a number refers to statement `s` that is a procedure call, then `Modifies (s, v)` holds for any variable `v` modified in the called procedure (or in any procedure called directly or indirectly from that procedure).

*   E.g. `y` is modified in the call statement `call computeCentroid;` from `main` because procedure `computeCentroid` calls procedure `readPoint` that modifies `y` in a read statement.

If a number refers to a container statement `s` (while or if statement), then `Modifies (s, v)` holds for any variable modified by any statement in the container `s` (including call statements).

*   E.g. Statement 5 contains statement 9 that modifies `x` (in procedure `readPoint` read statement); hence 5 modifies `x`.