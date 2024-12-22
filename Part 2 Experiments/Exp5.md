# Part 2 / Exp 5 - DNS

## To-Do

1. Configure DNS at tuxY3, tuxY4, tuxY2 (use DNS server services.netlab.fe.up.pt (10.227.20.3))
2. Verify if names can be used in these hosts (e.g., ping hostname, use browser)
3. Execute ping (new-hostname-in-the-Internet); observe DNS related packets in
Wireshark

## Questions

### How to configure the DNS service in a host?
O DNS pode ser configurado adicionando a linha `nameserver <IP>` no ficheiro `/etc/resolv.conf` de cada host (neste caso, tux23, tux24 e tux22). Por exemplo, para configurar o servidor DNS services.netlab.fe.up.pt (10.227.20.3), adicionamos a linha `nameserver 10.227.20.3`. Após esta configuração, os hosts utilizarão este servidor DNS para resolver nomes de domínio.

### What packets are exchanged by DNS and what information is transported?

O **DNS (Domain Name System)** utiliza dois tipos principais de pacotes: **queries** e **responses**. Estes pacotes são responsáveis por traduzir nomes de domínio em endereços IP (ou vice-versa) e por fornecer outras informações relacionadas.

#### Tipos de Pacotes

1. **Query Packets**
   - **Finalidade**: Enviados pelo cliente DNS (resolver) para o servidor DNS a fim de solicitar informações.
   - **Exemplos de Tipos de Consultas**:
     - `A`: Endereço IPv4 de um domínio.
     - `AAAA`: Endereço IPv6 de um domínio.
     - `MX`: Servidor de e-mail responsável por um domínio.
     - `CNAME`: Nome canónico (alias) de um domínio.
     - `PTR`: Consulta reversa (IP para nome de domínio).
     - `NS`: Servidores de nomes autoritativos de um domínio.
     - Outros: `SOA`, `TXT`, `SRV`, etc.

2. **Response Packets**
   - **Finalidade**: Enviados pelo servidor DNS para responder à query ou informar um erro.
   - **Conteúdo das Respostas**:
     - **Answer Section**: Dados resolvidos (ex.: endereço IP do domínio).
     - **Authority Section**: Lista de servidores autoritativos se o servidor atual não for autoritativo para o domínio consultado.
     - **Additional Section**: Informações suplementares úteis, como endereços IP de servidores de nomes autoritativos.

#### Informação Transportada

Os pacotes DNS contêm vários campos organizados em seções:

1. **Cabeçalho Comum**:
   - **Transaction ID**: Identificador único para associar a resposta à query correspondente.
   - **Flags**: Indicam o tipo de pacote (query ou response) e outras informações de controlo.
   - **Contagens**: Número de perguntas (queries), respostas (answers), autoridades e informações adicionais.

2. **Seções do Pacote**:
   - **Question Section**: Inclui o nome de domínio consultado e o tipo de consulta (ex.: `A`, `MX`).
   - **Answer Section**: Inclui os registros com os dados solicitados (nome, tipo, TTL, dados, etc.).
   - **Authority Section**: Informações sobre os servidores autoritativos.
   - **Additional Section**: Dados adicionais úteis, como os endereços IP dos servidores autoritativos.

3. **Códigos de Erro**:
   - Se a query não puder ser resolvida, são retornados erros como:
     - `NXDOMAIN` (domínio inexistente).
     - `SERVFAIL` (falha no servidor).

#### Protocolo de Transporte

- **UDP porta 53**: Utilizado para queries/responses padrão, devido à sua eficiência.
- **TCP porta 53**: Utilizado para transferências de zonas (zone transfers) ou pacotes grandes que excedem os limites do UDP.

#### Resumo do Fluxo

1. O cliente envia uma query (ex.: "Qual é o endereço IP de www.example.com?").
2. O servidor responde com o endereço IP solicitado ou um erro.

#### Relacionado com os Testes

- Ao executar `ping` ou usar um navegador nos hosts (`tux23`, `tux24`, `tux22`), poderás capturar os pacotes DNS no **Wireshark**, identificando queries e responses trocadas entre os hosts e o servidor DNS configurado (`10.227.20.3`).
