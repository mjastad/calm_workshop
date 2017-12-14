**************
**Docker Lab**
**************

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

In this lab participants will learn how to install and configure Docker.  Once Docker is installed and stable, participants
will learn to create and deploy a container...  Once completed, participants will containerize and deploy a 3 Tier LAMP
application..

Pre-requisites
**************

- CentOS Server v7 VM created:  centos-setup_ guide for docker.
- Docker 17.x Installed: docker-installation_ guide for docker.

Glossary
********


- **Image:** An image is a lightweight, stand-alone, executable package that includes everything needed to run a piece of software, including the code, a runtime, libraries, environment variables, and config files.

- **Dockerfile:** Dockerfile will define what goes on in the environment inside your container.

- **Container:** A container is a runtime instance of an image—what the image becomes in memory when actually executed. It runs completely isolated from the host environment by default, only accessing host files and ports if configured to do so.

- **Service:** In a distributed application, different pieces of the app are called “services.” For example, if you imagine a video sharing site, it probably includes a service for storing application data in a database, a service for video transcoding in the background after a user uploads something, a service for the front-end, and so on.  Services are really just “containers in production.” A service only runs one image, but it codifies the way that image runs—what ports it should use, how many replicas of the container should run so the service has the capacity it needs, and so on. Scaling a service changes the number of container instances running that piece of software, assigning more computing resources to the service in the process.


.. _docker-installation: calm_workshop_lab7_setup.rst
.. _centos-setup: calm_workshop_lab7_centos_config.rst
