**********************************
Nutanix REST API Explorer Overview
**********************************

.. contents::

Overview
********

The Nutanix REST API Explorer is written and formatted using an API framework called Swagger (governed by Apache license v2.0), used to describe and document RESTful APIs. The framework generates an interactive API document users can visualize and interact with, which Nutant’s commonly refer to as the Nutanix REST API Explorer.

The explorer is an expandable/collapsible, interactive hypertext document, organized by top-level resources (i.e. images, vms, hosts, storage containers, etc…), and their associated HTTP request operations (i.e**. GET, POST, DELETE, PUT**). When a resource operation is expanded, consumers are provided with descriptions, JSON Model definitions, and request/response panes for viewing messages and header and information.

Users navigate the explorer by scanning the document for a top-level resource (i.e. vms) they want to perform work on. Once a resource has been located, users can then expand the resource by performing a mouse-click on the resource name, exposing the supported HTTP request operations. Users can then interact with, and manipulate a resource instance using a given request operation from within the explorer view.

REST API Explorer - JSON Message-Body Declaration – POST /vms/
**************************************************************

The following illustration shows a message-body formatted in JSON used for creating a Virtual Machine (VM) using POST as defined using the Nutanix REST API Explorer.

|image0|

REST API Explorer - JSON Message Response Header – POST /vms/
*************************************************************

The following illustration shows a message-body formatted in JSON for creating a Virtual Machine (VM) using POST from within the Nutanix REST API Explorer.

|image1|


.. |image0| images:: ./media/image2.png

.. |image1| images:: ./media/image3.png
