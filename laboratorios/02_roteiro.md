# Roteiro do Experimento 02 - Configurar Senhas Seguras e SSH

- **Instituição:** Universidade de Brasília (UnB)
- **Departamento:** Departamento de Engenharia Elétrica (ENE)
- **Curso:** Engenharia de Redes de Comunicação
- **Disciplina:** ENE0011 - Laboratório de Redes
- **Professor responsável:** Prof. Dr. Laerte Peotta de Melo
- **Tema:** Configuração básica de switches, senhas criptografadas e acesso remoto seguro via SSH

---

> **Recurso Didático:** o simulador [Simul-Net: SSH](https://peotta.github.io/Simul-Net/#/sim/ssh) apresenta, passo a passo, a negociação de algoritmos, a troca de chaves, a autenticação e os túneis do protocolo, o que complementa a comparação entre Telnet e SSH da Questão 4.

## 1. Objetivo

O objetivo deste experimento é aplicar medidas básicas de segurança em dispositivos de rede Cisco, configurando senhas seguras e habilitando o acesso remoto via SSH. Dessa forma, busca-se garantir a proteção contra acessos não autorizados e preparar o ambiente para gerenciamento confiável e seguro.

## 2. Fundamentação Teórica

A configuração de senhas fortes e o uso do protocolo SSH são práticas essenciais de segurança em redes. Enquanto as senhas protegem o acesso local e remoto ao equipamento, o SSH substitui o Telnet ao oferecer criptografia nas comunicações, evitando que credenciais e dados trafeguem em texto claro. Além disso, o uso de interfaces lógicas como a VLAN1 e SVIs permite o gerenciamento remoto dos switches, reforçando a importância de boas práticas na administração de redes corporativas.



### 2.1 Linhas de Acesso ao Cisco IOS

- **Console**

  - Linha física usada para acesso direto ao equipamento.
  - Configuração: `line console 0`.

- **VTY**

  - Linhas virtuais usadas para acesso remoto (Telnet/SSH).
  - Configuração: `line vty 0 4` (pode variar em quantidade, ex: 0 15).

- **Auxiliar**

  - Linha usada para acesso via porta auxiliar (geralmente com modem).
  - Configuração: `line aux 0`.

- **TTY**

  - Linhas de terminal assíncrono, usadas em conexões seriais mais antigas.
  - Configuração: `line tty 0`.

  

<img width="1279" height="213" alt="Linhas de acesso do Cisco IOS" src="https://github.com/user-attachments/assets/adac3eaf-4dc7-4475-8684-86dd5bd1d366" />




### 2.2 Interface VLAN1

- **VLAN1** é a VLAN padrão em switches Cisco.

- A interface VLAN1 é uma **interface lógica** usada para dar ao switch um endereço IP.

- Esse IP não é para encaminhar pacotes entre redes, mas sim para **gerenciamento remoto** (Telnet, SSH, SNMP).
- **Aspecto de segurança:** a VLAN 1 é o padrão de fábrica e transporta o tráfego de controle (CDP, STP). Em redes de produção, recomenda-se uma VLAN de gerência dedicada, como feito no Experimento 09.



### 2.3 SVIs (Switch Virtual Interfaces)

- **SVIs** são interfaces virtuais criadas em VLANs no switch.
- Cada VLAN pode ter uma SVI associada, permitindo que o switch seja acessado via IP dentro daquela VLAN.
- Em switches camada 3, SVIs também podem ser usadas para **roteamento entre VLANs** (inter-VLAN routing).

### 2.4 Normas de Referência

O acesso remoto seguro e a gerência dos equipamentos são tratados nas RFCs abaixo.

| Assunto | Norma | Aplicação neste experimento |
| :--- | :--- | :--- |
| Telnet (inseguro) | [**RFC 854**](https://datatracker.ietf.org/doc/html/rfc854) | Protocolo de terminal remoto que trafega credenciais e dados em texto claro; deve ser desabilitado |
| Arquitetura do SSH | [**RFC 4251**](https://datatracker.ietf.org/doc/html/rfc4251) | Modelo geral do *Secure Shell* (SSH), seus serviços e considerações de segurança |
| Autenticação no SSH | [**RFC 4252**](https://datatracker.ietf.org/doc/html/rfc4252) | Autenticação do usuário por senha e por chave pública (`login local` e usuário local) |
| Transporte do SSH | [**RFC 4253**](https://datatracker.ietf.org/doc/html/rfc4253) | Negociação de algoritmos, troca de chaves, criptografia e integridade da sessão (versão 2 do protocolo) |
| Canais do SSH | [**RFC 4254**](https://datatracker.ietf.org/doc/html/rfc4254) | Multiplexação de canais sobre a conexão, como a sessão de terminal nas linhas VTY |
| Gerência remota | [**RFC 3411**](https://datatracker.ietf.org/doc/html/rfc3411) | Arquitetura do *Simple Network Management Protocol* (SNMP), citado como serviço de gerência acessado pela interface VLAN1 |

## 3. Topologia e Cenário



1. Crie um cenário no Cisco Packet Tracer com os seguintes dispositivos:

   - 1 Roteador 1941

   - 1 Switch 2960

   - 1 Dispositivo final (PC/Laptop)

   - O cenário deverá ser montado e configurado da seguinte forma:

     <img width="458" height="352" alt="Topologia do Experimento 02 no Packet Tracer" src="https://github.com/user-attachments/assets/e75f7b9f-fddb-4cd3-878f-187e940b9e9d" />




| **Dispositivo** | **Interface** | **Endereço IP** | **Máscara de sub-rede** | **Gateway padrão** |
| --------------- | ------------- | --------------- | ----------------------- | ------------------ |
| RTA             | G0/0          | 172.16.1.1      | 255.255.255.0           | N/D                |
| PC-A             | Placa de rede | 172.16.1.10     | 255.255.255.0           | 172.16.1.1         |
| SW1             | VLAN 1        | 172.16.1.2      | 255.255.255.0           | 172.16.1.1         |



O administrador de redes pediu que você prepare o RTA e SW1 para implantação. Antes de conectá-lo à rede, você deve ativar medidas de segurança.

## 4. Roteiro Prático Passo a Passo

### 4.1 Etapa 1: Implementar as Medidas Básicas de Segurança no Roteador

1. Configure o endereçamento IP em PC-A de acordo com a Tabela de Endereçamento.

2. Acesse o console do roteador através do terminal em PC-A.

3. Configure o nome do host como RTA.

   ```cisco
   Router>enable
   Router#conf t
   Router(config)#hostname RTA
   RTA(config)#^Z
   ```

4. Configure o endereçamento IP em RTA e ative a interface.

   ```cisco
   RTA#conf t
   RTA(config)#int G0/0
   RTA(config-if)#ip addr 172.16.1.1 255.255.255.0 
   RTA(config-if)#no shut
   RTA(config)#^Z
   ```

5. Criptografe todas as senhas em texto simples.

   ```cisco
   RTA#conf t
   RTA(config)#service password-encryption
   RTA(config)#^Z
   ```

6. Configure o comprimento mínimo para senhas para 10

   ```cisco
   RTA(config)#security passwords min-length 10
   ```

7. Configure uma senha secreta forte de sua escolha. Observação: escolha uma senha que você se
   lembre ou você precisará redefinir a atividade se estiver bloqueado no dispositivo.

   ```cisco
   RTA(config)#enable secret sua_senha
   ```

8. Desative a pesquisa de DNS. Para evitar que o sistema tente traduzir entradas erradas.

   ```cisco
   RTA(config)#no ip domain-lookup
   ```

9. Configure o nome de domínio como labredes.com

   ```cisco
   RTA(config)#ip domain-name labredes.com
   ```

10. Crie um usuário da escolha com uma senha forte.

    ```cisco
    RTA(config)#username seu_usuario secret sua_senha
    ```

11. Gere chaves RSA de 1024 bits.

     ```cisco
    RTA(config)# crypto key generate rsa
    How many bits in the modulus [512]: 1024
     ```

     **Listar chaves RSA**

     Use o comando específico para visualizar as chaves geradas.

     ```cisco
    Router# show crypto key mypubkey rsa
     ```

     No **Cisco IOS**, você **não pode gerar uma chave RSA diferente para cada usuário**. O modelo de funcionamento é o seguinte:

     - A chave **RSA** é gerada **no dispositivo** (roteador ou switch) e associada ao **hostname** e ao **domínio** configurados.
     - Essa chave é usada pelo **servidor SSH** do equipamento para autenticar conexões seguras.
     - Todos os usuários que acessam via SSH compartilham a mesma chave pública do dispositivo.
     - O que se deve fazer é criar **usuários locais** com senhas diferentes:

     

12. Bloqueie durante três minutos qualquer pessoa que não conseguiu fazer login depois de quatro tentativas em um período de dois minutos.

     ```cisco
    RTA(config)#login block-for 180 attempts 4 within 120
     ```

13. Habilite o SSH versão 2 e configure as linhas VTY para o acesso por SSH, usando os perfis de usuário local para autenticação.

     ```cisco
    RTA(config)#ip ssh version 2
    RTA(config)#line vty 0 15
    RTA(config-line)#transport input ssh
    RTA(config-line)#login local
     ```

14. Defina o tempo limite do modo EXEC para 6 minutos nas linhas VTY.

     ```cisco
    RTA(config-line)# exec-timeout 6
     ```

15. Salve a configuração na NVRAM.

     ```cisco
    RTA#write
    ou
    RTA#copy running-config startup-config 
     ```

16. Acesse o prompt de comando na área de trabalho do PC-A para estabelecer uma conexão SSH com o RTA .

     ```cmd
    C:\ > ssh
    Packet Tracer PC SSH
    Usage: SSH -l username target
     ```

     

### 4.2 Etapa 2: Implementar as Medidas Básicas de Segurança no Switch

1. Clique em SW1 e selecione a guia CLI.

2. Configure o nome de host como SW1.

3. Configure o endereçamento IP em SW1 VLAN1 e ative a interface.

   ```cisco
   SW1(config)#interface vlan 1
   SW1(config-if)#ip address 172.16.1.2 255.255.255.0
   SW1(config-if)#no shutdown
   SW1(config-if)#exit
   SW1(config)#ip default-gateway 172.16.1.1
   ```

4. Desative todas as portas não utilizadas.

   Em um switch, é uma boa prática de segurança desabilitar portas não utilizadas.    

   Um método para fazer isso é simplesmente desligar cada porta com o comando 'shutdown'. Isso exigiria acessar cada porta individualmente.     

   Existe um método de atalho para fazer modificações em várias portas ao mesmo tempo usando o comando **interface range**. 

   No SW1, todas as portas, exceto FastEthernet0/1 e GigabitEthernet0/1, podem ser desativadas com o seguinte comando:

   ```cisco
   SW1(config)#interface range f0/2-24, g0/2
   SW1(config-if-range)#shutdown
   ```

   O comando usou o intervalo de portas de 2-24 para as portas FastEthernet e, em seguida, um único intervalo de porta de Gigabitethernet0/2.

5. Criptografe todas as senhas em texto simples.

6. Configure uma senha secreta forte de sua escolha, para o enable.

7. Desative a pesquisa de DNS.

8. Configure o nome de domínio como labredes.com

9. Crie um usuário da escolha com uma senha forte.

10. Gere chaves RSA de 1024 bits.

11. Habilite o SSH versão 2 (`ip ssh version 2`), configure as linhas VTY para o acesso por SSH (`transport input ssh`) e use os perfis de usuário local para autenticação (`login local`).

12. Defina o tempo limite do modo EXEC para 6 minutos em todas as linhas VTY.

13. Salve a configuração na NVRAM.

    


## 5. Verificação e Testes

| Comando / ação | Equipamento | O que observar |
| :--- | :---: | :--- |
| `show running-config` | RTA e SW1 | Senhas de usuário e `enable secret` criptografadas (hash `$1$`, `$5$` ou tipo 7); ausência de senhas em texto claro |
| `show ip ssh` | RTA e SW1 | `SSH Enabled - version 2.0` |
| `show crypto key mypubkey rsa` | RTA e SW1 | Chave RSA de uso geral com 1024 bits |
| `show running-config \| section line vty` | RTA e SW1 | `transport input ssh`, `login local` e `exec-timeout 6 0` |
| `show ip interface brief` | RTA e SW1 | `GigabitEthernet0/0` e `Vlan1` com os endereços da tabela e estado `up/up` |
| `ping 172.16.1.1` | PC-A | Resposta do roteador |
| `ssh -l <usuario> 172.16.1.1` e `ssh -l <usuario> 172.16.1.2` | PC-A | Solicitação de senha e acesso ao modo EXEC |

## 6. Questões para Análise

1. Mostre um print da configuração das senhas no roteador (VTY e enable secret). Explique como cada uma protege o acesso ao dispositivo.
2. Apresente um print da configuração da interface VLAN1 no switch com IP e máscara. Explique a função dessa interface.
3. Apresente um print da configuração do gateway no switch. Justifique por que o gateway seria necessário?
4. Capture a tela do PC-A estabelecendo conexão SSH com o roteador. Explique a diferença entre usar SSH e Telnet em termos de segurança.

5. Que informações trafegam em texto claro em uma sessão Telnet e quais permanecem protegidas em uma sessão SSH? Por que o SSH exige o nome de domínio e a chave RSA?
6. Qual o efeito de `login block-for 180 attempts 4 within 120` e contra qual ataque ele atua?

## 7. Critérios de Avaliação

A avaliação será realizada com base na seguinte rubrica, totalizando exatamente 10,0 pontos:

| Critério de Avaliação | Pontuação Máxima |
| :--- | :---: |
| **Roteador:** Nome, endereçamento, senhas criptografadas e `enable secret` | 2,0 |
| **Acesso SSH no roteador:** Domínio, chave RSA, usuário local, VTY e SSH versão 2 | 2,0 |
| **Switch:** Nome, SVI da VLAN 1, *gateway*, portas desativadas e SSH | 2,0 |
| **Verificação:** Evidências dos comandos da Seção 5 e conexão SSH a partir do PC-A | 1,5 |
| **Respostas às questões:** Análise fundamentada das questões da Seção 6 | 2,0 |
| **Qualidade das evidências:** Capturas de tela legíveis | 0,5 |
| **Total** | **10,0** |

## 8. Entregáveis

1. Capturas de tela dos comandos de verificação da Seção 5, de RTA e SW1.
2. Captura da sessão SSH iniciada em PC-A.
3. Arquivo `.pkt` configurado.
4. Respostas às 6 questões da Seção 6.

## 9. Conclusão

Neste experimento, o roteador e o switch receberam as medidas básicas de proteção do acesso administrativo: senhas criptografadas, autenticação local, bloqueio por tentativas falhas, tempo limite de sessão e acesso remoto exclusivamente por SSH. Essas práticas são pré-requisito para os experimentos seguintes.

---

[← Anterior: Experimento 01](01_roteiro.md) | [Índice Geral (README)](../README.md) | [Próximo: Experimento 03 →](03_roteiro.md)
