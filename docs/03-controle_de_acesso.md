# 🕵🏾‍♀️ ACLs - Listas de Controle de Acesso

ACLs (Access Control Lists) são pequenos pacotes de ACEs do IOS que filtram os pacotes de dados com base nas informações registradas nos cabeçalhos dos pacotes. As ACLs podem ser aplicadas diretamente a uma interface do roteador e atuam nas camadas 3 e 4 da rede, sendo elas respectivamente a camada de rede e transporte.

> As ACLs não atual em pacotes originários do roteador.

As ACEs (Access Control Entries) por sua vez, são pequenas instruções de permissão ou negação de tráfego na rede e são verificadas em ordem sequencial durante a execução da ALC, num processo denominado "filtragem de pacotes".

