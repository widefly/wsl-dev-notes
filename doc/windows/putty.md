# PuTTY

## Background

- Accessing Linux is mostly performed via a text-based program called `SSH Client` (Secure Shell) , `terminal` or `console`.
- In Windows, to directly access WSL/Linux, simply run `wsl` or `cmd` then `wsl`.  
  However, it is not a general way to access Linux but is limited to Windows.  It also lacks of GUI and many useful features.
- In Linux, `SSH Client` is the de-facto standard because of its strong security, remote access, and many features like tunneling and remote commands.
- In fact, most developers use 3rd party SSH GUI clients.  For example, `PuTTY` is one of the most used SSH client for Windows.

## Installation

1. Install puTTY  
   https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
2. (Optional) Install Xming, a X-Windows manager for Windows.  
   NOTE: Ask development lead to obtain the installation file.

## Convert rsa key to putty key (\*.ppk)

- Without private key setup, SSH requires users to supply username and password to authenticate themselves on each SSH connection.
- To simplify the connection, that avoids supplying username and password every time, and to strengthen the authentication security, private/public key setup is highly recommended.
- A private personal private key should be generated in RSA or ed25519 format  
  NOTE: ed25519 is the modern format but RSA is the most compatible one

  ```bash
  ## Under WSL, Create private and public key in RSA format
  ssh-keygen -t rsa -b 4096 \
    -C "personal key generated on $(date +"%Y-%m-%dT%H:%M:%S")" \
    -f ~/.ssh/id_rsa_personal -q -P ""

  ## Under WSL, Create private and public key in ed5519 format
  ssh-keygen -t ed25519 \
    -C "personal key generated on $(date +"%Y-%m-%dT%H:%M:%S")" \
    -f ~/.ssh/id_ed25519_personal -q -P ""
  ```

  NOTE: Refer to [openssh](../ubuntu/openssh.md) for details.

- Since private key represents an user identity, the key must be stored and placed in a well protected location. In Ubuntu, private key is stored in ~/.ssh folder.
- In Windows, it is recommended to store in user directory, e.g. %userprofile%\\.ssh.
- RSA private key must be converted into putty format to perform authentication.  
  Run PuTTYgen, Conversions > Import key.
  - Select the RSA key (private key copied from the Ubuntu ~/.ssh)
  - Save putty private key (ppk) to a safe directory, e.g. %userprofile%\\putty.

## Recommended putty settings

Apply the following settings when creating a new putty profile. These settings can be applied to "Default Settings" as well.

- Connection > SSH > X11, "enable X11 forwarding"
- Connection > Data, enter the linux username on field "Auto-login username"
- Connection > Data, enter "xterm" on field terminal-type string
- Connection > SSH > Auth >Credentials, enter the ppk file on field "Private key file for authentication"
- Window > Appearance, under Font, select 'Consolas, 8-point'

## About x11 forwarding

- X-server, i.e. XMing on Windows, listens on ssh client's host port 6000 by default.
- In Putty, if x11 forwarding is enabled
  - ssh terminal has its environmental variable exported `DISPLAY=localhost:10.0`.  
    This instructs xWin app to connect to X-server via port 6010, instead of the default 6000.
  - A reverse forwarding ssh tunnel is established that forwards port 6010 to port 6000, i.e. sshd listens on port 6010 and forwards the traffic to Putty's localhost port 6000.
  - Thus, xWin app can run on the remote display.
- In vscode, x11 forwarding is enabled by default. This applies to remote WSL too.
  - In case of remote WSL, if unix domain socket /tmp/.X11-UNIX is found, it mean x-server is present.
  - If xWin app runs on the vscode terminal, vscode forwards the data to the unix domain to vscode client's localhost x-server, i.e. unix domain socket -> localhost's 6000.

## About 24-bit color

- To enable 24-bit color  
  Connection > Data, enter "xterm" on field terminal-type string
- Add lines to ~/.tmux.conf
  ```bash
  # Use the xterm-256color terminal
  set -g default-terminal "xterm"
  # Apply Tc
  set-option -ga terminal-overrides ",xterm:Tc"
  ```
- To test if terminal supports 24-bit color, run command
  ```awk
  awk 'BEGIN{
  s="/\\/\\/\\/\\/\\"; s=s s s s s s s s;
  for (colnum = 0; colnum<77; colnum++) {
  r = 255-(colnum*255/76);
  g = (colnum*510/76);
  b = (colnum*255/76);
  if (g>255) g = 510-g;
  printf "\033[48;2;%d;%d;%dm", r,g,b;
  printf "\033[38;2;%d;%d;%dm", 255-r,255-g,255-b;
  printf "%s\033[0m", substr(s,colnum+1,1);
  }
  printf "\n";
  }'
  ```
- To print a single line with color
  ```bash
  r=255; g=100; b=100; echo -e "\033[38;2;${r};${g};${b}mhello world\033[0;00m"
  ```

## App requires XWindows

- To test X-Windows, it needs to run the Xming.
- Install xfe and x11-apps on WSL
  ```bash
  sudo apt-get -y install xfe x11-apps
  ```
- Run some X-Windows app to test it, e.g.
  ```bash
  (xclock&); (xeyes&)
  ```
