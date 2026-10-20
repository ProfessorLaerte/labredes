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

### 2.5 Normas de Referência

Os temas desta fundamentação são regidos pelas seguintes RFCs (*Request for Comments*) da *Internet Engineering Task Force* (IETF).

| Assunto | Norma | Aplicação neste experimento |
| :--- | :--- | :--- |
| Arquitetura TCP/IP | [**RFC 1122**](https://datatracker.ietf.org/doc/html/rfc1122) | Define as camadas de enlace, internet, transporte e aplicação, e os requisitos de comunicação dos *hosts* |
| Protocolo IP | [**RFC 791**](https://datatracker.ietf.org/doc/html/rfc791) | Formato do datagrama IPv4, endereçamento e campo TTL (*Time to Live*) |
| Encaminhamento e rota padrão | [**RFC 1812**](https://datatracker.ietf.org/doc/html/rfc1812) | Requisitos dos roteadores IPv4: escolha da rota de maior prefixo, rota padrão e tabela de roteamento |
| Resolução de endereços | [**RFC 826**](https://datatracker.ietf.org/doc/html/rfc826) | *Address Resolution Protocol* (ARP): associa endereço IP a endereço MAC na rede local |
| Teste de conectividade | [**RFC 792**](https://datatracker.ietf.org/doc/html/rfc792) | *Internet Control Message Protocol* (ICMP): mensagens *Echo Request* e *Echo Reply* do `ping` |

O *Cisco Discovery Protocol* (CDP) é proprietário e não possui RFC; o protocolo padronizado equivalente é o *Link Layer Discovery Protocol* (LLDP), definido no IEEE 802.1AB. Hubs e switches Ethernet são regidos pelo IEEE 802.3, e não por RFCs.

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

     ```text
     Would you like to enter the initial configuration dialog? [yes/no]: yes

     Would you like to enter basic management setup? [yes/no]: yes

     Enter host name [Router]: R1

     Enter enable secret: lab

     Enter enable password: redes

     Enter virtual terminal password: labredes

     Configure SNMP Network Management? [no]: no

     management network from the above interface summary: FastEthernet0/1

     Configure IP on this interface? [yes]: yes

     IP address for this interface: 10.0.0.1

     Subnet mask for this interface [255.0.0.0] : 255.255.255.0

     Enter your selection [2]: 2
     ```

       

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

Antes de configurar as rotas, o estudante levanta o estado do equipamento: qual sistema operacional executa, quais interfaces possui, quem são os vizinhos diretos e que rotas já existem. Esse levantamento é a base do diagnóstico e das comparações feitas na Seção 5.

Todos os comandos desta seção são digitados no modo EXEC privilegiado (`R1#`) e **não alteram a configuração**. Execute-os em R1 e em R2, e capture a tela de cada saída para o relatório.

| Etapa | Pergunta a responder | Comando |
| :---: | :--- | :--- |
| 4.3.1 | Qual é a versão do IOS e como o roteador foi reiniciado? | `show version` |
| 4.3.2 | Quais interfaces existem e qual é o estado de cada uma? | `show ip interface brief`, `show interfaces`, `show ip interface` |
| 4.3.3 | Quais equipamentos Cisco estão diretamente conectados? | `show cdp`, `show cdp neighbors` |
| 4.3.4 | Quais protocolos de camada 3 estão habilitados? | `show protocols` |
| 4.3.5 | Que rotas existem e como foram inseridas? | `show ip route` |

> **Nota sobre o simulador:** o Packet Tracer apresenta saídas mais simples que as de um roteador físico, e alguns campos podem estar ausentes ou diferentes. Os campos descritos a seguir são os que se deve procurar em cada saída.

#### 4.3.1 Versão do Sistema e Última Reinicialização

```cisco
R1# show version        ! identificação do hardware, da imagem do IOS e do último reinício
```

| O que procurar na saída | Significado |
| :--- | :--- |
| Linha `Cisco IOS Software ... Version` | Versão do sistema operacional em execução |
| Linha `System image file` | Arquivo da imagem do IOS carregada |
| Linha `System returned to ROM by ...` | Forma da última reinicialização (por exemplo, `power-on` após ligar o equipamento ou `reload` após o comando de reinício) |
| Tempo de atividade (*uptime*) | Tempo decorrido desde a última inicialização |
| Quantidade e tipo de interfaces | Hardware disponível no roteador |
| Linha `Configuration register` | Define como o equipamento inicializa |

**Registre:** a versão do IOS e a forma da última reinicialização.

#### 4.3.2 Interfaces: Estado e Endereçamento

```cisco
R1# show ip interface brief   ! resumo de todas as interfaces, uma por linha
R1# show interfaces           ! detalhes de camada 1 e 2: MAC, MTU, banda, contadores de erros
R1# show ip interface         ! detalhes de camada 3: endereço, máscara, ACL e demais parâmetros IP
```

Comece por `show ip interface brief`, que oferece a visão geral. Cada coluna tem um significado:

| Coluna | Significado |
| :--- | :--- |
| `Interface` | Nome da interface (por exemplo, `FastEthernet0/0`) |
| `IP-Address` | Endereço IP configurado, ou `unassigned` se não houver |
| `OK?` | Indica se o endereço é válido para o IOS (normalmente `YES`) |
| `Method` | Origem do endereço: `manual` (digitado), `NVRAM` (lido da configuração salva), `DHCP` ou `unset` |
| `Status` | Estado **administrativo e de camada 1**: `up`, `down` ou `administratively down` (interface desativada com `shutdown`) |
| `Protocol` | Estado de **camada 2** (enlace): `up` quando a interface está operacional e recebe sinal de enlace do vizinho |

A combinação das duas últimas colunas indica a causa de uma falha:

| `Status` | `Protocol` | Interpretação |
| :---: | :---: | :--- |
| `up` | `up` | Interface operacional |
| `up` | `down` | Camada 1 em operação, mas problema de enlace (camada 2) |
| `down` | `down` | Sem sinal físico: cabo desconectado ou vizinho desligado |
| `administratively down` | `down` | Interface desativada pelo administrador; use `no shutdown` |

Em seguida, use `show interfaces` e `show ip interface` para aprofundar. Procure, em `show interfaces`, a primeira linha (`... is up, line protocol is up`), o endereço MAC (`address is`), o MTU (*Maximum Transmission Unit*) e os contadores de erros de entrada e saída. Em `show ip interface`, procure o endereço e a máscara da interface e a indicação de listas de acesso aplicadas.

**Registre:** o estado administrativo e o operacional (camadas 1 e 2) de cada interface.

#### 4.3.3 Vizinhos Cisco com o CDP

O CDP (Seção 2.4) permite descobrir equipamentos Cisco diretamente conectados.

```cisco
R1# show cdp              ! estado global do CDP: versão e temporizadores de envio
R1# show cdp neighbors    ! tabela de vizinhos diretamente conectados
```

| Coluna de `show cdp neighbors` | Significado |
| :--- | :--- |
| `Device ID` | Nome (*hostname*) do vizinho |
| `Local Intrfce` | Interface **deste** roteador pela qual o vizinho foi aprendido |
| `Holdtme` | Tempo (em segundos) restante até o registro ser descartado se não houver novos anúncios |
| `Capability` | Funções do vizinho, como roteador (`R`) ou switch (`S`); a legenda aparece no topo da saída |
| `Platform` | Modelo do equipamento vizinho |
| `Port ID` | Interface **do vizinho** conectada a este roteador |

Para obter mais detalhes, como o endereço IP e a versão do IOS do vizinho, use `show cdp neighbors detail`.

**Segurança:** todas essas informações são obtidas sem autenticação por quem estiver conectado ao enlace. Por isso, o CDP deve ser desabilitado em interfaces voltadas a redes não confiáveis (`no cdp enable`).

**Registre:** quais vizinhos foram descobertos e quais informações o CDP revelou sobre eles.

#### 4.3.4 Protocolos de Camada 3

```cisco
R1# show protocols        ! protocolos de camada 3 habilitados e estado das interfaces
```

Observe se o roteamento IP está habilitado globalmente (`Internet Protocol routing is enabled`) e, para cada interface, o endereço IP e o estado (`up` ou `down`). Compare com a saída de `show ip interface brief`.

**Registre:** quais protocolos roteados de camada 3 estão habilitados.

#### 4.3.5 Tabela de Roteamento

```cisco
R1# show ip route         ! tabela de roteamento IPv4 do roteador
```

| O que procurar na saída | Significado |
| :--- | :--- |
| Legenda de códigos no topo | Explica cada letra usada nas rotas (`C`, `S`, `L` e outras) |
| `Gateway of last resort` | Rota padrão em uso; `is not set` indica que ela não existe |
| Linhas com `C` | Redes **diretamente conectadas**, criadas quando a interface recebe IP e está `up/up` |
| Linhas com `L` | Endereço local da própria interface (dependendo da versão do IOS) |
| Linhas com `S` ou `S*` | Rotas **estáticas** inseridas pelo administrador (`S*` é a rota padrão) |
| `via <endereço>` e interface | Próximo salto e interface de saída |

Neste ponto, apenas as rotas de origem `C` devem aparecer, porque as rotas estáticas ainda não foram inseridas (Seção 5.2). Cada roteador conhece somente as redes de suas próprias interfaces.

**Registre:** quais redes constam na tabela e como cada rota foi inserida. Guarde a captura de tela para comparar com a tabela obtida após a configuração das rotas padrão.

## 5. Verificação e Testes

Esta seção demonstra que a conectividade entre redes distintas depende de rotas. O procedimento tem quatro etapas: testar antes de configurar as rotas, inserir as rotas padrão, verificar a tabela de roteamento e repetir os testes. Registre as capturas de tela de cada etapa para comparar o antes e o depois.

### 5.1 Teste de Conectividade Antes das Rotas

#### 5.1.1 Teste Fim a Fim

No *Command Prompt* do PC0, execute o `ping` até o Laptop0:

```text
C:\> ping 172.16.0.10     ! teste de ponta a ponta, de PC0 (10.0.0.10) para Laptop0
```

O teste deve **falhar**. Registre a mensagem recebida (por exemplo, `Destination host unreachable` ou `Request timed out`) e identifique **qual equipamento a emitiu**, pois o endereço de origem da resposta indica onde o pacote parou.

#### 5.1.2 Teste Progressivo (Isolamento da Falha)

Para localizar o ponto em que a comunicação deixa de funcionar, teste a partir do PC0 cada salto do caminho, do mais próximo ao mais distante:

| Ordem | Destino do `ping` | Verifica | Resultado esperado antes das rotas |
| :---: | :--- | :--- | :--- |
| 1 | `10.0.0.1` (R1 Fa0/1) | Enlace PC0-R1 e *gateway* | Sucesso |
| 2 | `192.168.0.1` (R1 Fa0/0) | Chegada a outra interface de R1 | Sucesso |
| 3 | `192.168.0.2` (R2 Fa0/0) | Alcance de R2 e retorno da resposta | Falha |
| 4 | `172.16.0.1` (R2 Fa0/1) | Alcance da rede de R2 | Falha |
| 5 | `172.16.0.10` (Laptop0) | Conectividade fim a fim | Falha |

Nos passos 3 a 5 a falha ocorre porque R2 não possui rota de volta para a rede `10.0.0.0/24`, onde está o PC0. R1 conhece `192.168.0.0/24` por ser diretamente conectada (rota `C`), mas nenhum dos dois roteadores conhece as redes que estão do outro lado. Compare esse resultado com a pergunta da Seção 6: **o que falta?**

> **Sobre a primeira tentativa:** o primeiro pacote de um `ping` pode falhar mesmo em uma rede correta, porque o *host* precisa antes resolver o endereço MAC do destino por ARP (*Address Resolution Protocol*, [**RFC 826**](https://datatracker.ietf.org/doc/html/rfc826)). Repita o comando antes de concluir que há falha.

### 5.2 Inserção das Rotas Padrão

Uma rota padrão (`0.0.0.0/0`) instrui o roteador a enviar a um próximo salto todo pacote cujo destino não corresponda a uma rota mais específica. Aqui, cada roteador usa o outro como saída para as redes que desconhece.

```cisco
! Em R1: pacotes para redes desconhecidas seguem para R2 (192.168.0.2)
R1# configure terminal
R1(config)# ip route 0.0.0.0 0.0.0.0 192.168.0.2
R1(config)# end
R1# write
```

```cisco
! Em R2: pacotes para redes desconhecidas seguem para R1 (192.168.0.1)
R2# configure terminal
R2(config)# ip route 0.0.0.0 0.0.0.0 192.168.0.1
R2(config)# end
R2# write
```

| Elemento do comando | Significado |
| :--- | :--- |
| `ip route` | Insere uma rota estática na tabela de roteamento |
| `0.0.0.0 0.0.0.0` | Rede de destino e máscara: correspondem a **qualquer** destino (`0.0.0.0/0`) |
| `192.168.0.2` / `192.168.0.1` | Próximo salto: interface do **outro** roteador, na rede que o roteador local já alcança diretamente |
| `write` | Grava a configuração na NVRAM para que seja mantida após reinício |

Se o próximo salto não estiver em uma rede diretamente conectada, o IOS não consegue resolver a saída e a rota não é instalada na tabela.

### 5.3 Verificação da Tabela de Roteamento

Em R1 e em R2, consulte novamente a tabela e compare com a captura da Seção 4.3.5:

```cisco
R1# show ip route         ! a rota padrão deve aparecer, além das rotas diretamente conectadas
```

| O que verificar | Resultado esperado |
| :--- | :--- |
| `Gateway of last resort` | Passa a indicar o próximo salto configurado (`192.168.0.2` em R1 e `192.168.0.1` em R2), em vez de `is not set` |
| Linha com `S*` | Rota estática candidata a rota padrão, para `0.0.0.0/0`, com o próximo salto (`via`) configurado |
| Linhas com `C` | Permanecem as mesmas redes diretamente conectadas |

Comente como a rota padrão aparece e em que ela difere das rotas `C`: a rota `C` é criada automaticamente pela interface, enquanto a rota `S*` foi inserida pelo administrador.

### 5.4 Repetição dos Testes

Refaça o teste fim a fim e o teste progressivo da Seção 5.1 e compare os resultados com os anteriores. Em seguida, confirme o caminho percorrido:

```text
C:\> ping 172.16.0.10      ! agora deve haver resposta
C:\> tracert 172.16.0.10   ! lista os roteadores no caminho: R1, R2 e o destino
```

O `tracert` deve mostrar o caminho `10.0.0.1` (R1), `192.168.0.2` (R2) e `172.16.0.10` (Laptop0). Cada linha corresponde a um roteador que decrementou o campo TTL (*Time to Live*, [**RFC 791**](https://datatracker.ietf.org/doc/html/rfc791)). Comente os resultados e explique por que o `ping` passou a funcionar.

Os testes também podem ser feitos a partir dos roteadores, sem o uso dos PCs:

```cisco
R1# ping 172.16.0.10      ! teste de R1 até o Laptop0; cada "!" indica resposta recebida
```

### 5.5 Problemas Comuns

| Sintoma | Causa provável | Verificação e correção |
| :--- | :--- | :--- |
| O `ping` ao *gateway* falha | Interface desligada, IP ou máscara incorretos | `show ip interface brief`; use `no shutdown` e corrija o endereço |
| O `ping` ao *gateway* funciona, mas o do outro lado não | Falta de rota em um dos roteadores | `show ip route` em R1 e em R2 |
| A rota padrão não aparece na tabela | Próximo salto digitado errado ou fora de uma rede diretamente conectada | Confira o endereço em `show running-config` e refaça o comando |
| O `ping` chega ao destino, mas a resposta não volta | Rota configurada em apenas um dos roteadores | Configure a rota nos dois sentidos |
| PC ou Laptop sem resposta fora da própria rede | *Gateway* padrão ausente ou incorreto no dispositivo final | Confira o endereço e o *gateway* conforme a tabela da Seção 3 |

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
