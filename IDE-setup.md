## IDE of choice
Not exactly an IDE but a Text Editor, [VSCode](https://code.visualstudio.com) is our weapon of choice, this doesn't mean you're forced to use it, it's just convenient.
If you like webstorm, or sublime better feel free to use them instead and share the reasons with the team, we may convert.

Just make sure they support the following extensions:

### ESLint
```
code --install-extension dbaeumer.vscode-eslint
```

### Prettier
```
code --install-extension esbenp.prettier-vscode
```

### GraphQL
```
code --install-extension GraphQL.vscode-graphql
```

### Import Cost
```
code --install-extension wix.vscode-import-cost
```

### Git Blame
```
code --install-extension waderyan.gitblame
```

### GLSL/WEBGL Related:
```
code --install-extension slevesque.shader
code --install-extension raczzalan.webgl-glsl-editor
code --install-extension dtoplak.vscode-glsllint
```

### Sauce (Min Theme)
```
code --install-extension miguelsolorio.min-theme
```

### Misc
```
code --install-extension GitHub.copilot
code --install-extension SimonSiefke.svg-preview
code --install-extension yzhang.markdown-all-in-one
```

### Here's also [@arzafran's](https://github.com/arzafran) personal VSCode Settings:

```json
{
  "workbench.startupEditor": "none",
  "window.autoDetectColorScheme": true,
  "workbench.preferredDarkColorTheme": "Min Dark",
  "workbench.preferredLightColorTheme": "Min Light",
  "editor.minimap.enabled": false,
  "breadcrumbs.enabled": false,
  "editor.renderWhitespace": "all",
  "emmet.includeLanguages": {
    "javascript": "javascriptreact"
  },
  "editor.tabSize": 2,
  "editor.codeActionsOnSave": {
    "source.organizeImports": true
  },
  "editor.insertSpaces": true,
  "editor.detectIndentation": false,
  "[jsonc]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[scss]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[css]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "diffEditor.ignoreTrimWhitespace": false,
  "editor.fontFamily": "'Monolisa', Menlo, Monaco, 'Courier New', monospace",
  "editor.formatOnSave": true,
  "editor.fontLigatures": true,
  "editor.lineHeight": 0,
  "editor.fontSize": 12,
  "eslint.format.enable": true,
  "eslint.packageManager": "pnpm",
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "git.autofetch": true,
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "emmet.triggerExpansionOnTab": true,
  "security.workspace.trust.untrustedFiles": "open",
  "editor.inlineSuggest.enabled": true,
  "github.copilot.enable": {
    "*": true,
    "yaml": false,
    "plaintext": true,
    "markdown": false,
    "javascriptreact": true
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "explorer.confirmDelete": false,
  "window.zoomLevel": 1,
  "workbench.colorTheme": "Min Light"
}
```
