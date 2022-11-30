<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#spa-correctness-grading)SPA Correctness Grading
===================================================

We will use AutoTester to test your submission. Ensure that your code is integrated correctly.

The following are the type of test cases that will be used for evaluation.

*   **Milestone 1**: Source program with < 100 lines, with ~500 (mostly syntactically valid queries).
*   **Milestone 2 & 3**: Source program with < 500 lines, with ~500 (mostly syntactically valid queries).

You can assume that integer size used in the test cases will be in the 32-bit unsigned bit range i.e.(`0` to `2,147,483,647`).

Failed test cases are analyzed, and are categorized as unique errors with different severity.

*   E.g. If a system outputs an extra punctuation in the output, which causes multiple test cases to fail, it is still counted as one minor error since it is just an output (but not logical) issue.
    
*   E.g. If a system goes into an infinite loop for a particular type of queries due to a logical issue, it is counted as one major error.
    

If your implementation does not return answers for a query after **5000ms**, we will assume that your system is stuck in a loop, and the test case will be considered as failed.

Failed test cases will be discussed at your first consultation after the submission.

[](#spa-efficiency-grading)SPA Efficiency Grading
=================================================

In **Milestone 3**, we will evaluate your system's efficiency that looks at optimization of your SPA. The key idea is to award careful implementation of optimization strategies while balancing the different trade-offs.

In addition, to discourage premature optimization at the expense of functionality, teams will only be considered marks for efficiency **if the SPA get 75% or more of the queries correct**.

The following metrics are used to determine the efficiency of your system:

1.  Time used for query processing (parsing + evaluation)
2.  Time used for program processing (pre-computation) time
3.  Total memory used for SPA

For each metric, the performance of the cohort is ranked and grouped into 4 ratings with corresponding marks:

1.  `Amazing`: 4
2.  `Good`: 2
3.  `Decent`: 1
4.  `Meh`: 0

Your efficiency score is then computed as `min( <sum of performance of 3 metrics>, 5 )`.

*   E.g. If your team received `Good`, `Decent`, `Meh` rating for query processing time, program processing time and total memory usage respectively, you will get `min (2 + 1 + 0 , 3) = 3` marks for efficiency.

The evaluation is designed to encourage a strong performance in one category and decent performance in others.

The following are the distribution of ratings:

1.  `Amazing`: 15%
2.  `Good`: 40%
3.  `Decent`: 30%
4.  `Meh`: 10%

**The 75% threshold for efficiency consideration, and the distribution of ratings, are subjected to change based on actual performance of the class without prior notice.**

[](#report-grading)Report Grading
=================================

The report will be graded in accordance to:

1.  High-level description of the system
    
2.  Design decisions considered and made
    
3.  Evidence of following good software engineering practices and principles
    

Specifically, it will be graded according to the Report Rubrics/Template, which will be linked once available. .

[](#code-review)Code Review
===========================

Your code implementation will be reviewed according to [Code Review Checklist](../../code-review-rubrics/Code-Review-Checklist.pdf).

[](#other-notes)Other Notes
===========================

The grading process is not mark-for-mark but ranking-based.

*   Having 1 unique error from AutoTester Evaluation or 1 piece of information missing from the report does not directly cause a certain number of marks to be deducted.
    
*   The issues identified will be used to rank the submissions and the marks will be derived from the ranking.
    

[](#individual-penalty--bonus)Individual Penalty / Bonus
========================================================

Individual penalty / bonus is possible if someone in the team is making very little / a lot of contributions.

Please let the teaching team know about such cases through the **peer review** which will be open right after each deadline. We will arrange a meeting with your team to discuss about such matters.