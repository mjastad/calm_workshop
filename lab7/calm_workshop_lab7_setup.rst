*********************
**Install Docker CE**
*********************

Install using the repository
****************************

Before you install Docker CE for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

**SET UP THE REPOSITORY**

Install required packages. yum-utils provides the yum-config-manager utility, and device-mapper-persistent-data and lvm2 are required by the devicemapper storage driver.

.. code-block:: bash 

  $ sudo yum install -y yum-utils \
    device-mapper-persistent-data \
    lvm2
  
Use the following command to set up the stable repository. You always need the stable repository, even if you want to install builds from the edge or test repositories as well.

.. code-block:: bash 

  $ sudo yum-config-manager \
      --add-repo \
      https://download.docker.com/linux/centos/docker-ce.repo

**INSTALL DOCKER CE**

Install the latest version of Docker CE, or go to the next step to install a specific version.

.. code-block:: bash 

  $ sudo yum install docker-ce

**Warning:** If you have multiple Docker repositories enabled, installing or updating without specifying a version in the yum install or yum update command will always install the highest possible version, which may not be appropriate for your stability needs.

If this is the first time you are installing a package from a recently added repository, you will be prompted to accept the GPG key, and the key’s fingerprint will be shown. Verify that the fingerprint is correct, and if so, accept the key. The fingerprint should match 060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35.

Docker is installed but not started. The docker group is created, but no users are added to the group.

On production systems, you should install a specific version of Docker CE instead of always using the latest. List the available versions. This example uses the sort -r command to sort the results by version number, highest to lowest, and is truncated.

.. code-block:: bash 

  $ yum list docker-ce --showduplicates | sort -r
    docker-ce.x86_64            17.09.ce-1.el7.centos             docker-ce-stable
    
The contents of the list depend upon which repositories are enabled, and will be specific to your version of CentOS (indicated by the .el7 suffix on the version, in this example). Choose a specific version to install. The second column is the version string. You can use the entire version string, but you need to include at least to the first hyphen. The third column is the repository name, which indicates which repository the package is from and by extension its stability level. To install a specific version, append the version string to the package name and separate them by a hyphen (-).

**Note:** The version string is the package name plus the version up to the first hyphen. In the example above, the fully qualified package name is docker-ce-17.06.1.ce.

.. code-block:: bash 

  $ sudo yum install <FULLY-QUALIFIED-PACKAGE-NAME>
  
Start Docker.

.. code-block:: bash 

  $ sudo systemctl start docker

Verify that docker is installed correctly by running the hello-world image.

.. code-block:: bash 

  $ sudo docker run hello-world

This command downloads a test image and runs it in a container. When the container runs, it prints an informational message and exits.

Docker CE is installed and running. You need to use sudo to run Docker commands. Continue to Linux postinstall to allow non-privileged users to run Docker commands and for other optional configuration steps.

