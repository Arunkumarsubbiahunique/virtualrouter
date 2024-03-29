Routing Multi-tenant Networks with Namespace-based IP Address Management
===================================================================================

This document describes a network with a Multitenant router and an multitenancy network that allows PCs with the same IP address 
across the tenants to be contectes with the control PC. This is achieved through the use of namespaces and virtual ethernet adapters that patch one namespace to another.

Network Diagram
===============
The network diagram is shown below:

.. image:: images/network-topology.jpeg
  :alt: Network Diagram

The network consists of the following devices:

* PCs: These are the computers that connect to the network.
* Multitenant Router: This is a software router that runs on the mutlitenant router and creates the mutlitenancy capable network. 
  
Network Configuration
=====================
The network is configured as follows:

* The PCs are configured with IP addresses in the 192.168.1.0/24 subnet.
* The Multitenant router is configured with the following interfaces:
   * eth0(192.168.2.35/22): This interface connects to the Management PC0 - 192.168.2.222/22.
   * eth1(10.0.0.1/24): This interface connects to the Controller PC1 - 10.0.0.10/24.
   * eth2(192.168.1.1/24): This interface connects to a switch that connects to group 1 of PC's with IP address in 192.168.1.0/24 network space.
   * eth3(192.168.1.1/24): This interface connects to a switch that connects to group 2 of PC's with IP address in 192.168.1.0/24 network space.

* The Multitenant router is configured with namespaces and vitual ethernet to connect the namespaces to create an overlay network on top of the Alpine router's physical network.


NameSpaces and Virtual Ethernet Adapters
========================================
Namespaces and virtual ethernet adapters are used to create the overlay network. Namespaces provide a way to isolate processes and resources on a Linux system. Virtual ethernet adapters provide a way to connect namespaces to each other.

Lab Documentation: Multi-tenant Router with duplicated Networks and DNAT
========================================================================
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

.. image:: images/pc01_eth0.png
  :alt: Step7

17. For PC2

.. image:: images/pc02_eth0.png
  :alt: Step8
18. For PC3

.. image:: images/pc03_eth0.png
  :alt: Step9
19. For Vrouter - Set the 4 virtual adapters as follows:

.. image:: images/install_alpine_step10.png
  :alt: Step10

.. image:: images/vrouter_eth1.png
  :alt: Step11

.. image:: images/vrouter_eth2.png
  :alt: Step12

.. image:: images/vrouter_eth3.png
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

.. image:: images/install_alpine_step22.png
  :alt: Step25

.. image:: images/install_alpine_step23.png
  :alt: Step25

37. After the 'alpine-setup' completed
38. Type 'Poweroff' command and hit 'enter'

39. After the 'PC's' and 'Vrouter' power offed, go to setting and navigate to storage and remove the bootable media from storage.

40. Turn on the PC's and Vrouter

41. On PC's and Vrouter
    mkdir -p /etc/nettop/network-setup.sh

Download the scripts from : https://github.com/Arunkumarsubbiahunique/network-setup/tree/main

1. On pc1
    ------

.. code-block:: bash
  :linenos:

    vi /etc/nettop/network-setup.sh
    hit 'i' key to edit the file
    copy and paste the below

    ip addr add 10.0.0.10/24 dev eth0
    ip link set up eth0
    ip route add default via 10.0.0.1 dev eth0

    hit 'esc' key and type ':wq' to save

    type 'crontab -e' and hit enter
    scroll all the way down using 'down arror' and 
    hit 'i' key to edit the file
    copy and paste the below
    @reboot sh /etc/nettop/network-setup.sh

    hit 'esc' key and type ':wq' to save

    type 'reboot' and hit enter

    after the pc01 rebooted successfully

    type 'ip a', hit enter and verify the ip address on eth0

    type 'ip route', hit enter and default gateway

code ...

2. On pc2
    ------

.. code-block:: bash
  :linenos:

    vi /etc/nettop/network-setup.sh
    hit 'i' key to edit the file
    copy and paste the below

    ip addr add 192.168.1.100/24 dev eth0
    ip link set up eth0
    ip route add default via 192.168.1.1 dev eth0

    hit 'esc' key and type ':wq' to save

    type 'crontab -e' and hit enter
    scroll all the way down using 'down arror' and 
    hit 'i' key to edit the file
    copy and paste the below
    @reboot sh /etc/nettop/network-setup.sh

    hit 'esc' key and type ':wq' to save

    type 'reboot' and hit enter

    after the pc02 rebooted successfully

    type 'ip a', hit enter and verify the ip address on eth0

    type 'ip route', hit enter and default gateway

code ...

3. On pc3
    ------

.. code-block:: bash
  :linenos:


    vi /etc/nettop/network-setup.sh
    hit 'i' key to edit the file
    copy and paste the below

    ip addr add 192.168.1.100/24 dev eth0
    ip link set up eth0
    ip route add default via 192.168.1.1 dev eth0

    hit 'esc' key and type ':wq' to save

    type 'crontab -e' and hit enter
    scroll all the way down using 'down arror' and 
    hit 'i' key to edit the file
    copy and paste the below
    @reboot sh /etc/nettop/network-setup.sh

    hit 'esc' key and type ':wq' to save

    type 'reboot' and hit enter

    after the pc03 rebooted successfully

    type 'ip a', hit enter and verify the ip address on eth0

    type 'ip route', hit enter and default gateway
code ...



4. On Vrouter
    -----------
    
.. code-block:: bash
  :linenos:

    apk add iproute2
    apk add iptables
    vi /etc/nettop/network-setup.sh
    hit 'i' key to edit the file
    copy and paste the below

    #!/bin/bash

    # Create namespaces
    ip netns add vr01
    ip netns add vr02
    ip netns add vr03

    # Move interfaces to namespaces
    ip link set eth1 netns vr01
    ip link set eth2 netns vr02
    ip link set eth3 netns vr03

    # Configure IP addresses within namespaces
    ip netns exec vr01 ip addr add 10.0.0.1/24 dev eth1
    ip netns exec vr01 ip link set eth1 up

    ip netns exec vr02 ip addr add 192.168.1.1/24 dev eth2
    ip netns exec vr02 ip link set eth2 up

    ip netns exec vr03 ip addr add 192.168.1.1/24 dev eth3
    ip netns exec vr03 ip link set eth3 up

    # Create and configure veth interfaces
    ip link add vr01-vr02-veth0 type veth peer vr02-vr01-veth0
    ip link set vr01-vr02-veth0 netns vr01
    ip link set vr02-vr01-veth0 netns vr02

    ip netns exec vr01 ip addr add 192.168.12.1/24 dev vr01-vr02-veth0
    ip netns exec vr01 ip link set vr01-vr02-veth0 up

    ip netns exec vr02 ip addr add 192.168.12.2/24 dev vr02-vr01-veth0
    ip netns exec vr02 ip link set vr02-vr01-veth0 up

    ip link add vr01-vr03-veth0 type veth peer vr03-vr01-veth0
    ip link set vr01-vr03-veth0 netns vr01
    ip link set vr03-vr01-veth0 netns vr03

    ip netns exec vr01 ip addr add 192.168.13.1/24 dev vr01-vr03-veth0
    ip netns exec vr01 ip link set vr01-vr03-veth0 up

    ip netns exec vr03 ip addr add 192.168.13.3/24 dev vr03-vr01-veth0
    ip netns exec vr03 ip link set vr03-vr01-veth0 up

    # Configure routing
    ip netns exec vr02 ip route add 10.0.0.0/24 via 192.168.12.1 dev vr02-vr01-veth0
    ip netns exec vr03 ip route add 10.0.0.0/24 via 192.168.13.1 dev vr03-vr01-veth0

    # Enable IP forwarding (optional, if needed)
    ip netns exec vr01 sysctl -w net.ipv4.ip_forward=1
    ip netns exec vr02 sysctl -w net.ipv4.ip_forward=1
    ip netns exec vr03 sysctl -w net.ipv4.ip_forward=1

    # NAT configuration
    ip netns exec vr02 iptables -t nat -A PREROUTING -i vns02-veth1 -p icmp -j DNAT --to-destination 192.168.1.100
    ip netns exec vr02 iptables -t nat -A PREROUTING -p tcp --dport 22 -j DNAT --to-destination 192.168.1.100:22
    ip netns exec vr03 iptables -t nat -A PREROUTING -i vns03-veth1 -p icmp -j DNAT --to-destination 192.168.1.100
    ip netns exec vr03 iptables -t nat -A PREROUTING -p tcp --dport 22 -j DNAT --to-destination 192.168.1.100:22

    hit 'esc' key and type ':wq' to save

    type 'crontab -e' and hit enter
    scroll all the way down using 'down arror' and 
    hit 'i' key to edit the file
    copy and paste the below
    @reboot sh /etc/nettop/network-setup.sh

    hit 'esc' key and type ':wq' to save

    type 'reboot' and hit enter

    after the Vrouter rebooted successfully

    type 'netns exec vr01 ip a', hit enter and verify the ip address on eth1

    type 'netns exec vr01 ip route', hit enter and verify the routes

    type 'netns exec vr02 ip a', hit enter and verify the ip address on eth2

    type 'netns exec vr02 ip route', hit enter and verify the routes

    type 'netns exec vr02 ip tables -t nat -L -n', hit enter and verify the iptables

    type 'netns exec vr03 ip a', hit enter and verify the ip address on eth3

    type 'netns exec vr03 ip route', hit enter and verify the routes

    type 'netns exec vr03 ip tables -t nat -L -n', hit enter and verify the iptables

code ...





