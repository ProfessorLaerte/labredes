Disciplina: **ENE0011 – Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica**

Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**

---
## Experimento 09 – OSPF Avançado: Falhas, Convergência, Custos e Engenharia de Tráfego


## Descrição Geral

Este laboratório é uma **continuação direta do Experimento 07 – Roteamento OSPF**, utilizando **a mesma topologia**, **as mesmas áreas**, **os mesmos endereçamentos IP** e **a configuração OSPF já funcional**.

A partir desse cenário estável, o aluno deverá lidar com **situações avançadas**, incluindo:
- Alterações de métricas;
- Eleição forçada de DR/BDR;
- Falhas de enlaces e roteadores;
- Análise de convergência;
- Avaliação crítica do uso de enlaces virtuais.

---

## Objetivos

- Analisar o comportamento do OSPF sob falhas reais;
- Avaliar impacto de custos OSPF na seleção de rotas;
- Controlar a eleição de DR e BDR;
- Medir tempo e impacto de convergência;
- Desenvolver raciocínio de **engenharia de tráfego com OSPF**;
- Avaliar limites arquiteturais do protocolo.

---

## Pré-requisitos

- Experimento 07 concluído e validado;
- Todas as áreas OSPF operacionais;
- Conectividade total entre LANs;
- Virtual Links funcionais (quando aplicável).

---

## Desafio 1 – Manipulação de Custos OSPF

1. Escolha **duas rotas possíveis** entre redes de áreas distintas.
2. Altere o **custo OSPF** de interfaces selecionadas para:
   - Forçar mudança do caminho preferencial;
   - Redirecionar tráfego por outra área.

Utilize:
- `ip ospf cost`
- `show ip route`
- `show ip ospf interface`

**Questões**
- Como o custo influencia a árvore SPF?
- A alteração impacta apenas o roteador local ou toda a área?

---

## Desafio 2 – Eleição Forçada de DR e BDR

1. Identifique redes multiacesso com DR/BDR eleitos.
2. Force:
   - Um roteador específico como DR;
   - Outro como BDR.

Utilize:
- `ip ospf priority`
- Reinicialização controlada de interfaces OSPF.

**Questões**
- O que acontece se o DR falhar?
- A eleição ocorre novamente? Em que condições?

---

## Desafio 3 – Falhas de Enlace e Convergência

1. Provoque falhas controladas:
   - Desligamento de interfaces;
   - Remoção temporária de enlaces;
   - Isolamento de uma área.

2. Meça:
   - Tempo de convergência;
   - Alterações na tabela de roteamento;
   - Mudanças na base LSDB.

Utilize:
- `debug ip ospf events`
- `show ip ospf database`
- `traceroute`

**Questões**
- Qual o impacto da hierarquia por áreas na convergência?
- Qual área sofre maior impacto?

---

## Desafio 4 – Análise Crítica de Enlaces Virtuais

1. Remova temporariamente o **Virtual Link** configurado no Experimento 07.
2. Observe:
   - Perda de rotas;
   - Isolamento lógico da área;
   - Comportamento do backbone.

3. Restaure o Virtual Link e compare os estados.

**Questões**
- Por que a Área 0 é crítica no OSPF?
- Por que enlaces virtuais são considerados soluções emergenciais?

---

## Desafio 5 – Diagnóstico de Problemas OSPF

O professor (ou o roteiro) irá introduzir **falhas propositalmente**, como:
- Interfaces OSPF em área incorreta;
- Router-ID duplicado;
- Máscaras inconsistentes;
- MTU mismatch;
- Autenticação incompatível (se aplicável).

O aluno deverá:
1. Identificar o problema;
2. Explicar o sintoma observado;
3. Corrigir a configuração;
4. Justificar tecnicamente a solução.

---

## Entregáveis

Relatório técnico contendo:
- Evidências de cada desafio;
- Comandos utilizados;
- Saídas relevantes;
- Análises críticas;
- Comparações antes/depois.

---

## Critérios de Avaliação

- Capacidade de diagnóstico;
- Clareza técnica das explicações;
- Correção das soluções;
- Entendimento conceitual do OSPF;
- Organização do relatório.

---

## Considerações Finais

Este laboratório consolida o OSPF como **protocolo de nível profissional**, aproximando o aluno de cenários reais de redes corporativas, operadoras e data centers.

---

> Laboratório desenvolvido para a disciplina **ENE0011 – Laboratório de Redes**  
> **Prof. Dr. Laerte Peotta de Melo**
