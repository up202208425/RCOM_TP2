# Part 2 / Exp 6 - TCP connections

## To-Do

1. Compile your download application in tuxY3
2. In tuxY3, restart capturing with Wireshark and run your application
3. Verify if file has arrived correctly, stop capturing and save the log
4. Using Wireshark, observe packets exchanged including:

– TCP control and data connections, and its phases (establishment, data, termination)
– Data transferred through the FTP control connection
– TCP ARQ mechanism
– TCP congestion control mechanism in action
– Note: use also the Wireshark Statistics tools (menu) to study the TCP phases, ARQ and congestion control mechanism

5. Repeat the download in tuxY3 but now, in the middle of the transfer, start a new download in tuxY2

– Use the Wireshark statistics tools to understand how the throughput of a TCP connection varies along the time

#### Devido à velocidade com que os ficheiros passavam de um tux para o outro, não fomos capazes de simular a colisão a meio de uma tranferência tal como proposto no passo nº5.

## Questions

### How many TCP connections are opened by your FTP application?
Duas conexões TCP:

- Conexão de controlo: usada para enviar comandos e receber respostas do servidor.
- Conexão de dados: usada para transferir o ficheiro.

### In what connection is transported the FTP control information?
Na conexão de controlo. Esta é responsável pela troca de comandos (ex.: `USER`, `PASS`, `LIST`, `RETR`) e pelas mensagens do servidor (ex.: respostas e códigos de status).

### What are the phases of a TCP connection?
Uma conecção TCP tem 3 fases:

- 1ª Estabelecimento (3-way handshake): Troca de pacotes SYN, SYN-ACK e ACK.
- 2ª Transferência de dados: Envio e receção de segmentos TCP com dados.
- 3ª Finalização (teardown): Troca de pacotes FIN e ACK para encerrar a conexão.


### How does the ARQ TCP mechanism work? What are the relevant TCP fields? What relevant information can be observed in the logs?
O mecanismo Automatic Repeat Request (ARQ) é usado para garantir a entrega confiável de dados.

Funcionamento:

- O remetente envia pacotes e espera confirmações (ACKs) do recetor.
- Se o ACK não for recebido num tempo limite (timeout), o pacote é retransmitido.
- Em caso de pacotes perdidos, retransmissões podem ser desencadeadas por 3 ACKs duplicados.

Campos relevantes no TCP:

- Seq (Sequence Number): indica o número do segmento enviado.
- ACK (Acknowledgment Number): confirma a receção de segmentos.
- Window Size: especifica a quantidade de dados que pode ser enviada sem esperar por confirmação.

Observações nos logs:

- Retransmissões (indicadas por repetições de números de sequência).
- ACKs duplicados ou atrasados.
- Variação do Window Size conforme o congestionamento.

- NOTA: Uma rede diz-se congestionada a partir do momento que se perdem pacotes.


### How does the TCP congestion control mechanism work? What are the relevant fields. How did the throughput of the data connection evolve along the time? Is it according to the TCP congestion control mechanism?
Cada emissor determina a capacidade da comunicação para poder enviar mais ou menos pacotes. Para isso há um parâmetro na conexão chamado CongestionWindow.

Funcionamento:

- O TCP ajusta dinamicamente a quantidade de dados enviada (tamanho da CongestionWindow, CWND), com base na perceção do congestionamento da rede.

Fases:

- Slow Start: CWND aumenta até atingir o limite imposto pela capacidade da rede.
- Congestion Avoidance: Após atingir o limiar (threshold), o aumento do CWND torna-se linear.
- Multiplicative Decrease: Em caso de congestionamento (timeout ou 3 ACKs duplicados), o CWND reduz.

Campos relevantes no TCP:

- Congestion Window (CWND): controla o número de pacotes que podem ser enviados. 
- Threshold: define o limite entre Slow Start e Congestion Avoidance.


### Is the throughput of a TCP data connections disturbed by the appearance of a second TCP connection? How?
Sim, ao fazer mais de uma conexão TCP a largura de banda será dividida entre as multiplas ligações, reduzindo a velocidade de cada uma; isto porque cada conexão adapta o seu CWND com base no congestionamento.
Em redes com largura de banda limitada, o throughput de cada conexão diminui proporcionalmente em relação ao número de conexões.
