Network with Overlay Router and Overlay Network for PCs with the Same IP Address
This document describes a network with a Overlay router and an overlay network that allows PCs with the same IP address to connect to the control PC. This is achieved through the use of namespaces and virtual ethernet adapters that patch one namespace to another.

Network Diagram
The network diagram is shown below:

Image of network diagramOpens in a new window
smartdraw.com
network diagram
Network Devices
The network consists of the following devices:

.. image:: image/network-topology.jpeg


PCs: These are the computers that connect to the network.
Overlay Router: This is the router that routes traffic between the PCs and the internet.
Overlay Router: This is a software router that runs on the Overlay router and creates the overlay network.
Network Configuration
The network is configured as follows:

The PCs are configured with IP addresses in the 192.168.1.0/24 subnet.
The Overlay router is configured with the following interfaces:
eth0: This interface connects to the internet.
eth1: This interface connects to the PCs.
eth2: This interface is used for the overlay network.
The overlay router is configured to create an overlay network on top of the Overlay router's physical network.
The PCs that are connected to the overlay network are assigned IP addresses in a different subnet than the PCs that are connected to the physical network.
NameSpaces and Virtual Ethernet Adapters
Namespaces and virtual ethernet adapters are used to create the overlay network. Namespaces provide a way to isolate processes and resources on a Linux system. Virtual ethernet adapters provide a way to connect namespaces to each other.

In the network described in this document, namespaces are used to create isolated network environments for the PCs that are connected to the overlay network. Virtual ethernet adapters are used to connect the namespaces to the Overlay router's eth2 interface.

How it Works
When a PC that is connected to the overlay network sends traffic to the internet, the traffic is first sent to the Overlay router's eth2 interface. The Overlay router then forwards the traffic to the overlay router. The overlay router decapsulates the traffic and forwards it to the appropriate PC.

Traffic that is sent from the internet to a PC that is connected to the overlay network is routed in the same way. The traffic is sent to the Overlay router's eth0 interface, and then forwarded to the overlay router. The overlay router encapsulates the traffic and forwards it to the appropriate PC.

Benefits of Using an Overlay Network
There are several benefits to using an overlay network:

Isolation: Overlay networks provide a way to isolate PCs from each other. This can be helpful for security purposes, as it can prevent PCs from communicating with each other if they are not supposed to.
Flexibility: Overlay networks can be easily expanded or contracted. This is because they are not tied to the physical network infrastructure.
Scalability: Overlay networks can be scaled to accommodate a large number of PCs.
Conclusion
Overlay networks are a powerful tool that can be used to create flexible and scalable networks. The network described in this document is a simple example of how overlay networks can be used. With a little creativity, overlay networks can be used to create a wide variety of network solutions.

I hope this documentation is helpful!