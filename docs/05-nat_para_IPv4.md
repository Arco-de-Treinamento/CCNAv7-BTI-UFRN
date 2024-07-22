# 🕸️ NAT para IPv4

O NAT (Network Address Translation) é um recurso de rede que permite converter endereços IPs privados em endereços públicos na rede. Roteadores que atuam com o NAT normalmente operam como dispositivos de borda e podem possuir mais de um endereço público para a tradução, presente no pool de NATs.

## NAT Estático

O NAT estático é um NAT que converte o endereço de rede privado em endereço global estático. Este tipo é especialmente útil para situações onde dispositivos internos precisam ser acessados remotamente, por SSH, por exemplo.

No uso do NAT estático deve-se garantir que existem endereços públicos suficientes para todas as aplicações.

### Criando um mapeamento de endereços

Ao utilizar convenções estáticas, o primeiro passo é criar um mapeamento entre o **endereço local interno** e o **endereço global interno**. Esse procedimento pode ser feito com:

```bash
ip nat inside source static <inside-local-address> <inside-global-address>
```

### Configurando as interfaces

Com o mapeamento criado, agora resta identificar as interfaces que estão participando da configuração do mapeamento de entrada e saída.

Nesse caso, para a interface de entrada, deve-se utilizar:

```bash
interface <interface-name>
ip address <inside-address> <inside-mask>
ip nat inside
exit
```

E para interface de saída do NAT, será utilizado:

```bash
interface <interface-name>
ip address <outside-address> <outside-mask>
ip nat outside
exit
```

### Visualizando o NAT estático

Visualizando a configuração do NAT estático:

```bash
show ip nat translations
```

## NAT dinâmico

O NAT dinâmico, por sua vez, utiliza um pool de endereços públicos e os atribui a endereços privados conforme a necessidade a partir de uma ordem de chegada. No NAT dinâmico os dispositivos não se apresentam para a rede externa necessariamente com o mesmo IP.

É necessário fornecer IPs públicos suficientes para que o NAT possa suprir a carga da rede.

### PAT - Sobrecarga de NAT

Utiliza as portas TCP e UDP para sobrecarregar os endereços do pool de IPs públicos do NAT. Desse modo, vários dispositivos diferentes podem se conectar a rede por um mesmo IP externo.

O PAT garante que os dispositivos conectados utilizem portas diferentes e as atribui na tabela de endereçamento para serem corretamente convertidos.
