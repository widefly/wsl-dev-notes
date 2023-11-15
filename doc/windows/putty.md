

## Installation
1. Install puTTY  
   https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
2. Install Xming (X-Windows manager under Windows)

## Convert rsa key to putty key (*.ppk)
- Create a data directory to keep this key file. In file explorer, goto %userprofile%, create a folder data\putty, e.g. C:\Users\xxx\data\putty
- Run PuTTYgen
- Conversions > Import key. 
  - Select the RSA key (private key)
  - Save private key (ppk) to the data\putty folder

## recommended putty settings
- Connection > SSH > X11, "enable X11 forwarding"
- Connection > Data, enter the linux username on field "Auto-login username" 
- Connection > Data, enter "linux" on field terminal-type string  
  `NOTE`: fix HOME/END key issue
- Connection > SSH > Auth >Credentials, enter the ppk file on field "Private key file for authentication"
- Window > Appearance, under Font, select 'Consolas, 8-point'

## App requires XWindows
- To test XWindows, it needs to run the Xming.
- Install xfe and x11-apps on WSL  
  ```bash
  sudo apt-get -y install xfe x11-apps
  (xclock&); (xeyes&)
  ```