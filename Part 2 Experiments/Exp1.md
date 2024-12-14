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
ARP (*Address Resolution Protocol*) packets são usado para mapear um IP address ao seu MAC address correspondente numa rede local.

### What are the MAC and IP addresses of ARP packets, and why?
ARP packets são enviados em broadcast, contendo dois endereços IP addresses no mesmo ARP packet: o IP da máquina de origem e o endereço IP da máquina de destino. O endereço MAC em ARP requests é `FF:FF:FF:FF:FF:FF` (broadcast). Com a receção do ARP request, o destino responde com o seu endereço MAC.

### What packets does the ping command generate?
Enquanto não obtém o endereço MAC da máquina de destino, o comando `ping` gera ARP packets para encontrar o address. Após fazer a ligação do endereço IP ao respetivo MAC, o comando gera pacotes ICMP (*Internet Control Message Protocol*) para echo requests and replies.

### What are the MAC and IP addresses of the ping packets?
Os endereços IP usados nos pacotes ICMP pertencem sempre à máquina de origem (`172.16.20.1` para o tux23) e à máquina de destino final (`172.16.20.254` para o tux24).

Os endereços MAC variam dependendo da etapa do encaminhamento:

- No primeiro salto (na rede de origem), o MAC de origem é o do tux23 (`00:c0:df:25:40:66`), e o MAC de destino é o do tux24 (gateway, `00:01:02:a1:35:69`).
  
- No último salto (na rede de destino), o MAC de origem é o do tux24 (interface na bridge de destino), e o MAC de destino é o do tux22.

### How to determine if a receiving Ethernet frame is ARP, IP, or ICMP?
O tipo de protocolo de cada frame Ethernet pode ser identificado na coluna "Protocol" ao capturar pacotes com o Wireshark.

Analisando a estrutura de um frame Ethernet:

- Endereço MAC de destino: primeiros 6 bytes.
- Endereço MAC de origem: seguintes 6 bytes.
- EtherType: últimos 2 bytes do cabeçalho Ethernet, que indicam o protocolo encapsulado: `0x0806`: ARP, `0x0800`: IP.

Para frames IP, o campo Protocol no cabeçalho IP indica o tipo de payload: `1`: ICMP.

### How to determine the length of a receiving frame?
O Wireshark apresenta o tamanho de cada frame capturada na coluna "Length", em bytes.

### What is the loopback interface, and why is it important?
O loopback é uma interface virtual que é sempre alcançável desde que pelo menos uma das interfaces IP do `switch` esteja operacional. Com isto, é possível verificar periodicamente se as ligações da rede estão configuradas corretamente.
