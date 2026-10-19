# Roteiro do Experimento 04 - VLAN e Roteamento Estático

- **Instituição:** Universidade de Brasília (UnB)
- **Departamento:** Departamento de Engenharia Elétrica (ENE)
- **Curso:** Engenharia de Redes de Comunicação
- **Disciplina:** ENE0011 - Laboratório de Redes
- **Professor responsável:** Prof. Dr. Laerte Peotta de Melo
- **Tema:** Segmentação de camada 2 com VLANs, troncos IEEE 802.1Q e roteamento inter-VLAN

---

## 1. Objetivo

Realizar o endereçamento de uma rede IP, configurar e verificar VLANs em switches Ethernet e implementar a interconexão entre VLANs por meio de roteamento em um roteador (*router-on-a-stick*).

Ao final da atividade, o estudante deverá ser capaz de:

- projetar um plano de VLANs e de endereçamento IP a partir de uma lista de equipamentos;
- criar VLANs, associar portas de acesso e configurar enlaces tronco IEEE 802.1Q;
- configurar o endereço de gerência dos switches em uma VLAN dedicada;
- configurar subinterfaces no roteador para o roteamento entre VLANs;
- diagnosticar falhas de conectividade intra-VLAN e inter-VLAN.

---

## 2. Fundamentação Teórica

Este laboratório aborda conceitos fundamentais para o projeto e a implementação de redes corporativas segmentadas:

- projeto de redes de computadores e endereçamento IP;
- VLANs (*Virtual Local Area Networks*) e sub-redes IP;
- roteamento estático e interconexão entre VLANs (*Inter-VLAN Routing*).

### 2.1 VLANs

Uma VLAN é um domínio de *broadcast* lógico definido no switch: quadros de uma VLAN não são encaminhados a portas de outra VLAN. Cada VLAN corresponde, em geral, a uma sub-rede IP distinta, e a comunicação entre VLANs exige um dispositivo de camada 3.

### 2.2 Enlaces de Acesso e Tronco (IEEE 802.1Q)

- **Porta de acesso:** pertence a uma única VLAN e envia/recebe quadros sem *tag* (`switchport mode access`).
- **Porta tronco:** transporta várias VLANs entre switches ou até o roteador. O padrão IEEE 802.1Q insere um *tag* de 4 bytes no quadro Ethernet, com o identificador de VLAN (VID, 12 bits, valores de 1 a 4094). Apenas a VLAN nativa trafega sem *tag* (`switchport mode trunk`).

### 2.3 Roteamento Inter-VLAN com Subinterfaces

Um único enlace tronco liga o switch ao roteador. No roteador, a interface física é dividida em subinterfaces, cada uma associada a uma VLAN por `encapsulation dot1q <VID>` e configurada com o endereço do *gateway* da VLAN.

### 2.4 Aspectos de Segurança

- A VLAN 1 é o padrão de fábrica e transporta o tráfego de controle (CDP, STP) e a VLAN nativa. Por isso, a gerência deste experimento utiliza uma **VLAN dedicada (VLAN 99)** e as VLANs de usuário não utilizam a VLAN 1.
- Portas de acesso não utilizadas devem ser desativadas, e as portas de acesso devem ter o modo fixado (`switchport mode access`) para evitar a negociação automática de tronco (*Dynamic Trunking Protocol* - DTP) por um dispositivo não autorizado.

---

## 3. Topologia e Cenário

### 3.1 Descrição do Caso

Você foi designado para projetar e implementar a configuração dos equipamentos de uma rede corporativa destinada a interligar **50 dispositivos de rede**, incluindo:

- estações de trabalho;
- servidores;
- impressoras de rede.

O prédio da empresa possui **3 andares** e **4 departamentos**: Administrativo, Diretoria, Marketing e Tecnologia. A distribuição dos equipamentos por andar e departamento segue a **Tabela 1**.

Considere ainda que:

- a maior parte do tráfego é **intradepartamental**;
- VLANs devem ser utilizadas para separar os domínios de *broadcast*;
- deve existir **uma VLAN exclusiva para gerência** dos dispositivos de rede;
- não há restrições quanto à disposição física das portas dos switches;
- existe uma sala de telecomunicações em cada andar.

### 3.2 Tabela 1 - Lista de Equipamentos

| Andar | Equipamento | Qtde | Departamento |
|:----:|-------------|:----:|--------------|
| 1º | Computador | 2 | Administrativo |
| 1º | Servidor | 1 | Administrativo |
| 1º | Impressora | 1 | Administrativo |
| 1º | Computador | 5 | Tecnologia |
| 1º | Servidor | 4 | Tecnologia |
| 1º | Impressora | 2 | Tecnologia |
| **1º** | **Total** | **15** | - |
| 2º | Computador | 2 | Administrativo |
| 2º | Servidor | 1 | Administrativo |
| 2º | Impressora | 1 | Administrativo |
| 2º | Computador | 2 | Diretoria |
| 2º | Impressora | 1 | Diretoria |
| 2º | Computador | 6 | Marketing |
| 2º | Servidor | 2 | Marketing |
| 2º | Impressora | 2 | Marketing |
| **2º** | **Total** | **17** | - |
| 3º | Computador | 1 | Administrativo |
| 3º | Computador | 3 | Diretoria |
| 3º | Servidor | 2 | Diretoria |
| 3º | Impressora | 2 | Diretoria |
| 3º | Computador | 5 | Marketing |
| 3º | Servidor | 1 | Marketing |
| 3º | Impressora | 1 | Marketing |
| 3º | Computador | 2 | Tecnologia |
| 3º | Impressora | 1 | Tecnologia |
| **3º** | **Total** | **18** | - |

### 3.3 Projeto da Rede

Uma possível solução de projeto considera:

- um **switch por andar**, visto que todos possuem menos de 24 dispositivos;
- interligação entre switches por meio de **uplinks em modo tronco (IEEE 802.1Q)**;
- um roteador com uma interface Fast Ethernet conectada ao Sw1;
- cinco VLANs: quatro departamentais e uma de gerência.

#### 3.3.1 Plano de Endereçamento IP

| VLAN ID | Nome | Departamento | Rede IP | Gateway |
|-------:|------|--------------|---------|---------|
| 2 | ADMINISTRACAO | Administração | 10.0.20.0/24 | 10.0.20.254 |
| 3 | DIRETORIA | Diretoria | 10.0.30.0/24 | 10.0.30.254 |
| 4 | MARKETING | Marketing | 10.0.40.0/24 | 10.0.40.254 |
| 5 | TECNOLOGIA | Tecnologia | 10.0.50.0/24 | 10.0.50.254 |
| 99 | GERENCIA | Gerência | 10.0.10.0/24 | 10.0.10.254 |

#### 3.3.2 Endereçamento das Subinterfaces do Roteador

| Subinterface | Endereço IP | VLAN ID |
|--------------|-------------|--------:|
| Fa0/0.99 | 10.0.10.254/24 | 99 |
| Fa0/0.2 | 10.0.20.254/24 | 2 |
| Fa0/0.3 | 10.0.30.254/24 | 3 |
| Fa0/0.4 | 10.0.40.254/24 | 4 |
| Fa0/0.5 | 10.0.50.254/24 | 5 |

<img width="766" height="567" alt="Topologia do Experimento 04 no Packet Tracer" src="https://github.com/user-attachments/assets/44657322-8eaa-4457-a598-d7e3bb2fe86d" />

O roteador fica no **primeiro andar**, conectado diretamente ao **Sw1** por uma interface Fast Ethernet, pois o equipamento não possui interface Gigabit Ethernet.

---

## 4. Roteiro Prático Passo a Passo

### 4.1 Preparação

1. Inicie o **Cisco Packet Tracer**.
2. Abra o arquivo de topologia fornecido: [Baixar arquivo do laboratório](./topologias/experimento_04_nao_configurado.pkt).

### 4.2 Criação de VLANs e Portas de Acesso nos Switches

Configure em cada switch: as VLANs, a associação das portas de acesso às VLANs corretas conforme a Tabela 1 e o modo de acesso das portas. O exemplo a seguir mostra o padrão para o Sw1; repita o procedimento no Sw2 e no Sw3, adaptando as portas.

```cisco
Sw1> enable
Sw1# configure terminal
Sw1(config)# vlan 99                       ! cria a VLAN de gerência
Sw1(config-vlan)# name GERENCIA            ! nome da VLAN
Sw1(config-vlan)# exit
Sw1(config)# interface FastEthernet0/1
Sw1(config-if)# switchport mode access     ! fixa a porta como acesso
Sw1(config-if)# switchport access vlan 2   ! associa à VLAN 2 (cria a VLAN se não existir)
Sw1(config-if)# interface FastEthernet0/5
Sw1(config-if)# switchport mode access
Sw1(config-if)# switchport access vlan 5
Sw1(config-if)# end
Sw1# show vlan brief                       ! confere VLANs e portas
```

### 4.3 Configuração IP de Gerência dos Switches

O endereço de gerência fica na VLAN 99 (SVI, *Switch Virtual Interface*). Os IPs são `10.0.10.1`, `10.0.10.2` e `10.0.10.3` para Sw1, Sw2 e Sw3.

```cisco
Sw1(config)# interface vlan 99
Sw1(config-if)# ip address 10.0.10.1 255.255.255.0   ! IP de gerência do Sw1
Sw1(config-if)# no shutdown
Sw1(config-if)# exit
Sw1(config)# ip default-gateway 10.0.10.254          ! gateway para alcançar outras redes
```

Repita no Sw2 (`10.0.10.2`) e no Sw3 (`10.0.10.3`).

### 4.4 Teste Sem Enlaces Tronco

Com as VLANs e os IPs configurados, mas sem os troncos:

- Execute `ping` de Adm3 (`10.0.20.9`) para Adm2 (`10.0.20.5`), ambos da VLAN 2. Por que o `ping` não funcionou? Quais comandos são necessários nos 3 switches?
- Execute `ping` de Adm3 (`10.0.20.9`, VLAN 2) para Dir3 (`10.0.30.4`, VLAN 3). Por que o `ping` não funcionou?

Configure os troncos entre os switches e o tronco do Sw1 para o roteador (a VLAN 99 e as VLANs 2 a 5 devem ser permitidas) e repita o primeiro teste.

### 4.5 Interconexão de VLANs via Roteador

Configure a interface Fast Ethernet do roteador com **cinco subinterfaces**, uma por VLAN, cada uma com o *gateway* da respectiva VLAN (Seção 3.3.2).

```cisco
Router> enable
Router# configure terminal
Router(config)# interface FastEthernet0/0
Router(config-if)# no shutdown                       ! ativa a interface física
Router(config-if)# interface FastEthernet0/0.99
Router(config-subif)# description GERENCIA
Router(config-subif)# encapsulation dot1q 99         ! associa a subinterface à VLAN 99
Router(config-subif)# ip address 10.0.10.254 255.255.255.0
Router(config-subif)# interface FastEthernet0/0.2
Router(config-subif)# description ADMINISTRACAO
Router(config-subif)# encapsulation dot1q 2
Router(config-subif)# ip address 10.0.20.254 255.255.255.0
```

Repita para as VLANs 3, 4 e 5. Em seguida:

- Verifique se é necessária alguma configuração na porta do switch conectada ao roteador e qual.
- Verifique a conectividade entre dispositivos de VLANs distintas.

---

## 5. Verificação e Testes

| Comando / ação | Equipamento | O que observar |
| :--- | :---: | :--- |
| `show vlan brief` | Switches | VLANs 2, 3, 4, 5 e 99 `active`, com as portas de acesso corretas |
| `show interfaces trunk` | Switches | Portas tronco em `trunking` e VLANs permitidas e ativas |
| `show ip interface brief` | Switches e roteador | SVI `Vlan99` e subinterfaces em `up/up` com os IPs do plano |
| `show ip route` | Roteador | Rotas `C` para as cinco redes |
| `ping` entre hosts da mesma VLAN | Hosts | Resposta (depende dos troncos) |
| `ping` entre hosts de VLANs distintas | Hosts | Resposta (depende das subinterfaces) |
| `ping 10.0.10.1` a partir de uma VLAN de usuário | Hosts | Resposta (comprova `ip default-gateway`) |

Exemplo ilustrativo de trecho do `show vlan brief`:

```text
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/2, Fa0/3, Fa0/4, Fa0/6
2    VLAN0002                         active    Fa0/1
5    VLAN0005                         active    Fa0/5
99   GERENCIA                         active
```

- **VLAN / Name:** identificador e nome (o nome padrão é `VLANnnnn` quando não foi definido).
- **Status:** `active` indica VLAN criada e utilizável.
- **Ports:** portas de **acesso** da VLAN. As portas de tronco não aparecem nessa coluna; portas não associadas permanecem na VLAN 1.

---

## 6. Questões para Análise

O relatório deve documentar detalhadamente:

1. As razões para as falhas de conectividade observadas nas etapas iniciais (intra-VLAN e inter-VLAN).
2. As configurações necessárias nos switches para a comunicação intra-VLAN.
3. As configurações necessárias para a comunicação inter-VLAN.
4. A finalidade do padrão **IEEE 802.1Q** e as vantagens de sua utilização.
5. Por que a gerência dos equipamentos foi colocada em uma VLAN dedicada, e não na VLAN 1?
6. O que ocorre com os quadros de uma VLAN que não esteja na lista de VLANs permitidas do tronco?

Todos os comandos executados devem ser registrados e explicados.

---

## 7. Critérios de Avaliação

A avaliação será realizada com base na seguinte rubrica, totalizando exatamente 10,0 pontos:

| Critério de Avaliação | Pontuação Máxima |
| :--- | :---: |
| **Plano de VLANs e endereçamento:** Coerente com a Tabela 1 e sem sobreposição | 1,5 |
| **VLANs e portas de acesso:** Criação e associação corretas nos três switches | 2,0 |
| **Troncos e gerência:** Troncos 802.1Q, SVI da VLAN 99 e *default gateway* | 2,0 |
| **Roteador:** Subinterfaces e roteamento inter-VLAN funcionando | 2,0 |
| **Testes:** Evidências legíveis das verificações da Seção 5 | 1,0 |
| **Respostas às questões:** Análise fundamentada das questões da Seção 6 | 1,5 |
| **Total** | **10,0** |

---

## 8. Entregáveis

1. Plano de VLANs e de endereçamento.
2. Arquivo `.pkt` configurado.
3. Saídas de `show vlan brief`, `show interfaces trunk` (switches) e `show ip interface brief` e `show ip route` (roteador).
4. Capturas dos testes de `ping` intra-VLAN e inter-VLAN, antes e depois das correções.
5. Respostas às 6 questões da Seção 6, com os comandos registrados e explicados.

---

## 9. Conclusão

Neste experimento, a rede corporativa foi segmentada em VLANs, interligada por troncos IEEE 802.1Q e roteada por subinterfaces. As falhas observadas antes dos troncos e do roteador evidenciaram que VLANs isolam domínios de *broadcast* e que a comunicação entre elas depende de um dispositivo de camada 3. A adoção de uma VLAN de gerência dedicada aplica uma prática de endurecimento reaproveitada nos experimentos seguintes.

---

[← Anterior: Experimento 03](03_roteiro.md) | [Índice Geral (README)](../README.md) | [Próximo: Experimento 05 →](05_roteiro.md)
