# traccarTakFlow
Node-Red Flow to forward Traccar Client device locations to ATAK via multicast or TAKServer. CoT icons will change colors based on speed and status and can be configured to populate as a pointer to show course direction of movement (not supported by iTAK) or as a Spot Icon. (used in conjunction with Ampledatas node-red-contrib-tak).

![traccar flow](/traccarTakFlow.png?raw=true "Node Red Flow")


REQUIREMENTS:

-Node-Red

-node-red-contrib-tak https://github.com/snstac/node-red-contrib-tak

-node-red-dashboard

-TAKServer (or Multicast supported devices)

-Traccar Server

-----------------------------

SETUP NODE-RED:

-https://nodered.org/docs/getting-started/

-options include installing node-red on your local PC, a Raspberry Pi or similar single board computer or mini PC, deploying node-red on a cloud server or virtual private server (VPS), Docker Container, or local virtual environment. After installing node-red you should be able to go to the node-red dashboard at http://*nodeRedIPaddress:1880 you may have to open appropriate ports (1880) to allow devices to access the node-red dashboard.

-ensure you install the "node-red-contrib-tak" node. If not there should be a prompt when you import traccarTakFlow in Node-Red that there are nodes that need to be installed and will automatically install them for you if you allow. In the event that isnt the case, in Node-Red: Menu (3 horizontal lines) > Manage palette > Install > Search "node-red-contrib-tak" > Install > Install

-follow same directions above for installing "node-red-dashboard"

-----------------------------

IMPORT .JSON FLOW TO NODE RED:

-in GitHub: click on "traccarTakFlow.json" > click on the download icon "Download raw file" > note where the "traccarTakFlow.json" file downloaded to, default is in your Downloads folder

-in Node-Red: click on menu icon (3 horizontal lines top right) > click on "Import" > click on "select a file to import" > go to Downloads folder and click on "traccarTakFlow.json" > Upload > Import

ALTERNATIVELY..

-you can just copy the whole "traccarTakFlow.json" code from GitHub and paste it into the Node-Red Import Clipboard.

-------------------------------

CONFIGURE TAKSERVER:

-TAKServer can be hosted on various platforms: Raspberry Pi, Mini PCs, your local PC, Cloud Servers or Virtual Private Servers (VPS), Docker Containers, or Virtual Environments. Regardless, if you're looking for instructions on setting up your TAKServer I recommend ATAKHQs guides https://www.atakhq.com/en/tak-server

-Configuring the TCP (TAKServer) node in Node-Red traccarTakFlow, Input your TAKServer IP and Port, and checkbox whether you're using SSL/TLS or not. If you are using SSL/TLS, ensure you upload you TAKServer certificates and key that are located in the directory of your TAKServer that stores all client certificates/keys (ie: ubuntu docker container is "/opt/tak/certs/files"). I Suggest creating a TAK client "node-red" to be used specifically for Node-Reds handling of forwarding data to TAK. In your certificates file you will want to copy and upload "node-red.pem" as the "Certificate", "node-red.key" as the "Private Key" and "ca.pem" as the "CA Certificate" in the TLS Configuration Properties in the TCP (TAKServer) node in Node-Red. You will also need to enter the Passphrase for your TAKServer truststore, this can be found in your TAKServers CoreConfig.xml file. Default is "atakatak".

-------------------------------

SETUP TRACCAR SERVER:

-https://www.traccar.org/download/

-Traccar Server can be hosted on various platforms: Raspberry Pi, Mini PCs, your local PC, Cloud Servers or Virtual Private Servers (VPS), Docker Containers, or Virtual Environments.

----------------------------------

CONFIGURE PING NODE:

-in Node-Red: configure ping node "repeat" > "interval" and set the frequency of how often the traccar server device locations are requested and then forwarded to TAK.

-----------------------------------

*OPTIONAL* CONFIGURE PING FREQUENCY NODE:

-in Node-Red: configure ping frequency node and adjust the "resend" time to your needed usage. This creates a UI switch that can be manipulated in http://noderedipaddress:1880/ui

-----------------------------------

CONFIGURE BOTH HTTP NODES:

-in Node-Red: input your Traccar Server IP address and port (default port is 8082)

-----------------------------------


USING THE TRACCARTAKFLOW:

-ensure you have your Node-Red server, Traccar Server, and TAKServer running.

-configure either the ping node or the ping frequency node. Configure both http req nodes with your traccar server credentials. Choose whether you want the flow to use spots or pointers to populate on TAK. Configure TAKServer info and credentials into the TCP node (unless using multicast).

-everytime a GET request is made from the ping nodes it will update TAK with the positions of all Traccar Server devices that are associated with the account that was used in the http req nodes.


------------------------------------

HOW THE TRACCARTAKFLOW WORKS:

-ping nodes trigger for both http GET requests to be sent to traccar to get device names and locations. Sort node sorts the devices by position ID number to more easily pair the proper devices names with the proper locations. The delay node helps to better associate the array by consistently making it in a specified index. Join node joins both the device and position arrays into one msg. The sort node sorts so every device name and position array is joined and sent as a separate msg. The json node grabs and inputs the data of the traccar device name and position into a json formatted CoT msg. The TAK node converts the json into a CoT XML formatted string to send to TAK through the TCP or UDP node.

--------------------------------------

TROUBLESHOOTING / KNOWN ISSUES:

-in the event the Node-Red service explodes and you lose connection to the Node-Red UI and cant reconnect, you may need to restart your Node-Red service

$ sudo service node-red restart


