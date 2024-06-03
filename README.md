# A Guide to Setting Up SSH for Visual Studio Code

This is a guide to setting up SSH in Visual Studio Code on Windows with or without WSL. If you are using WSL, I will assume you have Ubuntu as your distribution, the steps should be very similar if you are using a different one. This guide will walk through the steps to remote into one computer from another so you can use VS Code remotely.
## What you’ll need to get started
1. The two computers you want to set up for remote development. Both with Windows 10 or later installed.
2. [Visual Studio Code](https://code.visualstudio.com/download) installed on both machines
3. SSH keys on both computers, if you don’t have SSH keys generated on both machines follow [this guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent), make sure to add both keys to the SSH agent. (Note: If using WSL you need to generate separate SSH keys for WSL and Windows) (Note: ed25519 is the preferred algorithm for the keys, but both are acceptable.)
4. Optionally you can have [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/install) installed

#### Once you have the required materials installed and updated, we can start.

### Part 1 - SSH
1. The first step is going to enable the OpenSSH Client and Server on both machines. For these steps follow [this guide](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=gui) and come back when you're done.

2. If using WSL, also open a WSL terminal, either by typing wsl into powershell or typing in Ubuntu in the start menu and clicking on it. Follow the install section of [this guide](https://ubuntu.com/server/docs/openssh-server) to install the OpenSSH Client and Server on WSL. (It should come pre installed, but this will update it regardless)

3. Now we will add the ssh key from both machines onto each other, I recommend emailing yourself the public key from each machine, labeling each one in the subject line and copying each one into the authorized_keys file located in `C://Users/”your_username”/.ssh`. If the authorized key file doesn’t exist, open the folder in VS code and click add file on the left. Type in “authorized_keys” and hit enter. Lastly, copy the key in and save. We want the opposite key, so say I have a laptop and a desktop, the laptop key goes onto the desktop and vice versa. 

4. Next we are going to connect the two machines together through powershell to make sure everything works correctly.
    Open Powershell and type in 
     `ssh your_username@your_computer_name.local`
Where "your_username" and “your_computer_name” are the name of the machine you aren’t currently on, and “your_username” is your windows username. Then type in yes and hit enter.

5. *Optional* only worry about this if you are using WSL. This is where things get a little tricky. You need to add the key from your WSL to **both** machines. Again the key from WSL needs to be in **both** machines’ authorized_keys file. 

6. *Optional* for WSL, check it is working correctly by entering
    `ssh windows_username@your_computer_name.local -J wsl_username@localhost`
    Where the "windows_username", "your_computer_name", and "wsl_username" are replaced with their corresponding names. On the client machine, in most cases the laptop. Again type in yes and hit enter. You should see the terminal change to your WSL username. 
### Part 2 - VS Code
1. Okay next we are going to configure VS code  the first step is to install the SSH extensions. Open VS Code.

2. Click on the extensions button, see the picture below.
![An image of VS Code's extension button](https://i.imgur.com/SWgn9Vq.png)
3. Now type “remote development” into the search bar at the top
![An image of VS Code's remote devlopment extension page](https://i.imgur.com/sIkCpzg.jpg)
Click install, located below the title. Mine says uninstall, but it is in the same location.
4. Now type `ctrl + shift + p` at the same time and then type `Remote-SSH Add New Host` and hit enter you should see this
![An image of VS Code's add remote option](https://i.imgur.com/VfUoOrL.jpeg)
5. Now you can type either 
`ssh your_username@your_computer_name.local`
If just using Windows Or
		`ssh windows_username@your_computer_name.local -J wsl_username@localhost`
If using WSL and hit enter. Again replace the placeholders with their corresponding values.
6. If everything went according to plan you should see a normal VS Code window and you can select a folder on your remote machine!

## Part 3: Troubleshooting:
1. Make sure you didn’t accidentally add any characters to the keys when you copied them over.
2. Make sure that the OpenSSH server is started which is step 3 in [this guide](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=gui.).
3. You may need to open port 22 in WSL, try [this guide](https://www.cyberciti.biz/faq/ufw-allow-incoming-ssh-connections-from-a-specific-ip-address-subnet-on-ubuntu-debian/).
4. Double check that the Windows Firewall is allowing OpenSSH through. Also double check your antivirus is allowing traffic through port 22.

## Conclusion:
Thank you for following this guide. Please feel free to reach out if you have any questions. I’d be happy to help. You can reach me at jrh00045@mix.wvu.edu. 


