********************************
**NuCalm Blueprint Marketplace**
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

**Calm Glossary**
*****************

**Service:** One tier of a multiple tier application. This can be made up of 1 more VMs (or existing machines) that all have the same config and do the same thing **Application (App):** A whole application with multiple parts that are all working towards the same thing (for example, a Web Application might be made up of an Apache Server, a MySQL database and a HAProxy Load balancer. Alone each service doesn’t do much, but as a whole they do what they’re supposed to) **Macro:** A Calm construct that is evaluated before being ran on the target machine. Macros and Variables are denoted in the *@@{[name]}@@* format in the scripts.

Lab Overview
************

In this lab participants will learn how to manage NuCalm Blueprints within the NuCalm Marketplace.  After this lab
participants should know how to navigate and manage the Marketplace, publish blueprints to the market, deploy and/or clone
blueprints from the marketplace. 

**Part 1: Accessing and Navigating Calm**
*****************************************

Getting Familiar with the Tools

1. Connect to https://[HPOC-IP-ADDRESS]:9440
2. Login to Prism using the credentials specified above (use these credentials unless specified otherwise throughout this lab
3. Click on the Apps tab across the top of Prism

Welcome to Calm! Upon accessing this page you will now notice a new ribbon along the left ­ this is used to navigate through Calm.

You are, by default, dropped into the Applications tab and can see all the instances of applications that have been launched from a blueprint.

**Tab review:**

|image0|

**Part 2: Marketplace Control - Publish Blueprints**
****************************************************

Navigate the to Marketplace control by clicking (|image1|) icon located on the left tool ribbon.  This will open the Marketplace Control Center where pre-configured and self-authored blueprints are staged for publishing to the local Marketplace used for teaming and collaboration.

In this exercise we'll walk through the steps to:

1. Publish the pre-configured MongoDB Blueprint to the local Marketplace
2. Clone the Blueprint from the Marketplace for editing.
3. Edit the blueprint and launch as an application.

Make sure *Marketplace Blueprints* is selected along the top of the Blueprint grid. Locate the **Mongo** blueprint within the grid and click the checkbox.

|image2|

Once the **Mongo** Blueprint has been selcted, a catalog is diaplyed to the right of the grid with a blueprint description, category, and project assignment. Insure **Database** and the **Calm** project are selected and click *apply*.

|image3|




.. |image0| image:: ./media/image2.png
   
.. |image1| image:: ./media/image4.png

.. |image2| image:: ./media/image5.png

.. |image3| image:: ./media/image8.png


 
