# Format Conventions

Apex is designed mostly based on the Java language conventions and hence we should adhere the formatting of the source to its conventions. Following list documents the each rule and at the end of the rule, there is an elaborated example covering most of these rules.

###  Indentation
Code must be indented using a tab character and where applicable tab must be set as 4 space width.

### Bracing
Brace is an either {or }.
Start Brace must be in the same line as the statement and end brace must be on its own line, completing the indentation
Correct:
```sh
if (setABCBudgetId.Size() > 0) {
   CPABC_BudgetEntryUtility.updateBudgetEntriesBalance(setAPACAMERBudgetId);
}
```
Incorrect:
```sh
ifsetABCBudgetId.Size() > 0)
{
   CPABC_BudgetEntryUtility.updateBudgetEntriesBalance(setAPACAMERBudgetId);
}
```
###	Spacing
Along with indenting, spacing is the most important visual aspect that enhances the code readability. And hence correct spacing conventions must be followed as outlined below.
- Before opening a brace, there must be a space
Correct:
```sh
if (setAPACAMERBudgetId.Size() > 0) {
   CPABC_BudgetEntryUtility.updateBudgetEntriesBalance(setAPACAMERBudgetId);
}
```
Incorrect:
```sh
if (setAPACAMERBudgetId.Size() > 0){
   CPABC_BudgetEntryUtility.updateBudgetEntriesBalance(setAPACAMERBudgetId);
}
```
- There should NOT be any space before a comma and there must be a space SFAter a comma (except if that comma is part of a String literal.
Correct:
- String address, street, city;

Incorrect:
-	String address , street , city;
-	String address,street,city;

- There should a space SFAter every end-parenthesis ((), and there should NOT be a space before start parenthesis ()).

- There must be a space SFAter every binary operator (if it is unary operator, space must not be used).
Correct:
```sh
Integer a, b, c;
c = a + b;
c++;
```
Incorrect:
```sh
Integer a, b, c;
c = a+b;
c ++;
```
### Blank Lines
- Blank line must be added before starting and SFAter ending a code block (block of code is code enclosed within {} braces), unless it is a } else if { condition, and in such case, there must not be any blank line.
- There must be maximum one or two blank lines.
###	Line Wrapping
- All code lines must be wrapped to around 100 characters. This is a guideline so even if it exceeds by +/- 10 characters, it must be fine.

