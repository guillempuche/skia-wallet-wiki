# Commit convention

You'll need to follow the next Set of rules for creating an explicit commit history; which makes it easier to write automated tools on top of.

Every commit should follow the following structure:
_`type(package_name, context or *): explaination_in_present_tense`_

There are a small list of types (below there's the list) and packages (their names are in the code folder `packages`).

---

## List of types with examples

- **fix**: patches a bug in the codebase.

Eg. ```fix(entities, account): add the validation for `privateKey` when creating a new account```

- **feat**: introduce a new feature to the codebase.

Eg. `feat(ui-components, progress button): add a progress button`.

- **refactor**: restructure the code, change the internal behavior of method.

`refactor(ui-repositories, swap): split the code on multiple methods`
`refactor(entities, *): change name of a method` (asterisk _*_ means many different context have been refactored)

- **improv**: an improvement of the code without fixing the code and doing a refactor
- **docs**: documentation like comments, explanation for classes, methods, etc.

```docs(ui-common, error): add explanation for class `Error` ```

- **test**
- **chore**
- **style**
- **perf**
- **build**
