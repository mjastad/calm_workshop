*******************************************************
**Orchestration/Chammge Management Tools - Comparison**
*******************************************************

.. contents::

**Introduction**
****************

Development, deployment and maintenance of virtual servers requires a sophisticated set of tools. Updating your application is 
only one part of the process - next is managing the deployment of that application across servers. This is known as cloud 
orchestration or deployment automation. The terminology is fluid but the concept is the same - your application and all of 
the infrastructure behind it updated on every VM it lives on. This mission of cloud orchestration typically falls under 
DevOps.

Currently the leading tools in the Configuration Management marketplace are Puppet, Chef, and Ansible. While there’s no 
de-facto leader in the marketplace, each tool has its host of dedicated users. Chef’s celebrity tech companies are Facebook, 
Nordstrom, & Target, while Puppet is used by Walmart, 1-800 Flowers, & Wells Fargo. Ansible, released in 2013, is the newest 
of the three and is used by Capital One, Splunk, and NEC.

Selecting the best cloud configuration management system is a daunting task, regardless of whether you run servers on a 
single platform like Azure or across multiple vendors.


**Installation & Ease of Use**
******************************

**Puppet**

With Puppet, you set up a master server and install Puppet agents on each of your nodes (individual VMs). To install on 
individual VMs, you SSH into each one and run a script. On initial setup of that master server, you have the option to 
install the Puppet console and the master server on the same machine. Otherwise, you can set up a Puppet console on your 
development machine, and keep the master server in the cloud. Puppet does have a steep learning curve, though the Puppet 
Forge Community offers great administrative templates, modules, and discussions.

**Chef**

To set up Chef, you’ll use knife, Chef’s command-line tool that provides an interface between a workstation on your 
development machines and your Chef servers. You create cookbooks (instructions for automation), define environments, set 
roles, and more that are all pushed to a central Chef Server. That main Chef Server contains information on every node in 
your system, and Chef clients runs independently on each of these nodes. If you want to add more nodes, you can do so via 
knife bootstrap, passing in an IP address and password.

**Ansible**

Ansible is designed to be light and fast, so there’s no installation on each node. Instead, nodes are 
added via a config file on your master server, with SSH authorized keys added to each node. Ansible offers a variety of 
consulting and training services.

**User Interface**
******************

Open Source Puppet only has a CLI, while Puppet Enterprise has the CLI and a web UI. The bread and butter of Puppet are 
modules which contain the code that configures and manages your nodes. Installing modules is easy via the command line, but 
for anything more involved like creating users for access control and creating node groups, the Puppet Console is necessary.


**Chef vs Puppet**

Chef has a web UI, Chef Manage, but you’ll be doing most of your work via the command line with Knife. Chef is built with 
Ruby, so if you’re familiar with Rails, the syntax is straightforward. When you want to add new libraries to your cookbooks 
(Chef’s equivalent of Puppet’s modules), you add it as a dependency - just like adding gems.

**Ansible**

There’s Ansible Tower, Ansible’s enterprise edition and it’s web UI. It’s easier to configure and manage than Chef or 
Puppet’s web interfaces. Tower also makes use performance analytics, along with compliance and security functions from Red 
Hat.

**Code Base**
*************

For these cloud orchestration platforms, we judged code bases on the breadth of modules, preconfigured system configurations, 
and community created tools. Essentially, how much code is out there that my team and I can use to get this into our 
infrastructure?

**Chef**

Chef has the Chef Supermarket which contains over three thousand cookbooks contributed by over seventy-thousand chefs. 
Branding words aside, this means that there’s a lot of available modules you can install on your nodes to simplify system 
configurations. There’s the standard ones you’d expect: nginx, mysql, and docker. But there’s also cookbooks for 1password, 
redis, and even homebrew. Even though there’s only three thousand modules, the community is strong and modules receive 
consistent updates. Developers with Ruby experience tend to adopt Chef or Puppet so if that’s your language, easing into 
using pre-configured modules from their open source communities makes the decision easy.

**Puppet**

Puppet has the Puppet Forge, which is expansive, hovering around five thousand modules. Here, modules are separated by Puppet 
Supported (built by Puppet) and Puppet Approved, the top rated modules created by the community. Puppet recently put out a 
module supporting Azure servers, so if you’re in Microsoft land, there’s a place at the table for you. Like Chef, installation
is straightforward via your terminal. New modules are added to your Puppetfile.

**Ansible**

The Ansible Galaxy community is a helpful resource for tools and templates, and has more than three times as many 
contributors than the other tools in this comparison, and uses Ansible uses YAML Playbooks instead of recipes. Here, 
modules are called Ansible Roles. While we don’t have the exact number of roles available, there’s over a thousand roles 
just for mySQL, so you’ll find your flavor of SQL no matter what you’re looking for. There’s even a module for installing 
PHPmyAdmin. Language-wise, Ansible was built on Python. One thing I do like about the Galaxy interface is that it’s easy to 
filter by multiple categories and module versions.


**Scalability**
***************

This is what matters in the end - when you’re scaling systems to thousands and tens of thousands of nodes, you want the 
ability to keep every VM under the fold.

**Puppet**

Similar to Ansible, it’s fairly easy to add and remove server nodes to Puppet. The Puppet Master server component can 
quickly pick up on new server Puppet Agents to distribute commands for updates and configuration. Most services on Puppet 
run over HTTP like web applications, so it’s easy to create a load balancer with high availability/performance and you won’t 
see a drop in efficiency.

**Chef**

Chef Nodes are bootstrapped by the Chef Workstation and managed by Chef agents. Adding new nodes is done through your 
workstation machine, which adds them to a master list on the Chef master Server. Each of these nodes has a ‘run-list’, 
which is basically everything it needs to get up to the desired state, so setup is automated after that initial point. 
Chef gets updates from each server node every 30 minutes, and logs the status of the server for compliance requirements.

**Ansible**

Ansible has powerful orchestration capabilities. As mentioned before - there’s no serious installation on each node. 
As long as you have SSH authorized keys for each node, you can add as many as you’d like directly from the config file on 
the master server.

**Summary**
***********

So in the end, which platform is best? Well, it depends on your needs. 
Personally, I like Ansible because I came from a Python development background, and AWS has created OpsWorks for Ansible, 
which makes it easier to integrate into your system if you’re using AWS exclusively.

The clear factor here is that all three (Chef, Puppet, and Ansible) of those cloud configuration management tools all have 
expansive communities and modules, so there’s no worry on a lack of resources.

Many companies run multiple cloud management solutions, and just as many run multiple public, private and/or hybrid cloud 
environments. It’s important to evaluate multiple open source solutions before investing in an enterprise license. While all 
three limit you to around ten nodes on the open source versions, it’s a great way to test a cloud management solution being 
implemented in one of your divisions.


