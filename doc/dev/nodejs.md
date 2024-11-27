# Nodejs/nvm settings

## nvm (node version manager)
- Install nvm by running the bash script  
  Refer to https://github.com/nvm-sh/nvm  
  The install script will add some lines on ~/.bashrc
  ```bash
  ## nvm
  ## NOTE: ~/.nvm is the nvm cloned repository
  export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
  [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm  
  ```
- Basic commands
  ```bash
  nvm ls                               ## check installed nodejs versions
  nvm current                          ## check currently used nodejs version
  nvm ls-remote                        ## list available nodejs LTS versions for install
  nvm install node                     ## install the latest nodejs version (prefer to to install a selected version)
  nvm install <ver>                    ## install a specific nodejs version, e.g. nvm install 22
  nvm use <ver>                        ## user/activate a nodejs version, e.g. nvm use 22
  ```
## nodejs
- Install nodejs, e.g. version 22.x.x
  ```bash
  nvm install 22               ## install nodejs 22.x.x
  ```
- Check versions
  ```bash
  nvm ls                       ## check installed nodejs versions
  nvm current                  ## check currently used nodejs version
  nvm ls-remote                ## list available nodejs LTS versions for install
  ```
- Use/Activate a specific nodejs version  
  ```bash
  nvm use 22                   ## For example, use nodejs 22.x.x 
  which node                   ## find out the actual node location
  node -v                      ## find out the actual node version
  ```

## pnpm - performant npm
- Install pnpm by script  
  https://github.com/pnpm/pnpm
