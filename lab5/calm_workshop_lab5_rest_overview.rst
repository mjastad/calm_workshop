*****************
REST API Overview
*****************

The REST (Web Services) APIs (i.e. WS-API) allow users to author scripts or integrate with 3^rd^ platform solutions, designed to automate the execution of system administration commands against a Nutanix endpoint or resource. The API is designed to expose backend data of a Nutanix cluster that can be read or written to using Hypertext Transfer Protocol (HTTP).

Hypertext Transfer Protocol (HTTP)

The Hypertext Transfer Protocol (HTTP) is an application-level protocol for distributed, collaborative, hypermedia information systems, and is the foundation for data communication for the World Wide Web. HTTP is a generic and stateless protocol which can be used for other purposes (i.e. Web Services, or RESTful API’s), including extensions of built-in request methods, error codes, and headers.

HTTP is based on the client-server architecture model and a stateless request/response protocol that operates by exchanging messages across a reliable TCP/IP connection.

An HTTP "client" is a program (Web browser, thick-client, script, etc.) that establishes a connection to a server for the purpose of sending one or more HTTP request messages. An HTTP "server" is a program (i.e. Apache web-server running on the CVM) that accepts connections in order to serve HTTP requests by sending HTTP response messages.

Client: sends a request-line to the server in the form of a request-method, URI, and protocol-version, followed by a MIME-like message containing request modifiers, client information, and possibly entity-body content (i.e. JSON) over a TCP/IP connection.

Server: responds with a status-line, including the message's protocol-version and a status-code (success or error code), followed by a MIME-like message containing server information, entity meta information, and possibly entity-body content (i.e. JSON).

HTTP Uniform Resource Identifier (URI)

HTTP makes use of the Uniform Resource Identifier (URI) to identify a given resource and establish a connection. Once the connection is established, HTTP messages are passed between client and server. These messages include requests from client to server and responses from server to client: HTTP-message = <Request> | <Response> ; [^1]HTTP/1.1 messages

Uniform Resource Identifiers (URI) are formatted, case-insensitive strings containing name, location, etc., to identify a resource, for example, a website, or a web service (i.e. abs_path = /PrismGateway/Services/rest/v2.0/vms/). A general syntax of URI used for HTTP is as follows:

[^2]URI = “http:” “//” host [“:” port] [abs_path [“?” query]]

HTTP Request/Response Message

HTTP request(s) and response(s) use a generic message format for transferring required data. This generic message format consists of the following:

start-line [request-line | status-line]:

The request-line is initiated by the client and begins with a request-method specifying the action to be taken, followed by a request-URI identifying the resource the action is to be performed on, and ending with the protocol-version to determine the handshaking. The following are list of common [^3]request-methods found in most REST API implementations.

GET: Retrieves information from a specified server for a given URI (resource).

POST: Sends data to a specified server to create a resource for a given URI (resource).

PUT: Update data for a specified resource for a given URI (resource).

DELETE: Remove a resource instance for a given URI (resource).

e.g. request-line: GET /vms/ HTTP/1.1

A status-line is initiated by the server and consists of the protocol-version followed by a numeric status-code and associated textual phrase. The status-code element is a 3-digit integer where the first digit defines the class of response, and last 2 digits define detail.

1XX: [Informational] Request was received and in-process.

2XX: [Success] Request successfully received understood, and accepted.

3XX: [Redirection] further action is required to complete request

4XX: [Client Error] request contains incorrect syntax or can’t be fulfilled.

5XX: [Server Error] server failed to fulfill the request.

e.g. status-line: HTTP/1.1 200 OK

header [field-name “:” [field-value]]:

The header provides information that qualifies the request or response messages, to include the message-body (assuming it exists). Header field-names and field-values vary depending on if the message is a request or response, and if there’s a message-body attached:

message-body (optional):

The message carries the entity-body associated with the request or response. If entity-body is present, **content-type **and content-length header lines specify the nature of the body (in the context of a REST API’s content-type: application/json, content-length: size of JSON in bytes).


JSON Message-Body

Both the request and response message payloads in REST API implementations use a lightweight, self-describing data-interchange referred to as JavaScript Object Notation (JSON) used to structure data (as text) composed of objects containing <key,value> pairs, or arrays of objects. Since JSON format is text, it can easily be sent to and from a server, and consumed by any programming language.

JSON syntax is derived from JavaScript Object Notation syntax where:

Data is organized using key,value pairs, and separated by commas (“,”).

Curly braces (“{“, “}”) hold objects containing key,value pairs.

Square brackets ([ ]) hold arrays of objects.

A key/value pair consists of a field name (in double quotes), followed by a colon, followed by a value: e.g. "model":"nx3160". In JSON, values must be one of the following data types:

String (values are in double quote “ ” notation)

Number (must be integer or floating point)

Object (surrounded by curly bracket { } notation. Can have embedded objects.)

Array (square bracket [ ] notation, indexed by integers starting at 0)

Boolean (literal: true or false)

Null (written as null)

The following JSON example defines a Nutanix “disks” array, containing 3 “disk” objects:

{“disks":[
    {“disk”:{ "vendor":"seagate", "type":"hdd", "capacity":8000000000000,“encrypted”: false}, “device_bus”=”scsi”},
    {“disk”:{ "vendor":"seagate", "type":"hdd", "capacity":8000000000000,“encrypted”: false}, “device_bus”=”scsi”},
    {“disk”:{ "vendor":"toshiba", "type":"ssd",  "capacity":1200000000000,“encrypted”: false}, “device_bus”=”scsi”}
]}


Nutanix REST API Explorer Overview
**********************************

The Nutanix REST API Explorer is written and formatted using an API framework called Swagger (governed by Apache license v2.0), used to describe and document RESTful APIs. The framework generates an interactive API document users can visualize and interact with, which Nutant’s commonly refer to as the Nutanix REST API Explorer.

The explorer is an expandable/collapsible, interactive hypertext document, organized by top-level resources (i.e. images, vms, hosts, storage containers, etc…), and their associated HTTP request operations (i.e**. GET, POST, DELETE, PUT**). When a resource operation is expanded, consumers are provided with descriptions, JSON Model definitions, and request/response panes for viewing messages and header and information.

Users navigate the explorer by scanning the document for a top-level resource (i.e. vms) they want to perform work on. Once a resource has been located, users can then expand the resource by performing a mouse-click on the resource name, exposing the supported HTTP request operations. Users can then interact with, and manipulate a resource instance using a given request operation from within the explorer view.

REST API Explorer - JSON Message-Body Declaration – POST /vms/

The following illustration shows a message-body formatted in JSON used for creating a Virtual Machine (VM) using POST as defined using the Nutanix REST API Explorer.
