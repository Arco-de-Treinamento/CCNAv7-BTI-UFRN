# 📡 Modos de execução

As configurações do [IOS (Internetwork Operating System®)](https://www.cisco.com/c/pt_br/support/docs/ios-nx-os-software/ios-software-releases-110/13178-15.html) são divididas em diferentes modos de execução identificadas a partir do prefixo da CLI:

| Prefixo          | Modo de execução                  | Comando              |
| ---------------- | --------------------------------- | -------------------- |
| >                | Modo usuário                      | -                    |
| #                | Modo privilegiado                 | enable               |
| (config)#        | Modo de configuração global       | conf t               |
| (config-if)#     | Modo de configuração de interface | int  [inteface name] |
| (config-router)# | Modo de configuração de roteador  | router [protocolo]   |

> Em qualquer um dos modos, para consultar a tabela de aplicações disponíveis, ensira o comando '?'

