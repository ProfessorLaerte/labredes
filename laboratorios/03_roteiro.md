# Roteiro do Experimento 03 - Configurar Sub-redes com VLSM

- **Instituição:** Universidade de Brasília (UnB)
- **Departamento:** Departamento de Engenharia Elétrica (ENE)
- **Curso:** Engenharia de Redes de Comunicação
- **Disciplina:** ENE0011 - Laboratório de Redes
- **Professor responsável:** Prof. Dr. Laerte Peotta de Melo
- **Tema:** Endereçamento IPv4, dimensionamento de blocos CIDR e projeto de sub-redes com VLSM

---

## 1. Objetivo

O objetivo deste experimento é aplicar a técnica de VLSM para planejar e configurar sub-redes em uma rede IPv4, garantindo que cada departamento receba 
a quantidade adequada de endereços IP, com eficiência e segurança na utilização dos recursos de endereçamento.

## 2. Fundamentação Teórica

O uso de CIDR e VLSM permite dividir uma rede em sub-redes de tamanhos diferentes, otimizando o espaço de endereçamento e evitando desperdício de IPs. 
Além disso, configurações de segurança como SSH e criptografia de senhas garantem conectividade confiável e proteção contra acessos não autorizados.



### 2.1 CIDR

- **CIDR** significa *Classless Inter-Domain Routing* ([**RFC 4632**](https://datatracker.ietf.org/doc/html/rfc4632)).
- A notação é escrita como `IP/prefixo`.
- O **prefixo** indica quantos bits (dos 32 do IPv4) são usados para identificar a rede.
- Os bits restantes são usados para endereçar **hosts**.



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

- `/25` → 25 bits para rede, 7 bits para hosts → 2⁷ − 2=126 hosts utilizáveis.
- `/30` → 30 bits para rede, 2 bits para hosts → 2² − 2=2 hosts utilizáveis. 

**Exceções**:

- `/31`: usado em links ponto a ponto, permite 2 endereços sem broadcast ([**RFC 3021**](https://datatracker.ietf.org/doc/html/rfc3021)).

- `/32`: identifica apenas um host específico, sem espaço para outros.

  

**Relação entre Máscara, Bits e Hosts**

| **CIDR** | **Máscara Decimal** | **Bits para Hosts** | **Hosts Utilizáveis** |
| -------- | ------------------- | ------------------- | --------------------- |
| **/25**  | 255.255.255.128     | 7                   | 126                   |
| **/26**  | 255.255.255.192     | 6                   | 62                    |
| **/27**  | 255.255.255.224     | 5                   | 30                    |
| **/28**  | 255.255.255.240     | 4                   | 14                    |
| **/29**  | 255.255.255.248     | 3                   | 6                     |
| **/30**  | 255.255.255.252     | 2                   | 2                     |
| **/31**  | 255.255.255.254     | 1                   | 2 (ponto a ponto)     |
| **/32**  | 255.255.255.255     | 0                   | 1 (host único)        |



### 2.3 Benefícios do VLSM

- **Eficiência**: reduz desperdício de endereços IP.
- **Flexibilidade**: cada sub-rede pode ter o tamanho exato necessário.
- **Escalabilidade**: facilita o crescimento da rede sem precisar redesenhar tudo.



## 3. Topologia e Cenário

### 3.1 Histórico e Cenário

<img width="1110" height="491" alt="Topologia do Experimento 03 no Packet Tracer" src="https://github.com/user-attachments/assets/63bd0377-8a2d-4ac2-9c25-cf18b4198c7a" />


No cenário fornecido ([Baixar arquivo do laboratório](./topologias/experimento_03_nao_configurado.pkt)), as conexões entre o roteador R1 e o servidor já estão configuradas. 

Você deve criar um novo esquema de endereçamento IPv4 que acomode 4 sub-redes usando a rede 192.168.0.0/24. 

O departamento de TI precisa de 25 hosts. 

O departamento de vendas precisa de 50 hosts.

A sub-rede para a equipe administrativa precisa de 100 hosts. 

Uma sub-rede para convidados será futuramente adicionada para acomodar 25 hosts e deve ser planejada. 

Você também deve concluir as configurações básicas de segurança e as configurações de interface no R1. 

Em seguida, você definirá a interface SVI e as configurações básicas de segurança nos comutadores SW1, SW2 e SW3.



### 3.2 Tabela de Documentação das Redes

| **Nome da Rede** | **Endereço da Rede** | **Broadcast**   | **Máscara CIDR** | **Hosts Utilizáveis** | **Primeiro Host** | **Último Host** |
| ---------------- | -------------------- | --------------- | ---------------- | --------------------- | ----------------- | --------------- |
| Rede original    | 192.168.0.0          | 192.168.0.255   | /24              | 254                   | 192.168.0.1       | 192.168.0.254   |
| Administrativo   |                      |                 |                  |                       |                   |                 |
| Vendas           |                      |                 |                  |                       |                   |                 |
| TI               |                      |                 |                  |                       |                   |                 |
| Convidados       |                      |                 |                  |                       |                   |                 |



### 3.3 Tabela de Endereçamento

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



## 4. Roteiro Prático Passo a Passo

### 4.1 Endereçamento IPv4

- Use **192.168.0.0/24** para criar sub-redes que atendem aos requisitos do host.

  - Administrativo: 100 hosts

  - Vendas: 50 hosts

  - TI: 25 hosts

  - Convidados futuramente adicionada: 25 hosts

- Documente as informações das sub-redes, nome da rede, endereço da rede, endereço de broadcast, quantidade de hosts, primeiro e ultimo endereço utilizável da rede, na Tabela de Documentação das Redes.

- Documente os endereços IPv4 que foram atribuídos na Tabela de Endereçamento.

  

### 4.2 Configurações dos Computadores

- Defina as configurações de endereço IPv4, máscara de sub-rede, gateway padrão e DNS server nos atribuídos dos PCs, usando seu esquema de endereçamento.

  

### 4.3 Configurações de R1 e dos Switches

- Configure o nome do dispositivo conforme a Tabela de Endereçamento.

- Desative a pesquisa do DNS.

- Atribua **labredes** como a senha criptografada do enable.

- Criptografe todas as senhas em texto simples.

- Crie um banner para avisar às pessoas que o acesso não autorizado é proibido.

  ```cisco
  Router(config)# banner motd #
  ATENÇÃO: Acesso restrito!
  Somente usuários autorizados podem entrar.
  Tentativas não autorizadas serão registradas.#
  ```

- Configure o SSH:

  - Defina o nome de domínio como labredes.com
  - Gere uma chave RSA de 1024 bits.
  - Configure as linhas VTY para acesso SSH.
  - Use perfis de usuário local para autenticação.

- Criptografe todas as senhas em texto simples.

- Configure o console e as linhas VTY para encerrar sessão após cinco minutos de inatividade.

- Apenas em R1

  - Configure e ative todas as interfaces que se conectam aos switches das sub-redes de acordo com seu planejamento.
  - Exija que um mínimo de 10 caracteres seja usado para todas as senhas.
  - Bloqueie durante três minutos qualquer pessoa que não conseguiu fazer login depois de quatro tentativas em um período de dois minutos.

- Apenas nos Switches

  - Configure a interface SVI VLAN1 com endereço e máscara de sub-rede IPv4 de acordo com o seu esquema de endereçamento.
  - Configure o gateway padrão.





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

Neste experimento, o bloco `192.168.0.0/24` foi dividido com VLSM em sub-redes de tamanhos distintos (/25, /26 e dois /27), atendendo cada departamento sem sobreposição e sem desperdício, e os dispositivos receberam configurações de segurança de acesso. A prática consolida o cálculo de sub-redes e prepara o uso de VLANs no Experimento 04.

---

[← Anterior: Experimento 02](02_roteiro.md) | [Índice Geral (README)](../README.md) | [Próximo: Experimento 04 →](04_roteiro.md)
