<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

Q: When do we need a whitespace between tokens, and when do we not need a whitespace?

A: Between two alphanumeric tokens, there must be at least one whitespace in between the tokens. E.g. `read x1` is valid as two tokens (perhaps part of a read statement with a keyword `read` and a variable name), while `readx1` should be treated as one token (such as a variable name).

Between one alphanumeric token and one non-alphanumeric token, or between two non-alphanumeric tokens, there need not be a whitespace in between the tokens. E.g. Both `x1;` and `x1 ;` can be read as two tokens `x1` and `;`. Both `;}` and `; }` can be read as two tokens `;` and `}`.  
  
  

Q: Does every semicolon needs to be followed by a newline in SIMPLE? For example, is this valid?

    procedure A {
        x = 1; y = 3; z = 5;
    }
    

A: No, every semicolon do not need to be followed by a newline in SIMPLE. In this case, the program is valid. In fact, statement 1 refers to "x = 1;", statement 2 refers to "y = 3;" and so on. The statement number is incremented every time a new statement is encountered.  
  
  

Q: Does every semicolon needs to be followed by a newline in PQL? For example, is this valid?

    assign a;
    variable v;
    Select a pattern a(v, _)
    

A: No, semicolons do not need to be followed by a newline in PQL queries. However, bear in mind that due to AutoTester limitations, queries (including the declaration itself), can only span 2 lines. This means the query can only look like:

    assign a; variable v;
    Select a pattern a(v, _)
    

or

    assign a;
    variable v; Select a pattern a(v, _)
    

  
  
  

Q: Can a statement or query have multiple whitespace spread over multiple lines? For example, is this valid?

    procedure A {
        x =
              1      +
      y
    ;
    }
    

    assign a;
    Select a pattern a(_, "1                +   y")
    

A: Yes, it is valid. You should ensure that your parser is able to parse this by recognizing all types of whitespace. In this case, statement 1 refers to "x = 1 + y;", and the query will be valid and returns the answer 1.  
  
  

Q: According to SIMPLE language rules, procedures have no nesting. Does "no nesting" mean that procedures cannot be defined within other procedures, or does it mean something else?

A: Yes. For example, this is an invalid program.

    procedure A {
        procedure B {
            read num 1; } }
    

  
  
  

Q: Is it grammatically correct to print a procedure? Can we use keywords for procedure names and variable names?

    procedure procedure {
        print procedure; }
    

A: Concrete Syntax grammar (CSG) for SIMPLE program states that the print statement has the syntax of "print var\_name". In this case, "print procedure" is valid and it will be parsed as a variable name instead of a procedure name. An additional note is that "procedure names can be the same as variable names" as stated in the grammar rules as well. Note that keywords can also be used as variable names and procedure names as well.  
  
  

Q: Is it grammatically correct to use keywords for synonym names? For example:

    assign pattern; variable Select;
    Select Select pattern pattern(Select, _)
    

A: Yes, it is valid and your parser should not fail to parse this query.  
  
  

Q: What should I do if the system faces an invalid SIMPLE source code?

A: Terminate your program without evaluating any queries. You may also choose to display helpful messages in the CLI for your ease of debugging.  
  
  

Q: For Parent\*(s1, s2), may I clarify the precise meaning for "nested"? For example, can I say stmt 2 is directly nested inside ProcB, and indirectly nested inside ProcA?

    procedure ProcA {
    1.  call ProcB; }
    procedure ProcB {
    2.  read a;
    3.  print a; }
    

A: No. In full SPA terminology, the word nested refers to the fact that s1 is a container statement and s2 appears directly (or indirectly) in the statement list of s1.  
  
  

Q: What is the maximum size / length of digits / characters for INTEGER and NAME are?

A: You can make reasonable assumptions about them.  
  
  

Q: What is the maximum level of nesting in SIMPLE?

A: You can make reasonable assumptions about them.  
  
  

Q: Can we use an SQL database to answer queries?

A: No. 