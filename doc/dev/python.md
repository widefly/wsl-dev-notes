# Python settings

## pyenv
### Install pyenv and different python versions
- Install dependencies for pyenv
  ```bash
  sudo apt update; sudo apt install -y build-essential libssl-dev zlib1g-dev \
  libbz2-dev libreadline-dev libsqlite3-dev curl \
  libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
  ```
- Install pyenv by running the bash script
  ```bash
  curl -fsSL https://pyenv.run | bash
  ```
- Append the below to ~/.bashrc
  ```bash
  ## NOTE:
  ## - Original pyenv instruction has no checking of $PYENV_ROOT/bin that may already been existed in the PATH var
  ## - This avoids the duplication
  export PYENV_ROOT="$HOME/.pyenv"
  if [[ ! $PATH =~ "$PYENV_ROOT/bin" ]]; then
    [[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
  fi
  eval "$(pyenv init - bash)"
  ```
### pyenv useful commands
- Check pyenv version  
  ```bash
  $ which pyenv && pyenv --version
  > /home/sylvia/.pyenv/bin/pyenv  
  > pyenv 2.3.32
  ```
- List available python versions to install  
  ```bash
  $ pyenv install --list | grep '3.8.1'
  > ...  
  > 3.8.13  
  > 3.8.14  
  > 3.8.15  
  > 3.8.16  
  > 3.8.17  
  > 3.8.18  
  ```
- Install specific Python version  
  ```bash
  $ pyenv install  3.8.16
  > Downloading Python-3.8.16.tar.xz...  
  > -> https://www.python.org/ftp/python/3.8.16/Python-3.8.16.tar.xz  
  > Installing Python-3.8.16...  
  > Installed Python-3.8.16 to /home/sylvia/.pyenv/versions/3.8.16  
  ```
- Switch to a python version
  ```bash
  $ pyenv shell <version>     
  $ pyenv shell 3.8.16 && python -V   ## switch to 3.8.16

  ```
- Create a virtual python env. Activate/deactivate an env
  ```bash
  $ pyenv virtualenv 3.8.16 proj_test
  > Looking in links: /tmp/tmp7un0f_ue
  > Requirement already satisfied: setuptools in /home/sylvia/.pyenv/versions/3.8.16/envs/proj_test/lib/python3.8/site-packages (56.0.0)
  > Requirement already satisfied: pip in /home/sylvia/.pyenv/versions/3.8.16/envs/proj_test/lib/python3.8/site-packages (22.0.4)
  ```
  ```bash
  $ pyenv activate proj_test          ## activate the env
  > (proj_test) sylvia@PC-INTERN:[~]:
  $ pyenv deactivate
  > sylvia@PC-INTERN:[~]:             ## deactivate the env
  ```
- Delete the virtualenv
  ```bash
  $ pyenv uninstall <env>
  ```
- List all pyenv installed pythons  
  ```bash
  $ pyenv versions   ## * indicates current python env
  > * system (set by /home/sylvia/.pyenv/version)  
  >   3.8.16
  ```
- Update pyenv
  ```bash
  $ pyenv update
  ```

## vscode integration
- First, the python should be under a folder like ~/projects/projTest
- In vscode, connect to WSL and open the folder.
- Under the project folder, create a virtualenv named, e.g. projTest
  ```
  pyenv virtualenv 3.8.16 projTest
  ```
- In command palette, search "workspace settings", copy the below to the settings.json
  ```json
  {
    "workbench.editor.revealIfOpen": true,
    "cSpell.words": [
    ],
    "[python]": {
      "editor.defaultFormatter": "ms-python.black-formatter"
    },
    "python.formatting.blackArgs": [
      "--line-length",
      "120"
    ],
    "python.defaultInterpreterPath": "${env:HOME}/.pyenv/versions/3.8.16/envs/proj_test/bin/python3",
    "eslint.format.enable": true,
    "editor.formatOnPaste": true,
    "editor.formatOnSave": true,
    "python.analysis.typeCheckingMode": "basic",
    "python.analysis.extraPaths": [
      "./src/lib"
    ],
    "python.analysis.exclude": [
      "./out", 
      "./temp",
      "./.venv",
      "./winEnv"
    ]
  }
  ```
  NOTE: "python.defaultInterpreterPath" is an important setting to tell vscode to use the virtual env

## install opencv
- Install opencv 4.8.x.x
  ```bash
  pip install opencv-python==4.8.0.76
  pip install opencv-contrib-python==4.8.0.76
  ```