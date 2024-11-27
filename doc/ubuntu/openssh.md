# OpenSSH setup

## Installation
```bash
sudo apt-get install -y openssh-server
```

## Configure openssh server 
Edit /etc/ssh/sshd_config, apply changes a few lines of settings
```bash
sudo vi /etc/ssh/sshd_config
```

```bash
## Avoid kill idle session
## Send keepalive message to client every xx seconds
ClientAliveInterval 10
## Sets the maximum number of keepalive messages that can be sent without a response from the client before the connection is terminated.
## This helps prevent connections from being kept open indefinitely if the client is unresponsive.
ClientAliveCountMax 3
```

To apply changes  
```bash
sudo systemctl restart ssh
```

To list all effective sshd settings
```bash
## List all settings
sudo sshd -T 
## List desired settings, e.g. ClientAliveInterval, ClientAliveCountMax, TCPKeepAlive
sudo sshd -T | grep -i alive
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
  IdentityFile ~/.ssh/id_ed25519_personal

# default
Host *
  IdentityFile ~/.ssh/id_ed25519_personal
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
[[ -f ~/.ssh/config ]] && chmod 600 ~/.ssh/config
[[ -f ~/.ssh/authorized_keys ]] && chmod 600 ~/.ssh/authorized_keys

## Generate RSA key
if [[ ! -f ~/.ssh/id_rsa_personal ]]; then
    echo ""
    echo -n "Generate ~/.ssh/id_ed25519_personal (Y|N)? "
    read answer
    if [[ $answer =~ ^y|Y ]]; then
        echo "Generating ~/.ssh/id_ed25519_personal..."
        ssh-keygen -t ed25519 -C "personal key generated on $(date +"%Y-%m-%dT%H:%M:%S")" -f ~/.ssh/id_ed25519_personal -q -P ""
    fi
fi
```

## Generate key pair (optional)
```bash
ssh-keygen -t ed25519 \
-C "personal key generated on $(date +"%Y-%m-%dT%H:%M:%S")" \
-f ~/.ssh/id_ed25519_personal -q -P ""
```

## start/restart/stop openssh server
```bash
## Start ssh 
sudo systemctl start ssh

## Restart ssh 
sudo systemctl restart ssh

## Stop ssh
sudo systemctl stop ssh
```