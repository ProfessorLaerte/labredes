Disciplina: **ENE0011 – Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica** 

Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**

---
## Experimento 04 – Roteamento Estático


## Descrição Geral

Este laboratório tem como objetivo introduzir e consolidar os **conceitos fundamentais de roteamento estático em redes IP**, permitindo ao estudante compreender como rotas configuradas manualmente possibilitam a comunicação entre redes distintas.

A atividade enfatiza o papel do **endereçamento IP**, da **tabela de rotas** e da **decisão de encaminhamento**, servindo como base conceitual para o estudo posterior de protocolos de roteamento dinâmico.

---

## Objetivos

Ao final deste laboratório, o estudante deverá ser capaz de:

- Configurar corretamente o endereçamento IP de redes distintas
- Implementar roteamento estático em roteadores
- Compreender o papel do gateway padrão nas estações
- Analisar a comunicação entre redes por meio de `ping` e `traceroute`
- Relacionar decisões de roteamento com o funcionamento do plano de dados

---

## Fundamentação Teórica

Os conceitos abordados neste laboratório incluem:

- Endereçamento IP
- Sub-redes e CIDR
- Encaminhamento IP
- Roteamento estático
- Tabelas de rotas

Esses tópicos estão diretamente relacionados ao conteúdo discutido em aula e aos materiais de apoio da disciplina.

---

## Topologia Lógica

A topologia utilizada no experimento é composta por múltiplas redes interligadas por roteadores, conforme ilustrado.

### Visão geral da topologia


<img width="713" height="421" alt="image" src="https://github.com/user-attachments/assets/88801712-4eab-45d5-bde5-0d8dd80b5bb2" />

- **Rede 10:** 192.168.10.0/24  
- **Rede 20:** 192.168.20.0/24  
- **Rede 30:** 192.168.30.0/24  
- **Rede 40:** 192.168.40.0/24  
- **Rede 50:** 192.168.50.0/24  
- **Rede 60:** 192.168.60.0/24  

Cada LAN deve conter **pelo menos dois hosts**, devidamente configurados com endereço IP e gateway padrão.

---

## Ambiente de Laboratório

- Dispositivos:
  - Roteadores 
  - Hosts Linux
- Ferramentas de teste:
  - `ping`
  - `traceroute`

---

## Atividades Práticas

### Etapa 1 – Montagem do cenário
- Criar a topologia conforme o diagrama fornecido
- Conectar corretamente roteadores e redes locais

### Etapa 2 – Endereçamento IP
- Configurar endereços IP em todas as interfaces
- Definir corretamente as máscaras de sub-rede
- Configurar o gateway padrão em cada host

### Etapa 3 – Configuração de roteamento estático
- Implementar rotas estáticas em **todos os roteadores**
- Garantir que todas as redes sejam alcançáveis
- Verificar a tabela de rotas após a configuração

### Etapa 4 – Testes de conectividade
- Utilizar `ping` para testar comunicação entre hosts de redes diferentes
- Utilizar `traceroute` para analisar o caminho percorrido pelos pacotes

---

## Questões para o Relatório

O relatório do laboratório deve documentar **todos os comandos executados**, incluindo capturas de tela quando necessário, e responder às seguintes questões:

1. Como as rotas estáticas foram configuradas para que toda a rede se tornasse funcional?
2. É necessário configurar rotas nas estações de trabalho das LANs? Por quê?
3. Quais rotas ficam definidas automaticamente nas estações?
4. Caso cada rede utilizasse uma tecnologia de enlace diferente, haveria impacto no funcionamento do roteamento? Justifique.
5. O que é retornado pelo comando `ping` entre estações de redes distintas? Explique seu funcionamento.
6. O que é retornado pelo comando `traceroute` entre estações de redes distintas? Expl
