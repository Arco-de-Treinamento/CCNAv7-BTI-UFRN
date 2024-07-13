# 🕵🏾‍♀️ ACLs - Listas de Controle de Acesso

ACLs (Access Control Lists) são pequenos pacotes de ACEs do IOS que filtram os pacotes de dados com base nas informações registradas nos cabeçalhos dos pacotes. As ACLs podem ser aplicadas diretamente a uma interface do roteador e atuam nas camadas 3 e 4 da rede, sendo elas respectivamente a camada de rede e transporte.

> As ACLs não atual em pacotes originários do roteador.

As ACEs (Access Control Entries) por sua vez, são pequenas instruções de permissão ou negação de tráfego na rede e são verificadas em ordem sequencial durante a execução da ALC, num processo denominado "filtragem de pacotes".

## 🕵🏾‍♀️ Direção de aplicação de uma ACL

As ACLs oferecem politicas que podem ser aplicadas tanto ao tráfego de entrada como também ao tráfego de saída de dados.

* **ACL de entrada**: filtra os pacotes da rede antes de serem roteados para a interface de saída do roteador. Normalmente utilizada em cenários onde a rede conectada a interface de entrada é a única origem de dados que necessita ser filtrada.

* **ACL de saída**: responsável por filtrar os pacotes após o seu roteamento, independente da interface de entrada. As ACLs de saídas costumam ser utilizadas em situações onde os pacotes de dados são provenientes de várias interfaces de entrada.

## 🔏 Criando ACLS

As ACLs podem ser do tipo **padrão** ou **estendida**, onde na estendida temos uma gama bem maior de filtros disponíveis. Vale ressaltar que as ACLs numeradas possuem uma faixa de atuação de acordo com ID recebido, que será comentada posteriormente.

### ACLs IPv4 Padrão

Uma ACL IPv4 padrão atua filtrando pacotes com base apenas no endereço IPv4 de origem. Sua configuração pode ser feita com o comando:

```bash
conf t
access-list <acl-number> <permit/deny> <ip-address> <wildcard-mask>
```

Nesse caso temos uma negação **`deny any`** implícita ao fim do prompt, que indica que todo o tráfego, com exceção dos pacotes provenientes do ip permitido, será negado.

### ACLs IPv4 Estendidas

Uma ACL IPv4 estendida filtram os pacotes com base no endereço IPv4 de destino, protocolo utilizado além de portas TCP ou UDP de origem utilizadas.

```bash
conf t
access-list <acl-number> <permit/deny> <protocol> <ip-address> <wildcard-mask> <packets> <net-protocol>
```

#### Propriedades de uma ACL estendida

| Ação executada | Definição                  |
|----------------|----------------------------|
| deny           | Specify packets to reject  |
| permit         | Specify packets to forward |
| remark         | Access list entry comment  |

| Protocolos     | Definição                           |
|----------------|-------------------------------------|
| ahp            | Authentication Header Protocol      |
| eigrp          | Cisco's EIGRP routing protocol      |
| esp            | Encapsulation Security Payload      |
| gre            | Cisco's GRE tunneling               |
| icmp           | Internet Control Message Protocol   |
| ip             | Any Internet Protocol               |
| ospf           | OSPF routing protocol               |
| tcp            | Transmission Control Protocol       |
| udp            | User Datagram Protocol              |
| <0-65535>      | Port number                         |
| ftp            | File Transfer Protocol (21)         |
| pop3           | Post Office Protocol v3 (110)       |
| smtp           | Simple Mail Transport Protocol (25) |
| telnet         | Telnet (23)                         |
| www            | World Wide Web (HTTP, 80)           |

| Endereço | Definição                                       |
|----------|-------------------------------------------------|
| A.B.C.D  | Destination address                             |
| any      | Any destination host                            |
| eq       | Match only packets on a given port number       |
| gt       | Match only packets with a greater port number   |
| host     | A single destination host                       |
| lt       | Match only packets with a lower port number     |
| neq      | Match only packets not on a given port number   |
| range    | Match only packets in the range of port numbers |

### 🔢 ACLs numeradas

Uma ACL numerada tem uma faixa de atuação segundo o ID recebido:

| ID          | Area de atuação                          |
|-------------|------------------------------------------|
| <1-99>      | IP standard access list                  |
| <100-199>   | IP extended access list                  |
| <1100-1199> | Extended 48-bit MAC address access list  |
| <1300-1999> | IP standard access list (expanded range) |
| <200-299>   | Protocol type-code access list           |
| <2000-2699> | IP extended access list (expanded range) |
| <700-799>   | 48-bit MAC address access list           |
| rate-limit  | Simple rate-limit specific access list   |
| template    | Enable IP template acls                  |

### ACLs nomeadas

As ACLs podem ser nomeadas para trazer informações sobre a sua finalidade. Para criar ACLs nomeadas é utilizado o comando **`ip access-list`**, com a seguinte sintaxe:

```bash
conf t
ip access-list <acl-name> <extended/standard> <acl-name>
```

A partir desse ponto é iniciado a ferramente de configuração onde podemos definir as propriedades da ACL.


## 🔓 Aplicando ACLs

Após criada e configurada, uma ACL IPv4 deve ser vinculada a uma interface para entrar em vigor. Para adicionar uma ACL  a uma interface deve-se utilizar

```bash
conf t
interface <interface-name>
ip access-group <acl-name/acl-number> <in/out>
end
```