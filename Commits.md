# Commit convention

You'll need to follow the next Set of rules for creating an explicit commit history; which makes it easier to write automated tools on top of.

Every commit should follow the following structure:
_`type(none or package_name | * when there are multiple packages, max three contexts or *): explaination_in_present_tense`_ or _`type(*): explaination_in_present_tense`_ (the last structure is when there's a cross change of none/multiple packages and multiple contexts)

There are a small list of types (below there's the list) and packages (their names are in the code folder `packages`).

---

## List of types with examples

- **fix**: patches a bug in the codebase.

_fix(entities, account, wallet): add the validation for `privateKey` when creating a new account_

- **feat**: introduce a new feature to the codebase.

_feat(nx): change default branch_ (it excludes the package name because it changes a file on the project general configuration)

_feat(ui-components, progress button): add a progress button_

- **refactor**: restructure the code, change the internal behavior of method.

_refactor(ui-repositories, swap): split the code on multiple methods_
_refactor(*, api): delete the creation of an instance_ (asterisk * means multiple packages have been touched)

_refactor(entities, *): change name of a method_ (asterisk * means many contexts have been refactored)

- **improv**: an improvement of the code without fixing the code and doing a refactor
- **docs**: documentation like comments, explanation for classes, methods, etc.

_docs(ui-common, error): add explanation for class `Error`_
_docs(readme): add the section Contribute_

- **chore**: periodic management task, like upgrading the dependencies, uploading documentation, etc.
- **build**: debugging (like edit [VSCode configuration](https://github.com/Future-Wallet/skia-wallet/blob/main/.vscode)), modify git configuration like the file [`.gitignore`](https://github.com/Future-Wallet/skia-wallet/blob/main/.gitignore), modify the build process, add new NPM commands, etc.
