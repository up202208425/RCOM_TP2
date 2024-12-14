# Part 2 / Exp 2 - Implement two bridges in a switch

## To-Do

1. Connect and configure `E1` of `tuxY2`, and register its IP and MAC addresses.

Depois de configurar o `E1` do `tux22`, o endereço IP é `72.16.21.1` e o endereço MAC é `00:e0:7d:b5:8c:8e`

2. Create two bridges in the switch: `bridgeY0` and `bridgeY1`.  
3. Remove the ports where `tuxY3`, `tuxY4`, and `tuxY2` are connected from the default bridge (`bridge`) and add them to the corresponding ports of `bridgeY0` and `bridgeY1`.  
4. Start packet capture on `tuxY3.eth1`.  
5. In `tuxY3`, ping `tuxY4` and then ping `tuxY2`.
6. Stop the capture and save the log.  
7. Start new packet captures on `tuxY2.eth1`, `tuxY3.eth1`, and `tuxY4.eth1`.  
8. In `tuxY3`, perform a broadcast ping (`ping -b 172.16.Y0.255`) for a few seconds.  
9. Observe the results, stop the captures, and save the logs.  
10. Repeat steps 7, 8, and 9, but this time:  
    - Perform the broadcast ping in `tuxY2` (`ping -b 172.16.Y1.255`).  

ping broadcast in tuxY2 (ping -b 172.16.Y1.255)


## Questions

### How to configure bridgeY0?
A bridge20 foi configurada para servir de meio de ligação entre os computadores tux23 e tux24, formando uma subrede. Eliminamos as configurações e ligações por omissão que o switch implementou aquando da ligação entre os computadores e configuramos novas portas/ligações a cada um dos computadores.

### How many broadcast domains are there? How can you conclude it from the logs?
Existem dois broadcast domains pois existem duas bridges implementadas. Analisando os logs, reparamos que o ping do tux23 conseguiu resposta do tux24 mas não do tux22, pois estão inseridos em duas bridges diferentes.
