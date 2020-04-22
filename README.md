# Configure StandardJS + eslint on VSC for Vue projects

## Configuration files

### Settings.json (VSC settings)

```json
{
  // ===
  // Spacing
  // ===

  "editor.insertSpaces": true,
  "editor.tabSize": 2,
  "editor.trimAutoWhitespace": true,
  "files.trimTrailingWhitespace": true,
  "files.eol": "\n",
  "files.insertFinalNewline": true,
  "files.trimFinalNewlines": true,

  // ===
  // Event Triggers
  // ===

  // This setup explicitly uses prettier extension for html files, and disables auto format on saving in general. Then, explicitly enables auto format on save while using prettier-standard extension for .js files.

  // What formats what
  // Prettier extension: html, scss and css files. Template and style tags in .vue files. Formatted with alt+shift+F (ctrl+shift+I on Ubuntu)
  // Prettier-StandardJS extension: js files (with alt+shift+F, ctrl+shift+I on Ubuntu). Script tags in .vue files (on save)

  "editor.formatOnType": false,
  "editor.formatOnSave": false,

  // Sets prettier as default formatter for any file, overwriting later as needed
  "editor.defaultFormatter": "esbenp.prettier-vscode",

  // Disables VSC JavaScript syntax validation
  "javascript.validate.enable": false,

  "eslint.validate": [
    "vue",
    "javascript",
    "javascriptreact",
    "vue-html",
    "html"
  ],
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "javascript.updateImportsOnFileMove.enabled": "always",

  // ===
  // Vetur config
  // https://vuejs.github.io/vetur/setup.html#vs-code-config
  // ===

  // Disables the default template validation of Vetur, used when a specific eslint configuration is required for .vue templates.
  "vetur.validation.template": false,

  "vetur.format.defaultFormatter.html": "prettier",
  "vetur.format.defaultFormatter.css": "prettier",
  "vetur.format.defaultFormatter.scss": "prettier",

  // Prevents formatting the script block of .vue files when using the shortcut to autoFormat. Ideally, the default formatter would be set to prettier-standard, but the option is not supported by Vetur as of now.
  "vetur.format.defaultFormatter.js": "none",
  "vetur.format.defaultFormatter.ts": "none",

  // ===
  // Config by language
  // ===

  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascript]": {
    // Format .js files AND scripts in .vue files on save
    // Format .js files (but NOT scripts in .vue files) with autoformat shortcut
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "numso.prettier-standard-vscode"
  },
  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[scss]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[css]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[jsonc]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[sql]": {
    "editor.defaultFormatter": "mtxr.sqltools"
  },
  "[yaml]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[markdown]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.wordWrap": "wordWrapColumn",
    "editor.wordWrapColumn": 80
  }
}
```

This setup explicitly uses prettier extension for html files, and disables auto format on saving in general. Then, explicitly enables auto format on save while using prettier-standard extension for .js files.

`"javascript.validate.enable": false`

Disables VSC JavaScript syntax validation

`"vetur.validation.template": false`

Disables the default template validation of Vetur, used when a specific eslint configuration is required for .vue templates.

`"vetur.format.defaultFormatter.js": "none"`

Prevents formatting the script block of .vue files when using the shortcut alt+shift+F (autoFormat). Ideally, the default formatter would be set to prettier-standard, but the option is not available.

An alternative file `settings-no-autoformatting.json` settings will not autoformat the `<template>` block of `.vue` files.

### .eslintrc.js

```javascript
module.exports = {
  root: true,
  env: {
    node: true
  },
  extends: ["standard", "plugin:vue/essential"],
  rules: {
    "no-console": process.env.NODE_ENV === "production" ? "error" : "off",
    "no-debugger": process.env.NODE_ENV === "production" ? "error" : "off"
  },
  parserOptions: {
    parser: "babel-eslint"
  }
};
```

## Required VSC extensions

- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- [Prettier-Standard - JavaScript formatter](https://marketplace.visualstudio.com/items?itemName=numso.prettier-standard-vscode)
- [Prettier - Code Formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- [Vetur](https://marketplace.visualstudio.com/items?itemName=octref.vetur)

## Required packages

- Standard `npm install standard --save-dev`
- Eslint-plugin-vue `npm install --save-dev eslint eslint-plugin-vue` (already installed if you use [vue cli](https://cli.vuejs.org/) default settings to create a project)

## Setup and usage

1. Install the above extensions in VSC
2. Install the required packages (`npm i`)
3. Access the user settings.json in VSC and add the above options, or use a project specific config (copy and paste the `.vscode` folder in the root of your project)
4. Copy / paste or add the `.eslintrc.js` file in the root folder
5. To format a `.js` file, or the `template` and `style` tags of a `.vue` file, use the shortcut alt+shift+F (windows) / ctrl+shift+I (ubuntu)
6. Formatting:
   - To format any non `.vue` file, and template and style blocks in `.vue` files. Use the shortcut of VSC to format a document (`ctrl+shift+I`).
   - To format script blocks in `.vue` files, save.
   - In a few cases, auto formatting of `html` blocks might be undesirable, for example when using template literals (new lines might be added where not wanted). In these cases, there are two options:
     - Disable any formatting on the next `html` element with `<!-- prettier-ignore-attribute -->`
     - Disable any formatting ONLY on the attributes of the next `html` element with `<!-- prettier-ignore-attribute -->`
     - It's also possible to disable prettier for a specific attribute, e.g.:

```html
<!-- prettier-ignore-attribute :href -->
<a
  :href="`https://${webConfig.view_contact_page.body.company_website}/`"
  :title="`Website: ${webConfig.view_contact_page.body.company_website}`"
>
  {{ webConfig.view_contact_page.body.company_website }}</a
>
```

## Considerations

- Auto formatting doesn’t mean that the code will always be formatted in the same way regardless of the starting code.
  - In function declarations inside .vue files where more than one space precedes the opening parenthesis of the arguments (e.g. `function doSomething (...args) {/* code */}`), the extra space will not be automatically trimmed.
  - If the text content of a html tag is placed immediately after the “>” of the opening tag, auto format will not place the text content in the next line automatically. The same applies if the closing tag “</” is placed right after the element text content.
- Beware of any existing configuration files that may be interfering with the desired setup (.prettierrc, .eslintrc.js in sub folders, .eslintrc, eslint or prettier configurations in general configuration files such as package.json...).
- Open VSC from whatever folder contains the package.json file. If your project contains both backend and frontend, and the frontend is contained in a subfolder, you must open the frontend subfolder with VSC, NOT the parent folder of the entire project.
- I recommend to add a `.prettierrc.js` file to the root of the project with:

```javascript
module.exports = {
  bracketSpacing: true,
  endOfLine: 'lf',
  htmlWhitespaceSensitivity: 'strict',
  printWidth: 80,
  proseWrap: 'never',
  quoteProps: 'as-needed',
  semi: false,
  singleQuote: false,
  tabWidth: 2,
  trailingComma: 'es5',
  useTabs: false
}
```

## What formats what

- Prettier extension: html, scss and css files. Template and style tags in .vue files. Formatted with alt+shift+F (ctrl+shift+I on Ubuntu)
- Prettier-StandardJS extension: js files (with alt+shift+F, ctrl+shift+I on Ubuntu). Script tags in .vue files (on save)
