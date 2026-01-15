Disciplina: **ENE0011 – Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica**

Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**

---
## Experimento 07 – Roteamento OSPF (Áreas, DR/BDR e Enlaces Virtuais)


##  Descrição Geral

Este laboratório tem como objetivo o **planejamento, configuração e análise do protocolo OSPF (Open Shortest Path First)** em uma topologia hierárquica com **múltiplas áreas**, explorando conceitos fundamentais como **vizinhança, adjacência, eleição de DR/BDR, LSAs, roteadores de borda de área (ABR)** e **enlaces virtuais (Virtual Links)**.

O experimento é dividido em **duas partes complementares**.

**Dica:** Lista de Comandos [Acessar](./comandos.md) 

> **Observações:**  Este laboratório faz parte da disciplina Protocolos de Transporte e Roteamento e deve ser executado após os laboratórios de Roteamento Estático, RIP e OSPF primeira parte, pois pressupõe conhecimento prévio de tabelas de rotas e convergência.

---

##  Objetivos

Ao final deste laboratório, o estudante deverá ser capaz de:

- Planejar uma topologia OSPF hierárquica
- Configurar OSPF em múltiplas áreas
- Identificar e analisar vizinhanças e adjacências OSPF
- Compreender o processo de eleição de DR e BDR
- Interpretar anúncios de estado de enlace (LSAs)
- Configurar e avaliar enlaces virtuais OSPF
- Analisar impactos do backbone OSPF na tabela de rotas

---

##  Fundamentação Teórica

Este laboratório aborda os seguintes conceitos, conforme os anexos originais:

- Protocolo HELLO e formação de vizinhança
- Adjacências OSPF
- OSPF em redes multiacesso
- Eleição de **Designated Router (DR)** e **Backup Designated Router (BDR)**
- Tipos de roteadores OSPF:
  - Internal Router
  - Area Border Router (ABR)
  - Backbone Router
  - Autonomous System Boundary Router (ASBR)
- Tipos de LSAs (Router, Network e Summary)
- Hierarquia de áreas OSPF
- Enlaces Virtuais (Virtual Links)

---

##  Topologia do Laboratório

A topologia utilizada é composta por:

### Áreas OSPF
- **Área 0 (Backbone)**
- **Área 100 – Engenharia Elétrica**
- **Área 200 – CPD**
- **Área 300 – Engenharia da Computação**

### Redes envolvidas (exemplo)
- 172.16.5.0/24  
- 172.16.10.0/24  
- 172.16.20.0/24  
- 172.16.30.0/24  
- 172.16.40.0/24  
- 172.16.50.0/24  
- 172.16.60.0/24  
- 192.168.60.0/24  
- 192.168.73.0/24  
- 192.168.74.0/24  

<img width="989" height="546" alt="image" src="https://github.com/user-attachments/assets/23b41bd8-10e5-467a-b0c2-3310eae7780f" />




Cada área possui pelo menos uma LAN com hosts finais.

---

##  Ambiente de Laboratório

- Emulador: **PNetLab**
- Roteadores: Cisco IOS ou FRRouting
- Hosts finais: PCs Linux
- Ferramentas de análise:
  - `ping`
  - `traceroute`
  - `show ip route`
  - `show ip ospf`
  - `show ip ospf neighbor`
  - `show ip ospf interface`
  - `show ip ospf database`
  - `debug ip ospf adj`

---

##  Atividades Práticas

###  PARTE I – Configuração Básica do OSPF

1. Montar a topologia conforme o diagrama fornecido
2. Configurar o endereçamento IP em todas as interfaces
3. Ativar o OSPF em todos os roteadores
4. Associar corretamente cada interface à sua **área OSPF**
5. Garantir que todas as interfaces estejam ativas
6. Verificar:
   - tabelas de roteamento
   - vizinhos OSPF
   - adjacências formadas

### Análises solicitadas
- Identificar o DR e o BDR em redes multiacesso
- Interpretar mensagens HELLO
- Analisar LSAs de roteador e de rede

---

## PARTE II – OSPF Hierárquico e Backbone

1. Confirmar conectividade entre todas as áreas via **Área 0**
2. Identificar quais roteadores atuam como:
   - Internal Router
   - ABR
   - Backbone Router
3. Comparar tabelas de roteamento entre roteadores internos e de borda
4. Avaliar o papel do backbone na propagação das rotas

---

## PARTE III – Enlaces Virtuais (Virtual Links)

### Motivação
Os enlaces virtuais são utilizados para:
- Conectar áreas não contíguas ao backbone
- Restaurar conectividade lógica com a Área 0

### Configuração (exemplo)

```bash
interface loopback 0
 ip address 1.1.1.1 255.255.255.0
router ospf 1
 router-id 1.1.1.1
 area 100 virtual-link 3.3.3.3
```
#### Criar interfaces loopback para definição explícita de Router-ID

1. Configurar virtual links entre ABRs
2. Verificar a criação da interface virtual OSPF
3. Analisar alterações nas tabelas de roteamento
   
**Discussão obrigatória**

- Quais problemas existem no uso de enlaces virtuais?
- Por que virtual links não são recomendados como solução permanente?

## Questões para o Relatório

1. Como o OSPF organiza a topologia em áreas?
2. Qual o papel do DR e do BDR em redes multiacesso?
3. Como os LSAs contribuem para a convergência da rede?
4. Qual a importância da Área 0 no OSPF?
5. Em que situações enlaces virtuais são necessários?
6. Compare este cenário com o da primeira aula (backbone simples), justificando com tabelas de roteamento.

## Resultados Esperados

Ao final do laboratório, o estudante deverá compreender que:

- O OSPF utiliza estado de enlace e visão topológica
- A hierarquia por áreas reduz tráfego e melhora escalabilidade
- A Área 0 é essencial para o funcionamento correto do protocolo
- DR e BDR reduzem a quantidade de adjacências
- Enlaces virtuais são soluções excepcionais e complexas


