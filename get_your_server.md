# Set up your server

> This document is part of the [*Linux Server Setup*](README.md) project and is to be viewed within this context.

## Get your server
Go to https://lightsail.aws.amazon.com and log in.<br>
Click on *Create Instance*:<br>
<kbd><img src="readme_images/create-instance.PNG" width=550></kbd>
<br>Choose the following configurations for your instance:

* Select your instance location according to your own location.
* Under *Pick your instance image* -> *Select a platform* choose *Linux/Unix*.
* Under *Pick your instance image* -> *Select a blueprint* first click on *OS Only*, then choose *Ubuntu 16.04 LTS*.
* Under *Pick your instance image* -> *OPTIONAL* click *Change SSH key pair* and create a new SSH key pair by...
    * clicking on *Create New*
    * choosing your location
    * choosing a name for your key pair
    * click on *Generate Key Pair*
    * download the key by clicking *Download Key*
* Under *Choose your instance plan* choose a suitable payment model.
* Under *Identify your instance* choode a name for your instance.

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
* Follow the instructions.

<br>

## Author

**Elisabeth Strunk**<br>
<img src="readme_images/GitHub-Mark-32px.png" width=22> https://github.com/ElisabethStrunk<br>
<img src="readme_images/LI-In-Bug.png" width=22> https://www.linkedin.com/in/elisabeth-strunk/<br>