# Part 2 / Exp 3 - Configure a Router in Linux

## To-Do

1. Transform tuxY4 (Linux) into a router
– Configure also eth2 interface of tuxY4 and add it to bridgeY1
– Enable IP forwarding
– Disable ICMP echo-ignore-broadcast

2. Observe MAC addresses and IP addresses in tuxY4.eth1 and tuxY4.eth2
- no tux24.eth1 IP `172.16.20.254` e o endereço MAC `00:01:02:a1:35:69`
- no tux24.eth2 IP `172.16.21.253` e o endereço MAC `00:c0:df:04:20:8c`

3. Reconfigure tuxY3 and tuxY2 so that each of them can reach the other
- no tux23, comando `route add -net 172.16.Y1.0/24 gw 172.16.20.254`
- no tux24, comando `route add -net 172.16.Y0.0/24 gw 172.16.21.253`
- confirmamos a reconfiguração com `route -n`
- nós usamos o comando `ping` no tuxY3 e no tux24 para veriicar a conectividade com o tux22.

4. Observe the routes available at the 3 tuxes (route -n)
5. Start capture at tuxY3
6. From tuxY3, ping the other network interfaces (172.16.Y0.254, 172.16.Y1.253,
172.16.Y1.1) and verify if there is connectivity
7. Stop the capture and save the logs
8. Start capture in tuxY4; use 2 instances of Wireshark, one per network interface
9. Clean the ARP tables in the 3 tuxes
10. In tuxY3, ping tuxY2 for a few seconds.
11. Stop captures in tuxY4 and save logs

## Questions

### What routes are there in the tuxes? What are their meaning?
- No tux23 e no tux22, existem rotas que utilizam o tux24 como gateway, uma vez que este é configurado como router entre as duas redes.
- No tux23, a rota para a rede do tux22 (`172.16.21.0/24`) utiliza o gateway `172.16.20.254` (interface do tux24 na bridge20).
- No tux22, a rota para a rede do tux23 (`172.16.Y0.0/24`) utiliza o gateway `172.16.21.253` (interface do tux24 na bridge21).
Isto significa que a ligação entre as redes passa obrigatoriamente pelo tux24.

### What information does an entry of the forwarding table contain?

Cada entrada na fowarding table contém:

- Endereço de destino: o endereço IP ou sub-rede para onde o pacote deve ser enviado.
- Gateway: o endereço IP respetivo que encaminhará o pacote.
- Netmask: define o intervalo de endereços cobertos pela entrada.
- Interface: a interface de rede utilizada para enviar o pacote (ex.: eth1, eth2).

### What ARP messages, and associated MAC addresses, are observed and why?

Os pacotes ARP observados entre o tux23 e o tux22 contêm apenas os endereços MAC do tux23 e do tux24. 

Isso acontece pelos seguintes motivos:

- O tux23 não conhece o MAC do tux22 diretamente.
- Ele envia um ARP request para encontrar o endereço MAC do gateway (tux24).
- O tux24, sendo o router, encaminha os pacotes para o tux22.

Portanto, o ARP ocorre apenas entre o remetente (tux23) e o seu gateway (tux24).

### What ICMP packets are observed and why?

Os pacotes ICMP observados são do tipo Echo Request e Echo Reply, gerados pelo comando `ping` ou `traceroute`.

Quando fazemos o comando `ping`, acontece o seguinte:
- O Echo Request é enviado do tux23 para o tux22, solicitando uma resposta.
- O Echo Reply é enviado de volta pelo tux22 como confirmação da receção.
- A presença desses pacotes no Wireshark confirma que a rede está configurada corretamente e a conectividade foi estabelecida.

### What are the IP and MAC addresses associated to ICMP packets and why? 

Cada pacote ICMP observado através do `ping` no tux23 contém como endereço de origem o IP do tux23 (tux que iniciou o `ping`) e o endereço de destino o IP do tux22. No entanto, esses pacotes contém o endereço MAC do tux24, pois é este tux que faz a ligação entre as duas bridges. Isso ocorre porque o tux24 funciona como router.
