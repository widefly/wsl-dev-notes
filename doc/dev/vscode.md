# vscode

## Installation
- Download [here](https://code.visualstudio.com/download)

## user settings
- In the command palette, search "user settings (json)". Some settings are recommended, e.g. font size
  ```json
  {
    ....

    // default font size
    // - 10 is good for 27" Monitor
    // - 11 is good for small screen, e.g. notebook 12"
    "editor.fontSize": 10.5,
    "markdown.preview.fontSize": 10.5,

    // Jump to the opened file on upon F12, i.e. go to definition
    "workbench.editor.revealIfOpen": true,

    // terminal
    // - For Windows, suggested to open cmd.exe as default. 
    //   Otherwise, it use powershell
    "terminal.integrated.fontSize": 10.5,
    "terminal.integrated.defaultProfile.windows": "Command Prompt",

    // git
    "scm.showHistoryGraph": false,
    "git.enableSmartCommit": true,  

    // auto format on save
    "editor.formatOnPaste": true,
    "editor.formatOnSave": true,

    // misc
    "editor.minimap.enabled": false,


  }
  ```

## Extensions
In the vscode search field, enter the below to install extensions.  These can be installed via extension GUI.

- vscode on WSL/remote SSH host
  - WSL  
    `ext install ms-vscode-remote.remote-wsl` 
  - Remote - SSH  
    `ext install ms-vscode-remote.remote-ssh` 

- Essential
  - Code Spell Checker  
    `ext install streetsidesoftware.code-spell-checker`
  - Prettier - Code formatter  
    `ext install esbenp.prettier-vscode`
  - Git Graph  
    `ext install mhutchie.git-graph`
  - Markdown All in One  
    `ext install yzhang.markdown-all-in-one`
  - Rest Client  
    `ext install humao.rest-client`
  - PDF Viewer  
    `ext install mathematic.vscode-pdf`
  - Excel Viewer  
    `ext install grapecity.gc-excelviewer`
  - YAML  
    `ext install redhat.vscode-yaml`
  - vscode-icons  
    `ext install vscode-icons-team.vscode-icons`
- Python
  - Python  
    `ext install ms-python.python`
  - Pylance  
    `ext install ms-python.vscode-pylance`
  - Black Formatter  
    `ext install ms-python.black-formatter`
- C#/.NET
  - C#  
    `ext install ms-dotnettools.csharp`
- MSSQL
  - SQL Server (mssql)  
    `ext install ms-mssql.mssql`
- Shell script
  - ShellCheck  
    `ext install timonwong.shellcheck`
  - Shell-format  
    `ext install foxundermoon.shell-format`
- TypeScript/JavaScript
  - ESLint  
    `ext install dbaeumer.vscode-eslint`
- C/C++
  - C/C++  
    `ext install ms-vscode.cpptools`
  - C/C++ Extension Pack  
    `ext install ms-vscode.cpptools-extension-pack`

## github integration
- (content to be added)
