# Roteiro do Experimento 01 - Introdução aos Dispositivos de Redes

- **Instituição:** Universidade de Brasília (UnB)
- **Departamento:** Departamento de Engenharia Elétrica (ENE)
- **Curso:** Engenharia de Redes de Comunicação
- **Disciplina:** ENE0011 - Laboratório de Redes
- **Professor responsável:** Prof. Dr. Laerte Peotta de Melo
- **Tema:** Dispositivos de interconexão, arquitetura TCP/IP e modelos de rede

---

## 1. Objetivo

Conhecer os dispositivos básicos de rede e realizar operações básicas de configuração, coleta de informações e diagnóstico em um roteador Cisco.

Ao final da atividade, o estudante deverá ser capaz de:

- diferenciar *hub*, switch e roteador quanto à camada de atuação e ao critério de encaminhamento;
- configurar interfaces e senhas básicas em roteadores Cisco pelo diálogo inicial e pela *Command Line Interface* (CLI);
- coletar informações do equipamento com `show version`, `show interfaces`, `show ip interface brief`, `show protocols` e `show ip route`;
- identificar vizinhos Cisco com o *Cisco Discovery Protocol* (CDP);
- explicar a origem das rotas diretamente conectadas e configurar rotas padrão estáticas.

## 2. Fundamentação Teórica

### 2.1 Dispositivos de Interconexão

| Dispositivo | Camada (OSI) | Critério de encaminhamento | Domínios de colisão / broadcast |
| :--- | :---: | :--- | :--- |
| *Hub* | 1 (física) | Repete o sinal elétrico em todas as portas | Um único domínio de colisão e um de *broadcast* |
| Switch | 2 (enlace) | Tabela de endereços MAC (*Media Access Control*) | Um domínio de colisão por porta; um domínio de *broadcast* (sem VLANs) |
| Roteador | 3 (rede) | Tabela de roteamento (endereço IP de destino) | Um domínio de *broadcast* por interface |

O roteador interliga redes IP (*Internet Protocol*) distintas. Cada interface pertence a uma sub-rede, e o pacote é encaminhado conforme a rota de maior prefixo que corresponde ao destino.

### 2.2 Cisco IOS

O *Internetworking Operating System* (IOS) é o sistema operacional dos roteadores e switches Cisco, acessado pela CLI. Os principais modos são:

| Modo | Prompt | Acesso |
| :--- | :---: | :--- |
| EXEC do usuário | `Router>` | Estado inicial; comandos de consulta limitados |
| EXEC privilegiado | `Router#` | Comando `enable`; permite `show` completos e `copy` |
| Configuração global | `Router(config)#` | Comando `configure terminal` (`conf t`) |
| Configuração de interface | `Router(config-if)#` | Comando `interface <nome>` |

A configuração em execução (*running-config*) fica na memória RAM e só é preservada após o reinício se for gravada na NVRAM (*Non-Volatile RAM*) com `write` ou `copy running-config startup-config`.

### 2.3 Tabela de Roteamento e Rota Padrão

Uma rota pode ser **diretamente conectada** (código `C`, criada quando a interface recebe IP e está `up/up`), **estática** (código `S`, inserida pelo administrador) ou aprendida por protocolo de roteamento. A **rota padrão** (`0.0.0.0/0`) é usada quando nenhuma rota mais específica corresponde ao destino. O próximo salto de uma rota estática deve ser um endereço alcançável por uma rede diretamente conectada.

### 2.4 Cisco Discovery Protocol (CDP)

O CDP é um protocolo proprietário da Cisco, de camada 2, que anuncia identificação, plataforma, interfaces e capacidades aos vizinhos diretamente conectados. Do ponto de vista de segurança, esse anúncio revela informações do equipamento e deve ser desabilitado em interfaces voltadas a redes não confiáveis (`no cdp enable`).

## 3. Topologia e Cenário

1. Crie um cenário no Cisco Packet Tracer com os seguintes dispositivos:
   - 2 Roteadores (1841)
   - 2 Switches (2960)
   - 2 Dispositivos finais (PCs/Laptops)
   - O cenário deverá ser montado e configurado da seguinte forma:

<img width="924" height="492" alt="Topologia do Experimento 01 no Packet Tracer" src="https://github.com/user-attachments/assets/0bf34e6e-11b7-4ebc-9215-0194af1df709" />

| **Dispositivo** | **Interface** | **Endereço IP** | **Máscara de Sub-rede** | **Gateway padrão** |
| --------------- | ------------- | --------------- | ----------------------- | ------------------ |
| R1              | Fa0/1         | 10.0.0.1        | 255.255.255.0           | N/D |
| R1              | Fa0/0         | 192.168.0.1     | 255.255.255.0           | N/D |
| PC0             | Fa0           | 10.0.0.10       | 255.255.255.0           | 10.0.0.1 |
| R2              | Fa0/1         | 172.16.0.1      | 255.255.0.0             | N/D |
| R2              | Fa0/0         | 192.168.0.2     | 255.255.255.0           | N/D |
| Laptop0         | Fa0           | 172.16.0.10     | 255.255.0.0             | 172.16.0.1 |



## 4. Roteiro Prático Passo a Passo

### 4.1 Configuração dos Dispositivos Finais

- PCs e Laptops devem ser configurados utilizando a interface gráfica.

- Switches não precisam ser configurados.

  

### 4.2 Configuração dos Roteadores

1. Clique no roteador, na aba *"Physical"* acione o botão para ligar o dispositivo e acesse a aba CLI. Aguarde a inicialização do sistema.

2. O equipamento entrará em modo de configuração (#setup):

   - Entre com as informações solicitadas:

     - Would you like to enter the initial configuration dialog? [yes/no]: **yes**

     - Would you like to enter basic management setup? [yes/no]: **yes**

     - Enter host name [Router]: **R1**

     - Enter enable secret: **lab**

     - Enter enable password: **redes**

     - Enter virtual terminal password: **labredes**

     - Configure SNMP Network Management? [no]: **no**

     - management network from the above interface summary: **FastEthernet0/1**

     - Configure IP on this interface? [yes]: **yes**

     - IP address for this interface: **10.0.0.1**

     - Subnet mask for this interface [255.0.0.0] : **255.255.255.0**

     - Enter your selection [2]: **2**

       

   Neste ponto foram configuradas as informações básicas do equipamento como: *hostname*, senhas, e a interface Fa0/1.    

   Configurando a interface Fa0/0 do roteador:

   ```cisco
   enable
   conf t
   int fa0/0
   ip add 192.168.0.1 255.255.255.0
   no shut
   ^z
   write
   ```

   **_Agora o Roteador R1 está configurado, repita os mesmos passos para o roteador R2, adaptando as informações (as senhas podem ser as mesmas)_**



### 4.3 Coleta de Informações



1. Colete as informações abaixo sobre o equipamento (R1 ou R2) e a sua configuração. 

   - Utilize os comandos:
     - `show version`
     - `show interfaces`
     - `show ip interface`
     - `show ip interface brief`


     - Versão do sistema operacional do Cisco IOS;


     - Forma pela qual o roteador foi reinicializado pela última vez;


     - Interfaces de Rede existentes no roteador: Status administrativo e operacional de Camada
       1 (física) e 2 (enlace)


       



2. O protocolo CDP (Seção 2.4) permite obter informações sobre equipamentos Cisco diretamente conectados ao seu equipamento. Verifique se existe algum equipamento vizinho da Cisco e identifique quais informações podem ser obtidas.

   - Utilize os comandos: 

     ```cisco
     show cdp
     
     show cdp neighbors
     ```

     


3. Identifique quais são os protocolos roteados de camada 3 que estão habilitados no roteador. 

   - Utilize o comando:

     ```cisco
     show protocols
     ```




4. Tabelas de roteamento. Visualize a tabela de roteamento dos roteadores R1 e R2. Existem rotas para alguma rede? Caso existam, como essas rotas foram inseridas na tabela?

   - Utilize o comando:

     ```cisco
     show ip route
     ```

  

## 5. Verificação e Testes

### 5.1 Testes de Conectividade

1. Realize testes de conectividade entre os dispositivos finais:

   - PC0 para Laptop
   - Utilize o comando `ping` no *Command Prompt* do PC0.
     - Por que o teste não funcionou? O que falta?

   

2. Inserindo rota padrão nos roteadores

   - Utilize os comandos:

     - Em R1:

       ```cisco
       enable
       conf t
       ip route 0.0.0.0 0.0.0.0 192.168.0.2
       ```

       

     - Em R2:

       ```cisco
       enable
       conf t
       ip route 0.0.0.0 0.0.0.0 192.168.0.1
       ```

   

3. Verifique novamente as tabelas de roteamento e comente como as rotas aparecem.

   

4. Refaça os testes novamente e comente os resultados

   

---

## 6. Questões para Análise

1. Qual a diferença entre *hub*, switch e roteador quanto à camada de atuação e ao critério de encaminhamento?
2. Quais informações o `show version` fornece sobre o equipamento? Qual é a versão do IOS e como o roteador foi reinicializado pela última vez?
3. Qual a diferença entre o estado administrativo e o estado operacional (camadas 1 e 2) de uma interface em `show ip interface brief`?
4. Que informações o CDP revelou sobre o vizinho? Que risco de segurança esse anúncio representa?
5. Por que o `ping` de PC0 para Laptop0 falhou antes das rotas padrão? Como as rotas iniciais da tabela (`C`) foram inseridas?
6. Após as rotas padrão, como elas aparecem em `show ip route` e por que o `ping` passou a funcionar?

## 7. Critérios de Avaliação

A avaliação será realizada com base na seguinte rubrica, totalizando exatamente 10,0 pontos:

| Critério de Avaliação | Pontuação Máxima |
| :--- | :---: |
| **Topologia e endereçamento:** Dispositivos conectados e IPs, máscaras e *gateways* conforme a tabela | 2,0 |
| **Configuração dos roteadores:** Diálogo inicial, nomes, senhas e interfaces ativas | 2,0 |
| **Coleta de informações:** Evidências de `show version`, `show ip interface brief`, `show cdp neighbors`, `show protocols` e `show ip route` | 1,5 |
| **Rotas padrão e conectividade:** Configuração das rotas e `ping` bem-sucedido entre PC0 e Laptop0 | 2,0 |
| **Respostas às questões:** Análise fundamentada das questões da Seção 6 | 2,0 |
| **Qualidade das evidências:** Capturas de tela legíveis em todas as respostas | 0,5 |
| **Total** | **10,0** |

## 8. Entregáveis

1. Arquivo `.pkt` da topologia configurada.
2. Capturas de tela dos comandos de coleta (Seção 4.3) e das tabelas de roteamento antes e depois das rotas padrão.
3. Capturas de tela dos testes de conectividade da Seção 5.
4. Respostas às 6 questões da Seção 6. **Capturas de tela devem acompanhar todas as respostas.**

## 9. Conclusão

Neste experimento, o estudante configurou roteadores Cisco, coletou informações operacionais e observou que a conectividade entre redes distintas exige rotas: as rotas diretamente conectadas surgem das interfaces ativas, e as demais devem ser inseridas, como as rotas padrão estáticas. A prática estabelece a base de configuração e diagnóstico utilizada nos experimentos seguintes.

---

[Índice Geral (README)](../README.md) | [Próximo: Experimento 02 →](02_roteiro.md)
