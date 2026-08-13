# Roteiro do Experimento 02:

## Configurar Senhas Seguras e SSH



### OBJETIVO

O objetivo deste experimento é aplicar medidas básicas de segurança em dispositivos de rede Cisco, configurando senhas seguras e habilitando o acesso remoto via SSH. Dessa forma, busca-se garantir a proteção contra acessos não autorizados e preparar o ambiente para gerenciamento confiável e seguro.

### INTRODUÇÃO TEÓRICA

A configuração de senhas fortes e o uso do protocolo SSH são práticas essenciais de segurança em redes. Enquanto as senhas protegem o acesso local e remoto ao equipamento, o SSH substitui o Telnet ao oferecer criptografia nas comunicações, evitando que credenciais e dados trafeguem em texto claro. Além disso, o uso de interfaces lógicas como a VLAN1 e SVIs permite o gerenciamento remoto dos switches, reforçando a importância de boas práticas na administração de redes corporativas.



## Conceitos complementares

### Linhas de acesso ao Cisco IOS

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

  

<img width="1279" height="213" alt="Captura de tela de 2026-08-08 16-33-38" src="https://github.com/user-attachments/assets/adac3eaf-4dc7-4475-8684-86dd5bd1d366" />




### Interface VLAN1

- **VLAN1** é a VLAN padrão em switches Cisco.

- A interface VLAN1 é uma **interface lógica** usada para dar ao switch um endereço IP.

- Esse IP não é para encaminhar pacotes entre redes, mas sim para **gerenciamento remoto** (Telnet, SSH, SNMP).



### SVIs (Switch Virtual Interfaces)

- **SVIs** são interfaces virtuais criadas em VLANs no switch.
- Cada VLAN pode ter uma SVI associada, permitindo que o switch seja acessado via IP dentro daquela VLAN.
- Em switches camada 3, SVIs também podem ser usadas para **roteamento entre VLANs** (inter-VLAN routing).



### DESCRIÇÃO DA ATIVIDADE



1. Crie um cenário no Cisco Packet Tracer com os seguintes dispositivos:

   - 1 Roteador 1941

   - 1 Switch 2960

   - 1 Dispositivo final (PC/Laptop)

   - O cenário deverá ser montado e configurado da seguinte forma:

     <img width="458" height="352" alt="Captura de tela de 2026-08-08 15-36-25" src="https://github.com/user-attachments/assets/e75f7b9f-fddb-4cd3-878f-187e940b9e9d" />




| **Dispositivo** | **Interface** | **Endereço IP** | **Máscara de sub-rede** | **Gateway padrão** |
| --------------- | ------------- | --------------- | ----------------------- | ------------------ |
| RTAG0           | 0/0           | 172.16.1.1      | 255.255.255.0           | N/D                |
| PCA             | Placa de rede | 172.16.1.10     | 255.255.255.0           | 172.16.1.1         |
| SW1             | VLAN 1        | 172.16.1.2      | 255.255.255.0           | 172.16.1.1         |



O administrador de redes pediu que você prepare o RTA e SW1 para implantação. Antes de conectá-lo à rede, você deve ativar medidas de segurança.

### Instruções

#### Etapa 1: Implementar as Medidas Básicas de Segurança no Roteador

1. Configure o endereçamento IP em PCA de acordo com a Tabela de Endereçamento.

2. Acesse o console do roteador através do terminal em PC-A.

3. Configure o nome do host como RTA.

   ```
   Router>enable
   Router#conf t
   Router(config)#hostname RTA
   RTA(config)#^Z
   ```

4. Configure o endereçamento IP em RTA e ative a interface.

   ```
   RTA#conf t
   RTA(config)#int G0/0
   RTA(config-if)#ip addr 172.16.1.1 255.255.255.0 
   RTA(config-if)#no shut
   RTA(config)#^Z
   ```

5. Criptografe todas as senhas em texto simples.

   ```
   RTA#conf t
   RTA(config)#service password-encryption
   RTA(config)#^Z
   ```

6. Configure o comprimento mínimo para senhas para 10

   ```
   RTA(config)#security passwords min-length 10
   ```

7. Configure uma senha secreta forte de sua escolha. Observação: escolha uma senha que você se
   lembre ou você precisará redefinir a atividade se estiver bloqueado no dispositivo.

   ```
   RTA(config)#enable secret sua_senha
   ```

8. Desative a pesquisa de DNS. Para evitar que o sistema tente traduzir entradas erradas.

   ```
   RTA(config)#no ip domain-lookup
   ```

9. Configure o nome de domínio como labredes.com

   ```
   RTA(config)#ip domain-name labredes.com
   ```

10. Crie um usuário da escolha com uma senha forte.

    ```
    RTA(config)#username seu_usuario secret sua_senha
    ```

11. Gere chaves RSA de 1024 bits.

     ```
    RTA(config)# crypto key generate rsa
    How many bits in the modulus [512]: 1024
     ```

     **Listar chaves RSA**

     Use o comando específico para visualizar as chaves geradas.

     ```
    Router# show crypto key mypubkey rsa
     ```

     No **Cisco IOS**, você **não pode gerar uma chave RSA diferente para cada usuário**. O modelo de funcionamento é o seguinte:

     - A chave **RSA** é gerada **no dispositivo** (roteador ou switch) e associada ao **hostname** e ao **domínio** configurados.
     - Essa chave é usada pelo **servidor SSH** do equipamento para autenticar conexões seguras.
     - Todos os usuários que acessam via SSH compartilham a mesma chave pública do dispositivo.
     - O que se deve fazer é criar **usuários locais** com senhas diferentes:

     

12. Bloqueie durante três minutos qualquer pessoa que não conseguiu fazer login depois de quatro tentativas em um período de dois minutos.

     ```
    RTA(config)#login block-for 180 attempts 4 within 120
     ```

13. Configure as linhas VTY para o acesso por SSH e use os perfis locais de usuário local para autenticação.

     ```
    RTA(config)#line vty 0 15
    RTA(config-line)#transport input ssh
    RTA(config-line)#login local
     ```

14. Defina o tempo limite do modo EXEC para 6 minutos nas linhas VTY.

     ```
    RTA(config-line)# exec-timeout 6
     ```

15. Salve a configuração na NVRAM.

     ```
    RTA#write
    ou
    RTA#copy running-config startup-config 
     ```

16. Acesse o prompt de comando na área de trabalho do PCA para estabelecer uma conexão SSH com o RTA .

     ```cmd
    C:\ > ssh
    Packet Tracer PC SSH
    Usage: SSH -l username target
     ```

     

#### Etapa 2: Implementar as Medidas Básicas de Segurança no Switch

1. Clique em SW1 e selecione a guia CLI.

2. Configure o nome de host como SW1.

3. Configure o endereçamento IP em SW1 VLAN1 e ative a interface.

   ```
   SW1(config)#ip default-gateway 172.16.1.1
   ```

4. Desative todas as portas não utilizadas.

   Em um switch, é uma boa prática de segurança desabilitar portas não utilizadas.    

   Um método para fazer isso é simplesmente desligar cada porta com o comando 'shutdown'. Isso exigiria acessar cada porta individualmente.     

   Existe um método de atalho para fazer modificações em várias portas ao mesmo tempo usando o comando **interface range**. 

   No SW1, todas as portas, exceto FastEtherNet0/1 e GigabiteTherNet0/1, podem ser desativadas com o seguinte comando:

   ```
   SW1(config)#ip default-gateway 172.16.1.1
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

11. Configure as linhas VTY para o acesso por SSH e use os perfis locais de usuário local para autenticação.

12. Defina o tempo limite do modo EXEC para 6 minutos em todas as linhas VTY.

13. Salve a configuração na NVRAM.

    


# Questões

1. Mostre um print da configuração das senhas no roteador (VTY e enable secret). Explique como cada uma protege o acesso ao dispositivo.
2. Apresente um print da configuração da interface VLAN1 no switch com IP e máscara. Explique a função dessa interface.
3. Apresente um print da configuração do gateway no switch. Justifique por que o gateway seria necessário?
4. Capture a tela do PC-A estabelecendo conexão SSH com o roteador. Explique a diferença entre usar SSH e Telnet em termos de segurança.

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
