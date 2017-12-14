****************************************
**Configuring the NTNX DockerPlugin VM**
****************************************

The plugin has been qualified on Centos 7, Rhel 7.3 and Ubuntu 16.04.2. It may work for older versions, but hasn't been  tested.

1. Install a Docker version >= 1.13 by following the instructions here
2. yum install iscsi-initiator-utils
3. systemd-tmpfiles --create
4. systemctl start iscsid
5. systemctl enable iscsid
6. Ensure iscsid is running using: systemctl status iscsid

Once you have configured the host VM as described above the plugin can be installed with the following command:

.. code-block:: bash

  $ docker plugin install ntnx/nutanix_volume_plugin:latest PRISM_IP="prism-ip" DATASERVICES_IP="dataservices-ip" PRISM_PASSWORD="prism-passwd" PRISM_USERNAME="username" DEFAULT_CONTAINER="some-storage-container" --alias nutanix
    PRISM_IP: Prism management IP address of the Nutanix cluster

- DATASERVICES_IP: Data service IP configured in the Nutanix cluster
- PRISM_USERNAME: Username of a Prism user
- PRISM_PASSWORD: The corresponding password for the Prism user
- DEFAULT_CONTAINER: This is the name of an existing storage container on the Nutanix cluster. This storage container will be used to store the created persistent volumes

Check if plugin is installed and enabled using: 

.. code-block:: bash

  $ docker plugin ls
  ID                  NAME                DESCRIPTION                        ENABLED
  db1d93096fb9        nutanix:latest      Nutanix volume plugin for docker   true
  
  
