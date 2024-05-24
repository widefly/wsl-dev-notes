# Nodejs/nvm settings

## nvm (node version manager)
### Install nvm and different nodejs versions
- Install nvm by running the bash script  
  Refer to https://github.com/nvm-sh/nvm (Install and Update Script)
  The install script will add some lines below to ~/.bashrc
  ```bash
  ## nvm
  ## NOTE: ~/.nvm is the nvm cloned repository
  export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
  [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm  
  ```
### pyenv useful commands
- Check pyenv version: `nvm --version`
- List available nodejs versions to install: `nvm ls-remote`
  List installed nodejs version: `nvm list`
- Install specific nodejs version: e.g. `nvm install 18` or `nvm install 18.20.3`
- Switch to a python version: `nvm use 18`
- Uninstall a nodejs version: `nvm uninstall 18.20.3`

## vscode integration

