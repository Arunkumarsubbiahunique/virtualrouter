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
   * eth0(192.168.2.35/22): This interface connects to the Management PC0 - 192.168.2.222/22.
   * eth1(10.0.0.1/24): This interface connects to the Controller PC1 - 10.0.0.1/24.
   * eth2(192.168.1.1/24): This interface connects to a switch that connects to group 1 of PC's with IP address in 192.168.1.0/24 network space.
   * eth3(192.168.1.1/24): This interface connects to a switch that connects to group 2 of PC's with IP address in 192.168.1.0/24 network space.

* The overlay router is configured with namespaces and vitual ethernet to connect the namespaces to create an overlay network on top of the Alpine router's physical network.


NameSpaces and Virtual Ethernet Adapters
========================================
Namespaces and virtual ethernet adapters are used to create the overlay network. Namespaces provide a way to isolate processes and resources on a Linux system. Virtual ethernet adapters provide a way to connect namespaces to each other.

Lab Documentation: Virtual Router with Overlay Networks and DNAT
================================================================
This document describes a network lab setup with three PCs (PC1, PC2, PC3), a virtual router (alpinelinux), and virtual networks using namespaces and veth interfaces. 
The lab demonstrates routing, connectivity, and DNAT functionalities.

Hardware and Software:

* VirtualBox
* Alpine Linux x86_64 virtual images
* Host-only network adapters
* Bridge adpater

Lab Setup:

1. Download the virtual box for flavour of the operating system
   https://www.virtualbox.org/wiki/Downloads

2. Install the virtualbox in accordance to your operating system by following the installation guide
   https://www.wikihow.com/Install-VirtualBox#:~:text=1%20Open%20Terminal.%20%20...%20Terminal%20from%20the,minutes.%20When%20you%20see%20your%20computer...%20More%20   

3. Download the alpine_linux_x86_64 virtual image 


.. image:: images/alpine_image.png
  :alt: Alpine Image

4. Setting Up PCs on virtualbox:
   Install Alpine x86_64 on virtualbox as PC1, PC2, and PC3.

5. clieck on the New button

.. image:: images/install_alpine_step1.png
  :alt: Step1 

6. Enter the name for PC1, PC2, PC3 and Vrouter respectively for each time while repeating the steps
7. Navigate the path the downloaded alpine image
8. Choose Linux as its type, Linux 64-bit as its version

.. image:: images/install_alpine_step2.png
  :alt: Step2

9. Click Next and leave it to default

.. image:: images/install_alpine_step3.png
  :alt: Step3

10. Click Next and leave it to default

.. image:: images/install_alpine_step4.png
  :alt: Step4 
11. Click on Finish button

.. image:: images/install_alpine_step4.png
  :alt: Step4 

12. Note before starting the pc setup the network adapters accordingly by navigating to settings
13. Click on setting button

.. image:: images/install_alpine_step1.png
  :alt: Step6
14. Navigate to the network tab on side bar

.. image:: images/install_alpine_step6.png
  :alt: Step6
15. Set the network adapter for PC's accordingly
16. For PC1

.. image:: images/install_alpine_step6.png
  :alt: Step7

17. For PC2

.. image:: images/install_alpine_step8.png
  :alt: Step8
18. For PC1

.. image:: images/install_alpine_step9.png
  :alt: Step9
19. For Vrouter - Set the 4 virtual adapters as follows:

.. image:: images/install_alpine_step10.png
  :alt: Step10

.. image:: images/install_alpine_step25.png
  :alt: Step11

.. image:: images/install_alpine_step26.png
  :alt: Step12

.. image:: images/install_alpine_step27.png
  :alt: Step13

20. Start the PC's and Vrouter, by clicking the start button, this will open the terminal.

.. image:: images/install_alpine_step11.png
  :alt: Step14

21. Login to alpine with default username 'root' and password as ''
22. Type 'setup-alpine' and enter the interactive setup

.. image:: images/install_alpine_step12.png
  :alt: Step15

23. The setup-alpine script offers the following configuration options:
24. Keyboard Layout : 'us'

.. image:: images/install_alpine_step13.png
  :alt: Step16
25. Keyboard Variant : 'us'

.. image:: images/install_alpine_step14.png
  :alt: Step17
27. Hostname: 'PC01' or 'PC02' or 'PC03' or 'vrouter'

.. image:: images/install_alpine_step16.png
  :alt: Step17
28. Network: 'none'

.. image:: images/install_alpine_step20.png
  :alt: Step18

29.  DNS Servers:'8.8.8.8'
30. Root password: 'set root password of your choice'
31. Timezone: 'Asia/Singapore'

.. image:: images/install_alpine_step21.png
  :alt: Step18

32. HTTP/FTP Proxy:'none'
33.  Mirror:'skip'
34. Setup a user:'no'
35. SSH:'OpenSSH'
36. Disk Mode:'sys'

.. image:: images/install_alpine_step23.png
  :alt: Step25
