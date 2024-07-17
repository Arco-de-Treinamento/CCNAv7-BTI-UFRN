# 🗝️ Controle de acesso do usuário

## 🗝️ Controle de acesso ao console 

O acesso aos dispositivos de rede da Cisco ocorre em diferentes níveis de controle, como visto anteriormente. Cada um deles possuí suas próprias limitações, sendo o mais baixo deles o modo usuário. 
Podemos impor limitações ao acesso desse modo, inserindo uma senha para acesso do console, como mostrado abaixo:

```bash
conf t
line console 0
password <password>
login
```

## 🗝️ Controle de acesso do modo privilegiado (enable)

O modo privilegiado de usuário permite o acesso e gerenciamento a parte das funcionalidades das interfaces e do próprio roteador, trazendo um risco significativo a segurança. Para habilitar uma senha para o modo privilegiado, deve-se executar o seguinte comando:

```bash
en
conf t
enable secret <password>
```

## 🗝️ Controle de acesso de conexões VTY (Telnet/SSH)

Ainda é possível adicionar a mesma camada de proteção para conexões via SSH ou Telnet. Para isso, devemos inserir uma senha para o acesso do sistema de rede.

```bash
conf t
line vty <first-line-number> <last-line-number>
password <password>
login
```

> **`<first-line-number>`** e **`<last-line-number>`** se referem ao intervalo de linhas de conexão que serão configurados. Este valor pode receber um numero inteiro de 0 a 15 , sendo normalmente configurado entre 0 e 15 para atribuir a todas as portas.

## 🎲 Criptografia de senhas

As senhas utilizadas para acesso ao console e aos modos privilegiado, privilegiado e VTY também podem ser criptografadas no sistema, para aumentar a segurança fornecida a rede.

```bash
conf t
service password-encryption
```

## ⚠️ Criando banners de aviso

O IOS da Cisco também permite a criação de pequenos avisos que serão exibidos ao usuário em caso de falha no acesso. Para isso, deve-se utilizar o comando **`banner`**:

```bash
conf t
banner motd #
<text>
#
```
