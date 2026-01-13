Disciplina: **ENE0011 – Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica**

Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**

---
# Laboratório 06 – OSPF (Open Shortest Path First)

## Descrição Geral

Este laboratório tem como objetivo a **configuração prática do protocolo OSPF (Open Shortest Path First)** em uma topologia com **múltiplas áreas**, permitindo ao estudante compreender o funcionamento de **protocolos de roteamento por estado de enlace**, bem como os conceitos de **área backbone, áreas não-backbone, ABR e convergência**.

A atividade enfatiza a organização hierárquica do OSPF e sua importância para **escalabilidade e desempenho** em redes IP de médio e grande porte.

**Dica:** Lista de Comandos | [Acessar](./labs/comandos.md) |

> **Observações** Este laboratório integra a disciplina Protocolos de Transporte e Roteamento e deve ser realizado após os laboratórios de Roteamento Estático e RIP, pois pressupõe compreensão prévia de tabelas de rotas e convergência.

---

## Objetivos

Ao final deste laboratório, o estudante deverá ser capaz de:

- Configurar corretamente o protocolo OSPF em múltiplos roteadores
- Compreender o papel da **Área 0 (backbone)**
- Implementar e identificar **múltiplas áreas OSPF**
- Verificar a formação de adjacências OSPF
- Analisar a tabela de rotas resultante da convergência
- Relacionar topologia lógica e organização por áreas

---

## Fundamentação Teórica

Os conceitos abordados neste laboratório incluem:

- Protocolos de roteamento por estado de enlace
- OSPF e algoritmo SPF (Dijkstra)
- Áreas OSPF e hierarquia
- Área backbone (Área 0)
- LSAs e convergência
- Métricas e custo de enlace

---

## Cenário de Simulação

A topologia de simulação é composta por **três roteadores** interligados em **Área 0**, cada um conectado a uma **área distinta**:

- **Área 0 (Backbone):** interliga os roteadores
- **Área 1:** rede 172.16.0.0/26
- **Área 2:** rede 10.0.0.0/29
- **Área 3:** rede 192.168.1.0/28

<img width="1327" height="795" alt="image" src="https://github.com/user-attachments/assets/00bdefd1-3932-4633-b55b-d3be6ea75094" />


Cada área contém uma LAN com um host.

---

## Ambiente de Laboratório

- Emulador de redes: **PNetLab**
- Dispositivos:
  - Roteadores (Cisco IOS ou FRRouting)
  - Hosts finais (PCs)
- Ferramentas de verificação:
  - `ping`
  - `traceroute`
  - `show ip route`
  - `show ip ospf neighbor`
  - `show ip ospf database`

---

## Endereçamento IP

| Equipamento | Interface | Endereço IP | Máscara | Área OSPF |
|------------|----------|-------------|---------|-----------|
| R1 | Se0/0/0 | 200.10.1.2 | 255.255.255.252 | 0 |
| R1 | Se0/0/1 | 200.10.1.5 | 255.255.255.252 | 0 |
| R1 | Fa0/0 | 172.16.0.1 | 255.255.255.192 | 1 |
| R2 | Se0/0/0 | 200.10.1.10 | 255.255.255.252 | 0 |
| R2 | Se0/0/1 | 200.10.1.1 | 255.255.255.252 | 0 |
| R2 | Fa0/0 | 10.0.0.1 | 255.255.255.248 | 2 |
| R3 | Se0/0/0 | 200.10.1.6 | 255.255.255.252 | 0 |
| R3 | Se0/0/1 | 200.10.1.9 | 255.255.255.252 | 0 |
| R3 | Fa0/0 | 192.168.1.1 | 255.255.255.240 | 3 |
| PC1 | Fa | 172.16.0.2 | 255.255.255.192 | 1 |
| PC2 | Fa | 10.0.0.2 | 255.255.255.248 | 2 |
| PC3 | Fa | 192.168.1.2 | 255.255.255.240 | 3 |

---

## Atividades Práticas

### Parte 1 – Preparação dos Roteadores

1. Resetar os roteadores para configuração padrão
```bash
R1> enable
R1# conf t
R1 (config)# config-register 0x2102
R1 (config)# end
R1# write erase
R1# reload
System configuration has been modified. Save? [yes/no]: no Proceed with reload? [confirm] <TECLE ENTER>

Aguarde o reboot.

Would you like to enter the initial configuration dialog? [yes/no]: no
```

2. Definir o nome de cada roteador

```bash
router> enable
router# conf t
Router (config)# hostname <nome roteador> R1 (config)# int fa0
R1 (config-if)# ip address <ip>  <mascara> R1 (config-if)# no shut
R1 (config-if)# exit
```
> **Observação**: os itens entre <> devem ser substituídos pelos valores reais, NÃO COLOCAR O <>

3. Configurar endereços IP nas interfaces FastEthernet e Seriais

```bash
R1 (config)# int se0
R1 (config-if)# ip address <ip>  <mascara> R1 (config-if)# clock rate 64000
R1 (config-if)# no shut
R1 (config-if)# exit
R1 (config)# int se1
R1 (config-if)# ip address <ip>  <mascara> R1 (config-if)# no shut
R1 (config-if)# exit
```


4. Ativar todas as interfaces (`no shutdown`)



5. Configurar `clock rate` nas interfaces DCE

---

### Parte 2 – Configuração do OSPF

Em cada roteador:

```bash
router ospf 1
network <id_rede> <wildcard> area <numero_area>
```
#### Comandos
```bash
R1 (config)# router ospf 1
R1 (config-router)# network <id de rede> <mascara> area <x>
R1 (config-router)# network <id de rede> <mascara> area <x>
R1 (config-router)# network <id de rede> <mascara> area <x>
```


- Utilizar o process ID 1
- Associar corretamente cada rede à sua área OSPF correspondente
- Garantir conectividade entre as áreas por meio da Área 0


### Parte 3 – Verificação da Convergência

- Verificar vizinhanças OSPF
- Analisar a tabela de rotas
- Testar conectividade entre hosts de áreas diferentes
- Confirmar que todas as redes são alcançáveis

### Questões para o Relatório

- Como o OSPF organiza a topologia em áreas?
- Qual o papel da Área 0 no funcionamento do protocolo?
- O que acontece se uma área não estiver conectada ao backbone?
- Como o OSPF calcula o melhor caminho?
- Quais vantagens o OSPF apresenta em relação ao RIP?

### Resultados Esperados

Ao final do laboratório, o estudante deverá compreender que:

- O OSPF utiliza uma visão completa da topologia local
- A hierarquia por áreas melhora escalabilidade
- A convergência é mais rápida que em protocolos vetor de distância
- A Área 0 é essencial para a troca de informações entre áreas

---

- OSPF é adequado para redes médias e grandes

