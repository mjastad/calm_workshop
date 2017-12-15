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

- CentOS Server v7 VM created:  centos-setup_ guide for docker.
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
