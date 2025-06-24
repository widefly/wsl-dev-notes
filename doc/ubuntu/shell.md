# Bash shell configurations

## Essential tools
- Before installing essential packages, update the package index and upgrade the existing packages
```bash
sudo apt-get -y update; sudo apt-get -y upgrade
```
-  Install some useful packages
```bash
sudo apt-get -y install \
build-essential cmake pkg-config git \
acl tmux ffmpeg tree vim unzip curl pv jq expect bc less openssh-server
```

-  Install x11 utilities (optional)
```bash
sudo apt-get -y install xfe x11-apps
```

## Configure essential config files
### ~/.tmux.conf
tmux is a terminal multiplexer 
```bash
## Run the script below
cat > ~/.tmux.conf << "EOF"
# set default shell to load and prevent loading .profile for each new session
set-option -g default-command "/bin/bash"

# remap prefix from 'C-b' to 'C-a'
unbind C-b
set-option -g prefix C-a
bind-key C-a send-prefix

#split panes using | and -
# new pane use original path => -c "#{pane_current_path}"
bind | split-window -h   -c "#{pane_current_path}"
bind - split-window -v   -c "#{pane_current_path}"
unbind '"'
unbind %

# reload config file (change file location to your the tmux.conf you want to use)
bind r source-file ~/.tmux.conf

# switch panes using Alt-arrow without prefix
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D

# Enable mouse mode (tmux 2.1 and above)
set -g mouse on

# Enable mouse control (clickable windows, panes, resizable panes)
#set -g mouse-select-window on
#set -g mouse-select-pane on
#set -g mouse-resize-pane on

# don't rename windows automatically
set-option -g allow-rename off

# clear history
bind c clear-history

# color
#set -g default-terminal "screen-256color"
#set -g mode-style fg=white,bg=blue
#set -g mode-style fg=black,bg=yellow

# Use the xterm-256color terminal
set -g default-terminal "xterm"

# Apply Tc
set-option -ga terminal-overrides ",xterm:Tc"
EOF
```

### ~/.inputrc
user-specific configuration file used by the GNU Readline library
```bash
## Run the script below
cat > ~/.inputrc << "EOF"
"\e[A": history-search-backward
"\e[B": history-search-forward
"\e[C": forward-char
"\e[D": backward-char
## fix the HOME/END key issue using xterm
"\e[1~": beginning-of-line
"\e[4~": end-of-line
## fix the HOME/END key issue using other terminal
"\e[7~": beginning-of-line
"\e[8~": end-of-line

## enable paste command + input text without waiting, e.g. python + multiline code
set enable-bracketed-paste off

## optional
set bell-style none
set completion-ignore-case on

EOF
```

### ~/.vimrc
vim config file
```bash
## Run the script below
cat > ~/.vimrc << "EOF"
:set background=dark
:set tabstop=4
:set softtabstop=4
":set noexpandtab           " makefile needs real TAB
:set expandtab
:set shiftwidth=4
":set autoindent
:set ruler
:set showmode
:set nu
:set statusline+=%F         " show full path file being edited (%f shows only filename)
:set laststatus=2           " enable bottom statusbar
:syntax on                  " turn on syntax highlight
EOF
```

### ~/.bashcfg.sh
useful shell script to be sourced by ~/.bashrc
```bash
## Run the script below
cat > ~/.bashcfg.sh << "EOF"
##################### history ##############################
#Enlarge the history.. change the value by adding a 0 at the end
HISTSIZE=10000
HISTFILESIZE=20000

# Enable vi to edit command history, and map HOME/END key
set -o vi
bind -m vi-insert '"\e[1~": beginning-of-line'
bind -m vi-insert '"\e[4~": end-of-line'

##################### prompt ##############################
# color prompt (long: username & hostname & directory)
# root has red highlight
if [ "$USER" != "root" ]; then
    export PS1="\[\033[38;5;11m\]\u\[$(tput sgr0)\]\[\033[38;5;15m\]@\[$(tput bold)\]\[$(tput sgr0)\]\[\033[38;5;33m\]\h\[$(tput sgr0)\]\[$(tput sgr0)\]\[\033[38;5;15m\]:\[$(tput sgr0)\]\[\033[38;5;6m\][\w]:\[$(tput sgr0)\]\[\033[38;5;15m\] \[$(tput sgr0)\]"
else
    export PS1="\[\033[38;5;161m\]\u\[$(tput sgr0)\]\[\033[38;5;15m\]@\[$(tput sgr0)\]\[\033[38;5;33m\]\h\[$(tput sgr0)\]\[\033[38;5;15m\]:\[$(tput sgr0)\]\[\033[38;5;10m\][\w]\[$(tput sgr0)\]\[\033[38;5;15m\]: \[$(tput sgr0)\]"
fi


#color prompt (directory only and max depth of 3)
#export PS1="\[$(tput sgr0)\]\[\033[38;5;6m\][\w]:\[$(tput sgr0)\]\[\033[38;5;15m\] \[$(tput sgr0)\]"
export PROMPT_DIRTRIM=4

##################### alias ##############################
alias update="sudo apt-get update; sudo apt-get -y upgrade"
alias dmesg="dmesg -T"

##################### Additional PATH ##############################
## Add PATH: local bin
PATH_TO_ADD=${HOME}/.local/bin
## Add PATH: mssql server client tools
[[ -d /opt/mssql-tools18/bin  ]] && PATH_TO_ADD=$PATH_TO_ADD:/opt/mssql-tools18/bin
## CUDA 12.1.x
[[ -d /usr/local/cuda-12.1/bin ]] && PATH_TO_ADD=$PATH_TO_ADD:/usr/local/cuda-12.1/bin

## Export PATH
[[ -z $TMUX ]] && export PATH=$PATH_TO_ADD:$PATH

##################### Additional export ##############################
[[ -z $TMUX ]] && [[ -d /usr/share/dotnet ]] && export DOTNET_ROOT=/usr/share/dotnet

## CUDA and cuDNN, e.g. CUDA 12.1.x and cuDNN 8.9.7
CUDA_LIB=/usr/local/cuda-12.1/lib64:/usr/local/cuda-12.1/lib64/stubs:/usr/local/cuda-12.1/extras/CUPTI/lib64
CUDNN_LIB=~/cuda/cudnn/cudnn-8.9.7-cuda-12.1.1/lib
LIB_TO_ADD=$CUDA_LIB:$CUDNN_LIB

export LD_LIBRARY_PATH=$LIB_TO_ADD:$LD_LIBRARY_PATH

##################### terminal ##############################
# fix strange character
export TERM=xterm
export NCURSES_NO_UTF8_ACS=1
export LANG="C.UTF-8"

## Instruct xWin app to connect via display number 10, i.e. port 6010
export DISPLAY=:10.0

# undefine CTRL-s/CTRL-q to stop/resume
stty stop undef
stty start undef
stty -ixon -ixoff

EOF
```
### add to ~/.bashrc
Add the following line to ~/.bashrc
```bash
### source standard bash cfg
source ~/.bashcfg.sh
```

### timezone
- Configure system timezone
- Timezone is a global system setting (not user setting)
```bash
## list all available timezones
timedatectl list-timezones
## find out the one desired, e.g. Hongkong
timedatectl list-timezones | grep -i hongkong
## apply new timezone, e.g. Hongkong
sudo timedatectl set-timezone Hongkong
```

### sudoers
- Add an username to sudoers eliminates entering password for every sudo command.
- Add the following line after running a command `sudo visudo` (replace xxx with the desired username)
```bash
xxxxx ALL=(ALL) NOPASSWD:ALL
```

### .vimrc for root
First, run as root and copy .vimrc
```bash
sudo su -
```
