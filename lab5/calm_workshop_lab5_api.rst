********************************
**NuCalm REST APIs and Macros**
********************************

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

In this lab participants will learn how to access the REST API Explorer from NTNX Prism Central.  Navigate the API Explorer
and use the NuCalm REST API's to retrieve, update, delete and create NuCalm Blueprints and Applications...

Requirements:
*************

- REST/HTTP Overview:  REST-HTTP-Overview_
- NTNX REST API Explorer Overview:  NTNX-REST-API-Explorer-Overview_
- CentOS Server v7 VM created:  configure-centos-server-v7_ guide for docker.
- Python 2.7.x
- Git Hub Account: https://github.com



The Automation Lab starts with an introduction to NuCalm REST API, and associated JSON. The lab is a set of exercises designed to walk participants through navigating the REST API Explorer, locating Bluepint and Appication commands, executing the commands, and observing the results using both the Swagger generated API Explorer (Prism), and Postman (3^rd^ party API toolchain). Participants will then deploy several commands using python.

Create a Development Environment
********************************

We'll need to make sure the python 2.7 runtime has all appropriate packages, sepcifically *pip* and *requests*. 

- Pip is a tool for installing and managing Python packages.
- Requests is a Python package used to conmunicate over http.

**Add the EPEL Repository**

Pip is part of Extra Packages for Enterprise Linux (EPEL), which is a community repository of non-standard packages for the RHEL distribution. First, we’ll install the EPEL repository.

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


Accessing the REST API's
************************

The REST API explorer may not be accessible via Prism Central - specifically in the case of AOS v5.5.  The explorer can be launched by explicitly typing the *url* in the address bar as follows:

.. code-block:: bash

  https://[IP-ADDRESS]:9440/api/nutanix/v3/api_explorer/index.html
  


.. _configure-centos-server-v7: ../lab6/calm_workshop_lab6_config_centos.rst
.. _REST-HTTP-Overview: calm_workshop_lab5_rest_overview.rst
.. _NTNX-REST-API-Explorer-Overview: calm_workshop_ntnx_api_explorer_overview.rst
