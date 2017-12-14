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
      return html.format(name=os.getenv("NAME", "nucalm"), hostname=socket.gethostname(), visits=visits)

  if __name__ == "__main__":
      app.run(host='0.0.0.0', port=80)

Now we see that *pip install -r requirements.txt* installs the Flask and Redis libraries for Python, and the app prints the environment variable NAME, as well as the output of a call to *socket.gethostname()*. Finally, because Redis isn’t running (as we’ve only installed the Python library, and not Redis itself), we should expect that the attempt to use it here will fail and produce the error message.

**Note:** Accessing the name of the host when inside a container retrieves the container ID, which is like the process ID for a running executable.

That’s it! You don’t need Python or anything in requirements.txt on your system, nor will building or running this image install them on your system. It doesn’t seem like you’ve really set up an environment with Python and Flask, but you have.

Build the Application
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

Run the Application
*******************

Run the app, mapping your machine’s port 4000 to the container’s published port 80 using -p:

.. code-block:: bash

  $ docker run -p 4000:80 calmWorkshop

You should see a message that Python is serving your app at http://0.0.0.0:80. But that message is coming from inside the container, which doesn’t know you mapped port 80 of that container to 4000, making the correct URL http://localhost:4000.

Go to that URL in a web browser to see the display content served up on a web page, including “Hello World” text, the container ID, and the Redis error message.

*You can also use the curl command in a shell to view the same content.

.. code-block:: bash

  $ curl http://localhost:4000

    <h3>Hello nucalm!</h3><b>Hostname:</b> 8fc990912a14<br/><b>Visits:</b> <i>cannot connect to Redis, counter disabled</i>

This port remapping of 4000:80 is to demonstrate the difference between what you EXPOSE within the Dockerfile, and what you publish using docker run -p. In later steps, we’ll just map port 80 on the host to port 80 in the container and use http://localhost.

Hit CTRL+C in your terminal to quit.
  
Now let’s run the app in the background, in detached mode:

.. code-block:: bash

  $ docker run -d -p 4000:80 calmWorkshop
  
You get the long container ID for your app and then are kicked back to your terminal. Your container is running in the background. You can also see the abbreviated container ID with docker container ls (and both work interchangeably when running commands):

.. code-block:: bash

  $ docker container ls
    CONTAINER ID        IMAGE               COMMAND             CREATED
    1fa4ab2cf395        friendlyhello       "python app.py"     28 seconds ago

You’ll see that CONTAINER ID matches what’s on http://localhost:4000.

Now use docker container stop to end the process, using the CONTAINER ID, like so:

.. code-block:: bash

  $ docker container stop 1fa4ab2cf395

Image sharing
*************

To demonstrate the portability of what we just created, let’s upload our built image and run it somewhere else. After all, you’ll need to learn how to push to registries when you want to deploy containers to production.

A registry is a collection of repositories, and a repository is a collection of images—sort of like a GitHub repository, except the code is already built. An account on a registry can create many repositories. The docker CLI uses Docker’s public registry by default.

**Note:** We’ll be using Docker’s public registry here just because it’s free and pre-configured, but there are many public ones to choose from, and you can even set up your own private registry using Docker Trusted Registry.


**Log in with your Docker ID**

If you don’t have a Docker account, sign up for one at cloud.docker.com. Make note of your username.

Log in to the Docker public registry on your local machine.

.. code-block:: bash

  $ docker login

**Tag the image**

The notation for associating a local image with a repository on a registry is username/repository:tag. The tag is optional, but recommended, since it is the mechanism that registries use to give Docker images a version. Give the repository and tag meaningful names for the context, such as get-started:part2. This will put the image in the get-started repository and tag it as part2.

Now, put it all together to tag the image. Run docker tag image with your username, repository, and tag names so that the image will upload to your desired destination. The syntax of the command is:

.. code-block:: bash

  docker tag image username/repository:tag

For example:

.. code-block:: bash

  $ docker tag calmWorkshop dogfish/get-started:part2

Run docker images to see your newly tagged image. (You can also use docker image ls.)

.. code-block:: bash

  $ docker images
    REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
    almWorkshop              latest              d9e555c53008        3 minutes ago       195MB
    dogfish/get-started      part2               d9e555c53008        3 minutes ago       195MB
    python                   2.7-slim            1c7128a655f6        5 days ago          183MB
    ...
    
Publish the image
*****************

Upload your tagged image to the repository:

.. code-block:: bash

  $ docker push username/repository:tag

Once complete, the results of this upload are publicly available. If you log in to Docker Hub, you will see the new image there, with its pull command.

Pull and run the image from the remote repository
From now on, you can use docker run and run your app on any machine with this command:

.. code-block:: bash

  $ docker run -p 4000:80 username/repository:tag

If the image isn’t available locally on the machine, Docker will pull it from the repository.

.. code-block:: bash

  $ docker run -p 4000:80 dogfish/get-started:part2
    Unable to find image 'dogfish/get-started:part2' locally
    part2: Pulling from dogfish/get-started
    10a267c67f42: Already exists
    f68a39a6a5e4: Already exists
    9beaffc0cf19: Already exists
    3c1fe835fb6b: Already exists
    4c9f1fa8fcb8: Already exists
    ee7d8f576a14: Already exists
    fbccdcced46e: Already exists
    Digest: sha256:0601c866aab2adcc6498200efd0f754037e909e5fd42069adeff72d1e2439068
    Status: Downloaded newer image for john/get-started:part2
    
Running on http://0.0.0.0:80/ (Press CTRL+C to quit)

**Note:** If you don’t specify the :tag portion of these commands, the tag of :latest will be assumed, both when you build and when you run images. Docker will use the last version of the image that ran without a tag specified (not necessarily the most recent image).

No matter where docker run executes, it pulls your image, along with Python and all the dependencies from requirements.txt, and runs your code. It all travels together in a neat little package, and the host machine doesn’t have to install anything but Docker to run it.


.. _docker-installation: calm_workshop_lab7_setup.rst
.. _centos-setup: calm_workshop_lab7_centos_config.rst
.. _ntnx-plugin-setup: calm_workshop_lab7_ntnx_vol_driver_install.rst
