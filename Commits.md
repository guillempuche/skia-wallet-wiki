# Commit convention

You'll need to follow the next Set of rules for creating an explicit commit history; which makes it easier to write automated tools on top of.

Every commit should follow the following structure:
_`type(scope): explaination_in_present_tense`_ or _`type(*): explanation_in_present_tense`_ (the last structure is when there's a cross change of none/multiple packages and multiple contexts)

There are a small list of types (below there's the list) and packages (their names are in the code folder `packages`).

## List of types with examples

- **fix**: patches a bug in the codebase.

_fix(entities): add the validation for `privateKey` when creating a new account_

- **feat**: introduce a new feature to the codebase.

_feat(repo): change default branch_
_feat(ui-components): add a progress button_

- **refactor**: restructure the code, change the internal behavior of method.

_refactor(ui-repositories): split the code on multiple methods for the swap functionality_
_refactor(*): delete the creation of an instance in the API_ (asterisk * means multiple packages have been touched)

_refactor(entities): change name of a method `getValue`_

- **improv**: an improvement of the code without fixing the code and doing a refactor
- **docs**: documentation like comments, explanation for classes, methods, etc.

_docs(common): add explanation for class `Error`_
_docs(*): add the section Contribute_

- **chore**: periodic management task, like upgrading the dependencies, uploading documentation, etc.
- **build**: edit the debugging flow (eg. [VSCode configuration](https://github.com/Future-Wallet/skia-wallet/blob/main/.vscode)), modify git configuration (eg. the file [`.gitignore`](https://github.com/Future-Wallet/skia-wallet/blob/main/.gitignore)), modify the build process, edit scripts, add new NPM commands, etc.

## List of scopes

- *entities*: Anything related to the package Entities
- *entities*: Anything related to the package Entities
- *entities*: Anything related to the package Entities