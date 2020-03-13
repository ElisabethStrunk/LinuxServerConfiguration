# Set up a new user

> This document is part of the [*Linux Server Setup*](README.md) project and is to be viewed within this context.

Originally this project started out as a student project for the [Udacity Full Stack Web Developer Nanodegree Program](https://www.udacity.com/course/full-stack-web-developer-nanodegree--nd004), called *Linux server configuration project*. In order for a Udacity grader to be able to inspect my work, I had to set up a new user with sudo permissions.<br>

The following describes the steps neccessary to set up such a new user.<br><br>
Example: user with username *"grader"*<br>
<br>

## Set up a new user with sudo permission
SSH into your server.<br>
Add a new user:
```bash
sudo adduser grader
```
During the creation process, you are asked to provide a password. Type in the password and confirm it by typing it in a second time.<br>
<br>
A list of users with sudo rights can be found in */etc/sudoers*.<br>
In Ubuntu, there also is a file */etc/sudoers.d* that gets included in */etc/sudoers*. System updates can override *sudoers*, but not *sudoers.d*. That means added super users will not be lost through a system update, if you set them up in *sudoers.d* instead of *sudoers*.<br>
So, to give the grader user sudo rights:

* Create the *sudoers.d* file for your new user:
    ```bash
    sudo touch /etc/sudoers.d/grader
    ```
* Open the file you just created (e.g. with vim):
    ```bash
    sudo vim /etc/sudoers.d/grader
    ```
* Add the following line to the grader file:
    ```bash
    grader ALL=(ALL) NOPASSWD:ALL
    ```
<br>

## Secure SSH connection to the new user
Create an SSH key pair for grader using the ssh-keygen tool:

* On your local machine (**not** on your server!) open a bash session and run:
    ```bash
    ssh-keygen
    ```
* During the keygen process, enter the file (incl. path) in which to save the key (*grader_key* for example).
* During the keygen process, enter in a password and confirm it by typing it in a second time.
* Two files will be generated in the specified path:

    * the private key (*grader_key*) and 
    * the public key (*grader_key.pub*)

* Copy the contents of the file *grader_key.pub* (on your local machine) for later.
* SSH into your server.
* On your server change into grader:
    ```bash
    su grader
    ```
* Change into the home directory:
    ```bash
    cd ~
    ```
* Create a new directory called *.ssh*:
    ```bash
    mkdir .ssh
    ```
* Create a file called *authorized_keys*:
    ```bash
    sudo touch ~/.ssh/authorized_keys
    ```
* Open the file you just created (e.g. with vim):
    ```bash
    sudo vim ~/.ssh/authorized_keys
    ```
* Paste the content of the file *grader_key.pub* you copied earlier on your local machine into the *authorized_keys* file.
* Save and exit.
* Adjust the permissions on the *.ssh* directory and the *authorized_keys* file:
    ```
    sudo chmod 700 .ssh
    sudo chmod 644 .ssh/authorized_keys
    ```
* Give *grader* ownership over the *authorized_keys* file:
    ```
    sudo chown -R grader.grader /home/grader/.ssh
    ```
* Check the */etc/ssh/sshd_config* file if *PasswordAuthentication* is set to *no*:
    ```bash
    cat /etc/ssh/sshd_config
    ```
* If it is not set to no, set it to no by editing */etc/ssh/sshd_config* (e.g. with vim).
* Restart SSH:
    ```
    sudo service ssh restart
    ```

Now on a remote machine you can use the private key file (grader_key) to connect to the grader user on the server - using the same procedure as you did before with the default user and the Lightsail key pair.<br>
<br>

## Author

**Elisabeth Strunk**<br>
<img src="readme_images/GitHub-Mark-32px.png" width=22> https://github.com/ElisabethStrunk<br>
<img src="readme_images/LI-In-Bug.png" width=22> https://www.linkedin.com/in/elisabeth-strunk/<br>