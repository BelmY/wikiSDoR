I'm writing this to help out future novice users trying to set up OpenWebRX on a Raspberry Pi 2. 

If you run your receiver from your home internet connection, it is likely that your machine is behind the NAT and firewall implemented in your router. It is a box like this:

![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/21/Adsl_connections.jpg/1280px-Adsl_connections.jpg)

*(Image under CC-BY-SA 3.0 by [Wikipedia user Asim18](https://commons.wikimedia.org/wiki/User:Asim18).)*

By default, incoming TCP connections from the Internet are dropped by SOHO routers, so others outside on the Internet will not be able to connect to your receiver.

OpenWebRX runs on TCP port 8073 by default, you will have to set up **port forwarding** at the admin page of the router, to forward the port 8073 TCP connection requests coming from the Internet to the computer running OpenWebRX.

There are good guides on how to do that:
  * [portforward.com](http://portforward.com) has step-by-step guides for many routers.
  * There is a general explanation on [howtogeek.com](http://www.howtogeek.com/66214/how-to-forward-ports-on-your-router/).
