# Developer note using WSL2

## Overview
1. Windows Subsystem for Linux, i.e. `WSL` enables running of Linux distributions on Windows host.  
1. Ubuntu is one of the most widely used Linux distributions and the preferred choice of WSL installation.  
1. Key reasons of Ubuntu popularity
   - large community
   - strong support, e.g. Long-Term Support (TLS)
   - vast amount of knowledge base and tutorials in the web
   - extensive support by Cloud provider, e.g. AWS, Azure, GCP, AliCloud
1. Windows is renowned for its
   - user friendly desktop GUI experience
   - comprehensive utilities and apps
   - software and hardware compatibility.  
  
   Linux is renowned for its
   - low memory footprint, high performance and terminal based
   - flexible development and deployment env, especially for server side/backend env.
   - extensive CLIs and utilities
   - Very cost effective in deployment and ongoing maintenance, due to its open source nature
   - high scalability and security
1. WSL as a development platform has the best of both worlds
   - Familiar GUI and desktop experience
   - Cloud Linux alike env, i.e. you dev and test on WSL/Linux like on cloud.
1. Seamless Windows host + WSL/Linux integration
   - Filesystem, e.g. WSL accesses files from Windows host, and vice versa
   - Network access, e.g. WSL accesses MSSQL on Windows host.
   - Remote development, e.g. vscode + WSL
   - Multiple Linux distributions
   

## Windows settings
Follow the steps below to configure the environment.
1. Recommended Windows [settings](./doc/windows/basic.md).

## WSL/Ubuntu Settings
1. Windows Subsystem for Linux, i.e. [WSL](./doc/windows/wsl.md).
2. Ubuntu shell [settings](./doc/ubuntu/shell.md)

## SSH client/server settings
1. openssh server [settings](./doc/ubuntu/openssh.md)
2. Putty, i.e. ssh client, [settings](./doc/windows/putty.md)
3. Create a putty profile for WSL  
   - Hostname: localhost
   - Connection > Data, enter the Linux username to "AUto-login username"
   - Connection > SSH > Auth > Credentials, select the ppk private file to "Private key file for authentication"
   - Save the profile by entering a name "WSL" in saved sessions and press save.

## Development tools
1. Configure [vscode](./doc/dev/vscode.md)
2. Configure [python](./doc/dev/python.md)