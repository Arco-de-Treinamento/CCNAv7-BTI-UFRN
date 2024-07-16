# 🕸️ NAT para IPv4

O NAT (Network Address Translation) é um recurso de rede que permite converter endereços IPs privados em endereços públicos na rede. Roteadores que atuam com o NAT normalmente operam como dispositivos de borda e podem possuir mais de um endereço público para a tradução, presente no pool de NATs.

## NAT Estático

O NAT estático é um NAT que converte o endereço de rede privado em endereço global estático. Este tipo é especialmente útil para situações onde dispositivos internos precisam ser acessados remotamente, por SSH, por exemplo.

No uso do NAT estático deve-se garantir que existem endereços públicos suficientes para todas as aplicações.

## NAT dinâmico

O NAT dinâmico, por sua vez, utiliza um pool de endereços públicos e os atribui a endereços privados conforme a necessidade a partir de uma ordem de chegada. No NAT dinâmico os dispositivos não se apresentam para a rede externa necessariamente com o mesmo IP.

É necessário fornecer IPs públicos suficientes para que o NAT possa suprir a carga da rede.

### PAT - Sobrecarga de NAT

Utiliza as portas TCP e UDP para sobrecarregar os endereços do pool de IPs públicos do NAT. Desse modo, vários dispositivos diferentes podem se conectar a rede por um mesmo IP externo.

O PAT garante que os dispositivos conectados utilizem portas diferentes e as atribui na tabela de endereçamento para serem corretamente convertidos.
