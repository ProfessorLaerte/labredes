Disciplina: **ENE0011 – Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica**

Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**

---

## Experimento 08 – VLAN Avançado, Trunking, Segurança e Diagnóstico de Redes

## Descrição Direta

Este laboratório é uma **continuação direta do Experimento 03**, utilizando **a mesma topologia física e lógica**, o mesmo plano de endereçamento IP e as mesmas VLANs previamente configuradas.

A partir desta base funcional, o aluno deverá **expandir, reforçar e validar** a rede, incorporando **novos requisitos técnicos**, **mecanismos de segurança** e **procedimentos de diagnóstico**.

---

## Objetivos

- Consolidar o entendimento sobre **VLANs e Inter-VLAN Routing**;
- Configurar e validar **trunks IEEE 802.1Q**;
- Implementar **boas práticas de segurança em switches**;
- Diagnosticar falhas de conectividade em redes segmentadas;
- Analisar o comportamento da rede sob configurações incorretas;
- Desenvolver capacidade de **debug e troubleshooting**.

---

## Pré-requisitos

- Laboratório 03 concluído e funcional;
- Conhecimentos de:
  - VLANs;
  - Trunking;
  - Router-on-a-Stick;
  - Endereçamento IP;
  - Comandos básicos Cisco IOS.

---

## Descrição da Atividade

A atividade está dividida em **cinco desafios progressivos**.  
Cada desafio **depende da correta implementação do anterior**.

---

## Desafio 1 – Validação Completa da Topologia

1. Verifique se:
   - Todas as VLANs existem em todos os switches;
   - As portas de acesso estão corretamente associadas;
   - Os enlaces entre switches estão configurados como **trunk**;
   - O roteador possui todas as subinterfaces ativas.

2. Utilize comandos como:
   - `show vlan brief`
   - `show interfaces trunk`
   - `show ip interface brief`
   - `show running-config`

**Questão:**  
Quais evidências comprovam que a rede está corretamente segmentada em VLANs?

---

## Desafio 2 – Trunking e Native VLAN

1. Configure explicitamente:
   - Encapsulamento **IEEE 802.1Q** nos enlaces trunk;
   - Uma **VLAN nativa diferente da VLAN 1**.

2. Teste:
   - Comunicação intra-VLAN;
   - Comunicação inter-VLAN.

**Questões:**
- Qual a função da VLAN nativa?
- Quais riscos estão associados ao uso da VLAN 1 como VLAN nativa?

---

## Desafio 3 – Segurança em Portas de Acesso (Port Security)

Implemente **Port Security** nas portas de acesso dos switches:

- Limite o número máximo de endereços MAC;
- Defina a ação em caso de violação;
- Teste o comportamento conectando dispositivos diferentes na mesma porta.

**Questões:**
- O que ocorre quando a política é violada?
- Como essa configuração contribui para a segurança da rede?

---

## Desafio 4 – Diagnóstico de Falhas Induzidas

O professor (ou o roteiro) irá **introduzir propositalmente erros**, como:
- VLAN inexistente associada a uma porta;
- Trunk configurado como access;
- Subinterface do roteador sem encapsulamento;
- Gateway incorreto em um host.

O aluno deverá:
1. Identificar a falha;
2. Explicar o impacto na comunicação;
3. Corrigir a configuração.

**Questão:**  
Quais comandos foram mais úteis para identificar o problema? Justifique.

---

## Desafio 5 – Testes e Análise de Tráfego

1. Execute testes de conectividade:
   - Intra-VLAN;
   - Inter-VLAN;
   - Gerência dos switches.

2. Utilize ferramentas de simulação (ex.: modo *Simulation* do Packet Tracer) para:
   - Observar encapsulamento;
   - Analisar o caminho dos pacotes;
   - Identificar em qual camada ocorre o processamento.

**Questão:**  
Em qual ponto da rede ocorre a decisão de roteamento entre VLANs?

---

## Entregáveis

O aluno deverá entregar um **relatório técnico** contendo:

- Diagrama lógico da rede;
- Configurações realizadas;
- Respostas às questões propostas;
- Evidências dos testes;
- Análise crítica dos erros induzidos.

---

## Critérios de Avaliação

- Correção técnica das configurações;
- Clareza das explicações;
- Capacidade de diagnóstico;
- Uso adequado de comandos;
- Organização do relatório.

---

## Equipamentos e Software

- Computadores do laboratório;
- **Cisco Packet Tracer**.

---

## Observações Finais

Este laboratório tem como foco **pensamento crítico**, **análise de redes reais** e **comportamento sob falhas**, aproximando o aluno de cenários encontrados em ambientes corporativos.

---

> Laboratório desenvolvido para a disciplina **ENE0011 – Laboratório de Redes**  
> **Prof. Dr. Laerte Peotta de Melo**
