# Run Skia Wallet on your machine

---

## Requirements for your computer

Follow the next steps to install all the necessary software:

1. Install an IDE (a software to open the code). We recommend [Visual Studio Code](https://code.visualstudio.com/). If you don't want to install anything on your computer, you can use [Gitpod](https://gitpod.io/#https://github.com/Future-Wallet/skia-wallet) or [Stackblitz](https://stackblitz.com/github/Future-Wallet/skia-wallet); if you do so, you don't need to do the next steps.
2. Install NodeJS version 16 or later. There are two ways of installing it:
   - [Easiest] Install only NodeJS version downloading from https://nodejs.org/en/download/.
   - [Recommended] Having multiple NodeJS versions and switching between them with Node Version Manager or [NVM](https://github.com/nvm-sh/nvm/blob/master/README.md#installing-and-updating).
3. Download the Skia Wallet's code on a folder of your computer. Open the terminal and go to your choosen directory and run `git clone https://github.com/Future-Wallet/skia-wallet.git`.
4. Via terminal, go inside the new folder called running `cd skia-wallet/` and install the dependencies with `npm install` or `npm i`.
5. Install on your computer Nx with `npm i -g nx`.

---

## Run the website

1. Open your terminal and go to the main directory (the folder where you have the Skia Wallet's code). It sould have the name `skia-wallet`.
2. Run `nx run ui-wallet:serve`. Nx should be running the website.
3. Open this URL `http://localhost:4200/` on your browser, a

---

## Run code inside packages `common`, `entities` and `repositories`

If you want to test part of the code located in the core layers (in the packages called `common`, `entities` and `repositories`), it could be difficult, because it's used on outside layers ([learn more about layers here](https://github.com/Future-Wallet/skia-wallet/wiki/Code-structure)).

To make your life easy, you have two ways:

- [Work in progress][Recommended] Via tests
- Via NPM. Steps:
   1. Create a file in any choosen package/layer and it needs to be inside the folder `src` that each package already has. Eg. File name `_dev.ts` located on the path `packages/repositories/src/_dev.ts`.
   2. Add a NPM command in the main `package.json` of the choosen package. Eg. located on `packages/repositories/package.json`

```json
...
"name": "@skiawallet/repositories",
"scripts": {
   ...
   "dev": "ts-node src/_dev.ts"
}
...
```

   3. Open your terminal on the root folder of Skia Wallet code and run the command using Nx. Eg. `nx <new_npm_command> <choosen_package>`. Eg. `nx dev repositories`)

---

## Advanced concepts for Nx

Nx ecosystem provides many [plugins](https://nx.dev/community#create-nx-plugin). Here are the ones you could find useful:

- The plugin and command `@nrwl/workspace` for renaming, moving, removing... the packages. More info [here](https://nx.dev/packages/workspace).
