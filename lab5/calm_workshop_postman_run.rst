*************************************
**Postman Execute/Save REST Request**
*************************************

Copy the *Request URL* from the **POST** */apps/list* 

|image0|

Paste it to Postman, and select POST from the command drop down

|image1|

Configure *Authorization* by setting the Type to *Basic Auth* and using the *Prism Central* credentials:

|image2|

Configure the **Headers** Key(s) *Content-Type* and *Accept* with Value *application/json* as shown below:

|image3|

Configure the Body with *raw* and select JSON (application/json), and paste the payload information into the text area used earlier as shown below:

|image4|

Click **Send** within Postman.  If configured sucessfully, you should see **Status: 200 OK**, and valid response data in the **Body** as shown below: 

|image5|


Click Save and assign a **Request name:** *Nutanix Calm (NuCalm) List Applications* to persist the call to **Collection:** *Nutanix*, and **Subfolder:** *V3:Apps*.  

|image6|

.. |image0| image:: ./media/image10.png
.. |image1| image:: ./media/image12.png
.. |image2| image:: ./media/image13.png
.. |image3| image:: ./media/image14.png
.. |image4| image:: ./media/image15.png
.. |image5| image:: ./media/image16.png
.. |image7| image:: ./media/image17.png

