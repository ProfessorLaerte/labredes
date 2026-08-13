# Roteiro do Experimento 03:

## Configurar Sub-redes com VLSM



### OBJETIVO

O objetivo deste experimento é aplicar a técnica de VLSM para planejar e configurar sub-redes em uma rede IPv4, garantindo que cada departamento receba 
a quantidade adequada de endereços IP, com eficiência e segurança na utilização dos recursos de endereçamento.

### INTRODUÇÃO TEÓRICA

O uso de CIDR e VLSM permite dividir uma rede em sub-redes de tamanhos diferentes, otimizando o espaço de endereçamento e evitando desperdício de IPs. 
Além disso, configurações de segurança como SSH e criptografia de senhas garantem conectividade confiável e proteção contra acessos não autorizados.



## Conceitos complementares

## CIDR

- **CIDR** significa *Classless Inter-Domain Routing*.
- A notação é escrita como `IP/prefixo`.
- O **prefixo** indica quantos bits (dos 32 do IPv4) são usados para identificar a rede.
- Os bits restantes são usados para endereçar **hosts**.



**Relação entre Máscara, Bits e Hosts**

| **CIDR** | **Máscara Decimal** | **Bits de Host** | **Hosts Utilizáveis** |
| -------- | ------------------- | ---------------- | --------------------- |
| **/8**   | 255.0.0.0           | 24               | 16.777.214            |
| **/16**  | 255.255.0.0         | 16               | 65.534                |
| **/24**  | 255.255.255.0       | 8                | 254                   |



## VLSM

- **Variable Length Subnet Mask (VLSM)** é a técnica de usar **máscaras de sub-rede de diferentes tamanhos** dentro da mesma rede.
- Enquanto o CIDR define um prefixo fixo para uma rede, o VLSM permite **dividir a rede em sub-redes menores**, cada uma com a máscara adequada ao número de hosts necessários.
- Isso evita desperdício de endereços IP, pois cada sub-rede recebe apenas o espaço que realmente precisa.

**Exemplo**:

- `/25` → 25 bits para rede, 7 bits para hosts → 2⁷ − 2=126 hosts utilizáveis.
- `/30` → 30 bits para rede, 2 bits para hosts → 2² − 2=2 hosts utilizáveis. 

**Exceções**:

- `/31`: usado em links ponto a ponto, permite 2 endereços sem broadcast.

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



## Benefícios do VLSM

- **Eficiência**: reduz desperdício de endereços IP.
- **Flexibilidade**: cada sub-rede pode ter o tamanho exato necessário.
- **Escalabilidade**: facilita o crescimento da rede sem precisar redesenhar tudo.



# Experimento

## Histórico/Cenário 

<img width="1110" height="491" alt="image" src="https://github.com/user-attachments/assets/63bd0377-8a2d-4ac2-9c25-cf18b4198c7a" />


No cenário fornecido, a conexão entre o roteadores R1 e o servidor estão configurados. 

Você deve criar um novo esquema de endereçamento IPv4 que acomode 4 sub-redes usando a rede 192.168.0.0/24. 

O departamento de TI precisa de 25 hosts. 

O departamento de vendas precisa de 50 hosts.

A sub-rede para a equipe administrativa precisa de 100 hosts. 

Uma sub-rede para convidados será futuramente adicionada para acomodar 25 hosts e deve ser planejada. 

Você também deve concluir as configurações básicas de segurança e as configurações de interface no R1. 

Em seguida, você definirá a interface SVI e as configurações básicas de segurança nos comutadores SW1, SW2 e SW3.



## Instruções

### Endereçamento IPv4

- Use **192.168.0.0/24** para criar sub-redes que atendem aos requisitos do host.

  - Administrativo: 100 hosts

  - Vendas: 50 hosts

  - TI: 25 hosts

  - Convidados futuramente adicionada: 25 hosts

- Documente as informações das sub-redes, nome da rede, endereço da rede, endereço de broadcast, quantidade de hosts, primeiro e ultimo endereço utilizável da rede, na Tabela de Documentação das Redes.

- Documente os endereços IPv4 que foram atribuídos na Tabela de Endereçamento.

  

### Configurações dos computadores

- Defina as configurações de endereço IPv4, máscara de sub-rede, gateway padrão e DNS server nos atribuídos dos PCs, usando seu esquema de endereçamento.

  

### Configurações de R1 e dos Swicthes

- Configure o nome do dispositivo conforme a Tabela de Endereçamento.

- Desative a pesquisa do DNS.

- Atribua **labredes** como a senha criptografada do enable.

- Criptografe todas as senhas em texto simples.

- Crie um banner para avisar às pessoas que o acesso não autorizado é proibido.

  ```
  Router(config)# banner motd #
  ATENÇÃO: Acesso restrito!
  Somente usuários autorizados podem entrar.
  Tentativas não autorizadas serao registradas.#
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

- A penas nos Switches

  - Configure a interface SVI VLAN1 com endereço e máscara de sub-rede IPv4 de acordo com o seu esquema de endereçamento.
  - Configure o gateway padrão.





## Tabela de Documentação das Redes

| **Nome da Rede** | **Endereço da Rede** | **Broadcast**   | **Máscara CIDR** | **Hosts Utilizáveis** | **Primeiro Host** | **Último Host** |
| ---------------- | -------------------- | --------------- | ---------------- | --------------------- | ----------------- | --------------- |
| Rede original    | 192.168.0.0          | 192.168.255.255 | /24              | 254                   | 192.168.0.1       | 192.168.0.254   |
| Administrativo   |                      |                 |                  |                       |                   |                 |
| Vendas           |                      |                 |                  |                       |                   |                 |
| TI               |                      |                 |                  |                       |                   |                 |
| Convidados       |                      |                 |                  |                       |                   |                 |



## Tabela de Endereçamento

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



### Requisitos de conectividade

- Usando navegadores Web nos computadores ADM, Vendas e TI, navegue para www.lab3.com.

- Todos os PCs devem poder executar ping em todos os outros dispositivos.

  

# Questões

1. **Teste de conexão SSH**   Realize um teste de conexão via SSH em um **switch** e em um **roteador**. Documente os **comandos utilizados** e as **saídas obtidas** (print da tela). Explique como o resultado confirma que o acesso remoto seguro foi configurado corretamente.

   

2. **Tabela de Documentação**   Preencha a **Tabela de Documentação das Redes** com os endereços de rede, broadcast, máscara CIDR, quantidade de hosts e intervalo de endereços utilizáveis. Inclua a tabela preenchida no relatório e justifique como o cálculo de cada sub-rede atende às necessidades de cada departamento. 

   

3. **Configuração de PCs**   Configure os PCs dos departamentos (ADM, Vendas e TI) com **IP, máscara, gateway e DNS** de acordo com o esquema de endereçamento. Mostre prints das configurações realizadas e explique como cada parâmetro garante a conectividade com a rede e o servidor.

   

4. **Teste de Conectividade**   Execute testes de **ping** entre os PCs e o servidor, além de acessar o site **[www.lab3.com](https://www.lab3.com)** pelo navegador. Documente os prints das respostas e justifique como esses testes comprovam que o esquema de endereçamento e as configurações de segurança foram aplicados corretamente.



---



EQUIPAMENTO E MATERIAL
Microcomputadores do laboratório.

---




BIBLIOGRAFIA   
[1] J. F. Kurose e K. W. Ross – Computer Networks: A Top-Down Approach. (5th ed.). Pearson
Addison-Wesley, 2009.   
[2] W. Stallings - Data and Computer Communications. Prentice-Hall, 2006.   
[3] F. C. Xavier - Roteadores Cisco. (2a ed.). Novatec, 2010.   
[4] Cisco Networking Academy, disponível em http://cisco.netacad.net.   
[5] Sites diversos sobre protocolo RIP na Internet.   

**Adatptado de www.netacad.com**
