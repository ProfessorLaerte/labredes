# Roteiro do Experimento 03 - Configurar Sub-redes com VLSM

- **Instituição:** Universidade de Brasília (UnB)
- **Departamento:** Departamento de Engenharia Elétrica (ENE)
- **Curso:** Engenharia de Redes de Comunicação
- **Disciplina:** ENE0011 - Laboratório de Redes
- **Professor responsável:** Prof. Dr. Laerte Peotta de Melo
- **Tema:** Endereçamento IPv4, dimensionamento de blocos CIDR e projeto de sub-redes com VLSM

> **Pré-requisito:** Este laboratório reaproveita as configurações de segurança e de acesso por SSH do [**Experimento 02**](02_roteiro.md).

---

## 1. Objetivo

O objetivo deste experimento é aplicar a técnica de VLSM (*Variable Length Subnet Mask*) para planejar e configurar sub-redes em uma rede IPv4, garantindo que cada departamento receba 
a quantidade adequada de endereços IP, com eficiência e segurança na utilização dos recursos de endereçamento.

## 2. Fundamentação Teórica

O uso de CIDR (*Classless Inter-Domain Routing*) e VLSM permite dividir uma rede em sub-redes de tamanhos diferentes, otimizando o espaço de endereçamento e evitando desperdício de IPs. Esta seção apresenta a notação CIDR, a técnica VLSM, um método de projeto com exemplo resolvido e os erros mais comuns. O exemplo resolvido usa outra rede e outras demandas, para que o plano do experimento seja elaborado pelo estudante. Além disso, configurações de segurança como SSH e criptografia de senhas garantem conectividade confiável e proteção contra acessos não autorizados.

### 2.1 CIDR

- **CIDR** significa *Classless Inter-Domain Routing* ([**RFC 4632**](https://datatracker.ietf.org/doc/html/rfc4632)).
- A notação é escrita como `IP/prefixo`.
- O **prefixo** indica quantos bits (dos 32 do IPv4) são usados para identificar a rede.
- Os bits restantes são usados para endereçar **hosts**.

Um endereço IPv4 tem 32 bits, divididos em quatro octetos. A máscara de sub-rede é a forma decimal do prefixo: seus bits `1` marcam a parte de rede e seus bits `0` marcam a parte de host. Por exemplo, `/26` equivale a 26 bits `1` seguidos de 6 bits `0`, ou `255.255.255.192`.

Dado o número de bits de host $h$ (igual a $32$ menos o prefixo), valem as relações:

- **tamanho do bloco** (total de endereços da sub-rede): $2^{h}$;
- **hosts utilizáveis**: $2^{h}-2$, pois o primeiro endereço identifica a **rede** e o último é o de ***broadcast***, e ambos não podem ser atribuídos a equipamentos.

**Relação entre Máscara, Bits e Hosts**

| **CIDR** | **Máscara Decimal** | **Bits de Host** | **Hosts Utilizáveis** |
| -------- | ------------------- | ---------------- | --------------------- |
| **/8**   | 255.0.0.0           | 24               | 16.777.214            |
| **/16**  | 255.255.0.0         | 16               | 65.534                |
| **/24**  | 255.255.255.0       | 8                | 254                   |

### 2.2 VLSM

- **Variable Length Subnet Mask (VLSM)** é a técnica de usar **máscaras de sub-rede de diferentes tamanhos** dentro da mesma rede.
- Enquanto o CIDR define um prefixo fixo para uma rede, o VLSM permite **dividir a rede em sub-redes menores**, cada uma com a máscara adequada ao número de hosts necessários.
- Isso evita desperdício de endereços IP, pois cada sub-rede recebe apenas o espaço que realmente precisa.

**Exemplo**:

- `/25` → 25 bits para rede, 7 bits para hosts → $2^{7}-2=126$ hosts utilizáveis.
- `/30` → 30 bits para rede, 2 bits para hosts → $2^{2}-2=2$ hosts utilizáveis.

**Exceções**:

- `/31`: usado em links ponto a ponto, permite 2 endereços sem broadcast ([**RFC 3021**](https://datatracker.ietf.org/doc/html/rfc3021)).
- `/32`: identifica apenas um host específico, sem espaço para outros.

**Relação entre Máscara, Bits e Hosts**

| **CIDR** | **Máscara Decimal** | **Bits para Hosts** | **Tamanho do Bloco** | **Hosts Utilizáveis** |
| -------- | ------------------- | ------------------- | -------------------- | --------------------- |
| **/25**  | 255.255.255.128     | 7                   | 128                  | 126                   |
| **/26**  | 255.255.255.192     | 6                   | 64                   | 62                    |
| **/27**  | 255.255.255.224     | 5                   | 32                   | 30                    |
| **/28**  | 255.255.255.240     | 4                   | 16                   | 14                    |
| **/29**  | 255.255.255.248     | 3                   | 8                    | 6                     |
| **/30**  | 255.255.255.252     | 2                   | 4                    | 2                     |
| **/31**  | 255.255.255.254     | 1                   | 2                    | 2 (ponto a ponto)     |
| **/32**  | 255.255.255.255     | 0                   | 1                    | 1 (host único)        |

### 2.3 Benefícios do VLSM

- **Eficiência**: reduz desperdício de endereços IP.
- **Flexibilidade**: cada sub-rede pode ter o tamanho exato necessário.
- **Escalabilidade**: facilita o crescimento da rede sem precisar redesenhar tudo.

### 2.4 Método de Projeto com VLSM

O projeto de sub-redes segue uma sequência fixa. A ordem decrescente de tamanho é essencial: ao alocar primeiro os blocos maiores, cada bloco menor já nasce alinhado, e não surgem lacunas nem sobreposições.

1. **Levante a demanda de cada sub-rede.** Some aos hosts os endereços de infraestrutura que ocupam a mesma sub-rede: o *gateway* e, quando houver, o IP de gerência dos switches. O experimento pede ainda a reserva para crescimento futuro (a sub-rede de convidados).
2. **Ordene as sub-redes da maior para a menor demanda.**
3. **Escolha o menor prefixo que comporte a demanda**, isto é, o menor valor de $h$ tal que $2^{h}-2 \geq$ demanda.
4. **Aloque os blocos em sequência**, a partir do início da rede original. O endereço de rede de cada bloco deve ser **múltiplo do tamanho do bloco**. O próximo bloco começa no endereço seguinte ao *broadcast* do anterior, ajustado a esse múltiplo.
5. **Calcule** para cada sub-rede: endereço de rede, primeiro host (rede + 1), último host (*broadcast* − 1) e *broadcast* (rede + tamanho do bloco − 1).
6. **Confira** se os intervalos não se sobrepõem e se todos cabem na rede original.

**Exemplo resolvido** (rede `172.16.10.0/24`, diferente da rede do experimento). Demandas: sub-rede A com 60 hosts, B com 20, C com 12 e um enlace ponto a ponto entre roteadores.

| Sub-rede | Demanda | Prefixo escolhido | Raciocínio |
| :--- | :---: | :---: | :--- |
| A | 60 | `/26` | `/27` oferece 30 (insuficiente); `/26` oferece 62 |
| B | 20 | `/27` | `/28` oferece 14 (insuficiente); `/27` oferece 30 |
| C | 12 | `/28` | `/28` oferece 14 |
| Enlace | 2 | `/30` | `/30` oferece exatamente 2 |

| Sub-rede | Endereço de rede | Máscara | Primeiro host | Último host | Broadcast |
| :--- | :--- | :--- | :--- | :--- | :--- |
| A | `172.16.10.0` | `/26` | `172.16.10.1` | `172.16.10.62` | `172.16.10.63` |
| B | `172.16.10.64` | `/27` | `172.16.10.65` | `172.16.10.94` | `172.16.10.95` |
| C | `172.16.10.96` | `/28` | `172.16.10.97` | `172.16.10.110` | `172.16.10.111` |
| Enlace | `172.16.10.112` | `/30` | `172.16.10.113` | `172.16.10.114` | `172.16.10.115` |

Verificação do alinhamento: o bloco de 64 começa em 0, o de 32 em 64 (múltiplo de 32), o de 16 em 96 (múltiplo de 16) e o de 4 em 112 (múltiplo de 4). Os endereços de `172.16.10.116` a `172.16.10.255` permanecem livres para crescimento futuro.

### 2.5 Erros Comuns e Aspectos de Segurança

| Erro | Efeito | Como evitar |
| :--- | :--- | :--- |
| Contar apenas os hosts e esquecer o *gateway* e o IP de gerência | A sub-rede esgota antes do previsto | Somar os endereços de infraestrutura à demanda |
| Alocar da menor para a maior sub-rede | Blocos desalinhados e lacunas | Ordenar da maior para a menor demanda |
| Sobreposição de intervalos | Roteamento ambíguo e conectividade intermitente | Conferir o *broadcast* de cada bloco contra o início do seguinte |
| Atribuir a um host o endereço de rede ou de *broadcast* | O equipamento não se comunica | Usar apenas o intervalo do primeiro ao último host |
| Máscara diferente da planejada em um dispositivo | O equipamento considera locais ou remotos os destinos errados | Conferir máscara e *gateway* com `show ip interface brief` e com a configuração do host |
| *Gateway* fora da sub-rede do host | Sem comunicação com outras redes | Usar um endereço do mesmo bloco, em geral o primeiro ou o último host |

**Segurança:** a separação em sub-redes é o pré-requisito para controlar o tráfego entre departamentos, por exemplo com listas de controle de acesso (ACL), e para isolar a rede de convidados dos recursos internos. A divisão correta também evita que endereços de gerência dos switches fiquem expostos na mesma sub-rede dos usuários.

### 2.6 Normas de Referência

O endereçamento e o projeto de sub-redes se apoiam nas RFCs abaixo.

| Assunto | Norma | Aplicação neste experimento |
| :--- | :--- | :--- |
| Sub-redes IPv4 | [**RFC 950**](https://datatracker.ietf.org/doc/html/rfc950) | Procedimento padrão de divisão de uma rede em sub-redes por máscara |
| CIDR e agregação | [**RFC 4632**](https://datatracker.ietf.org/doc/html/rfc4632) | *Classless Inter-Domain Routing* (CIDR): notação de prefixo `/n` e agregação de rotas |
| Tamanho variável de máscara | [**RFC 1878**](https://datatracker.ietf.org/doc/html/rfc1878) | Tabela de sub-redes de tamanho variável, com número de hosts por prefixo, base do dimensionamento com VLSM |
| Enlaces ponto a ponto | [**RFC 3021**](https://datatracker.ietf.org/doc/html/rfc3021) | Uso de prefixos `/31` em enlaces ponto a ponto, sem endereços de rede e de *broadcast* |
| Endereçamento privado | [**RFC 1918**](https://datatracker.ietf.org/doc/html/rfc1918) | Faixa `192.168.0.0/24`, reservada a redes privadas, usada como bloco de partida do projeto |

## 3. Topologia e Cenário

### 3.1 Cenário e Requisitos

<img width="1110" height="491" alt="Topologia do Experimento 03 no Packet Tracer" src="https://github.com/user-attachments/assets/63bd0377-8a2d-4ac2-9c25-cf18b4198c7a" />

No cenário fornecido ([Baixar arquivo do laboratório](./topologias/experimento_03_nao_configurado.pkt)), as conexões entre o roteador R1 e o servidor já estão configuradas. Os endereços do servidor (`200.1.1.1`) e da interface de R1 voltada a ele (`200.1.1.2`) também já constam na Tabela de Endereçamento. O restante do endereçamento e das configurações é de responsabilidade do estudante.

Você deve criar um novo esquema de endereçamento IPv4 que acomode 4 sub-redes usando a rede `192.168.0.0/24`, conforme a demanda abaixo:

| Sub-rede | Demanda de hosts | Observação |
| :--- | :---: | :--- |
| Administrativo | 100 | Equipe administrativa |
| Vendas | 50 | Departamento de vendas |
| TI | 25 | Departamento de TI |
| Convidados | 25 | Será adicionada futuramente, mas **deve ser planejada agora** |

Além do endereçamento, o experimento exige:

1. as configurações básicas de segurança e de interface no roteador R1;
2. a interface SVI (*Switch Virtual Interface*) da VLAN (*Virtual Local Area Network*) 1 e as configurações básicas de segurança nos switches SW1, SW2 e SW3;
3. os testes de conectividade e de acesso remoto da Seção 5.

### 3.2 Tabela de Documentação das Redes

Preencha a tabela com os resultados do seu plano VLSM (método da Seção 2.4). A linha da rede original já está preenchida como referência.

| **Nome da Rede** | **Endereço da Rede** | **Broadcast**   | **Máscara CIDR** | **Hosts Utilizáveis** | **Primeiro Host** | **Último Host** |
| ---------------- | -------------------- | --------------- | ---------------- | --------------------- | ----------------- | --------------- |
| Rede original    | 192.168.0.0          | 192.168.0.255   | /24              | 254                   | 192.168.0.1       | 192.168.0.254   |
| Administrativo   |                      |                 |                  |                       |                   |                 |
| Vendas           |                      |                 |                  |                       |                   |                 |
| TI               |                      |                 |                  |                       |                   |                 |
| Convidados       |                      |                 |                  |                       |                   |                 |

### 3.3 Tabela de Endereçamento

Preencha com os endereços de cada dispositivo, a partir dos intervalos definidos na Tabela de Documentação. Os campos em branco são do estudante.

| Dispositivo        | Interface | End. IP / Prefixo | Gateway   |
| ------------------ | --------- | ----------------- | --------- |
| Server (WEB e DNS) | fa0/0     | 200.1.1.1         | 200.1.1.2 |
| R1                 | eth0/0/0  | 200.1.1.2         | NA        |
|                    | eth0/1/0  |                   | NA        |
|                    | fa0/0     |                   | NA        |
|                    | fa0/1     |                   | NA        |
| SW1                | VLAN1     |                   |           |
| SW2                | VLAN1     |                   |           |
| SW3                | VLAN1     |                   |           |
| Vendas-1           | NIC       |                   |           |
| Vendas-2           | NIC       |                   |           |
| TI-1               | NIC       |                   |           |
| TI-2               | NIC       |                   |           |
| ADM-1              | NIC       |                   |           |
| ADM-2              | NIC       |                   |           |

Observações para o preenchimento:

- A interface de R1 que atende uma sub-rede recebe um endereço dessa sub-rede e passa a ser o **gateway** de todos os dispositivos dela (PCs e switch). Identifique na topologia a qual sub-rede cada interface de R1 se liga.
- Adote uma convenção única para o *gateway* (por exemplo, o primeiro host utilizável de cada sub-rede) e use-a em todas as sub-redes.
- Os IPs de gerência dos switches (SVI da VLAN 1) pertencem à sub-rede da qual o switch faz parte e usam como *gateway* a interface de R1 dessa sub-rede.
- Nenhum endereço atribuído pode coincidir com o endereço de rede ou de *broadcast* da sub-rede, nem com o de outro dispositivo.

## 4. Roteiro Prático Passo a Passo

### 4.1 Planejamento do Endereçamento IPv4

O planejamento antecede qualquer configuração: um erro no plano se propaga para todos os equipamentos.

1. Aplique o método da Seção 2.4 à rede **192.168.0.0/24**, considerando as demandas da Seção 3.1 e somando à demanda de cada sub-rede o *gateway* e, nas que tiverem switch, o IP de gerência.
2. Preencha a **Tabela de Documentação das Redes** (Seção 3.2): nome, endereço da rede, *broadcast*, máscara CIDR, quantidade de hosts utilizáveis, primeiro e último endereço utilizável.
3. Preencha a **Tabela de Endereçamento** (Seção 3.3) com os endereços de cada dispositivo.

**Lista de conferência do plano**, antes de seguir para a configuração:

| Verificação | Resultado esperado |
| :--- | :--- |
| Cada sub-rede comporta os hosts, o *gateway* e o IP de gerência | Sim, com folga calculada |
| Os blocos foram alocados da maior para a menor sub-rede | Sim |
| O endereço de rede de cada bloco é múltiplo do tamanho do bloco | Sim |
| Há sobreposição entre intervalos | Não |
| Todos os blocos cabem em `192.168.0.0/24` | Sim |
| Nenhum dispositivo usa o endereço de rede ou de *broadcast* | Sim |

### 4.2 Configuração dos Computadores

Em cada PC, abra **Desktop > IP Configuration** (no Packet Tracer) e preencha os campos conforme a Tabela de Endereçamento:

| Campo | Valor |
| :--- | :--- |
| *IPv4 Address* | Endereço do PC, dentro da sub-rede do seu departamento |
| *Subnet Mask* | Máscara correspondente ao prefixo escolhido (na forma decimal) |
| *Default Gateway* | Endereço da interface de R1 na sub-rede do PC |
| *DNS Server* | `200.1.1.1` (servidor WEB e DNS) |

O *gateway* é necessário para alcançar qualquer destino fora da sub-rede, como o servidor; o DNS é necessário para resolver o nome `www.lab3.com`.

### 4.3 Configuração de R1 e dos Switches

Esta etapa reaproveita os comandos de segurança do [Experimento 02](02_roteiro.md). A tabela relaciona cada requisito ao comando correspondente. Os comandos entre `<...>` devem ser substituídos pelos valores do seu plano.

#### 4.3.1 Requisitos Comuns a R1 e aos Switches

| Requisito | Comando |
| :--- | :--- |
| Nome do dispositivo conforme a Tabela de Endereçamento | `hostname <nome>` |
| Desativar a pesquisa de DNS | `no ip domain-lookup` |
| Senha criptografada do modo privilegiado: `labredes` | `enable secret labredes` |
| Criptografar senhas em texto simples | `service password-encryption` |
| Banner de acesso restrito | `banner motd #<texto>#` |
| Nome de domínio `labredes.com` | `ip domain-name labredes.com` |
| Usuário local com senha forte | `username <usuario> secret <senha>` |
| Chave RSA de 1024 bits | `crypto key generate rsa` |
| SSH versão 2 | `ip ssh version 2` |
| Acesso remoto apenas por SSH, com usuário local | `transport input ssh` e `login local` nas linhas VTY |
| Encerrar sessões ociosas após 5 minutos | `exec-timeout 5` no console e nas linhas VTY |

```cisco
Router> enable
Router# configure terminal
Router(config)# hostname <nome>                       ! nome do dispositivo
<nome>(config)# no ip domain-lookup                   ! evita atraso ao digitar comando errado
<nome>(config)# enable secret labredes                ! senha do modo privilegiado, armazenada com hash
<nome>(config)# service password-encryption          ! ofusca as demais senhas em texto simples
<nome>(config)# banner motd #
ATENÇÃO: Acesso restrito!
Somente usuários autorizados podem entrar.
Tentativas não autorizadas serão registradas.#
<nome>(config)# ip domain-name labredes.com           ! necessário à geração da chave RSA
<nome>(config)# username <usuario> secret <senha>     ! credencial para o login local
<nome>(config)# crypto key generate rsa               ! informe 1024 quando solicitado
How many bits in the modulus [512]: 1024
<nome>(config)# ip ssh version 2                      ! somente a versão 2 do protocolo
<nome>(config)# line console 0
<nome>(config-line)# exec-timeout 5                   ! encerra a sessão após 5 minutos ociosos
<nome>(config-line)# exit
<nome>(config)# line vty 0 15
<nome>(config-line)# transport input ssh              ! bloqueia o Telnet
<nome>(config-line)# login local                      ! autentica no usuário local
<nome>(config-line)# exec-timeout 5
<nome>(config-line)# end
```

O banner informa que o acesso não autorizado é proibido, o que tem valor de aviso jurídico além de técnico. O caractere `#` delimita o início e o fim do texto, que não pode conter esse caractere.

#### 4.3.2 Requisitos Apenas em R1

| Requisito | Comando |
| :--- | :--- |
| Configurar e ativar as interfaces ligadas aos switches, conforme o plano | `ip address <gateway> <mascara>` e `no shutdown` |
| Exigir mínimo de 10 caracteres nas senhas | `security passwords min-length 10` |
| Bloquear por 3 minutos após 4 falhas de login em 2 minutos | `login block-for 180 attempts 4 within 120` |

```cisco
R1(config)# interface <interface>                     ! repita para cada interface ligada a um switch
R1(config-if)# ip address <gateway> <mascara>         ! gateway da sub-rede, conforme a Tabela de Endereçamento
R1(config-if)# no shutdown                            ! ativa a interface
R1(config-if)# exit
R1(config)# security passwords min-length 10          ! vale para senhas configuradas daqui em diante
R1(config)# login block-for 180 attempts 4 within 120 ! mitiga ataques de força bruta
```

> **Ordem dos comandos:** o comprimento mínimo de senha só vale para senhas definidas **depois** dele. A senha `labredes` do `enable secret` tem 8 caracteres e seria recusada se `min-length 10` fosse aplicado antes. Por isso, configure `enable secret` e o usuário local antes de `security passwords min-length 10`, ou escolha senhas de pelo menos 10 caracteres para o usuário.

#### 4.3.3 Requisitos Apenas nos Switches

| Requisito | Comando |
| :--- | :--- |
| SVI da VLAN 1 com endereço e máscara do plano | `interface vlan 1` e `ip address <ip> <mascara>` |
| Ativar a SVI | `no shutdown` |
| *Gateway* padrão | `ip default-gateway <gateway>` |

```cisco
SW1(config)# interface vlan 1
SW1(config-if)# ip address <ip> <mascara>             ! IP de gerência dentro da sub-rede do switch
SW1(config-if)# no shutdown                           ! a SVI inicia desativada
SW1(config-if)# exit
SW1(config)# ip default-gateway <gateway>             ! um switch de camada 2 não roteia; usa o gateway para o tráfego próprio
```

Repita para SW2 e SW3, com os respectivos nomes e endereços.

**Aspecto de segurança:** neste experimento a gerência utiliza a VLAN 1, que é o padrão de fábrica. Em redes de produção, usa-se uma VLAN de gerência dedicada, o que será praticado no [Experimento 09](09_roteiro.md).

#### 4.3.4 Salvamento da Configuração

```cisco
<nome># write                                         ! grava a configuração na NVRAM
```

Execute `write` em todos os dispositivos. Sem isso, a configuração é perdida ao reiniciar. Em seguida, siga para a Seção 5 para validar o resultado.

## 5. Verificação e Testes

### 5.1 Requisitos de Conectividade

- Usando navegadores Web nos computadores ADM, Vendas e TI, navegue para www.lab3.com.

- Todos os PCs devem poder executar ping em todos os outros dispositivos.

  - O registro DNS de `www.lab3.com` deve existir no servidor (Services > DNS no Packet Tracer). Caso não exista, crie um registro do tipo A apontando para o IP do servidor.

### 5.2 Comandos de Verificação

| Comando / ação | Equipamento | O que observar |
| :--- | :---: | :--- |
| `show ip interface brief` | R1 e switches | Interfaces `up/up` e SVIs com os IPs do plano de endereçamento |
| `show ip route` | R1 | Rotas `C` (e `L`) para cada sub-rede planejada |
| `show running-config \| section line vty` | R1 e switches | `transport input ssh`, `login local` e `exec-timeout 5 0` |
| `ping` | Cada PC | Resposta de todos os PCs, dos *gateways* e do servidor |
| `ssh -l <usuario> <IP>` | PC | Acesso por SSH a um switch e ao roteador |
| Navegador (`www.lab3.com`) | ADM, Vendas e TI | Página do servidor exibida |

## 6. Questões para Análise

1. **Teste de conexão SSH**   Realize um teste de conexão via SSH em um **switch** e em um **roteador**. Documente os **comandos utilizados** e as **saídas obtidas** (print da tela). Explique como o resultado confirma que o acesso remoto seguro foi configurado corretamente.

   

2. **Tabela de Documentação**   Preencha a **Tabela de Documentação das Redes** com os endereços de rede, broadcast, máscara CIDR, quantidade de hosts e intervalo de endereços utilizáveis. Inclua a tabela preenchida no relatório e justifique como o cálculo de cada sub-rede atende às necessidades de cada departamento. 

   

3. **Configuração de PCs**   Configure os PCs dos departamentos (ADM, Vendas e TI) com **IP, máscara, gateway e DNS** de acordo com o esquema de endereçamento. Mostre prints das configurações realizadas e explique como cada parâmetro garante a conectividade com a rede e o servidor.

   

4. **Teste de Conectividade**   Execute testes de **ping** entre os PCs e o servidor, além de acessar o site `www.lab3.com` pelo navegador. Documente os prints das respostas e justifique como esses testes comprovam que o esquema de endereçamento e as configurações de segurança foram aplicados corretamente.

5. Mostre, para cada departamento, como o prefixo escolhido atende ao número de hosts, ao *gateway* e (no caso dos switches) ao IP de gerência. Qual a folga de endereços de cada sub-rede?

## 7. Critérios de Avaliação

A avaliação será realizada com base na seguinte rubrica, totalizando exatamente 10,0 pontos:

| Critério de Avaliação | Pontuação Máxima |
| :--- | :---: |
| **Plano VLSM:** Tabela de Documentação das Redes preenchida e correta, sem sobreposição | 2,0 |
| **Tabela de Endereçamento e PCs:** IPs, máscaras, *gateways* e DNS coerentes com o plano | 1,5 |
| **Configuração do R1 e dos switches:** Segurança básica, banner, SSH e SVIs | 2,0 |
| **Conectividade:** `ping` entre todos os dispositivos e acesso a `www.lab3.com` | 1,5 |
| **Teste de SSH:** Acesso a um switch e a um roteador, com evidências | 1,0 |
| **Respostas às questões:** Análise fundamentada das questões da Seção 6 | 2,0 |
| **Total** | **10,0** |

## 8. Entregáveis

1. Tabela de Documentação das Redes e Tabela de Endereçamento preenchidas.
2. Arquivo `.pkt` configurado.
3. Evidências dos testes da Seção 5 (capturas de tela).
4. Respostas às 5 questões da Seção 6.

## 9. Conclusão

Neste experimento, o bloco `192.168.0.0/24` foi dividido com VLSM em sub-redes de tamanhos distintos (/25, /26 e dois /27), atendendo cada departamento sem sobreposição e sem desperdício, e os dispositivos receberam configurações de segurança de acesso. A prática consolida o cálculo de sub-redes e prepara o uso de VLANs no [Experimento 04](04_roteiro.md).

---

[← Anterior: Experimento 02](02_roteiro.md) | [Índice Geral (README)](../README.md) | [Próximo: Experimento 04 →](04_roteiro.md)
