<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#example-queries)Example Queries
===================================

In this section, we will use [Code 6](#code-6) to answer some queries.

For simplicity, procedures `First` and `Third` are excluded from statement numbering.

Note that for each scenario, there are multiple ways to write a query.

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


[](#meaningful-queries)Meaningful Queries
=========================================

**Q1. What are the procedures in the program call another procedure?**

    procedure p, q;
    Select p such that Calls (p, _)


or

    procedure p, q;
    Select p.procName such that Calls (p, q)


Answer: `First, Second`

Note that we can select attributes in the select clause.

Wildcard `_` can be used as a placeholder for an unconstrained design entity (procedure in this case). It can only be used when the context uniquely implies the type of the argument denoted by `_`. In this case, we infer from the program design model that `_` stands for the design entity "procedure".

**Q2: Which procedures directly call procedure "Third" and modify the variable "i"?**

    procedure p, q;
    Select p such that Calls (p, q) with q.procName = "Third" such that Modifies (p, "i")


or

    procedure p;
    Select p such that Calls (p, "Third") and Modifies (p, "i")


Answer: `Second`

**Q3: Which procedures call procedure "Third" directly or indirectly?**

    procedure p;
    Select p such that Calls* (p, "Third")


Answer: `First, Second`

**Q4: Which procedures are called from "Second" in a while loop?**

    procedure p; call c; while w;
    Select p such that Calls ("Second", p) and Parent (w, c) with c.procName = p.procName


Answer: `Third`

**Q5: Is there an execution path from statement 2 to statement 9?**

    Select BOOLEAN such that Next* (2, 9)


Answer: `TRUE`

**Q6: Is there an execution path from statement 2 to statement 9 that passes through statement 8?**

    Select BOOLEAN such that Next* (2, 8) and Next* (8, 9)


Answer: `FALSE`

**Q7: Find assignments to variable "x" located in a loop, that can be reached (in terms of control flow) from statement 1.**

    assign a; while w;
    Select a pattern a ("x", _) such that Parent* (w, a) and Next* (1, a)


or

    assign a; while w;
    Select a such that Modifies (a, "x") and Parent* (w, a) and Next* (1, a)


Answer: `4`

**Q8: Which statements can be executed between statement 5 and statement 12?**

    stmt s;
    Select s such that Next* (5, s) and Next* (s, 12)


Answer: `3, 4, 5, 6, 7, 8, 9, 10, 11`

**Q9: Which assignments directly affect value computed at assignment 10?**

    assign a;
    Select a such that Affects (a, 10)


Answer omitted.

<!--
**Q9: Which assignments directly or indirectly affect value computed at assignment 10?**

    assign a;
    Select a such that Affects* (a, 10)


Answer: `1, 2, 4, 6, 8, 9`
-->

**Q10. Which are the pair of assignments that affect each other?**

    assign a1, a2;
    Select <a1, a2> such that Affects (a1, a2)


or

    assign a1, a2;
    Select <a1.stmt#, a2> such that Affects (a1, a2)


Answer omitted.

**Q11. Find all pairs of procedures p and q such that p calls q.**

    procedure p, q;
    Select <p, q> such that Calls (p, q)


Answer: `First Second, Second Third`

[](#query-formation)Query Formation
===================================

The following queries do not refer to a SIMPLE program source code. They are meant to help you understand how to use PQL.

**Q12. Find all statements whose statement number is equal to some constant.**

    stmt s; constant c;
    Select s with s.stmt# = c.value


**Q13. Find procedures whose name is the same as the name of some variable.**

    procedure p; variable v;
    Select p with p.procName = v.varName


**Q14. Find statements that is followed by statement 10.**

    stmt s, s1;
    Select s.stmt# such that Follows* (s, s1) with s1.stmt#=10


**Q15. Find three while loops nested one in another.**

    while w1, w2, w3;
    Select <w1, w2, w3> such that Parent* (w1, w2) and Parent* (w2, w3)


Note that the query returns all the instances of three nested while loops in a program, not just the first one that is found.

**Q16. Find all assignments with variable "x" at the left-hand side located in some while loop, and that can be reached (in terms of control flow) from statement 60.**

    assign a; while w; stmt s;
    Select a such that Parent* (w, a) and Next* (60, s) pattern a("x", _) with a.stmt# = s.stmt#


**Q17. Find all while statements with "x" as a control variable.**

    while w;
    Select w pattern w ("x", _)


**Q18. Find assignment statements where variable x appears on the left hand side.**

    assign a;
    Select a pattern a ("x", _)


**Q19. Find assignments with expression x\*y+z on the right hand side.**

    assign a;
    Select a pattern a (_, "x * y + z")


**Q20. Find assignments in which x\*y+z is a sub-expression of the expression on the right hand side.**

    assign a;
    Select a pattern a (_, _"x * y + z"_)


**Q21. Find all assignments to variable "x" such that value of "x" is subsequently re‑assigned recursively in an assignment statement that is nested inside two loops.**

    assign a1, a2; while w1, w2;
    Select a2 pattern a1 ("x", _) and a2 ("x", _"x"_)
              such that Affects (a1, a2) and Parent* (w2, a2) and Parent* (w1, w2)


[](#meaningless-queries)Meaningless Queries
===========================================

SPA should be able to evaluate any syntactically and semantically valid query, even if some of these queries do not have meaning or make little sense.

**Q22. Is the value 12 equals to 12?**

    Select BOOLEAN with 12 = 12


Answer: `TRUE`

**Q23. Is statement 12 an assignment?**

    assign a;
    Select BOOLEAN with a.stmt# = 12


**Q24. If statement 12 is an assignment statement, find all the assignment statements.**

    assign a, a1;
    Select a1 with a.stmt# = 12


or

    assign a, a1;
    Select a1 with 12 = a.stmt#


Note that it is possible to swap the values to be compared around.

**Q25. If procedure "Second" calls procedure "Third", find all the combinations of an assignment-while statement pairs.**

    assign a; while w;
    Select <a, w> such that Calls ("Second", "Third")


**Q26. If there exist an assignment that modifies the variable "y", find all the combinations of two-procedures pair.**

    procedure p, q; assign a;
    Select <p, q> such that Modifies (a, "y")


**Q27. Is there a procedure that calls some other procedure in the program?**

    Select BOOLEAN such that Calls (_,_)
