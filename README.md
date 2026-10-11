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

- `laboratorios/` - Roteiros dos experimentos práticos (dispositivos de rede, segurança, VLANs, VLSM, redundância, troubleshooting e serviços de infraestrutura DHCP e DNS).
  - `topologias/` - Arquivos de simulação e topologias do Cisco Packet Tracer (`.pkt`).
- `images/` - Diagramas técnicos e ilustrações esquemáticas das práticas de laboratório.

Cada laboratório possui um roteiro próprio contendo:
- Objetivo do experimento;
- Fundamentação teórica;
- Roteiro de atividades;
- Comandos a serem utilizados;
- Orientações para entrega.

---

## Laboratórios

### Laboratórios de Redes

| Laboratório | Tema | Roteiro | Packet Tracer |
|:------------:|:------|:-------:|:-------------:|
| 01 | Introdução aos Dispositivos de Redes | [Acessar](./laboratorios/01_roteiro.md) | - |
| 02 | Configurar Senhas Seguras e SSH | [Acessar](./laboratorios/02_roteiro.md) | - |
| 03 | Configurar Sub-redes com VLSM | [Acessar](./laboratorios/03_roteiro.md) | [Baixar Arquivo](./laboratorios/topologias/experimento_03_nao_configurado.pkt) |
| 04 | VLAN e Roteamento Estático | [Acessar](./laboratorios/04_roteiro.md) | [Baixar Arquivo](./laboratorios/topologias/experimento_04_nao_configurado.pkt) |
| 05 | Topologia Redundante com VLANs, EtherChannel, Roteamento Inter-VLAN e Hardening de Camada 2 | [Acessar](./laboratorios/05_roteiro.md) | [Baixar Arquivo](./laboratorios/topologias/experimento_05_nao_configurado.pkt) |
| 06 | Desafio de Resolução de Problemas (Troubleshooting) | [Acessar](./laboratorios/06_roteiro.md) | [Baixar Arquivo](./laboratorios/topologias/experimento_06_nao_resolvido.pkt) |

### Laboratórios de Serviços de Rede

| Laboratório | Tema | Roteiro | Ambiente / Simulador |
|:------------:|:------|:-------:|:--------------------:|
| 07 | Configuração e Análise do Protocolo DHCP | [Acessar](./laboratorios/07_roteiro.md) | PNetLab (Linux) |
| 08 | Configuração e Análise do Serviço DNS | [Acessar](./laboratorios/08_roteiro.md) | PNetLab (Linux) |

---

## Metodologia

As atividades laboratoriais serão desenvolvidas de forma **prática e supervisionada**, envolvendo:
- Configuração direta de equipamentos;
- Análise de saídas de comandos;
- Observação do comportamento da rede;
- Registro técnico das atividades realizadas.

> O aluno deve seguir rigorosamente os **roteiros** e registrar os resultados conforme solicitado. 

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
- **XAVIER, Fabrício de Castro.** Roteadores Cisco. 2. ed. São Paulo: Novatec, 2010.

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

## Uso de Inteligência Artificial

Em conformidade com a Política de Integridade Acadêmica e Uso Responsável da Inteligência Artificial da Faculdade de Tecnologia (FT/UnB), aprovada pelo Conselho da FT em 30/09/2026, estabelecem-se as seguintes diretrizes:

### Declaração Docente
Declara-se que este material didático foi elaborado com apoio de ferramentas de inteligência artificial generativa (Claude, ChatGPT e Gemini), sob estrita supervisão, revisão técnica e responsabilidade do professor responsável.

### Diretrizes para os Estudantes
- **Regime Geral da Disciplina:** O uso de ferramentas de IA generativa é **permitido** como instrumento de apoio ao aprendizado em todas as atividades laboratoriais (pesquisa conceitual de RFCs, esclarecimento de sintaxe de comandos, interpretação de saídas e suporte à redação técnica dos relatórios).
- **Responsabilidade Acadêmica:** O estudante é o único responsável pela exatidão técnica, autoria e originalidade de suas entregas. Todas as configurações práticas, comandos executados e capturas de telas e comandos devem ser obrigatoriamente produzidos pelo estudante.
- **Condutas Vedadas:**
  - Fabricar, adulterar ou simular saídas de comandos, capturas de telas e comandos ou evidências de laboratório;
  - Apresentar análises e conclusões elaboradas com o uso de IA sem reflexão, revisão ou contribuição intelectual própria;
  - Inserir dados sensíveis, credenciais de acesso ou informações confidenciais em plataformas públicas de IA.
