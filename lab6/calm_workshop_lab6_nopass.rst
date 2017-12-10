*************************************
**SSH Passwordless Login – CentOS 7**
*************************************

SSH is a client and server protocol, and it helps us to access the remote system over the network through the encrypted tunnel. Whenever the client access the server, the client downloads the secure key from the server and at the same time-server also downloads the key from a client. Those two keys make the encrypted tunnel between the server and client, so that data transfer very securely over the network.
SSH is widely used as the alternative to FTP, as you know any thing that uses TCP network asks password to collect data. SSH is also a TCP service, and it requires a password to access the remote machine. If the organization has a large number of servers, every time admin has to enter the password to access the remote system. It is a pain to enter the password multiple times; SSH comes with new feature called password less login, that helps to access the remote machine without entering the password.
To enable the password less login, we have to put the public key entry of client host name and user detail on the remote server. That key entry will be on the following file (~/.ssh/authorized_keys) (~=Home directory of the user) according to your remote user.
Follow the steps to create the password less login. Here we have two machines with two different usernames

**Assumptions:**

1. We'll assume you've successfulyl deployed the MySQL Application and is currently running.
2. You've successfully created a CentOS Server v7 VM  to host *Ansible*.

**Create remote users**
***********************

Create/Add a new user *ansible*, on each of the VMs as part of the MySQL Application (i.e. *MySQLMaster, MySQLSlave*).

.. code-block:: bash

  $ adduser ansible
  $ passwd ansible
  Changing password for user test.
  New password:   (type: P@$$w0rd)
  Retype new password: (type: P@$$w0rd)
  passwd: all authentication tokens updated successfully
  $


  
  

