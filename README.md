# Udacity_FullStack_LinuxServerConfiguration
Linux server configuration project of the [Udacity Full Stack Web Developer Nanodegree Program](https://www.udacity.com/course/full-stack-web-developer-nanodegree--nd004).<br>
<br>

## Get your server
Go to https://lightsail.aws.amazon.com and log in.<br>
Click on *Create Instance*:<br>
<kbd><img src="readme_images/create-instance.PNG" width=550></kbd>
<br>Choose the following configurations for your instance:

* Select your instance location according to your own location.
* Under *Pick your instance image* -> *Select a platform* choose *Linux/Unix*
* Under *Pick your instance image* -> *Select a blueprint* first click on *OS Only*, then choose *Ubuntu 16.04 LTS*
* Under *Pick your instance image* -> *OPTIONAL* click *Change SSH key pair* and create a new SSH key pair by...
    * clicking on *Create New*
    * choosing your location
    * choosing a name for your key pair (profile-server)
    * click on *Generate Key Pair*
    * download the key by clicking *Download Key*
* Under *Choose your instance plan* choose a suitable payment model ($3.50 USD - First month free)
* Under *Identify your instance* choode a name for your instance (profile-server-elisabeth-strunk)

Click *Create instance*.<br>
You will be redirected to the *Instances* tab.<br>
Your new instance is in the progress of being activated. Wait for the activation process to finish. You can see, if the activation process has already finished by looking at the visualization of your new instance:<br>
**Before:**<br>
<kbd><img src="readme_images/instance-activation-pending.PNG" width=350></kbd>
<br>**After:**<br>
<kbd><img src="readme_images/instance-running.PNG" width=350></kbd>
<br>

## Access your server via SSH
Use PuTTY or the tool of your choice to connect to your instance. Instructions on how to do this are provided by Amazon.<br>
To view the instructions:

* Click on your instance to go to the instance's site.
* Make sure you are on the *Connect* tab:<br>
<kbd><img src="readme_images/instance-site-connect.PNG" width=550></kbd>
* Click that little *(?)* next to *Connect using your own SSH client*.
<br>

## Secure your server
Make sure the software packages installed on the server are up to date:

* Update the source list:
    ```bash
    sudo apt-get update
    ```
* Upgrade installed packages:
    ```bash
    sudo apt-get upgrade
    ```

<br>Change the SSH port from 22 to 2200:

* Make sure to configure the Lightsail firewall first:

    * Go to your instance's site on https://lightsail.aws.amazon.com.
    * Go to the *Networking* tab:<br>
    <kbd><img src="readme_images/instance-site-networking.PNG" width=550></kbd>
    * In the *Firewall* section add another rule "Custom TCP 2200"

* Use PuTTY or the tool of your choice to connect to your instance (see *Access your server via SSH* section of this readme)
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

<br>Configure the Uncomplicated Firewall (UFW):

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


## Author

**Elisabeth Strunk**<br>
<img src="readme_images/GitHub-Mark-32px.png" width=22> https://github.com/ElisabethStrunk<br>
<img src="readme_images/LI-In-Bug.png" width=22> https://www.linkedin.com/in/elisabeth-strunk/

## Acknowledgments

* Huge thanks to [Michael Wales](https://github.com/walesmd) who authored the Udacity course this project is based upon
* Many thanks to [Daniel Abrão](https://github.com/jungleBadger) who provides [instructions](https://gist.github.com/PurpleBooth/109311bb0361f32d87a2) on how to use Apache, Python 3, venv and wsgi together
* Many thanks to Kundan Singh who wrote an [article](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps#step-four-%E2%80%93-configure-and-enable-a-new-virtual-host) on "How To Deploy a Flask Application on an Ubuntu VPS"
* Many thanks to [Kenneth Reitz](https://www.kennethreitz.org) who authored an [Ubuntu manpage](http://manpages.ubuntu.com/manpages/eoan/man1/pipenv.1.html) about using [pipenv](https://github.com/pypa/pipenv) to manage Python packages when deploying network services
* Many thanks to [Website for Students](https://websiteforstudents.com) for providing a [tutorial](https://websiteforstudents.com/installing-the-latest-python-3-7-on-ubuntu-16-04-18-04/) on how to upgrade Python on Ubuntu 16.04
