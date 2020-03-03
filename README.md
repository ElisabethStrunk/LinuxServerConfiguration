# Udacity_FullStack_LinuxServerConfiguration
Linux server configuration project of the [Udacity Full Stack Web Developer Nanodegree Program](https://www.udacity.com/course/full-stack-web-developer-nanodegree--nd004).<br>
For this project I took a baseline installation of a Linux server and prepared it to host one of my web applications. It included securing my server from a number of attack vectors, as well as installing and configuring a database server to serve my [*Item Catalog*](https://github.com/ElisabethStrunk/Udacity_FullStack_ItemCatalog) application as a WSGI app.<br>
<br>
This README gives an overview over the work I did for this project. It also includes links to detailed descriptions of all steps I did along the way. These descriptions are written in a tutorial-like form, so they can be used as guidelines for myself and the whole GitHub community when setting up a Linux server to serve Python 3.7 applications as WSGI apps.<br>
A list of third-party resources I used to complete this project can be found at the end of this README in the *Acknowledgements* section.<br>
<br>
You are very welcome to visit my server:<br>
**IP address**: 3.127.90.165<br>
**URL**:        http://3.127.90.165/<br>
<br>

## Summary of configurations made

I __set up my server__ on an AWS Lightsail instance choosing *Ubuntu 16.04 LTS* for the operating system.<br>
A detailed description of all the steps I performed to achieve this can be found [here](get_your_server.md).<br>

I __secured my server__ by implementing the following security measures:

* Updated all pre-installed packages
* Changed the default SSH port (incl. adjusting the Lightsail firewall)
* Set up an Uncomplicated Firewall (UFW)
* Made sure that logging in as root user via SSH is not possible

A detailed description of all the steps can be found [here](secure_your_server.md).<br>

I __set up a new user__ with sudo permissions so that a Udacity grader will be able to inspect my work. I also disabled password authentication and generated an SSH key pair for the new user, to secure SSH connections to my server.<br>
A detailed description of all the steps I performed to achieve this can be found [here](give_grader_access.md).<br>

I __deployed my [*Item Catalog*](https://github.com/ElisabethStrunk/Udacity_FullStack_ItemCatalog) project__ onto my server; a Python 3.7 CRUD application developed with Flask and SQLite.<br>
I performed the following steps to successfully deploy my project:

* Configured and installed a custom build of Python 3.7, incl. SQLite extensions
* Installed Apache2
* Configured and installed a custom build of mod_wsgi
* Loaded the mod_wsgi module into Apache
* Set up a virtual environment for my application to run in
* Created a WSGI application from my [*Item Catalog*](https://github.com/ElisabethStrunk/Udacity_FullStack_ItemCatalog) project
* Set up a virtual host to serve my WSGI app
* Made sure that the .git directories on my server are not publicly accessible via a browser

A detailed description of all the steps I performed, as well as the reasons for the neccessity of custom builds of Python and mod_wsgi can be found [here](deploy_your_project.md).<br>
<br>

## Summary of software installed 
In the following a summary of all software I installed (without dependencies) is given.<br>
<br>
### Custom-build software:

* Python 3.7.2, incl. SQLite extensions
* mod_wsgi 4.6.5, compiled for the custom Python 3.7.2 build

### Packages from the dpkg packaging system:

* apache2
* apache2-dev
* build-essential
* checkinstall
* git
* libapache2-mod-wsgi-py3
* libbz2-dev
* libc6-dev
* libffi-dev
* libgdbm-dev
* liblzma-dev
* libncurses5-dev
* libnss3-dev
* libpq-dev
* libreadline6-dev
* libreadline-dev
* libreadline-gplv2-dev
* libsqlite3-dev
* libssl-dev
* lzma
* lzma-dev
* python3-pip
* python-dev
* python-virtualenv
* tk8.5-dev
* tk-dev
* wget
* zlib1g-dev

### Python packages:

* bleach
* Flask
* httplib2
* oauth2client
* requests
* SQLAlchemy

<br>

## Author

**Elisabeth Strunk**<br>
<img src="readme_images/GitHub-Mark-32px.png" width=22> https://github.com/ElisabethStrunk<br>
<img src="readme_images/LI-In-Bug.png" width=22> https://www.linkedin.com/in/elisabeth-strunk/<br>
<br>

## Acknowledgments

* Huge thanks to [Michael Wales](https://github.com/walesmd) who authored the Udacity course this project is based upon.
* Many thanks to [Daniel Abrão](https://github.com/jungleBadger) who provides [instructions](https://github.com/jungleBadger/-nanodegree-linux-server-troubleshoot/blob/master/python3+venv+wsgi/README.md) on how to use Apache, Python 3, venv and wsgi together.
* Many thanks to Kundan Singh who wrote an [article](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps#step-four-%E2%80%93-configure-and-enable-a-new-virtual-host) on "How To Deploy a Flask Application on an Ubuntu VPS".
* Many thanks to [*Website for Students*](https://websiteforstudents.com) for providing a [tutorial](https://websiteforstudents.com/installing-the-latest-python-3-7-on-ubuntu-16-04-18-04/) on how to upgrade Python on Ubuntu 16.04.
* Many thanks to [*fastzombies*](https://stackoverflow.com/users/6615775/fastzombies) who provided a [bash code snippet](https://stackoverflow.com/a/38648131/10917711) that helped me to successfully build the SQLite module into the Python custom build.
* Many thanks to [Gareth Johnson](https://github.com/garethbjohnson) who authored an [article](https://medium.com/@garethbjohnson/serve-python-3-7-with-mod-wsgi-on-ubuntu-16-d9c7ab79e03a) on how to *Serve Python 3.7 with 'mod_wsgi' on Ubuntu 16*.
* Many thanks to Graham Dumpleton, who authored several user guides on mod_wsgi. For this document, I used his [*Virtual Environments*](https://modwsgi.readthedocs.io/en/develop/user-guides/virtual-environments.html) guide on how to use Python virtual environments with mod_wsgi and his [*Quick Configuration Guide*](https://modwsgi.readthedocs.io/en/develop/user-guides/quick-configuration-guide.html).
* Many thanks to [Bennett McElwee](https://github.com/bennettmcelwee), who describes a [method](https://stackoverflow.com/a/17916515/10917711) to make sure that .git directories cannot be publicly accessed via a browser.
