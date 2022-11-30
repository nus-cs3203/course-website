<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#example-queries)Example Queries
===================================

In this section, we will use [Code 5](#code-5) to answer some queries.

For simplicity, procedures `main`, `readPoint` and `printResults` are excluded from statement numbering.

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
    

[](#queries-with-no-such-that-and-pattern-clause)Queries with no such-that and pattern clause
=============================================================================================

**Q1. What are the procedures in the program?**

    procedure p;
    Select p
    

The declaration `procedure p` refers to entities of type procedures found in the SIMPLE program that is analyzed. This query returns as a result all the procedures in the program. The results are displayed as a list of procedure names.

With respect to [Code 5](#code-5), the results of evaluating the query are: `main, readPoint, printResults, computeCentroid`

The order of the names of the procedures does not matter. Hence, another correct result is: `printResults, main, readPoint, computeCentroid`

**Q2. What are the variables in the program?**

    variable v;
    Select v
    

The query returns all variable names: `flag, count, cenX, cenY, x, y, normSq`

[](#queries-with-one-such-that-clause)Queries with one such-that clause
=======================================================================

**Q3. Which statements follow assignment 6 directly or indirectly?**

    stmt s;
    Select s such that Follows* (6, s)
    

Answer: `7, 8, 9`

**Q4. Which variables have their values modified in statement 6?**

    variable v;
    Select v such that Modifies (6, v)
    

Answer: `count`

**Q5. Which variables are used in assignment 14?**

    variable v;
    Select v such that Uses (14, v)
    

Answer: `cenX, cenY`

**Q6. Which procedures modify variable "x"?**

    variable v; procedure p;
    Select p such that  Modifies (p, "x")
    

Answer: `main, computeCentroid, readPoint`

**Q7. Find assignments within a loop.**

    assign a; while w;
    Select a such that Parent* (w, a)
    

Answer: `6, 7, 8`

**Q8. Which is the parent of statement #7?**

    stmt s;
    Select s such that Parent (s, 7)
    

Answer: `5`

[](#queries-with-one-pattern-clause)Queries with one pattern clause
===================================================================

With reference to [Code 5](#code-5), the following questions can be translated into PQL queries:

**Q9. Find assignments that contain expression count + 1 on the right hand side**

    assign a;
    Select a pattern a ( _ , "count + 1")
    

Answer: `6`

**Q10. Find assignments that contain sub-expression cenX \* cenX on the right hand side and normSq on the left hand side**

    assign a;
    Select a pattern a ( "normSq" , _"cenX * cenX"_)
    

Answer: `14`

Same query can be written using a different variable name:

    assign newa;
    Select newa pattern newa ( "normSq" , _"cenX * cenX"_)
    

Answer: `14`

[](#queries-with-one-pattern-clause-and-one-such-that-clause)Queries with one pattern clause and one such-that clause
=====================================================================================================================

**Q11: Find while loops with assignment to variable "count"**

    assign a; while w;
    Select w such that Parent* (w, a) pattern a ("count", _)
    

Answer: `5`

**Q12. Find assignments that use and modify the same variable**

    assign a; variable v;
    
    Select a such that Uses (a, v) pattern a (v, _)
    

Answer: `6, 7, 8, 12, 13`

Note that many questions can be correctly translated in multiple ways into PQL. Moreover, results returned by a query must satisfy all conditions (clauses) of the query at the same time. Changing the order of conditions (clauses) in a query does not change the query result; but changing the order of conditions may affect query evaluation time. For example:

**Q13: Find assignments that use and modify the variable "x"**

    assign a;
    Select a pattern a ("x", _) such that Uses (a, "x")
    

or

    assign a;
    Select a such that Uses (a, "x") pattern a ("x", _)
    

Answer: `none`

**Q14: Find assignments that are nested directly or indirectly in a while loop and modify the variable "count"**

    assign a; while w;
    Select a such that Parent* (w, a) pattern a ("count", _)
    

or

    assign a; while w;
    _Select a pattern a ("count", _) such that Parent* (w, a)
    

Answer: `6`