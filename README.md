Disciplina: **ENE0011 - Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica** 

Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**

---

## Apresentação

A disciplina **ENE0011 - Laboratório de Redes** tem como objetivo proporcionar aos alunos experiências práticas relacionadas aos conceitos fundamentais de redes de computadores, protocolos de comunicação e dispositivos de interconexão.

Ao longo do curso, os estudantes irão configurar, analisar e testar redes de computadores utilizando equipamentos reais e ambientes de simulação, consolidando os conhecimentos teóricos adquiridos em disciplinas correlatas.

Este repositório reúne os **roteiros de laboratório**, **materiais de apoio** e **instruções práticas** necessárias para a execução das atividades.

---

## Objetivos da Disciplina

- Compreender o funcionamento de dispositivos de rede (hubs, switches, roteadores);
- Configurar e operar roteadores e switches, com ênfase em equipamentos Cisco;
- Analisar protocolos de comunicação nas camadas de rede e transporte;
- Interpretar informações operacionais e de diagnóstico de equipamentos de rede;
- Desenvolver raciocínio prático para identificação e resolução de problemas em redes;
- Aplicar boas práticas de segurança em dispositivos de rede (senhas seguras, SSH, hardening de camada 2).

---

## Organização do Repositório

A estrutura do repositório segue o padrão abaixo:

- `laboratorios/` - Roteiros atuais dos experimentos, com foco em endereçamento, VLAN, segurança (senhas seguras, SSH, VLSM, redundância e hardening de camada 2) e um desafio de troubleshooting.
  - `topologias/` - Arquivos de simulação e topologias do Cisco Packet Tracer (`.pkt`).
- `modelo_relatorio.md` - Modelo oficial de relatório a ser preenchido e entregue pelos alunos.
- `labs/` - Pasta mantida como referência histórica, contendo materiais de edições anteriores da disciplina (Experimentos 01 a 11, comandos OSPF e VMs).

Cada laboratório possui um roteiro próprio contendo:
- Objetivo do experimento;
- Fundamentação teórica;
- Roteiro de atividades;
- Comandos a serem utilizados;
- Orientações para entrega.

---

## Laboratórios

| Laboratório | Tema | Roteiro | Packet Tracer |
|:------------:|:------|:-------:|:-------------:|
| 01 | Introdução aos Dispositivos de Redes | [Acessar](./laboratorios/01_roteiro.md) | - |
| 02 | Configurar Senhas Seguras e SSH | [Acessar](./laboratorios/02_roteiro.md) | - |
| 03 | Configurar Sub-redes com VLSM | [Acessar](./laboratorios/03_roteiro.md) | [Baixar Arquivo](./laboratorios/topologias/experimento_03_nao_configurado.pkt) |
| 04 | VLAN e Roteamento Estático | [Acessar](./laboratorios/04_roteiro.md) | [Baixar Arquivo](./laboratorios/topologias/experimento_04_nao_configurado.pkt) |
| 05 | Topologia Redundante com VLANs, EtherChannel, Roteamento Inter-VLAN e Hardening de Camada 2 | [Acessar](./laboratorios/05_roteiro.md) | [Baixar Arquivo](./laboratorios/topologias/experimento_05_nao_configurado.pkt) |
| 06 | Desafio de Resolução de Problemas (Troubleshooting) | [Acessar](./laboratorios/06_roteiro.md) | [Baixar Arquivo](./laboratorios/topologias/experimento_06_nao_resolvido.pkt) |

---

## Metodologia

As atividades laboratoriais serão desenvolvidas de forma **prática e supervisionada**, envolvendo:
- Configuração direta de equipamentos;
- Análise de saídas de comandos;
- Observação do comportamento da rede;
- Registro técnico das atividades realizadas.

> O aluno deve seguir rigorosamente os **roteiros** utilizando o modelo de **relatório** [Acessar](./modelo_relatorio.md) e registrar os resultados conforme solicitado. 

---

## Avaliação

A avaliação da disciplina será baseada em:
- Execução correta dos laboratórios;
- Qualidade dos registros e respostas apresentadas;
- Participação e envolvimento durante as atividades;
- Cumprimento dos prazos estabelecidos.

> Os critérios específicos de cada experimento serão informados no respectivo roteiro.

---

## Requisitos

- Conhecimentos básicos de redes de computadores;
- Familiaridade com sistemas operacionais;
- Noções iniciais de protocolos TCP/IP;
- Atenção às normas de uso do laboratório.

---

## Bibliografia Básica

- **DOYLE, Jeff; CARROLL, Jennifer DeHaven**. Routing TCP/IP: volume 1. Indianapolis: Cisco Press, 1998.
- **FOROUZAN, Behrouz A.** Protocolo TCP/IP. 3. ed. Porto Alegre: AMGH, 2010.

## Bibliografia Complementar

- **KUROSE, James F.; ROSS, Keith W**. Redes de computadores e a Internet: uma abordagem top-down. 6. ed. São Paulo: Pearson, 2013.
- **TANENBAUM, Andrew S.; WETHERALL, David J.** Redes de computadores. 5. ed. São Paulo: Pearson, 2011.
- **STALLINGS, William.** Comunicações de dados e redes de computadores. 10. ed. São Paulo: Pearson, 2014.
- **COMER, Douglas E.** Interligação de redes com TCP/IP: princípios, protocolos e arquitetura. 5. ed. Rio de Janeiro: Elsevier, 2006.

## Documentos Normativos e Técnicos

- **POSTEL, Jon.** Transmission Control Protocol. RFC 793. Marina del Rey: ISI, 1981.
- **POSTEL, Jon.** User Datagram Protocol. RFC 768. Marina del Rey: ISI, 1980.
- **MOY, John T.** OSPF version 2. RFC 2328. Marina del Rey: ISI, 1998.
- **REKHTER, Yakov et al.** A border gateway protocol 4 (BGP-4). RFC 4271. Marina del Rey: ISI, 2006.

---

## Observações Importantes

- É responsabilidade do aluno **salvar e documentar** corretamente todas as configurações realizadas;
- Alterações não autorizadas nos equipamentos podem acarretar penalidades acadêmicas;
- Dúvidas técnicas devem ser registradas e discutidas durante as aulas práticas.

---

## Professor Responsável

**Prof. Dr. Laerte Peotta de Melo**

---

> Este repositório é de uso acadêmico e destina-se exclusivamente às atividades da disciplina ENE0011 - Laboratório de Redes.
