# WSL

## WSL Installation and Ubuntu
1. WSL is `Windows Subsystem for Linux`.  
   `NOTE:` Use newer WSL2 instead of the older WSL1.  WSL2 has many performance improvement over WSL1.
2. To setup WSL2, simply install an Ubuntu Linux via Microsoft Store.  
   Recommended to install `Ubuntu 22.04.2 LTS`
3. Once installed, the Ubuntu Linux terminal can be opened by a Windows app named `Ubuntu 22.04.2 LTS`


## Important WSL settings
### Port forwarding (localhost to WSL Linux)
- When a WSL Linux instance is running, it was assigned with an IP address that is different from that of the Windows host.  For example, Windows host: `192.168.0.11` and Ubuntu: `172.18.167.234`
- By default, listening port on the WSL Linux cannot be accessed by Windows's localhost address.  
  For example, http://localhost:3000 will not connect to the listening port 3000 on WSL Linux.
- To enable localhost port forwarding, i.e. Windows's localhost port forwarding to WSL Linux,  
  Add the following config to `%UserProfile%/.wslconfig`
  ```ini
  [wsl2]
  localhostForwarding=true
  ```
- To test if the localhost port forwarding is working.  
  ```bash
  ## Linux
  nc -l 4001           ## start a process to listen on port 4001
  ```  
  On Windows, open a chrome browser, and enter http://localhost:4001. If some http protocol data is seen in the Linux terminal, it means port forwarding is enabled.  For example, it looks like  
  ```
  GET / HTTP/1.1
  Host: localhost:4004
  Connection: keep-alive
  sec-ch-ua: "Google Chrome";v="119", "Chromium";v="119", "Not?A_Brand";v="24"
  ...
  ```

## Essential packages and shell configuration
- Refer to shell [note](../ubuntu/shell.md).

### Keep WSL running
This recommended settings needs installation of openssh server. Refer to openssh [note](../ubuntu/openssh.md).
- WSL will be auto shutdown if there is no active Linux terminal or top level processes running.
- To keep WSL Linux alive in background, create a simple cmd file to auto start openssh server, e.g. startwsl.cmd, and place it under folder `%appdata%\Microsoft\Windows\Start Menu\Programs\Startup`  
  ```cmd
  :: This keep a process running under init
  wsl bash -c "nohup bash -c 'while true; do sleep 1h; done &' &>/dev/null "
  :: This starts openssh service
  wsl sudo service ssh start
  timeout /t 3
  ```  
  `NOTE:` This requires installation of openssh server 


## Basic WSL command
- Check WSL version and the installed Linux  
  ```cmd
  > wsl -l -v                       
    NAME            STATE           VERSION
  * ubuntu18.04     Running         2
    Ubuntu-16.04    Stopped         2
    Ubuntu-18.04    Stopped         2
    Ubuntu-22.04    Stopped         2  
  ```  
  `NOTE`: * indicates default WSL Linux 
- To start an Ubuntu terminal, either  
  -  Run the app, e.g. `Ubuntu 22.04.2 LTS`
  -  Run the command prompt, then run `wsl`.  This runs the default Linux.  
     To run a specific Linux, `wsl -d xxxx` where xxxx is Linux distribution name.
  -  Access via ssh.  Recommended to use `Putty` to connect to Ubuntu openssh server.
- To set a Linux distribution as the default WSL Linux, run `wsl --set-default xxxx` where xxxx is the Linux distribution name.

## Copy files between Windows host and Linux
- When an Linux distribution is installed, its system root directory is mapped to Windows network shares under `\\wsl$`
- For example, `\\wsl$\Ubuntu-22.04` is the network share that maps to Ubuntu 22.04 Linux.
- To facilitate files/folders access, map a network drive, e.g. W:\, to the network share.