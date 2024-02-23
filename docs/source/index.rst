Routing Overlay Network for PCs with the Same IP Addresses - Overlay Router
===========================================================================

This document describes a network with a Overlay router and an overlay network that allows PCs with the same IP address to connect to the control PC. This is achieved through the use of namespaces and virtual ethernet adapters that patch one namespace to another.

Network Diagram
===============
The network diagram is shown below:

.. image:: images/network-topology.jpeg
  :alt: Network Diagram
The network consists of the following devices:

* PCs: These are the computers that connect to the network.
* Overlay Router: This is a software router that runs on the Overlay router and creates the overlay network. 
  
Network Configuration
=====================
The network is configured as follows:

* The PCs are configured with IP addresses in the 192.168.1.0/24 subnet.
* The Overlay router is configured with the following interfaces:
   * eth0: This interface connects to the Management PC0.
   * eth1: This interface connects to the Controller PC1.
   * eth2: This interface connects to a switch that connects to group 1 of PC's with IP address in 192.168.1.0/24 network space.
   * eth3: This interface connects to a switch that connects to group 2 of PC's with IP address in 192.168.1.0/24 network space.

* The overlay router is configured with namespaces and vitual ethernet to connect the namespaces to create an overlay network on top of the Alpine router's physical network.


NameSpaces and Virtual Ethernet Adapters
========================================
Namespaces and virtual ethernet adapters are used to create the overlay network. Namespaces provide a way to isolate processes and resources on a Linux system. Virtual ethernet adapters provide a way to connect namespaces to each other.

In the network described in this document, namespaces are used to create isolated network environments for the PCs that are connected to the overlay network. Virtual ethernet adapters are used to connect the namespaces to the VyOS router's eth2 interface.