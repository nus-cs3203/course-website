[](#1-kicking-off)1\. Kicking Off
=================================

At the start, your team can set up an overall plan for the prototype development.

[](#11-project-errands)1.1. Project Errands
-------------------------------------------

1.  Schedule Sprint 0 planning meeting
    
2.  Agenda
    
    *   Allocate responsibilities to team members
        
    *   Set up communication channels
        
    *   Agree on project management tool (Jira or GitHub)
        
    *   If Jira, add Lecturer, TA and tutor to the account
        
3.  Fix time for a weekly meeting within the team
    
4.  Fix a 15 mins slot for standup (may be 3-4 days after tutorial where you have one standup happening with tutor so that these are equally spanned)
    
5.  Fix a time (immediately after demo with tutor on last day of sprint) for sprint retrospective (20 mins will be good)
    

[](#12-within-the-sprints)1.2. Within the Sprints
-------------------------------------------------

1.  Spend time on understanding about SPA
    
2.  Spend time for research on Design considerations (which design pattern could be good and why)
    
3.  Come up with abstract APIs for the PKB and Query Evaluator
    
4.  Then, your team can:
    
    *   Consider writing system acceptance test cases before development
        
        *   Your SPA prototype should pass these system acceptance tests at the end of the iteration.
            
        *   Each test case should consist of a SIMPLE source code, a set of PQL queries, with test description and their expected results.
            
    *   Develop SPA prototype
        
    *   Run system acceptance tests
        
    *   Document details as you evolve (for report)
        

[](#13-important-notes-for-successful-outcome)1.3. Important Notes for Successful Outcome
-----------------------------------------------------------------------------------------

1.  A feature is considered as developed and completed only when it is fully tested.
    
    *   Write as many unit tests as needed for each function written.
        
    *   Automate running test cases (saves lots of time).
        
2.  Peer review is very important
    
    *   Any code written MUST be peer reviewed for code quality, adherence to coding guidelines etc.

[](#2-testing)2\. Testing
=========================

We will test your SPA at the end of the project using AutoTester. Plan for testing (and other quality controls such as reviews), and let quality control be an integral part of your implementation approach.

*   Plan to spend 1/3 of your effort on testing.
    
*   Store your test cases so that you can replay them as needed. Use **AutoTester** for regression testing (automatically replaying system tests).
    
*   Write system acceptance tests before you start development of the SPA prototype.
    
*   Test your Source Processor on a variety of SIMPLE source code.
    
*   Test each type of query conditions (g., Follows, Modifies and pattern) and various combinations of query conditions (e.g., Follows + pattern and Modifies + pattern) that are allowed in the prototype.
    
*   Write unit and integration tests as soon as you start writing code. Some tests may be written even before you write code, based on the given requirements.
    

[](#3-checklist---source-processor--pkb)3\. Checklist - Source Processor / PKB
==============================================================================

For the source parser:

1.  Consider implementing a predictive (recursive descent, top-down) parser, which is the simplest solution for the prototype.
    
2.  Print meaningful error messages and **terminate parser execution** when it encounters the first error in the source program.
    
    *   Query evaluation should not be allowed.
    *   Production parsers perform error recovery which allows them to continue parsing after finding an error. Your parser does not have to do error recovery.
3.  Use the sample SIMPLE source code from [Downloads](Downloads) to verify that your parser is able to parse a relatively simple source code. Note that a successful parse does not guarantee its correctness.
    

For generating VarTable:

1.  Write concrete VarTable APIs (a public interface for a class implementing VarTable), based on the abstract VarTable APIs.
    
2.  Choose a data representation for VarTable, justify your choice, and implement its public interface operations.
    

For generating AST / information related to AST:

1.  Write concrete AST APIs (a public interface for a class implementing AST), based on the abstract AST APIs.
    
2.  Implement AST generation actions in polynomial time (PTIME).
    
3.  Choose a data representation for AST, justify your choice, and implement its public interface operations.
    
    *   Consider the need of storing an explicit AST in the PKB. Are there any other data representation that can capture information stored in an AST?

For generating information related to Modifies and Uses relationships between statements:

1.  Write concrete Modifies APIs (a public interface for a class implementing Modifies/Uses), based on the abstract Modifies/Uses APIs.
    
2.  Choose a data representation for Modifies, justify your choice, and implement its public interface operations.
    
3.  Repeat the above steps for Uses relationship.
    

[](#4-checklist---query-processing-subsystem)4\. Checklist - Query Processing Subsystem
=======================================================================================

1.  Write down validation rules for queries.
    
2.  Split Query Processor into Query Preprocessor and Query Evaluator.
    
    *   Query Preprocessor parses and validates each query, and then generates an internal representation (e.g., a query tree) for the query.
    *   Query Evaluator receives the internal representation of the query and evaluates it, consulting PKB when necessary.
3.  Implement Query Preprocessor in a similar way as the parser for SIMPLE.
    
4.  Implement Query Evaluator.