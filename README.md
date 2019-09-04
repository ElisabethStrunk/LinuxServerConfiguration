# Udacity_FullStack_LinuxServerConfiguration
Linux server configuration project of the [Udacity Full Stack Web Developer Nanodegree Program](https://www.udacity.com/course/full-stack-web-developer-nanodegree--nd004).<br>
<br>
<br>
<br>

## Summary of configurations made

I __set up my server__ on an AWS Lightsail instance choosing *Ubuntu 16.04 LTS* for the operating system.<br>
A detailed description of all the steps I performed can be found [here](get_your_server.md).<br>

I __secured my server__ by updating all pre-installed packages, changing the default SSH port (incl. adjusting the Lightsail firewall), as well as setting up an Uncomplicated Firewall (UFW).<br>
A detailed description of all the steps I performed can be found [here](secure_your_server.md).<br>

I __set up a new user__ with sudo permissions so that a Udacity grader will be able to inspect my work. I also disabled password authentication and generated an SSH key pair for the new user, to secure SSH connections to my server.<br>
A detailed description of all the steps I performed can be found [here](give_grader_access.md).<br>

I __deployed my [*Item Catalog*](https://github.com/ElisabethStrunk/Udacity_FullStack_ItemCatalog) project__ onto my server, a Python 3.7 CRUD application developed with Flask and SQLite.<br>
I performed the following steps to successfully deploy my project:

* Configured and installed a custom build of Python 3.7, incl. SQLite extensions
* Installed Apache2
* Configured and installed a custom build of mod_wsgi
* Loaded the mod_wsgi module into Apache
* Set up a virtual environment for my application to run in
* Created a WSGI application from my [*Item Catalog*](https://github.com/ElisabethStrunk/Udacity_FullStack_ItemCatalog) project
* Set up a virtual host to serve my WSGI app

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

* Huge thanks to [Michael Wales](https://github.com/walesmd) who authored the Udacity course this project is based upon
* Many thanks to [Daniel Abrão](https://github.com/jungleBadger) who provides [instructions](https://gist.github.com/PurpleBooth/109311bb0361f32d87a2) on how to use Apache, Python 3, venv and wsgi together
* Many thanks to Kundan Singh who wrote an [article](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps#step-four-%E2%80%93-configure-and-enable-a-new-virtual-host) on "How To Deploy a Flask Application on an Ubuntu VPS"
* Many thanks to [Website for Students](https://websiteforstudents.com) for providing a [tutorial](https://websiteforstudents.com/installing-the-latest-python-3-7-on-ubuntu-16-04-18-04/) on how to upgrade Python on Ubuntu 16.04
* Many thanks to [Gareth Johnson](https://github.com/garethbjohnson) who authored an [article](https://medium.com/@garethbjohnson/serve-python-3-7-with-mod-wsgi-on-ubuntu-16-d9c7ab79e03a) on how to *Serve Python 3.7 with `mod_wsgi` on Ubuntu 16*
* Many thanks to Graham Dumpleton, who authored several user guides on mod_wsgi. For this document, I used his [*Virtual Environments*](https://modwsgi.readthedocs.io/en/develop/user-guides/virtual-environments.html) guide on how to use Python virtual environments with mod_wsgi and his [*Quick Configuration Guide*](https://modwsgi.readthedocs.io/en/develop/user-guides/quick-configuration-guide.html)
