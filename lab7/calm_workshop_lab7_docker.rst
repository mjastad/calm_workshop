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

Requirements:
*************

- CentOS Server v7 VM created:  centos-setup_ guide for docker.
- Docker-CE 17.x Installed: docker-installation_ guide for docker.
- NTNX Docker Plugin Installed: ntnx-plugin-setup_ guide for docker.

Glossary:
*********


- **Image:** An image is a lightweight, stand-alone, executable package that includes everything needed to run a piece of software, including the code, a runtime, libraries, environment variables, and config files.

- **Dockerfile:** Dockerfile will define what goes on in the environment inside your container.

- **Container:** A container is a runtime instance of an image—what the image becomes in memory when actually executed. It runs completely isolated from the host environment by default, only accessing host files and ports if configured to do so.

- **Service:** In a distributed application, different pieces of the app are called “services.” For example, if you imagine a video sharing site, it probably includes a service for storing application data in a database, a service for video transcoding in the background after a user uploads something, a service for the front-end, and so on.  Services are really just “containers in production.” A service only runs one image, but it codifies the way that image runs—what ports it should use, how many replicas of the container should run so the service has the capacity it needs, and so on. Scaling a service changes the number of container instances running that piece of software, assigning more computing resources to the service in the process.

Create Dockerfile:
******************

Create an empty directory. Change directories (cd) into the new directory, create a file called Dockerfile, copy-and-paste the following content into that file, and save it. Take note of the comments that explain each statement in your new Dockerfile.

.. code-block:: bash

  # Use an official Python runtime as a parent image
  FROM python:2.7-slim

  # Set the working directory to /app
  WORKDIR /app

  # Copy the current directory contents into the container at /app
  ADD . /app

  # Install any needed packages specified in requirements.txt
  RUN pip install --trusted-host pypi.python.org -r requirements.txt

  # Make port 80 available to the world outside this container
  EXPOSE 80
  
  # Define environment variable
  ENV NAME World

  # Run app.py when the container launches
  CMD ["python", "app.py"]
  

Proxy servers can block connections to your web app once it’s up and running. If you are behind a proxy server, add the following lines to your Dockerfile, using the ENV command to specify the host and port for your proxy servers:

.. code-block:: bash

  # Set proxy server, replace host:port with values for your servers
  ENV http_proxy host:port
  ENV https_proxy host:port

This Dockerfile refers to a couple of files we haven’t created yet, namely app.py and requirements.txt. Let’s create those next.

Create the Application:
***********************

Create two more files, **requirements.txt** and **app.py**, and put them in the same folder with the Dockerfile. This completes our app, which as you can see is quite simple. When the above Dockerfile is built into an image, app.py and requirements.txt will be present because of that Dockerfile’s ADD command, and the output from app.py will be accessible over HTTP thanks to the EXPOSE command.

**requirements.txt**

.. code-block:: bash

  Flask
  Redis

**app.py**

.. code-block:: python

  from flask import Flask
  from redis import Redis, RedisError
  import os
  import socket

  # Connect to Redis
  redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)

  app = Flask(__name__)

  @app.route("/")
  def hello():
      try:
          visits = redis.incr("counter")
      except RedisError:
          visits = "<i>cannot connect to Redis, counter disabled</i>"

      html = "<h3>Hello {name}!</h3>" \
             "<b>Hostname:</b> {hostname}<br/>" \
             "<b>Visits:</b> {visits}"
      return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)

  if __name__ == "__main__":
      app.run(host='0.0.0.0', port=80)

Now we see that *pip install -r requirements.txt* installs the Flask and Redis libraries for Python, and the app prints the environment variable NAME, as well as the output of a call to *socket.gethostname()*. Finally, because Redis isn’t running (as we’ve only installed the Python library, and not Redis itself), we should expect that the attempt to use it here will fail and produce the error message.

**Note:** Accessing the name of the host when inside a container retrieves the container ID, which is like the process ID for a running executable.

That’s it! You don’t need Python or anything in requirements.txt on your system, nor will building or running this image install them on your system. It doesn’t seem like you’ve really set up an environment with Python and Flask, but you have.

Build the application
*********************

We are ready to build the app. Make sure you are still at the top level of your new directory. Here’s what ls should show:

.. code-block:: bash

  $ ls
    Dockerfile		app.py			requirements.txt
  
Now run the build command. This creates a Docker image, which we’re going to tag using -t so it has a friendly name.

.. code-block:: bash

  $ docker build -t calmWorkshop .

Where is your built image? It’s in your machine’s local Docker image registry:

.. code-block:: bash

  $ docker images

    REPOSITORY            TAG                 IMAGE ID
    calmWorkshop          latest              326387cea398
    
Tip: You can use the commands docker images or the newer docker image ls list images. They give you the same output.

Run the app
***********

Run the app, mapping your machine’s port 4000 to the container’s published port 80 using -p:

.. code-block:: bash

  $ docker run -p 4000:80 calmWorkshop

You should see a message that Python is serving your app at http://0.0.0.0:80. But that message is coming from inside the container, which doesn’t know you mapped port 80 of that container to 4000, making the correct URL http://localhost:4000.

Go to that URL in a web browser to see the display content served up on a web page, including “Hello World” text, the container ID, and the Redis error message.

*Hello Calm* in browser
  


.. _docker-installation: calm_workshop_lab7_setup.rst
.. _centos-setup: calm_workshop_lab7_centos_config.rst
.. _ntnx-plugin-setup: calm_workshop_lab7_ntnx_vol_driver_install.rst
