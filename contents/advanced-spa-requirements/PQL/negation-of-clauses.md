<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#negation-of-clauses)Negation of Clauses
===========================================

A clause can now be augmented with the `not` keyword to negate the set of answers that fits the synonyms used.

In this section, we will use [Code 6](#code-6) to answer some queries.

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

[](#negation-of-clauses-with-no-synonyms)Negation of Clauses with No Synonyms
===============================================================================

For a clause with no synonyms, a clause that has the result `TRUE` will return `FALSE` when negation is used, vice-versa.

**Q1**

        Select BOOLEAN such that not Calls ("First", "Second")

Answer: `FALSE`

Since `Calls ("First", "Second")` is true, then `not Calls ("First", "Second")` is false.

Therefore, the query should return `FALSE` as the answer.

**Q2**

        constant c;
        Select c with not 5 = 10

Answer: `0, 1, 2, 5`

Since `5 = 10` is false, then `not 5 = 10` is true.

Therefore, the query should return all constants `0, 1, 2, 5` as the answer.

[](#negation-of-clauses-with-one-synonym)Negation of Clauses with One Synonym
===============================================================================

For a clause with one synonym `x`, the negated clause will return the set of answers `X = A - B`, where `A` is the set of all possible answers that fits `x`, and `B` is the set of answers that fits the clause.

**Q3**

        procedure p;
        Select p such that not Calls* (p, "Third")

Answer: `Third`

There are 3 procedures for the synonym `p` to consider: `First`, `Second`, `Third`.

Since only procedure(s) `First` and `Second` fits the synonym `p` in `Calls* (p, "Third")`, then the remaining procedure(s) `Third` will fit the synonym `p` in `not Calls* (p, "Third")`.

Therefore, the query should return `Third` as the answer.

**Q4**

        procedure q;
        Select q such that not Calls* (_, q)

Answer: `First`

There are 3 procedures for the synonym `q` to consider: `First`, `Second`, `Third`.

Since only procedure(s) `Second` and `Third` fits the synonym `q` in `Calls* (_, q)`, then the remaining procedure(s) `First` will fit the synonym `q` in `not Calls* (_, q)`.

Therefore, the query should return `First` as the answer.

**Q5**

        while w;
        Select w pattern not w("y", _)

Answer: `3`

There is 1 while-stmt for the synonym `w` to consider: `3`.

Since no while-stmt(s) fits the synonym `w` in `w("y", _)`, then the remaining while-stmt(s) `3` will fit the synonym `w` in `not w("y", _)`.

Therefore, the query should return `3` as the answer.

**Q6**

        assign a;
        Select a with not 10 = a.stmt#

Answer: `1, 2, 4, 6, 8, 9, 11, 12`

As mentioned, we will ignore the statements in procedure `First` and `Third` for discussion.

There are a few assign-stmt for the synonym `a` to consider: `1, 2, 4, 6, 8, 9, 10, 11, 12`.

Since only assign-stmt(s) fits the synonym `a` in `10 = a.stmt#`, then the remaining assign-stmt(s) `1, 2, 4, 6, 8, 9, 11, 12` will fit the synonym `a` in `10 = a.stmt#`.

Therefore, the query should return `1, 2, 4, 6, 8, 9, 11, 12` as the answer.

[](#negation-of-clauses-with-two-synonyms)Negation of Clauses with Two Synonyms
===============================================================================

For a clause with two synonyms `x` and `y`, the negated clause will return the set of answers `X = A - B`, where `A` is the Cartesian Product of all possible `x` and all possible `y`, and `B` is the set of pairs of answers that fits the clause.

**Q7**

        procedure p, q;
        Select p such that not Calls* (p, q)

Answer: `First, Second, Third`

There are 9 combinations for the synonyms `p, q` to consider: `First First`, `First Second`, `First Third`, `Second First`, `Second Second`, `Second Third`, `Third First`, `Third Second`, `Third Third`.

Since only combination(s) `First Second` and `Second Third` fits the synonyms `p, q` in `Calls* (p, q)`, then the remaining combination(s) `First First`, `First Third`, `Second First`, `Second Second`, `Third First`, `Third Second`, `Third Third` will fit the synonyms `p, q` in `not Calls* (p, q)`.

Therefore, the query should return `First, Second, Third` as the answer.

**Q8**

        while w; variable v;
        Select <w, v> pattern not w(v, _)

Answer: `3 v, 3 x, 3 y, 3 z`

There are 5 combinations for the synonyms `w, v` to consider: `3 i`, `3 v`, `3 x`, `3 y`, `3 z`.

Since only combination(s) `3 i` fits the synonyms `w, v` in `w(v, _)`, then the remaining combination(s) `3 v`, `3 x`, `3 y`, `3 z` will fit the synonyms `w, v` in `not w(v, _)`.

Therefore, the query should return `3 v, 3 x, 3 y, 3 z` as the answer.

**Q8**

        while w; constant c;
        Select c with not c.value = w.stmt#

Answer: `0, 1, 2, 5`

Do think about how the answer can be derived.
