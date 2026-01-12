

Disciplina: **ENE0011 – Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica**

Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**

---
# Laboratório 03 – VLAN e Roteamento Estático

## Objetivo

Realizar o endereçamento de uma rede IP, configurar e verificar VLANs em switches Ethernet e implementar a interconexão entre VLANs por meio de roteamento estático em um roteador.

---

## Introdução Teórica

Este laboratório aborda conceitos fundamentais para o projeto e implementação de redes corporativas segmentadas, incluindo:

- Projeto de redes de computadores;
- Endereçamento IP;
- VLANs (*Virtual Local Area Networks*);
- Sub-redes IP;
- Roteamento estático;
- Interconexão entre VLANs (*Inter-VLAN Routing*).

---

## Descrição da Atividade

A atividade está dividida em quatro etapas principais.

---

### Etapa 1 – Descrição do Caso

Você foi designado para projetar e implementar a configuração dos equipamentos de uma rede corporativa destinada a interligar **50 dispositivos de rede**, incluindo:

- Estações de trabalho;
- Servidores;
- Impressoras de rede.

O prédio da empresa possui **3 andares** e **4 departamentos**:
- Administrativo  
- Diretoria  
- Marketing  
- Tecnologia  

A distribuição dos equipamentos por andar e departamento deve seguir a **Tabela 1 – Lista de Equipamentos**.

### Tabela 1 – Lista de Equipamentos

| Andar | Equipamento | Qtde | Departamento |
|:----:|-------------|:----:|--------------|
| 1º | Computador | 2 | Administrativo |
| 1º | Servidor | 1 | Administrativo |
| 1º | Impressora | 1 | Administrativo |
| 1º | Computador | 5 | Tecnologia |
| 1º | Servidor | 4 | Tecnologia |
| 1º | Impressora | 2 | Tecnologia |
| **1º** | **Total** | **15** | — |
| 2º | Computador | 2 | Administrativo |
| 2º | Servidor | 1 | Administrativo |
| 2º | Impressora | 1 | Administrativo |
| 2º | Computador | 2 | Diretoria |
| 2º | Impressora | 1 | Diretoria |
| 2º | Computador | 6 | Marketing |
| 2º | Servidor | 2 | Marketing |
| 2º | Impressora | 2 | Marketing |
| **2º** | **Total** | **17** | — |
| 3º | Computador | 1 | Administrativo |
| 3º | Computador | 3 | Diretoria |
| 3º | Servidor | 2 | Diretoria |
| 3º | Impressora | 2 | Diretoria |
| 3º | Computador | 5 | Marketing |
| 3º | Servidor | 1 | Marketing |
| 3º | Impressora | 1 | Marketing |
| 3º | Computador | 2 | Tecnologia |
| 3º | Impressora | 1 | Tecnologia |
| **3º** | **Total** | **18** | — |



Considere ainda que:

- A maior parte do tráfego é **intradepartamental**;
- VLANs devem ser utilizadas para separar os domínios de broadcast;
- Deve existir **uma VLAN exclusiva para gerência** dos dispositivos de rede;
- Não há restrições quanto à disposição física das portas dos switches;
- Existe uma sala de telecomunicações em cada andar.

---

### Etapa 2 – Projeto da Rede

Uma possível solução de projeto considera:

- Um **switch por andar**, visto que todos possuem menos de 24 dispositivos;
- Interligação entre switches por meio de **uplinks Gigabit Ethernet em modo trunk (IEEE 802.1Q)**;
- Um roteador com uma interface Ethernet;
- Cinco VLANs:
  - Quatro VLANs departamentais;
  - Uma VLAN de gerência.

#### Plano de Endereçamento IP

| VLAN | Departamento     | Rede IP        | Gateway         | VLAN ID |
|----:|------------------|----------------|-----------------|--------:|
| 1   | Gerência         | 10.0.10.0/24   | 10.0.10.254     | 1 |
| 2   | Administração    | 10.0.20.0/24   | 10.0.20.254     | 2 |
| 3   | Diretoria        | 10.0.30.0/24   | 10.0.30.254     | 3 |
| 4   | Marketing        | 10.0.40.0/24   | 10.0.40.254     | 4 |
| 5   | Tecnologia       | 10.0.50.0/24   | 10.0.50.254     | 5 |

---

### Etapa 3 – Configuração e Verificação de VLANs em Switches

1. Inicie o **Cisco Packet Tracer**.
2. Abra o arquivo de topologia fornecido. [Baixar arquivo do laboratório](https://drive.google.com/file/d/1VqMa11OYRS9bMHEfkWrAHtE9zrmCbiiz/view?usp=sharing)

3. Configure os switches para:
   - Criar as VLANs;
   - Associar as portas de acesso às VLANs corretas;
   - Configurar enlaces trunk entre os switches.

Também deve ser configurado o **endereço IP de gerência** em cada switch, utilizando a VLAN 1, bem como o *default gateway*.

<img width="766" height="567" alt="image" src="https://github.com/user-attachments/assets/44657322-8eaa-4457-a598-d7e3bb2fe86d" />


#### Configuração de VLANs no Sw1:


```bash
Sw1> enable
Sw1# conf t
Enter configuration commands, one per line. End with CNTL/Z. Sw1(config)# interface FastEthernet0/1
Sw1(config-if)# switchport access vlan 2
% Access VLAN does not exist. Creating vlan 2
Sw1(config-if)# switchport mode access
Sw1(config-if)# interface FastEthernet0/5
Swl (config–if)# switchport access vlan 5
% Access VLAN does not exist. Creating vlan 5
Sw1(config-if)# switchport mode access
Sw1(config-if)# interface FastEthernet0/24
Sw1(config-if)# switchport access vlan 1
Sw1(config-if)# switchport mode access
Sw1# sh vlan brief
VLAN Name  
 
Status 
 
Ports
1 default  
 
active 
 
Fa0/2, Fa0/3 , Fa0/4, Fa0/6
Fa0/7, Fa0/8, Fa0/9, Fa0/10 Fa0/1l, Fa0/12 , Fa0/13, Fa0/14 Fa0/15, Fa0/16, Fa0/17, Fa0/18 Fa0/19, Fa0/20, Fa0/21, Fa0/22 Fa0/23, Fa0/24, Gi0/l, Gi0/2
2 VLAN0002  
 
 
active 
Fa0/1
5 VLAN0005  
 
 
active 
Fa0/5
1002 fddi–default 
 
act/unsup
1003 token-ring-default 
act/unsup
1004 fddinet-default  
act/unsup
1005 trnet-default 
 
act/ un sup
Swl#
```
#### Configuração de VLANs no Sw2 :
#### Configuração de VLANs no Sw3 :

#### Configuração IP para gerência dos switches:

```bash
Sw1> enable
Sw1# conf t
Enter configuration commands, one per line. End with CNTL/Z.
Sw1(config)# interface vlan 1
w1(config-if)# ip address 10.0.10.1 255.255.255.0
Sw1(config-if)# no shutdown Sw1(config-if)#
%LDXX - Interface vlan 1, changed state to up Sw1(config-if)#ex
Sw1(config)#ip default-gateway 10.0.10.254
Sw2> enable
Sw2# conf t
Enter configuration commands, one per line. End with CNTL/Z.
Sw2(config)# interface vlan 1
Sw2(config-if)# ip address 10.0.10.2 255.255.255.0
Sw2(config-if)# no shutdown Sw2(config-if)#
%LDXX - Interface vlan 1, changed state to up Sw2(config-if)#ex
Sw2(config)#ip default-gateway 10.0.10.254
Sw3> enable
Sw3# conf t
Enter configuration commands, one per line. End with CNTL/Z.
Sw3(config)# interface vlan 1
Sw3(config-if)# ip address 10.0.10.3 255.255.255.0
Sw3(config-if)# no shutdown Sw3(config-if)#
%LDXX - Interface vlan 1, changed state to up Sw3(config-if)#ex
Sw3(config)#ip default-gateway 10.0.10.254
```

Após a configuração:

Caso executasse um comando ping de qualquer computador para outro em uma mesma VLAN, você obteria sucesso? Para responder a essa pergunta, vamos fazer o seguinte teste:

- Execute o comando ping de Adm3 (IP: 10.0.20.9) para Adm2 (IP: 10.0.20.5), ambos da VLAN 2.
  - Por que o ping não funcionou? Quais os comandos necessários nos 3 switches?
- Caso você executasse um comando ping de qualquer computador para outro em outra VLAN, você obteria sucesso? Para responder a essa pergunta, vamos fazer o seguinte teste:
  - Execute o comando ping de Adm3 (IP: 10.0.20.9) da VLAN2 para Dir3 (IP: 10.0.30.4) da VLAN3.
  - Por que o ping não funcionou?
---

### Etapa 4 – Interconexão de VLANs via Roteador

Nesta etapa, será implementado o **roteamento entre VLANs** utilizando subinterfaces no roteador.

#### Configuração de Switches e Roteador para Roteamento entre VLANs

O roteador deve ficar localizado no **primeiro andar**, conectado diretamente ao **switch Sw1** por meio de uma interface **Fast Ethernet**, uma vez que o equipamento não possui interface Gigabit Ethernet.

Essa interface deve ser configurada com **cinco subinterfaces**, de forma a permitir o roteamento de tráfego entre as **cinco VLANs** existentes. Para cada subinterface, deve ser configurado um **endereço IP** correspondente ao *default gateway* da respectiva VLAN, com a devida especificação do **VLAN ID**, conforme apresentado na Tabela 3.

#### Tabela 3 – Endereçamento das Subinterfaces do Roteador

| Subinterface | Endereçamento IP | VLAN ID |
|--------------|------------------|--------:|
| E0/1.1 | 10.0.10.254/24 | 1 |
| E0/1.2 | 10.0.20.254/24 | 2 |
| E0/1.3 | 10.0.30.254/24 | 3 |
| E0/1.4 | 10.0.40.254/24 | 4 |
| E0/1.5 | 10.0.50.254/24 | 5 |

#### Configuração do Roteador 

```bash
Router> en
Router# conf t
Enter configuration commands, one per line. End with CNTL/Z.
Router(config)# interface FastEthernet0/0.1
Router(config-subif)# description VLAN1
Router(config-subif)# encapsulation dot1q 1
Router(config-subif)# ip address 10.0.10.254 255.255.255.0
Router(config-subif)#
%LDXX - Line protocol on Interface FastEthernet0/0, changed state to up
%LDXX - Line protocol on Interface FastEthernet0/0.1, changed state to up
Router(config-subif)# interface FastEthernet0/0.2
Router(config-subif)# description VLAN2
Router(config-subif)# encapsulation dot1q 2
```

Após a configuração:

- É preciso fazer alguma configuração da porta do switch para o roteador? Qual?
- Verifique a conectividade entre dispositivos de VLANs distintas;
- Analise a necessidade de configuração da porta do switch conectada ao roteador.

---

## Questões para o Relatório

O relatório deve documentar detalhadamente:

a) As razões para falhas de conectividade observadas nas etapas iniciais;  
b) As configurações necessárias nos switches para comunicação intra-VLAN;  
c) As configurações necessárias para comunicação inter-VLAN;  
d) A finalidade do protocolo **IEEE 802.1Q** e as vantagens de sua utilização.

Todos os comandos executados devem ser registrados e explicados.

---

## Equipamentos e Materiais

- Computadores do laboratório;
- Software **Cisco Packet Tracer**.

---

## Bibliografia

1. KUROSE, J. F.; ROSS, K. W.  
   *Computer Networks: A Top-Down Approach*. 5ª ed. Pearson Addison-Wesley, 2009.

2. COMER, D. E.  
   *Internetworking with TCP/IP – Volume I*. 5ª ed. Prentice Hall, 2005.

3. Sites diversos sobre **VLANs** e **Roteamento Estático** na Internet.

---

> Este laboratório possui finalidade exclusivamente acadêmica, devendo as configurações e técnicas apresentadas serem utilizadas apenas em ambientes autorizados e controlados.
