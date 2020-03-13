# Deploy your project

> This document is part of the [*Linux Server Setup*](README.md) project and is to be viewed within this context.

Please note that the following instructions are suitable, if you want to deploy a **Python 3 Flask** application onto your server. Other kinds of applications (including Python 2 applications) need different prerequisites that are not covered here!<br>
<br>
Example: My [*Item Catalog*](https://github.com/ElisabethStrunk/Udacity_FullStack_ItemCatalog) project.<br>
<br>

## Prepare to deploy your Python 3 Flask project
### Configure the local timezone to UTC:

* Check the timezone:
    ```bash
    date
    ```
* If the timezone is not set to UTC, change the timezone to UTC:
    ```bash
    sudo timedatectl set-timezone UTC
    ```

<br>

### Prepare Python 3: 

* Install some general Python libraries:
    ```bash
    sudo apt-get install libpq-dev python-dev
    ```
* Install the needed Python version:<br>
    Ubuntu 16.04 comes with Python 3.5. If your application needs Python 3.6 or higher, you have to manually install the required Python version. (If your application runs fine with Python 3.5, skip the following steps.)<br>
    My [*Item Catalog*](https://github.com/ElisabethStrunk/Udacity_FullStack_ItemCatalog) application uses Python 3.6 features, so an upgrade is neccessary. The application was developed and tested on Python 3.7.2, so this version was chosen for the upgrade. 

    * Ubuntu 16.04's *apt-get* does not find the python3.7 package, so you need to install it from its source code. Furthermore, my application uses an SQLite database. So it is neccessary to configure the Python build accordingly. This requires the installation of the following packages first:
        ```bash
        sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev wget
        for pkg in build-essential zlib1g-dev libbz2-dev liblzma-dev libncurses5-dev libreadline6-dev libsqlite3-dev libssl-dev libgdbm-dev liblzma-dev tk8.5-dev lzma lzma-dev libgdbm-dev
        do
            sudo apt-get -y install $pkg
        done
        ```
    * Download the source code of the desired Python release from the Python download page:
        ```bash
        cd /tmp
        wget https://www.python.org/ftp/python/3.7.2/Python-3.7.2.tar.xz
        ```
    * Extract the file and install:
        ```bash
        tar -xf Python-3.7.2.tar.xz
        cd Python-3.7.2
        sudo ./configure --enable-optimizations
        sudo ./configure --enable-loadable-sqlite-extensions
        sudo ./configure --enable-shared
        sudo make
        sudo make install
        ```
    * Create the necessary links and cache to the new shared Python library:
        
        * Edit */etc/ld.so.conf* (e.g. with vim):
            ```bash
            sudo vim /etc/ld.so.conf
            ```
        * Add the following line (do not forget to keep an empty line at the end!):
            ```bash
            /usr/local/lib/python3.7
            ```
        * create the links and cache:
            ```bash
            /sbin/ldconfig -v
            ```
        
    * Check if the desired version of Python was successfully installed:
        ```bash
        python3 --version
        ```
<br>

### Install Apache:

* Install the Apache engine:
    ```bash
    sudo apt-get install apache2
    ```
* Check the successful installation of Apache:

    * On your local machine open your browser
    * Enter your instance's public IP address in the address bar
    * If the installation was successful, the browser should show the *Apache2 Ubuntu Default Page*

<br>

### Set up mod_wsgi:
Install and configure mod_wsgi functionalities for Apache to serve a Python mod_wsgi application:<br>

For mod_wsgi to work, the Python application's Python version needs to match the Python version for which mod_wsgi was compiled. If your Python application uses Python 3.5, you can simply use the easy-install option:
```bash
sudo apt-get install libapache2-mod-wsgi-py3
sudo a2enmod wsgi
```
If your application needs a different version, you will have to make custom builds of Python and mod_wsgi.<br>
My [*Item Catalog*](https://github.com/ElisabethStrunk/Udacity_FullStack_ItemCatalog) application will use Python 3.7, so the following steps are necessary:

* Install prerequisites:
    ```bash
    sudo apt update
    sudo apt-get install apache2-dev
    sudo apt install build-essential checkinstall
    sudo apt install libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev
    sudo apt install libffi-dev
    ```

* Build Python: Already done -> see previous section.

* Build mod_wsgi:

    * Download the source code of the desired release from the mod_wsgi download page:
        ```bash
        cd /tmp
        wget https://github.com/GrahamDumpleton/mod_wsgi/archive/4.6.5.tar.gz
        ```
    * Extract the file and build it using the previously installed Python build:
        ```bash
        tar xvfz 4.6.5.tar.gz
        cd mod_wdgi-4.6.5
        sudo ./configure --with-python=/usr/local/bin/python3.7
        sudo make
        sudo make install
        ```

* Load the mod_wsgi module into Apache:<br>
    In the last line of the console log you can see the path of the .so file that was created. Usually this will be */usr/lib/apache2/modules/mod_wsgi.so*.<br>
    
    * Look at *wsgi.load*:
        ```bash
        cat /etc/apache2/mods-available/wsgi.load
        ```
    * Make sure, that it contains the following line (using the path of the .so file from before):
        ```
        LoadModule wsgi_module /usr/lib/apache2/modules/mod_wsgi.so
        ```
    * Enable the module and restart Apache:
        ```bash
        sudo a2enmod wsgi
        apachectl stop
        apachectl start
        ```
    * Check, if the modifications were successful:
        ```bash
        cat /var/log/apache2/error.log
        ```
        You should see an entry with the following message:
        ```
        Apache/2.4.18 (Ubuntu) mod_wsgi/4.6.5 Python/3.7 configured -- resuming normal operations
        ```

<br>

### Prepare virtual environment tools:

* Install pip:
    ```bash
    sudo apt-get install python3-pip
    ```
* Install virtualenv for Python:
    ```bash
    sudo apt-get install python-virtualenv
    ```

<br>

### Install git:
```bash
sudo apt-get install git
```
<br>

## Deploy a project onto your server
Example: My [*Item Catalog*](https://github.com/ElisabethStrunk/Udacity_FullStack_ItemCatalog) project.<br>

### Clone and set up your project:
Clone your Python Flask project from a GitHub repository you created earlier:<br>

* Create your project folder:
    ```bash
    sudo mkdir /var/www/item_catalog
    ```
* Clone your repository into the folder:<br>
    ```bash
    cd /var/www/item_catalog
    sudo git clone https://github.com/ElisabethStrunk/Udacity_FullStack_ItemCatalog.git
    ```
* Rename the project to the name you want it referred to on your server:
    ```bash
    sudo mv ./Udacity_FullStack_ItemCatalog ./item_catalog
    ```

<br>

Add an *\_\_init\_\_.py* to your project:

* Create the file (if it does not already exist):
    ```bash
    touch /var/www/item_catalog/item_catalog/app/__init__.py
    ```
* Edit the file (e.g. with vim):
    ```bash
    sudo vim /var/www/item_catalog/item_catalog/app/__init__.py
    ```
* Add an import of the Flask application object (in my project the object is called *app*):
    ```python
    from .application import app
    ```

<br>

### Set up a virtual environment for your project to run in:

* Change into you project's directory:
    ```bash
    cd /var/www/item_catalog/item_catalog
    ```
* Create a virtual environment for your project:
    ```bash
    sudo virtualenv -p python3 venv
    ```
    Take note of the exact path of the root directory of the virtual environment: */var/www/item_catalog/item_catalog/venv*
* Activate the virtual environment:
    ```bash
    source venv/bin/activate
    ```
* Check if the Python version and the Python version for pip is the right one (as per your application's requirements):
    ```bash
    python --version
    pip --version
    ```
* Install all packages your project depends on, using pip and the python binary of your project's virtual environment. In case of my *Item Catalog* project:
    ```bash
    sudo ./venv/bin/python -m pip install "httplib2==0.13.1"
    sudo ./venv/bin/python -m pip install "Flask==1.1.1"
    sudo ./venv/bin/python -m pip install "oauth2client==4.1.3"
    sudo ./venv/bin/python -m pip install "requests==2.22.0"
    sudo ./venv/bin/python -m pip install "SQLAlchemy==1.3.6"
    sudo ./venv/bin/python -m pip install "bleach==3.1.0"
    ```
* Check, if all packages were installed within the virtual environment:
    ```bash
    pip list
    ```
* Check, if your application is runnable (virtual environment must be activated):
    ```bash
    python application.py
    ```
* Deactivate the virtual environment again:
    ```bash
    deactivate
    ```

<br>

### Create the .wsgi file:

* Create a .wsgi file in the outer directory of your project folder:
    ```bash
    sudo touch /var/www/item_catalog/item_catalog.wsgi
    ```
* Edit *item_catalog.wsgi* (e.g. by using *vim*):
    ```bash
    sudo vim /var/www/item_catalog/item_catalog.wsgi
    ```
* Add the following lines, using the path where your application directory is located:
    ```bash
    import sys
    import logging
    logging.basicConfig(stream=sys.stderr)
    sys.path.insert(0,"/var/www/item_catalog/item_catalog")
    from app import app as application
    ```

<br>

### Set up a virtual host for your project:

* Look up the exact path of the root directory of the virtual environment that you took note of earlier (*/var/www/item_catalog/item_catalog/venv* in my example).<br>
    Copy the path for later.
* Create an Apache site configuration file:
    ```bash
    sudo touch /etc/apache2/sites-available/item_catalog.conf
    ```
* Edit the configuration file (e.g. with vim):
    ```bash
    sudo vim /etc/apache2/sites-available/item_catalog.conf
    ```
* Add the following lines, using your instance's public IP address and the path of the root directory of the virtual environment you copied earlier:
    ```
    <VirtualHost *:80>
        ServerName YOUR_INSTANCES_IP_ADDRESS_HERE
        ServerAlias YOUR_REMOTE_NAME_SERVER_HERE (optional)
        ServerAdmin ubuntu@YOUR_INSTANCES_IP_ADDRESS_HERE

        WSGIDaemonProcess item_catalog python-home=YOUR_PATH_TO_VENV_ROOT_DIRECTORY_HERE
        WSGIProcessGroup item_catalog
        WSGIApplicationGroup %{GLOBAL}

        WSGIScriptAlias / /var/www/item_catalog/item_catalog.wsgi

        <Directory /var/www/item_catalog>
        <IfVersion < 2.4>
            Order allow,deny
            Allow from all
        </IfVersion>
        <IfVersion >= 2.4>
            Require all granted
        </IfVersion>
        </Directory>
        
        ErrorLog ${APACHE_LOG_DIR}/error.log
        LogLevel warn
        CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>
    ```
* Enable the virtual host:
    ```bash
    cd /etc/apache2/sites-available
    sudo a2ensite item_catalog.conf
    ```
* Activate the new Apache configurations:
    ```bash
    sudo service apache2 reload
    sudo service apache2 restart
    ```
    Or reboot your instance.
* Test your conf files:
    ```bash
    cd /etc/apache2
    apache2ctl configtest
    ```
* Fix any errors or warnings before continuing.

<br>

### Secure your .git directory
Make sure that your .git directory is not publicly accessible via a browser:

* Create an *.htaccess* file at the root of your web server:
    ```bash
    sudo touch /root/.htaccess
    ```
* Edit the file (e.g. with vim):
    ```bash
    sudo vim /root/.htaccess
    ```
* Add the following line:
    ```bash
    RedirectMatch 404 /\.git
    ```

This solution hides all .git directories and even other Git files like .gitignore and .gitmodules.<br>
<br>

## Author

**Elisabeth Strunk**<br>
<img src="readme_images/GitHub-Mark-32px.png" width=22> https://github.com/ElisabethStrunk<br>
<img src="readme_images/LI-In-Bug.png" width=22> https://www.linkedin.com/in/elisabeth-strunk/<br>