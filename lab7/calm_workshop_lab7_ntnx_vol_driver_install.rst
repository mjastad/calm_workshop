****************************************
**Configuring the NTNX Docker Plugin VM**
****************************************

The plugin has been qualified on Centos 7, Rhel 7.3 and Ubuntu 16.04.2. It may work for older versions, but hasn't been  tested.

**Pre-Requisites**

- Install Docker version 17.x : 


**Installation:**

Installed the iscsi initiators utilities:

.. clode-block:: bash

  $ yum install iscsi-initiator-utils


Start the iscsi daemon:

.. clode-block:: bash

  $ systemd-tmpfiles --create
  $ systemctl start iscsid
  $ systemctl enable iscsid
  
Ensure iscsid is running using: 

.. clode-block:: bash

  $ systemctl status iscsid

Once you've configured the Docker Host VM, the plugin can be installed as follows:

.. code-block:: bash

  $ docker plugin install ntnx/nutanix_volume_plugin:latest PRISM_IP="prism-ip" DATASERVICES_IP="dataservices-ip" PRISM_PASSWORD="prism-passwd" PRISM_USERNAME="username" DEFAULT_CONTAINER="some-storage-container" --alias nutanix
    
**Note:**
 
- PRISM_IP: Prism management IP address of the Nutanix cluster
- DATASERVICES_IP: Data service IP configured in the Nutanix cluster
- PRISM_USERNAME: Username of a Prism user
- PRISM_PASSWORD: The corresponding password for the Prism user
- DEFAULT_CONTAINER: This is the name of an existing storage container on the Nutanix cluster. This storage container will be used to store the created persistent volumes

Check if plugin is installed and enabled using: 

.. code-block:: bash

  $ docker plugin ls
  
  ID                  NAME                DESCRIPTION                        ENABLED
  db1d93096fb9        nutanix:latest      Nutanix volume plugin for docker   true
  
  
