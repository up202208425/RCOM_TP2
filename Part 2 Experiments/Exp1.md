# Part 2 / Exp 1 - Configure an IP Network

## To-Do

1. Connect `E1` of `tuxY3` and `E1` of `tuxY4` to the switch.  
2. Configure `eth1` interface of `tuxY3` and `eth1` interface of `tuxY4` using `ifconfig` and `route` commands.  
3. Register the IP and MAC addresses of the network interfaces.  
4. Use the `ping` command to verify connectivity between these computers.  
5. Inspect forwarding (`route -n`) and ARP (`arp -a`) tables.  
6. Delete ARP table entries in `tuxY3` (`arp -d ipaddress`).  
7. Start Wireshark in `tuxY3.eth1` and start capturing packets.  
8. In `tuxY3`, ping `tuxY4` for a few seconds.  
9. Stop capturing packets.  
10. Save the log and study it at home.  

## Questions

### What are the ARP packets and what are they used for?
ARP (*Address Resolution Protocol*) packets are used to map an IP address to its corresponding MAC address on a local network.

### What are the MAC and IP addresses of ARP packets, and why?
ARP packets are sent as broadcast messages, containing two IP addresses within the same ARP packet: the source machine's IP and the destination machine's IP. The destination MAC address in ARP requests is `FF:FF:FF:FF:FF:FF` (broadcast). Upon receiving the ARP request, the destination responds with its MAC address.

### What packets does the ping command generate?
When the MAC address of the destination machine is unknown, the *ping* command generates ARP packets to resolve the address. Once resolved, the command generates ICMP (*Internet Control Message Protocol*) packets for echo requests and replies.

### What are the MAC and IP addresses of the ping packets?
The IP addresses used in ICMP packets belong to the source and destination machines (`172.16.20.1` and MAC `00:c0:df:25:40:66` for tux23, and `172.16.20.254` and MAC `00:01:02:a1:35:69` for tux24). The MAC addresses correspond to the network interfaces of these machines and can be verified using the `arp -a` command or Wireshark captures.

### How to determine if a receiving Ethernet frame is ARP, IP, or ICMP?
The protocol type of an Ethernet frame is visible in the "Protocol" column in Wireshark. Additionally, the Ethernet frame header includes protocol identifiers (e.g., `0x0806` for ARP, `0x0800` for IP).

### How to determine the length of a receiving frame?
Wireshark displays the length of each captured frame in its "Length" column. This information is also present in the frame header.

### What is the loopback interface, and why is it important?
The loopback interface (`127.0.0.1`) is a virtual interface used for testing and diagnostics. It ensures that networking software operates correctly even without an active external connection, allowing developers to verify configurations and functionality locally.
