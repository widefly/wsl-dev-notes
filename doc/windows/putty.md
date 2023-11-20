# PuTTY

## Background
- Windows command prompt or direct Ubuntu command prompt lacks of many features.  
- In fact, most developers use 3rd party SSH client. For example, PuTTY is one of the most used SSH client on Windows.


## Installation
1. Install puTTY  
   https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
2. Install Xming, a X-Windows manager for Windows.  
   NOTE: Ask development lead to obtain the installation file.

## Convert rsa key to putty key (*.ppk)
- Without private key setup, SSH requires users to supply username and password to authenticate themselves on each SSH connection.
- To simplify the connection, that avoids supplying username and password every time, and to strengthen the authentication security, private/public key is highly recommended.
- A private personal private key should be generated in RSA format.
  ```bash
  ## Under WSL, Create private and public key in RSA format 
  ssh-keygen -t rsa -b 4096 -f <privateKeyFile> -q -P ""  
  ```
  NOTE: Refer to [openssh](../ubuntu/openssh.md) for details.
- Since private key represents an user identity, the key must be stored and placed in a well protected location. In Ubuntu, private key is stored in ~/.ssh folder.  
- In Windows, it is recommended to store in user directory, e.g. %userprofile%\putty.
- RSA private key must be converted into putty format to perform authentication.  
  Run PuTTYgen, Conversions > Import key. 
  - Select the RSA key (private key copied from the Ubuntu ~/.ssh)
  - Save putty private key (ppk) to the %userprofile%\putty directory.

## recommended putty settings
Apply the following settings when creating a new putty profile.  These settings can be applied to "Default Settings" as well.
- Connection > SSH > X11, "enable X11 forwarding"
- Connection > Data, enter the linux username on field "Auto-login username" 
- Connection > Data, enter "linux" on field terminal-type string  
  `NOTE`: fix HOME/END key issue
- Connection > SSH > Auth >Credentials, enter the ppk file on field "Private key file for authentication"
- Window > Appearance, under Font, select 'Consolas, 8-point'

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
