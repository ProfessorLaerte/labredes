Disciplina: **ENE0011 – Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica** 
 
Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**  
Monitores: **Caio Soares Rodriges / Huan Guilherme Ribeiro Miranda**   

---
## Experimento 05 – Protocolo RIP (Routing Information Protocol)


## Descrição Geral

Este laboratório tem como objetivo a **configuração e análise do protocolo de roteamento RIP (Routing Information Protocol)**, permitindo ao estudante compreender o funcionamento de **protocolos baseados em vetor de distância**, bem como observar suas **características de convergência lenta e limitações de escalabilidade**.

A atividade dá continuidade ao laboratório de **Roteamento Estático**, substituindo rotas manuais por **aprendizado dinâmico de rotas**, evidenciando vantagens e problemas inerentes a esse modelo de roteamento.

> **Observações**

Este laboratório integra a disciplina **Laboratório de Redes** e deve ser realizado em sequência ao laboratório de  [**Roteamento Estático**](./labs/lab-03.md), pois se apoia na mesma topologia e conceitos iniciais.


---

##  Objetivos

Ao final deste laboratório, o estudante deverá ser capaz de:

- Configurar o protocolo RIP em roteadores
- Diferenciar RIPv1 e RIPv2
- Analisar a propagação dinâmica de tabelas de roteamento
- Observar o comportamento da convergência em protocolos vetor de distância
- Identificar problemas como convergência lenta e loops de roteamento
- Compreender mecanismos clássicos de mitigação desses problemas

---

##  Fundamentação Teórica

Os conceitos abordados neste laboratório incluem:

- Tabelas de roteamento
- Algoritmos de roteamento
- Métricas de roteamento
- Protocolos vetor de distância
- RIP versão 1 e versão 2
- Convergência em redes IP

---

##  Topologia Lógica

A topologia utilizada neste experimento é a **mesma do laboratório de Roteamento Estático - experimento 04**.

### Redes envolvidas

- **Rede 10:** 192.168.10.0/24  
- **Rede 20:** 192.168.20.0/24  
- **Rede 30:** 192.168.30.0/24  
- **Rede 40:** 192.168.40.0/24  
- **Rede 50:** 192.168.50.0/24  
- **Rede 60:** 192.168.60.0/24  

Cada LAN deve conter **no mínimo dois hosts**, configurados com endereço IP e gateway padrão.

---

##  Ambiente de Laboratório

- Dispositivos:
  - Roteadores
  - Hosts Linux
- Ferramentas de teste:
  - `ping`
  - `traceroute`
  - `show ip route`
  - `debug ip rip` (quando aplicável)

---

##  Atividades Práticas

### Parte 1 – Configuração do Protocolo RIP

1. Montar a topologia conforme o laboratório anterior
2. Remover todas as rotas estáticas previamente configuradas
3. Habilitar o protocolo **RIPv2** em todos os roteadores
4. Configurar corretamente as redes anunciadas
5. Verificar a tabela de rotas após a convergência inicial

---

### Parte 2 – Atualização e Convergência de Rotas RIP

1. No roteador **R2**, desativar a interface associada à **Rede 30**
2. Verificar a remoção da rota da tabela de roteamento local e dos demais roteadores
3. Reativar a interface da Rede 30
4. Observar o processo de reconvergência da rede
5. Medir o tempo necessário para que todas as tabelas sejam atualizadas

---

### Parte 3 – Análise de Convergência e Estabilidade

Responder e analisar:

- Quanto tempo foi necessário para a rede convergir?
- Após o tempo de timeout (180 segundos), como a rota é apresentada na tabela?
- Qual o impacto da convergência lenta no funcionamento da rede?

---

### Parte 4 – Mecanismos de Mitigação de Problemas

Estudar e descrever no relatório o funcionamento dos seguintes mecanismos:

- Maximum Hop Count
- Split Horizon
- Poison Reverse Update
- Triggered Updates
- Hold-down Timers

Relacionar cada mecanismo aos problemas que ele busca mitigar.

---

##  Questões para o Relatório

O relatório deve documentar **todos os comandos executados** e responder às seguintes questões:

1. Como o RIP propagou as informações de roteamento na topologia?
2. Quais diferenças foram observadas em relação ao roteamento estático?
3. Qual o impacto da convergência lenta em redes maiores?
4. Como os mecanismos de mitigação ajudam a reduzir loops e instabilidade?
5. Em quais cenários o RIP deixa de ser uma solução adequada?

---

##  Resultados Esperados

Ao final do laboratório, o estudante deverá compreender que:

- Protocolos vetor de distância aprendem rotas por troca periódica de informações
- O RIP é simples, porém limitado em escalabilidade
- A convergência lenta pode causar indisponibilidade temporária
- Mecanismos adicionais são necessários para reduzir loops e inconsistências
- Protocolos mais modernos surgem para superar essas limitações

---

## 📚 Referências

- LOBATO, Luiz Carlos. *Protocolos de Roteamento IP*. RNP.
- COMER, Douglas E. *Interligação de Redes com TCP/IP*. Elsevier.
- KUROSE, James F.; ROSS, Keith W. *Redes de Computadores e a Internet*. Pearson.

---

> Laboratório desenvolvido para a disciplina **ENE0011 – Laboratório de Redes**  
> **Prof. Dr. Laerte Peotta de Melo**
