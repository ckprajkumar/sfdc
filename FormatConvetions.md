# Format Conventions

Apex is designed mostly based on the Java language conventions and hence we should adhere the formatting of the source to its conventions. Following list documents the each rule and at the end of the rule, there is an elaborated example covering most of these rules.

###  Indentation
Code must be indented using a tab character and where applicable tab must be set as 4 space width.

### Bracing
Brace is an either {or }.
Start Brace must be in the same line as the statement and end brace must be on its own line, completing the indentation
```sh
Correct:
if (setABCBudgetId.Size() > 0) {
   CPABC_BudgetEntryUtility.updateBudgetEntriesBalance(setAPACAMERBudgetId);
}
```
`Incorrect:`
`ifsetABCBudgetId.Size() > 0)`
`{`
  ` CPABC_BudgetEntryUtility.updateBudgetEntriesBalance(setAPACAMERBudgetId);`
`}`
