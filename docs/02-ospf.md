# 📡 Utilizando e configurando uma Rede OSPFv2

## 🖥️ Inicializando um processo OSPF

Em modo de configuração global execute o seguinte comando:

```bash
conf t
router ospf <ospf-id>
```

Onde **`ospf-id`** é um inteiro entre 1 e 65.535 e é selecionado pelo operador.

## 🖥️ Configurando uma interface loopback

Ao invés de depender de uma interface física, o ID do roteador pode ser atribuído a uma interface de loopback. Nesse caso, o roteador utilizará o endereço IPv4 como ID do roteador.

Em modo de configuração de interface, considerando que o roteador ainda tenha um ID, execute o seguinte comando:

```bash
interface Loopback 1
ip address 1.1.1.1 255.255.255.255
end
```

> O OSPF não precisa ser habilitado em uma interface para que ela seja eleita como o ID do roteador.

## 🖥️ Configurando um ID de roteador explicitamente

O ID de um roteador é um valor de 32 bits representado com um endereço IPv4. O ID do roteador serve como um identificador único para o roteador OSPF e será incluído em todos os pacotes, sendo necessário para participar de um domínio OSPF.

Em modo de configuração global, execute o seguinte comando:

```bash
router ospf <ospf-id>
router-id 1.1.1.1
end
```

## 🖥️ Modificar um ID de roteador

Quando configurado, um roteador OSPF ativo só permite alterar o ID após recarregar o processo OSPF ou redefini-lo.

Em modo de configuração global, execute o seguinte comando:

```bash
router ospf <ospf-id>
router-id 1.1.1.1
end

clear ip ospf process
```

> É aconselhavel optar pelo método de apagar o processo OSPF para redefinir o ID do roteador.

## 📡 Configurando o protocolo OSPF

Redes OSPF são redes ponto a ponto e devem ter as suas interfaces pertencentes especificadas. No OSPFv2 essa tarefa é realizada utilizando o comando network, com a sintaxe:

```bash
network <network-adress> <wildcard-mask> area <area-id>
```

Onde **`network-adress`** é o endereço IPv4 e **`wildcard-mask`** é a máscara de rede, sendo **`area-id`** o ID da área.

> A configuração do OSPF também pode ser feita diretamente na interface com o comando **`ip ospf`**.

Observe que a configuração serve para anunciar a rede quais interfaces participarão do OSPF.

### 🎭 A Wildcard Mask

A mascara de rede é normalmente o inverso da máscara de sub-rede configurada numa interface, utilizada no processo para identificar as interfaces que estão participando do OSPF. Por exemplo, caso a máscara de sub-rede seja **`255.255.255.0`**, a wildcard mask é **`0.0.0.255`**.

A wildcard pode ser obtida da através da subtração da máscara de sub-rede por 255.255.255.255:

$$
\begin{align*}
255.255.255.255 \\
- \quad 255.255.255.192 \\
\hline
000.000.000.063 \\
\end{align*}
$$

### 🎭 Máscara de sub-rede

A máscara de sub-rede é utilizada para indicar na rede quantos bits do endereço ip serão utilizados na identificação da rede, onde os bits restantes identificam os hosts. Por exemplo, para um endereço ip **`192.168.0.50`**, com uma máscara de sub-rede **`255.255.255.0`**, podemos identificar:

- **`192.168.0`** - Identifica a rede
- **`.50`** - Identifica o host

### 🔢 Obtendo a máscara de sub-rede a partir da notação CIDR

O **encaminhamento entre domínios sem classificação (CIDR)** é um padrão que permite os roteadores encaminharem pacotes de dados para os dispositivos com no sufixo CIDR, que representa os bits significativos na máscara de sub-rede. Por exemplo, tome o CIDR **`10.0.0.1/29`**. Nesse caso, temos o endereço **`10.0.0.1`** com os 29 primeiros bits utilizados para identificar a rede.

A partir do número de bits da identificação da rede, podemos obter o número de bits utilizados para identificar o host subtraindo de 32, com 3 bits para endereçamento.

Nesse caso, a máscara de sub-rede é dada por **`255.255.255.248`** **(2³)**.

### 0️⃣ Máscara Quad Zero

A máscara quad zero é uma alternativa ao uso da máscara wildcard na configuração do OSPF dada por **`0.0.0.0`**. Ao utilizar uma quad zero se perde a necessidade de calcular a wildcard, atribuindo uma interface individual para uma área OSPF, ao invés de um range de interfaces.

```bash
network <network-adress> 0.0.0.0 area <area-id>
```

A máscara quad zero também pode ser utilizada em conjunto com uma **all one (255.255.255.255)** para incluir todas as interfaces disponíveis em uma única área, com um único comando. Nesse caso se utiliza a sintaxe:

```bash
network 0.0.0.0 255.255.255.255 area <area-id>
```

### ❌ Removendo uma interface do OSPF

A remoção de uma interface do OSPF é feita com o comando **no**, que reverte o comando executado. A sintaxe é semelhante ao comando padrão:

```bash
no network <network-adress> <wildcard-mask> area <area-id>
```

### 📡 Configurando o OSPF diretamente na interface

Uma alternativa ao comando **network** é a configuração do protocolo OSPF na própria interface. Essa configuração pode ser feita diretamente com o comando **ip ospf**, após entrar na interface. Nesse caso, será necessário apenas indicar a **`area-id`**.

```bash
interface <interface-name> 
ip ospf area <area-id>
```

## 📡 Interface passiva

Por padrão, todas as interfaces habilitadas no protocolo OSPF podem receber e enviar mensagens na rede. Essa configuração pode trazer transtornos para a rede uma vez que consome banda desnecessariamente e aumenta os riscos de segurança.

Para mitigar os danos é apropriado habilitar uma interface como passiva em situações que ofereçam mais vulnerabilidade.

```bash
router ospf <ospf-id>
passive-interface <interface-name>
end
```

A configuração pode ser revisada com:

```bash
show ip protocols
```

Em complemento, ainda é possível configurar todas as interfaces disponíveis como passivas através do comando **default**, com a seguinte sintaxe:

```bash
passive-interface default
```

## 📡 OSPF Ponto-a-Ponto

Em situações onde temos apenas dois roteadores, numa conexão ponto-a-ponto entre R1 e R2, o uso da eleição de um DR e BDR se torna desnecessária. Para otimizar a rede e desativar o processo de eleição de DR/BDR é recomendado configurar interface como ponto-a-ponto:

```bash
interface <interface-name>
ip ospf network point-to-point
end
```

## 📡 Redes OSPF de multiacesso

Outro tipo comum de conexão entre switchs é a rede de multiacesso, do tipo broadcast, onde todos os dispositivos na rede enxergam todas as conexões broadcast.

Em casos como esse, o protocolo OSPF elege um **DR** e um **BDR** para gerenciar a rede e os LSAs (Link-State Advertisements).

> O DR é responsável por coletar e destribuir LSAs na rede. Cao o DR falhe, o BDR irá assumir toda a demanda da rede.

> LSAs são a descrição do estado local de um roteador ou rede, incluindointerfaces e adjacências. Um conjunto de LSAs formam a base de dados topológicas do protocolo.

