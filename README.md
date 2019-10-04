# Configure StandardJS + eslint on VSC for Vue projects

## Configuration files

### Settings.json (VSC settings)

```json
{
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "editor.formatOnType": false,
  "editor.formatOnSave": false,
  "[javascript]": {
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "numso.prettier-standard-vscode"
  },
  "javascript.validate.enable": false,
  "eslint.validate": [
    {
      "language": "vue",
      "autoFix": true
    },
    {
      "language": "javascript",
      "autoFix": true
    },
    {
      "language": "javascriptreact",
      "autoFix": true
    }
  ],
  "eslint.autoFixOnSave": true,
  "vetur.validation.template": false,
  "vetur.format.defaultFormatter.html": "prettier",
  "vetur.format.defaultFormatter.js": "none"
}
```

This setup explicitly uses prettier extension for html files, and disables auto format on saving in general. Then, explicitly enables auto format on save while using prettier-standard extension for .js files.

`"javascript.validate.enable": false`

Disables VSC JavaScript syntax validation

`"vetur.validation.template": false`

Disables the default template validation of Vetur, used when a specific eslint configuration is required for .vue templates.

`"vetur.format.defaultFormatter.js": "none"`

Prevents formatting the script block of .vue files when using the shortcut alt+shift+F (autoFormat). Ideally, the default formatter would be set to prettier-standard, but the option is not available.

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
2. Install the required packages
3. Access the user settings.json in VSC and add the above options
4. Copy / paste or add the content of the .eslintrc.js above to a .eslintrc.js file in the root folder
5. To format a .js file, or the template and style tag of a .vue file, use the shortcut alt+shift+F
6. To format the script tag of a .vue file, save the file

## Considerations

- Auto formatting doesn’t mean that the code will always be formatted in the same way regardless of the starting code.
  - In function declarations inside .vue files where more than one space precedes the opening parenthesis of the arguments (e.g. `function doSomething (...args) {/* code */}`), the extra space will not be automatically trimmed.
  - If the text content of a html tag is placed immediately after the “>” of the opening tag, auto format will not place the text content in the next line automatically. The same applies if the closing tag “</” is placed right after the element text content.
- Beware of any existing configuration files that may be interfering with the desired setup (.prettierrc, .eslintrc.js in sub folders, .eslintrc, eslint or prettier configurations in general configuration files such as package.json...).
- Open VSC from whatever folder contains the package.json file. If your project contains both backend and frontend, and the frontend is contained in a subfolder, you must open the frontend subfolder with VSC, NOT the parent folder of the entire project.
- I recommend to add a .prettierrc.js file to the root of the project with:

```javascript
module.exports = {
  tabWidth: 2,
  useTabs: false
};
```

## What formats what

- Prettier extension: html, scss and css files. Template and style tags in .vue files. Formatted with alt+shift+F (ctrl+shift+I on Ubuntu)
- Prettier-StandardJS extension: js files (with alt+shift+F, ctrl+shift+I on Ubuntu). Script tags in .vue files (on save)
