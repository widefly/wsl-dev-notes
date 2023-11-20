# Developer note using WSL2

## Overview
1. First, Windows Subsystem for Linux, i.e. `WSL` is the development platform that will be extensively used like developing on a real and typical Linux, e.g. Ubuntu.
2. All developed programs should run the WSL's instance instead of on the Windows host. For example, python scripts run on Ubuntu, instead of on Windows.
3. The key reason to use to WSL is to develop, debug, test the programs and configure the deployment scripts on the platform which is the same as that of the cloud Linux environment.

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