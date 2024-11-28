# Part 2 / Exp 1 - Configure an IP Network



## Questions

### What are the ARP packets and what are they used for?
ARP (*Address Resolution Protocol*) packets are used to establish the connection between an IP address and a MAC address.

### What are the MAC and IP addresses of ARP packets and why?
ARP packets are sent in broadcast, containing two IP addresses within the same ARP packet: the destination machine's IP (172.16.50.254, Tux54) and the source machine's IP (172.16.50.01, Tux53). As a response, Tux54 sends an ARP packet back to Tux53 containing its MAC address, 00:21:5a:c3:78:70.

### What packets does the ping command generate?
While the MAC address of the destination machine is unknown, the *ping* command generates ARP packets. Once the IP address is resolved to its corresponding MAC address, the command generates ICMP (*Internet Control Message Protocol*) packets to transfer information.

### What are the MAC and IP addresses of the ping packets?
The MAC and IP addresses used in ICMP packets belong to the machines communicating: Tux53 and Tux54.

### How to determine if a receiving Ethernet frame is ARP, IP, or ICMP?
The protocol designation of each frame is available in the "Protocol" column of Wireshark captures. Usually, the protocol is indicated in the first two bytes of the frame header.

### How to determine the length of a receiving frame?
Wireshark includes a column that shows the length of frames in bytes. Typically, the frame header contains this information as well.

### What is the loopback interface and why is it important?
The loopback interface is a virtual interface that is always reachable as long as at least one of the IP interfaces of the switch is operational. It allows periodic checks to verify if network connections are correctly configured.
