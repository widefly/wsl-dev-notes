# OpenSSH setup

## Installation
```bash
sudo apt-get install -y openssh-server
```

## Configure openssh server 
Edit /etc/ssh/sshd_config, apply changes a few lines of settings
```bash
sudo vi /etc/ssh/sshd_config`
```

```bash
...
## Avoid kill idle session
## Send keepalive message to client every xx seconds
ClientAliveInterval 10
## Sets the maximum number of keepalive messages that can be sent without a response from the client before the connection is terminated.
## This helps prevent connections from being kept open indefinitely if the client is unresponsive.
ClientAliveCountMax 3
...
```

To apply changes  
```bash
sudo systemctl restart ssh
```


## Configure ~/.ssh
```bash

## Create the ssh directory
if [[ ! -d ~/.ssh ]]; then
    mkdir ~/.ssh && chmod 700 ~/.ssh
else
    echo "~/.ssh already exist"
fi

## Create an standard config
if [[ ! -f ~/.ssh/config ]]; then
    cat > ~/.ssh/config << "EOF"
# Personal GitHub account
Host github.com
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa_personal

# default
IdentityFile ~/.ssh/id_rsa_personal
EOF
    chmod 700 ~/.ssh/config
else
    echo "~/.ssh/config already exist"
fi

### Create an empty authorized_keys
if [[ ! -f ~/.ssh/authorized_keys ]]; then
    touch ~/.ssh/authorized_keys
    chmod 700 ~/.ssh/authorized_keys
else
    echo "~/.ssh/authorized_keys already exist"
fi

## double check file permissions
[[ -d ~/.ssh  ]] && chmod 700 ~/.ssh
[[ -f ~/.ssh/config ]] && chmod 700 ~/.ssh/config
[[ -f ~/.ssh/authorized_keys ]] && chmod 700 ~/.ssh/authorized_keys

## Generate RSA key
if [[ ! -f ~/.ssh/id_rsa_personal ]]; then
    echo ""
    echo -n "Generate ~/.ssh/id_rsa_personal (Y|N)? "
    read answer
    if [[ $answer =~ ^y|Y ]]; then
        echo "Generating ~/.ssh/id_rsa_personal..."
        ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa_personal -q -P ""   
    fi
fi
```


## start/stop openssh server
```bash
## Start ssh 
sudo service ssh start

## Stop ssh
sudo service ssh stop
```