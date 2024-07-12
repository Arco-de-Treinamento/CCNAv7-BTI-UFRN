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
