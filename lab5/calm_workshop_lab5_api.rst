********************
**NuCalm REST APIs**
********************

.. contents::

**Connectivity Instructions:**
******************************

+------------+--------------------------------------------------------+
| IP         |                                           Cluster IP   |
+------------+--------------------------------------------------------+
| Username   |                                           Cluster User |
+------------+--------------------------------------------------------+
| Password   |                                           Cluster Pass | 
+------------+--------------------------------------------------------+

Lab Overview
************

The Automation Lab starts with an introduction to NuCalm REST API, and associated JSON. The lab is a set of exercises designed to walk participants through navigating the REST API Explorer, locating Blueprint and Appication REST requests, executing the requests, and observing the results using both the Swagger generated API Explorer (Prism Central). Participants will also deploy several commands using python.

NTNX REST API Explorer Requests:

- App
- Blueprint
- Role
- Project

Requirements:
*************

Reference:

- REST/HTTP Overview:  REST-HTTP-Overview_
- NTNX REST API Explorer Overview:  NTNX-REST-API-Explorer-Overview_
- Chrome Web Browser + Developer Tools
- Chrome JSON Editor: Chrome-JSON-Editor-Extension_

Development:

- CentOS Server v7 VM created:  configure-centos-server-v7_ 
- Python 2.7.x
- Git Hub Account: https://github.com


The Automation Lab starts with an introduction to NuCalm REST API, and associated JSON. The lab is a set of exercises designed to walk participants through navigating the REST API Explorer, locating Bluepint and Appication commands, executing the commands, and observing the results using both the Swagger generated API Explorer (Prism), and Postman (3^rd^ party API toolchain). Participants will then deploy several commands using python.

Create a CentOS Server v7 VM
****************************

Create a CentOS Server v7 VM on the assigned cluster using Prism Central using the folloiwng specifications:

- vCPU: 2x, 1x core/vCPU
- mem:  4 GiB
- network: bootcamp
- name: calm_lab_dev
- image: CentOS Server v7  (Disk)

  

Create a Development Environment
********************************


**Install pip, and requests:**

We'll need to make sure the python 2.7 runtime has all the appropriate packages, sepcifically *pip* and *requests*. We'll provision a CentOS Server VM to insure participants are working fromm a common-base.

- Pip is a tool for installing and managing Python packages.
- Requests is a Python package used to conmunicate over http.

*pip* is part of Extra Packages for Enterprise Linux (EPEL), which is a community repository of non-standard packages for the RHEL distribution. You'll be required to install the EPEL repository.

.. code-block:: bash

  $ yum install epel-release
  
As a matter of best practice we’ll update our package(s)
 
.. code-block:: bash
 
  $ yum -y update

Then let’s install python-pip and any required packages:

.. code-block:: bash

  $ yum -y install python-pip
  
View a list of helpful commands, and check the version of *pip* that is installed:

.. code-block:: bash

  $ pip --help
  $ pip -v
  
Once *pip has been installed and verified, we can now install *requests* as follows:

.. code-block:: bash

  $ pip install requests

    Collecting requests
      Downloading requests-2.18.4-py2.py3-none-any.whl (88kB)
        100% |████████████████████████████████| 92kB 6.9MB/s 
    Collecting certifi>=2017.4.17 (from requests)
      Downloading certifi-2017.11.5-py2.py3-none-any.whl (330kB)
        100% |████████████████████████████████| 337kB 3.4MB/s 
    Collecting chardet<3.1.0,>=3.0.2 (from requests)
      Downloading chardet-3.0.4-py2.py3-none-any.whl (133kB)
        100% |████████████████████████████████| 143kB 6.8MB/s 
    Collecting idna<2.7,>=2.5 (from requests)
      Downloading idna-2.6-py2.py3-none-any.whl (56kB)
        100% |████████████████████████████████| 61kB 10.4MB/s 
    Collecting urllib3<1.23,>=1.21.1 (from requests)
      Downloading urllib3-1.22-py2.py3-none-any.whl (132kB)
        100% |████████████████████████████████| 133kB 7.4MB/s 
    Installing collected packages: certifi, chardet, idna, urllib3, requests
    Successfully installed certifi-2017.11.5 chardet-3.0.4 idna-2.6 requests-2.18.4 urllib3-1.22


**Install Git:**

Participants will need access to Git to download or clone the calm-lab automation repository. 

Power-on the VM and login to the assigned *ip-address* as **user:** *root*, **password:** *nutanix/4u* using *ssh* or *putty*.

Install git:

.. code-block:: bash

  $ yum install git -y
  
Create a directory for development:

.. code-block:: bash

  $ mkdir /root/development
  
Change to the directory and run:

.. code-block:: bash

  $ git clone https://github.com/mjastad/automation.git

If all was successfull you should find a directory */root/automation/solution*

Edit */root/automation/solution/main.py* and set the connection variables for the assigned cluster.  Make rue the values are within quotes(**""**):

- USER:  Cluster Admin user name
- PASSWD: Cluster Admin user password
- IPADDRESS: Cluster IP Address

Be sure to comment **v2** imports and functions and uncomment **v3** imports as shown below:

.. code-block:: bash

  #!/usr/bin/env python

  """
  File: main.py: NTNX REST API Driver.
  """

  '''
  from v2.core.Connection import Connection
  from v2.core.Host import Host
  from v2.core.User import User
  from v2.services.VirtualMachineService import VirtualMachineService
  from v2.services.ImageService import ImageService
  from v2.services.StorageContainerService import StorageContainerService

  '''
  from v3.core.Connection import Connection
  from v3.core.Host import Host
  from v3.core.User import User
  from v3.services.VirtualMachineService import VirtualMachineService
  from v3.services.ImageService import ImageService
  from v3.services.StorageContainerService import StorageContainerService
  from v3.services.ApplicationService import ApplicationService
  from v3.services.BlueprintService import BlueprintService

  USER = "Cluster Admin user name"
  PASSWD = "Cluster Admin password!"
  IPADDRESS = "Cluster IP Address"
  PORT = "9440"

  def main():

    data = {'filter': '', 'offset': 0, 'length': 20}

    user = User(USER, PASSWD)
    host = Host(IPADDRESS, PORT)
    connection = Connection(user, host)

    #v2 API
    #_virtualMachines(connection)

    #v3 API
    getVirtualMachines(connection, data)
    getApplications(connection, data)
    getBlueprints(connection, data)

  if __name__ == "__main__":
    main()


Test the runtime and the code by running:

.. code-block:: bash
  
   $ python main.py

If successfull,  You should see VM, Blueprint and Application property output...

Configure Postman (Optional)
****************************

This lab will use Postman allowing you to preserve the v3 REST API Requests so they can be used later when demonstrating NuCalm REST API's HOW-TO with a partner or customer.  If you use another REST API tool, feel to use it in place of Postman.

The following steps assumes you have Postman installed.  

Create a **Collection** and give it a name (i.e. Nutanix), and create the following subfolders within the collection:

- V3:Apps
- V3:Blueprints
- V3:Roles
- V3:Projects

|image1|

Accessing the REST API's
************************

A link for launching the REST API Explorer may not be accessible via Prism Central - specifically in the case of AOS v5.5.  The explorer can be launched by explicitly typing the *url* in the address bar of your browser as follows:

**Note:** . The v3 REST API's for NuCalm can only be accessed via Prism Central(PC) *url*.

.. code-block:: bash

  https://[PC-IPADDRESS]:9440/api/nutanix/v3/api_explorer/index.html
  

|image0|

Once the API Explorer appears, be sure to authenticate or sign-in (as shown above) using the PC credentials.  Click **Explorer** to authenicate.  The explorer should refresh and display the REST API Targets + requests.



Issuing READ Requests
*********************

In this section we'll learn how to Navigate the REST API Explorer to read and gather information for:

- App(s)
- Blueprint(s)
- Project(s)
- Role(s)


Issuing a STATE-CHANGE Request
******************************

In this section we'll learn how to Navigate the REST API Explorer to execute a command that changes the state of an app or blueprint:

- Import a Blueprint(JSON)
- Create a Blueprint
- Launch Blueprint
- Delete an App

Automation of REST Endpoints
****************************

In this section we'll learn how to run python code instrumented to programmatically perform the commands we ran manually via REST API Explorer:

READ:

- App(s)
- Blueprint(s)
- Project(s)
- Role(s)

STATE-CHANGE:

- Import a Blueprint(JSON)
- Create a Blueprint
- Launch Blueprint
- Delete an App



.. _configure-centos-server-v7: ../lab6/calm_workshop_lab6_config_centos.rst
.. _REST-HTTP-Overview: calm_workshop_lab5_rest_overview.rst
.. _NTNX-REST-API-Explorer-Overview: calm_workshop_ntnx_api_explorer_overview.rst
.. _Chrome-JSON-Editor-Extension: https://chrome.google.com/webstore/detail/json-editor/lhkmoheomjbkfloacpgllgjcamhihfaj?hl=en

.. |image0| image:: ./media/image1.png
