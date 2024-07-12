# 🕵🏾‍♀️ ACLs - Listas de Controle de Acesso

ACLs (Access Control Lists) são pequenos pacotes de ACEs do IOS que filtram os pacotes de dados com base nas informações registradas nos cabeçalhos dos pacotes. As ACLs podem ser aplicadas diretamente a uma interface do roteador e atuam nas camadas 3 e 4 da rede, sendo elas respectivamente a camada de rede e transporte.

> As ACLs não atual em pacotes originários do roteador.

As ACEs (Access Control Entries) por sua vez, são pequenas instruções de permissão ou negação de tráfego na rede e são verificadas em ordem sequencial durante a execução da ALC, num processo denominado "filtragem de pacotes".

## 🕵🏾‍♀️ Direção de aplicação de uma ACL

As ACLs oferecem politicas que podem ser aplicadas tanto ao tráfego de entrada como também ao tráfego de saída de dados.

* **ACL de entrada**: filtra os pacotes da rede antes de serem roteados para a interface de saída do roteador. Normalmente utilizada em cenários onde a rede conectada a interface de entrada é a única origem de dados que necessita ser filtrada.

* **ACL de saída**: responsável por filtrar os pacotes após o seu roteamento, independente da interface de entrada. As ACLs de saídas costumam ser utilizadas em situações onde os pacotes de dados são provenientes de várias interfaces de entrada.
