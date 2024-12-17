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
  - sysctl net.ipv4.conf.eth1.accept_redirects=0
  - sysctl net.ipv4.conf.all.accept_redirects=0
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
Começamos por resetar as suas configurações, adicioná-lo à rede interna (à bridge correspondente) e atribuir um IP interno e um IP externo.

### What are the paths followed by the packets, with and without ICMP redirect enabled, in the experiments carried out and why?
Na primeira experiência, sem a ligação do tux22 ao tux24, os pacotes de dados foram reencaminhados (ICMP redirect) através do router implementado até ao endereço IP de destino.
Já na segunda experiência não houve qualquer reencaminhamento pois a ligação mais curta da rede estava disponível.
VER LOGS MAIS TARDE

Em suma:

- Sem ICMP Redirect: o router encaminha os pacotes para o gateway configurado, mesmo que o caminho seja maior. Ou seja, os pacotes vão passar por caminhos intermediários, mesmo que haja uma rota mais direta.

- Com ICMP Redirect: o router pode enviar um "redirect" para o dispositivo de origem, sugerindo que use um caminho mais direto (como um gateway mais próximo). Isso melhora a eficiência, evitando a necessidade de passar por um router intermediário quando o caminho direto está disponível.

### How to configure NAT in a commercial router?
Tal como proposto do guião, desativamos o NAT com o comando `/ip firewall nat disable 0` no terminal do router. Vimos que sem o NAT não obtivemos replies. De seguida, fizemos o comando `/ip firewall nat enable 0` no terminal do router e ao fazer um ping para fora, a resposta é recebida corretamente.

- Enable: `/ip firewall nat enable 0´
- Disable: `/ip firewall nat disable 0´

### What does NAT do?
O NAT (Network Address Translation) traduz endereços da rede local para um único endereço público, e viceversa. Assim, quando um pacote é enviado para uma rede externa, é enviado com o endereço publico como origem. Quando o computador de destino responde, envia a resposta para esse endereço público, que é depois traduzido de volta para o endereço local de destino que enviou o pacote em primeiro lugar. Deste modo, é possivel reduzir o numero de endereços publicos utilizados.

### What happens when tuxY3 pings the FTP server with the NAT disabled? Why?
Quando o tux23 tenta fazer ping ao servidor FTP (172.16.1.10) com o NAT desativado, o pedido de ping irá falhar.
Sem o NAT, o servidor FTP depende da routing table para determinar como devolver a resposta ICMP ao tux23.
Caso o servidor FTP ou os routers intermédios não tenham uma rota para a sub-rede do tux23, a resposta não será entregue.
Resumidamente, o tux23 consegue enviar o pedido ICMP ao servidor FTP. O servidor FTP pode gerar uma resposta ICMP, mas esta falha em chegar ao tux23 porque o servidor não consegue encaminhar o pacote de volta sem um IP traduzido pelo NAT.
