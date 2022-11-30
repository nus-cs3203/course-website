<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#teamwork)Teamwork
=====================

CS3203 is a team project and is evaluated as such.

Different students may contribute in different ways, according to their skills, abilities, and project plans set by the whole team.

However, all the students are expected to:

*   put similar effort into the project.
*   share equal responsibility for creating team spirit and making the teamwork as a whole.
*   deliver work products according to the project plans set by the whole team.

Should a problem arise, each student must be willing to work towards resolving the problem.

Do not hesitate to ask your consultation tutor for mediation. In the past, almost all problems could be helped if addressed early.

[](#prototype-revision)Prototype Revision
=========================================

There are two development milestones (Milestone 2 and 3) ahead, and SPA will get more complicated in time. What counts is that you succeed at the end, so set your priorities accordingly and adopt a long-term perspective.

You should analyze the outcome and feedback of Milestone 1 carefully, and think about how to revise your prototype.

It is advisable for you to spend some time (~ 1 week) revising the prototype to set up a firm foundation for the subsequent milestones. You do not have to implement any new functionality in this revision stage. Just clean, refine your design and implementation, improve design decisions.

Do not rush into extending your prototype immediately. You may need a different setup for a full-blown SPA. Assess your prototype in view of long-term development. **You may even decide to discard it and start anew.**

Here is a list of suggested activities for this revision stage:

1.  Reflect on major decisions regarding how you organize the SPA program. Adopt a simple and practical solution of how SPA components (Parser, Design Extractor, or Query Evaluator) access data stored in the PKB.
    
2.  Revise your abstract APIs (if appropriate) and concrete APIs.
    
3.  Refactor your code base using Milestone 1's feedback where appropriate.
    
4.  Propose and compare at least two alternative design decisions for each of the following:
    

*   Choice of the data representation for abstract syntax structure of SIMPLE program (e.g. AST)
    
*   Choice of the representation for different design abstractions such as `Modifies`
    
*   Strategies for Query Evaluator to process queries with multiple query clauses, which includes:
    
    *   The data structure used for storing intermediate results
        
    *   The algorithm to update the data structure storing intermediate results
        

[](#query-evaluator-implementation)Query Evaluator Implementation
=================================================================

As the increase in specification for PQL is substantial in [Advanced SPA Requirements](../advanced-spa-requirements/pql.md), here are some tips for working on Query Processing Subsystem / Query Evaluator from Milestone 2 onwards.

1.  Design the best possible Query Evaluator that can correctly evaluate any query without optimizations.
    
    *   Queries involving many clauses should be evaluated incrementally.
        
    *   Pay attention to computing and representing intermediate query results.
        
2.  Consider various optimization strategies.
    
    *   Prepare to experiment with different strategies in your Query Evaluator. You may want to consider using switches and boolean flags to employ different strategies plugged into your Query Evaluator.
3.  Consider caching intermediate query results.
    

The main factors impacting effective query evaluations are:

1.  Size of the intermediate results
    
2.  Evaluation time
    

Hence, optimizations should reduce the size of the intermediate result and/or evaluation time. You should assess and experiment with various optimization strategies before implementation.