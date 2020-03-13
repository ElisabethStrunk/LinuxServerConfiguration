# Secure your server

> This document is part of the [*Linux Server Setup*](README.md) project and is to be viewed within this context.

The following describes how to secure your Amazon Lightsail server from a number of attack vectors.<br>
<br>

## Update all pre-installed packages
Make sure the software packages installed on the server are up to date:

* Update the source list:
    ```bash
    sudo apt-get update
    ```
* Upgrade installed packages:
    ```bash
    sudo apt-get upgrade
    ```

<br>

## Change the default SSH port
Change the SSH port from 22 to 2200:

* Make sure to configure the Lightsail firewall first:

    * Go to your instance's site on https://lightsail.aws.amazon.com.
    * Go to the *Networking* tab:<br>
    <kbd><img src="readme_images/instance-site-networking.PNG" width=550></kbd>
    * In the *Firewall* section add another rule "Custom TCP 2200"

* Use PuTTY or the tool of your choice to connect to your instance (see the *Access your server via SSH* section of [this](get_your_server.md) document).
* Change the port number:

    * Open */etc/ssh/sshd_config* (e.g. with vim):
        ```bash
        sudo vim /etc/ssh/sshd_config
        ```
        The *sudo* is important because you need sudoer rights to change the content of the file.
    * Look for the section titled *# What ports, IPs and protocols we listen for*.
    * Change the number after *Port* from 22 to 2200.
    * Save your changes.

* Reboot your instance by clicking the *Reboot* button on your instance's site:<br>
<kbd><img src="readme_images/reboot-instance.PNG" width=550></kbd> 

After that, if you try connecting on port 22, you will find that this is no longer possible. Instead you have to connect to port 2200 now.
<br>

## Set up an Uncomplicated Firewall (UFW)
Configure the Uncomplicated Firewall (UFW):

* Check if UFW is active:
    ```bash
    sudo ufw status
    ```
* It should be inactive. If it is active, disable it using
    ```bash
    sudo ufw disable
    ```
* Change the settings:
    ```bash
    sudo ufw default deny incoming
    sudo ufw default allow outgoing
    sudo ufw allow 2200/tcp
    sudo ufw allow http
    sudo ufw allow ntp
    ```
* Enable UFW:
    ```bash
    sudo ufw enable
    ```

<br>

## Make sure that logging in as root user via SSH is not possible
It is good practice to prevent remote login for the root user to make brute force attacks even harder.<br>
To disable remote root login:

* Edit the *sshd_config* file (e.g. with vim):
    ```bash
    sudo vim /etc/ssh/sshd_config
    ```
* Look for the section titled *# Authentication* and make sure that *PermitRootLogin* is set to *no*.

<br>

## Author

**Elisabeth Strunk**<br>
<img src="readme_images/GitHub-Mark-32px.png" width=22> https://github.com/ElisabethStrunk<br>
<img src="readme_images/LI-In-Bug.png" width=22> https://www.linkedin.com/in/elisabeth-strunk/<br>