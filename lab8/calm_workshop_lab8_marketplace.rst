*************************************
**NuCalm Blueprint Marketplace Lab2**
*************************************

.. contents::


Connectivity Instructions:
**************************

+------------+--------------------------------------------------------+
| IP         |                                           Cluster IP   |
+------------+--------------------------------------------------------+
| Username   |                                           Cluster User |
+------------+--------------------------------------------------------+
| Password   |                                           Cluster Pass | 
+------------+--------------------------------------------------------+

Calm Glossary
*************

**Service:** One tier of a multiple tier application. This can be made up of 1 more VMs (or existing machines) that all have the same config and do the same thing **Application (App):** A whole application with multiple parts that are all working towards the same thing (for example, a Web Application might be made up of an Apache Server, a MySQL database and a HAProxy Load balancer. Alone each service doesn’t do much, but as a whole they do what they’re supposed to) **Macro:** A Calm construct that is evaluated before being ran on the target machine. Macros and Variables are denoted in the *@@{[name]}@@* format in the scripts.

Lab Overview
************

In this lab participants will learn how to manage Blueprints within the NuCalm Marketplace.  After this lab
participants should know how to navigate Marketplace Management and configure the Project Environment to deploy Blueprints
from the Marketplace.

In this exercise we'll walk through the steps to:

1. Publish a Blueprint from the Blueprint Workspace to the local Marketplace.
2. Use the Marketplace Manager to approve, assign roles and projects, and publish to the Marketplace.
3. Edit the Project Environment so the blueprint can be launched from the Marketplace as an application.

**Note:** This lab assumes pariticipants have Blueprints built and staged from previous exercises. 

Part 1: Accessing and Navigating Calm
*************************************

Getting Familiar with the Tools

1. Connect to https://[HPOC-IP-ADDRESS]:9440
2. Login to Prism using the credentials specified above (use these credentials unless specified otherwise throughout this lab
3. Click on the Apps tab across the top of Prism

Welcome to Calm! Upon accessing this page you will notice a new ribbon along the left used to navigate through Calm constructs.

Users are dropped into the Applications tab by default, and can see all the application instances that have been launched from a blueprint.

**Tab review:**

|image0|

Part 2: Blueprint Workspace - Publish Blueprints
************************************************

Navigate to the *Blueprint Workspace* by clicking (|image1|) icon located on the left tool ribbon.  This will open the Blueprint Workspace where self-authored blueprints are staged for editing, publishing, or launching as an application.

|image2|

Select an *Active* working Blueprint by clicking on the *Name* and opening the workspace.  With the workspace open, Click the |image3| action located at the top of the Blueprint workspace tool bar. 

|image4|

A modal dialog will appear.  Verify the *Name* and *Description*, and click the Publish button. 

|image5|


Part 3: Marketplace Manager - Approve Blueprint
***********************************************

**Note:** You must be logged in as Admin or have an Admin role to access the *Marketplace Manager*

Navigate to the Marketplace Manager by clicking (|image6|) icon located on the left tool ribbon.  This will open the Marketplace Manager where Blueprints are staged for MArketplace publication.  As you scroll the Blueprints, you will not see the Blueprint published from the *Blueprint Workspace*.  This is becuase the Blueprnt requires approval.

To approve the Blueprint, click the *Approval Pending* action located along the top tool-bar of the *Marketplace Manager* and then click the checkbox to the left of the *Blueprint Name*. You can choose to reject, approve, or launch the blueprint.

- Reject: Changes the state fo the blueprint publication and stages it in *Approval Pending*
- Approve: Approves the blueprint for publication.
- Launch: Launches the Blueprint as an application - the same as *Blueprint Workspace*

Click *Approve* to approve the Bluerpint for publication (it may require approval 2x).  Once approved 

|image7|



|image7|

Part 4: Edit + Debug + Launch Cloned Blueprint
**********************************************

Navigate the Blueprint workspace by clicking the (|image8|) icon located on the left tool ribbon.  This will open the Blueprint Workspace. 

|image9|

Click the red exclamation point to see a list fo error desriptions.  

|image10|

Fix each of the errors listed within the Blueprint.  Once all the errors have been corrected, make additional changes to each of the **Mongo** services (i.e. VM, Package, etc...) and launch the blueprint.  Continue to make chnages until the the Blueprint successfully deploys.  

|image11|



.. |image0| image:: ./media/image2.png

.. |image1| image:: ./media/image14.png
   
.. |image2| image:: ./media/image17.png

.. |image3| image:: ./media/image16.png

.. |image4| image:: ./media/image15.png

.. |image5| image:: ./media/image18.png

.. |image6| image:: ./media/image10.png

.. |image20| image:: ./media/image11.png

.. |image7| image:: ./media/image13.png

.. |image8| image:: ./media/image14.png

.. |image9| image:: ./media/image15.png

.. |image10| image:: ./media/image16.png

.. |image11| image:: ./media/image17.png

 
