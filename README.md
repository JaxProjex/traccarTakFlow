# traccarTakFlow
Node-Red Flow to forward Traccar server device locations to ATAK via multicast or TAKServer. Traccar Client CoT icons will change colors based on speed and status and can be configured to populate as a pointer to show course direction of movement (not supported by iTAK) or as a Spot Icon. (used in conjunction with Ampledatas node-red-contrib-tak). copy contents in "traccarTakFlow.json" and import to Node-Red.

![traccar flow](/screenshot1.png?raw=true "Node Red Flow")


REQUIREMENTS:

-Traccar Server

-TAKServer (or ATAK Devices for multicast)

-node-red-contrib-tak (docs/installation found here: https://github.com/snstac/node-red-contrib-tak)

=========================

CONFIGURATING THE NODES:

-Configure the "Ping" node to repeat in timed intervals for your needed usage, keep in mind the Traccar Client ping frequency thats configured for your traccar devices will need to be adjusted accordingly as well. 

-Configure both HTTP Req nodes and put in your Traccar Server IP or Domain, along with your Username/Password credentials for your Traccar Server.

-Configure whether you want CoT icons to populate as Generic Icon Pointers (Android preferred) or as Spots (iPhone preferred). Keep in mind, iTAK does not support Generic Icon pointers and will just populate pointers as a yellow icon (a-u-G).

-Configure your TAKServer connection in the TCP node, if using SSL add your clients client cert (.pem), client key (.key), and CA (.pem). If just utilizing multicast use UDP node. UDP will work if node red forwarding server is on same VPN or LAN as ATAK/iTAK client devices.

=========================

TESTING / TROUBLESHOOTING:

-To test if CoT messages are being sent and recieved by TAKServer, send the example JSON to the TAK node and see if it populates on your TAK client devices. 

-To test if your Traccar clients are getting forwarded to your TAKServer, utilize an android phone with Traccar Client App and Mock Location App. You can then create a route for the android phones GPS to follow in the Mock Location app to mimic an operational testing environment.


