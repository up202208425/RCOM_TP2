# Part 2 / Exp 4 - Configure a Commercial Router and Implement NAT

## To-Do

1. Connect ether1 of RC to the lab network on PY.12 (with NAT enabled by default) and ether2 of RC to a port on bridgeY1. Configure the IP addresses of RC through the router serial console

2. Verify routes
– in tuxY3 add routes for 172.16.Y1.0/24 and 172.16.1.0/24
– in tuxY4 add route for 172.16.1.0/24
– in tuxY2 add routes for 172.16.Y0.0/24 and 172.16.1.0/24
– in RC add route for 172.16.Y0.0/24

3. Using ping commands and Wireshark, verify if tuxY3 can ping all the network interfaces of tuxY2, tuxY4 and RC

4. In tuxY2
– Do the following:
sysctl net.ipv4.conf.eth1.accept_redirects=0
sysctl net.ipv4.conf.all.accept_redirects=0
– In tuxY2, change the routes to use RC as the gateway to subnet 172.16.Y0.0/24 instead of tuxY4
– In tuxY2, ping tuxY3
– Using capture at tuxY2, try to understand the path followed by ICMP ECHO and ECHO-REPLY packets (look at MAC addresses)
– In tuxY2, do traceroute tuxY3
– In tuxY2, change the routes to use again tuxY4 as the gateway to subnet 172.16.Y0.0/24 instead of RC. Do traceroute tuxY3
– Activate the acceptance of ICMP redirect at tuxY2 when there is no route to
172.16.Y0.0/24 via tuxY4 and try to understand what happens

5. In tuxY3, ping the FTP server (172.16.1.10) and try to understand what happens

6. Disable NAT functionality in router RC

7. In tuxY3 ping 1

## Questions

### How to configure a static route in a commercial router?


### What are the paths followed by the packets, with and without ICMP redirect enabled, in the experiments carried out and why?



### How to configure NAT in a commercial router?


### What does NAT do?



### What happens when tuxY3 pings the FTP server with the NAT disabled? Why?
