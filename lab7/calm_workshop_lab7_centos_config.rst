**********************************************
**Initial Server Setup with CentOS Server v7**
**********************************************

.. contents::

**Introduction**
****************

When you first create a new server, there are a few configuration steps that you should take early on as part of the basic setup. This will increase the security and usability of your server and will give you a solid foundation for subsequent actions.

**Step 1 - Create VM**
***********************

Create a VM using the CentOS Server v7 QCOW/Disk image and the following parameters:

- Name: Docker
- vCPU: 2x 
- Core/VCPU: 1x
- Memory: 4 GiB
- Network: bootcamp

**Step 2 - Root Login**
***********************

To log into your server, you will need to know your server's public IP address and the password for the "root" user's account. If you have not already logged into your server, you may want to follow the first tutorial in this series, How to Connect to Your Droplet with SSH, which covers this process in detail.

If you are not already connected to your server, go ahead and log in as the root user using the following command (substitute the highlighted word with your server's public IP address):

.. code-block:: bash 
  
  $ ssh root@SERVER_IP_ADDRESS
  
Complete the login process by accepting the warning about host authenticity, if it appears, then providing your root authentication (password or private key). If it is your first time logging into the server, with a password, you will also be prompted to change the root password.

**About Root**

The root user is the administrative user in a Linux environment that has very broad privileges. Because of the heightened privileges of the root account, you are actually discouraged from using it on a regular basis. This is because part of the power inherent with the root account is the ability to make very destructive changes, even by accident.

The next step is to set up an alternative user account with a reduced scope of influence for day-to-day work. We'll teach you how to gain increased privileges during the times when you need them.

**Step 3 - Create a New User**
********************************

Once you are logged in as root, we're prepared to add the new user account that we will use to log in from now on.

This example creates a new user called "demo", but you should replace it with a user name that you like:

.. code-block:: bash 
  
  $ adduser nucalm

Next, assign a password to the new user (again, substitute "nucalm" with the user that you just created):

.. code-block:: bash 
  
  $ passwd nucalm

Enter a password, and repeat it again to verify it.

**Step 4 - Root Privileges**
********************************

Now, we have a new user account with regular account privileges. However, we may sometimes need to do administrative tasks. To avoid having to log out of our normal user and log back in as the root account, we can set up what is known as "super user" or root privileges for our normal account. This will allow our normal user to run commands with administrative privileges by putting the word sudo before each command.
To add these privileges to our new user, we need to add the new user to the "wheel" group. By default, on CentOS 7, users who belong to the "wheel" group are allowed to use the sudo command.
As root, run this command to add your new user to the wheel group (substitute the highlighted word with your new user):

.. code-block:: bash 
  
  $ gpasswd -a nucalm wheel

Now your user can run commands with super user privileges! For more information about how this works, check out our sudoers tutorial.
