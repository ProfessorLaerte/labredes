Disciplina: **ENE0011 – Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica** 

Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**

---

## Apresentação

A disciplina **ENE0011 – Laboratório de Redes** tem como objetivo proporcionar aos alunos experiências práticas relacionadas aos conceitos fundamentais de redes de computadores, protocolos de comunicação e dispositivos de interconexão.

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

A estrutura de arquivos e diretórios do repositório está organizada da seguinte forma:

```text
labredes/
├── README.md                              # Apresentação da disciplina, diretrizes e índice geral
├── modelo_relatorio.md                    # Modelo oficial para confecção e entrega dos relatórios
│
├── laboratorios/                          # Diretório padrão da disciplina com os roteiros vigentes
│   ├── 01_roteiro.md                      # Exp 01: Introdução aos Dispositivos de Redes
│   ├── 02_roteiro.md                      # Exp 02: Configurar Senhas Seguras e SSH
│   ├── 03_roteiro.md                      # Exp 03: Configurar Sub-redes com VLSM
│   ├── 04_roteiro.md                      # Exp 04: VLAN e Roteamento Estático
│   ├── 05_roteiro.md                      # Exp 05: Topologia Redundante com VLANs, EtherChannel e Hardening
│   ├── 06_roteiro.md                      # Exp 06: Desafio de Resolução de Problemas (Troubleshooting)
│   └── topologias/                        # Arquivos de simulação do Cisco Packet Tracer (.pkt)
│       ├── experimento_03_nao_configurado.pkt
│       ├── experimento_04_nao_configurado.pkt
│       ├── experimento_05_nao_configurado.pkt
│       └── experimento_06_nao_resolvido.pkt
│
└── labs/                                  # Diretório com o histórico anterior da disciplina
    ├── README.md                          # Informações e índice do acervo histórico
    ├── lab-01.md a lab-11.md              # Roteiros anteriores (Dispositivos, RIP, OSPF, BGP, etc.)
    ├── comandos.md                        # Guia rápido de comandos Cisco IOS e OSPF
    ├── relatorio.md                       # Modelo histórico de relatório
    ├── VM-Cybersecurity.md                # Orientações sobre máquinas virtuais para práticas de cibersegurança
    └── arquivos/                          # Topologias de apoio dos experimentos históricos
        └── Exp-08 -Vlan_configurado.pkt
```

### Detalhamento dos Diretórios e Arquivos

- **`laboratorios/` (Padrão da Disciplina)**:  
  Diretório principal com as atividades práticas ativas do semestre. Cada arquivo `XX_roteiro.md` contém:
  - Cabeçalho de identificação institucional;
  - Objetivo e fundamentação teórica;
  - Descrição da atividade e comandos a serem utilizados;
  - Passo a passo da montagem/configuração;
  - Orientações e critérios para entrega do relatório.

  - **`laboratorios/topologias/`**:  
    Centraliza os arquivos de topologia pré-configurados ou de desafio do **Cisco Packet Tracer** (`.pkt`) vinculados aos experimentos atuais, prontos para download e uso direto pelos alunos.

- **`labs/` (Histórico da Disciplina)**:  
  Mantido como registro e acervo de consulta das edições anteriores da disciplina. Inclui 11 experimentos legados (com foco em analisadores de protocolo/Wireshark, protocolos de roteamento dinâmico RIP, OSPF e BGP, além de testes de segurança de roteamento), guias rápidos e orientações para uso de máquinas virtuais (VMs).

  - **`labs/arquivos/`**:  
    Armazena arquivos de topologia Packet Tracer utilizados nas atividades históricas.

- **Arquivos da Raiz**:  
  - **`README.md`**: Guia central da disciplina, com ementa, objetivos, metodologia, tabela de acesso rápido aos laboratórios e bibliografia.
  - **`modelo_relatorio.md`**: Estrutura padronizada em Markdown que cada estudante deve clonar e preencher com seus dados, procedimentos, evidências visuais e análises técnicas para a entrega das atividades.

---

## Laboratórios

| Laboratório | Tema | Link |
|:------------:|:------|------|
| 01 | Introdução aos Dispositivos de Redes | [Acessar](./laboratorios/01_roteiro.md) |
| 02 | Configurar Senhas Seguras e SSH | [Acessar](./laboratorios/02_roteiro.md) |
| 03 | Configurar Sub-redes com VLSM | [Acessar](./laboratorios/03_roteiro.md)<br>[Arquivo Packet Tracer](./laboratorios/topologias/experimento_03_nao_configurado.pkt) |
| 04 | VLAN e Roteamento Estático | [Acessar](./laboratorios/04_roteiro.md)<br>[Arquivo Packet Tracer](./laboratorios/topologias/experimento_04_nao_configurado.pkt) |
| 05 | Topologia Redundante com VLANs, EtherChannel, Roteamento Inter-VLAN e Hardening de Camada 2 | [Acessar](./laboratorios/05_roteiro.md)<br>[Arquivo Packet Tracer](./laboratorios/topologias/experimento_05_nao_configurado.pkt) |
| 06 | Desafio de Resolução de Problemas (Troubleshooting) | [Acessar](./laboratorios/06_roteiro.md)<br>[Arquivo Packet Tracer](./laboratorios/topologias/experimento_06_nao_resolvido.pkt) |

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

> Este repositório é de uso acadêmico e destina-se exclusivamente às atividades da disciplina ENE0011 – Laboratório de Redes.
