## Give grader access
In order for your project to be reviewed, the grader needs to be able to log in to your server.<br><br>
SSH into your server and set up a new user with sudo permission:<br>
Example: user with username *grader*
```bash
sudo adduser grader
```
During the creation process, you are asked to provide a password. Type in the password and confirm it by typing it in a second time. (udacity-1)<br>

A list of users with sudo rights can be found in */etc/sudoers*.
In Ubuntu, there also is a file */etc/sudoers.d* that gets included in */etc/sudoers*. System updates can override sudoers, but not sudoers.d. That means added super users will not be lost through a system update, if you set them up in *sudoers.d* instead of *sudoers*.<br>

So, to give the grader user sudo rights:<br>

* Create the sudoers.d file for your new user:
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

Create an SSH key pair for grader using the ssh-keygen tool:<br>

* On your local machine (**not** on your server!) open a bash session and run:
    ```bash
    ssh-keygen
    ```
* During the keygen process, enter the file (incl. path) in which to save the key (grader_key for example)
* During the keygen process, enter in a password and confirm it by typing it in a second time.
* Two files will be generated in the specified path:

    * the private key (grader_key) and 
    * the public key (grader_key.pub)

* Copy the contents of the file *grader_key.pub* (on your local machine) for later.
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
* Create a file called authorized_keys:
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

Now on a remote machine you can use the private key file (grader_key) to connect to the grader user on the server - using the same procedure as you did before with the default user and the Lightsail key pair.
<br>