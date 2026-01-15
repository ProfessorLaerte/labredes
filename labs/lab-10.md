Disciplina: **ENE0011 – Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica**

Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**

---
## Laboratório 09 – Ataques ao OSPF e Técnicas de Mitigação


## Descrição Geral

Este laboratório utiliza como base **as configurações finais do Laboratório 08 – OSPF Avançado**, no qual a topologia encontra-se totalmente funcional, com múltiplas áreas OSPF, eleição de DR/BDR, ajustes de custo e enlaces virtuais (quando aplicável).

A partir desse cenário operacional, o aluno irá **simular ataques ao protocolo OSPF**, analisar seus impactos e **implementar mecanismos de mitigação**, compreendendo riscos reais associados a protocolos de roteamento dinâmico.

---

## Objetivos

- Compreender vulnerabilidades inerentes ao protocolo OSPF;
- Simular ataques comuns a protocolos de roteamento;
- Analisar impactos de ataques na convergência e estabilidade da rede;
- Implementar mecanismos de mitigação e endurecimento (hardening);
- Desenvolver visão crítica sobre segurança de protocolos de roteamento.

---

## Pré-requisitos

- Laboratório 08 concluído e validado;
- Conectividade total entre todas as áreas OSPF;
- Conhecimento prévio de:
  - DR/BDR;
  - LSAs;
  - Custos OSPF;
  - Virtual Links;
  - Comandos de diagnóstico OSPF.

---

## Ameaças ao OSPF (Visão Geral)

O OSPF **não possui segurança nativa ativada por padrão**. Sem proteção adequada, ele está sujeito a ataques como:

- Injeção de pacotes HELLO falsos;
- Sequestro de DR/BDR;
- Injeção ou manipulação de LSAs;
- Criação de adjacências não autorizadas;
- Instabilidade proposital da topologia;
- Ataques de negação de serviço ao plano de controle.

---

## Desafio 1 – Formação de Adjacência Não Autorizada (Hello Spoofing)

1. Configure um roteador adicional (ou modifique um existente) para:
   - Participar indevidamente de uma área OSPF;
   - Anunciar-se com parâmetros compatíveis (Hello/Dead Interval, Area ID).

2. Verifique:
   - Formação de vizinhança;
   - Alterações na LSDB;
   - Impactos na tabela de rotas.

**Questões**
- Por que o OSPF aceita esse roteador?
- Quais campos do protocolo permitem esse ataque?

---

## Desafio 2 – Sequestro de DR (DR Hijacking)

1. Identifique uma rede multiacesso com DR/BDR eleitos.
2. Configure um roteador atacante com:
   - Prioridade OSPF maior;
   - Reinicialização controlada da interface.

3. Observe:
   - Mudança de DR;
   - Alterações no fluxo de LSAs.

**Questões**
- Quais impactos o atacante passa a ter como DR?
- Esse ataque afeta apenas a área local?

---

## Desafio 3 – Manipulação de Métricas (Path Manipulation)

1. Altere custos OSPF de forma maliciosa para:
   - Forçar caminhos subótimos;
   - Criar rotas artificiais mais atraentes.

2. Analise:
   - Árvore SPF;
   - Alterações nos caminhos dos pacotes.

**Questões**
- Por que o OSPF confia nesses anúncios?
- Esse ataque pode degradar desempenho sem derrubar a rede?

---

## Desafio 4 – Instabilidade e DoS no Plano de Controle

1. Provoque instabilidade OSPF por:
   - Flapping de interfaces;
   - Alterações frequentes de custo;
   - Reinício repetido de processos OSPF.

2. Observe:
   - Consumo de CPU;
   - Frequência de recomputação SPF;
   - Perda temporária de rotas.

**Questões**
- Como isso afeta a convergência?
- Quais roteadores sofrem maior impacto?

---

## Mitigações – Endurecimento do OSPF

Após a execução dos ataques, implemente as seguintes contramedidas.

---

### Mitigação 1 – Autenticação OSPF

Configure autenticação OSPF em todas as interfaces:

- Autenticação simples ou criptográfica;
- Mesma chave em todos os roteadores da área.

**Análise**
- O que acontece com roteadores não autenticados?
- Quais ataques passam a ser bloqueados?

---

### Mitigação 2 – Passive Interfaces

Configure interfaces que **não devem formar adjacências** como passivas:

```bash
router ospf 1
 passive-interface default
 no passive-interface <interface-necessaria>
```
**Análise**
- Como isso reduz a superfície de ataque?

### Mitigação 3 – Controle de Eleição de DR

- Ajuste prioridades OSPF;
- Defina DRs preferenciais;
- Evite eleição dinâmica em segmentos críticos.

### Mitigação 4 – Boas Práticas de Projeto

- Minimizar redes multiacesso;
- Evitar uso excessivo de Virtual Links;
- Manter backbone OSPF bem definido;
- Segmentar áreas de forma coerente.

### Testes Pós-Mitigações

- Repita os ataques anteriores.
- Compare:
  - Formação de vizinhanças;
  - Estabilidade da rede;
  - Tabelas de roteamento;
  - Base LSDB.

**Questão Final**
- Quais ataques foram efetivamente mitigados? Quais ainda exigem controles adicionais?

 ## Entregáveis

Relatório técnico contendo:
- Descrição dos ataques executados;
- Evidências (comandos e saídas);
- Impactos observados;
- Mitigações aplicadas;
- Comparação antes/depois;
- Análise crítica de segurança. 

## Critérios de Avaliação

- Clareza técnica;
- Capacidade de análise de riscos;
- Correção das mitigações;
- Entendimento do plano de controle;
- Qualidade do relatório.

## Considerações Finais

Este laboratório demonstra que **protocolos de roteamento também são alvos de ataque**, reforçando a importância de **segurança no plano de controle**, especialmente em redes corporativas, operadoras e infraestruturas críticas.
  
