# Roteiro do Experimento 01:

## Introdução aos dispositivos de Redes



### OBJETIVO

Conhecer os dispositivos básicos de rede. Realizar operações básicas em um roteador CISCO.

### INTRODUÇÃO TEÓRICA

Switch. Roteador. Comparação entre os dispositivos de rede. Roteadores Cisco. Sistema
Operacional Cisco IOS (Internetworking Operating System).

### DESCRIÇÃO DA ATIVIDADE

1. Crie um cenário no cisco Packet Tracer com os seguintes dispositivos:
   - 2 Roteadores (1841)
   - 2 Switches (2960)
   - 2 Dispositivos finais (PCs/Laptops)
   - O cenário deverá ser montado e configurado da seguinte forma:

<img width="924" height="492" alt="Captura de tela de 2026-08-04 15-52-58" src="https://github.com/user-attachments/assets/0bf34e6e-11b7-4ebc-9215-0194af1df709" />

| **Dispositivo** | **Interface** | **Endereço IP** | **Máscara de Sub-rede** |
| --------------- | ------------- | --------------- | ----------------------- |
| R1              | Fa0/1         | 10.0.0.1        | 255.255.255.0           |
| R1              | Fa0/0         | 192.168.0.1     | 255.255.255.0           |
| PC0             | Fa0           | 10.0.0.10       | 255.255.255.0           |
| R2              | Fa0/1         | 172.16.0.1      | 255.255.0.0             |
| R2              | Fa0/0         | 192.168.0.2     | 255.255.255.0           |
| Laptop0         | Fa0           | 172.16.0.10     | 255.255.0.0             |



### Configuração

- PCs e Laptops devem ser configurados utilizando a interface gráfica.

- Swicthes não precisam ser configurados .

  

### Configuração dos Roteadores

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

   ```
   enable
   conf t
   int fa0/0
   ip add 192.168.0.1 255.255.255.0
   no shut
   ^z
   write
   ```

   **_Agora o Roteador R1 está configurado, repita os mesmos passos para o roteador R2, adaptando as informações (as senhas podem ser as mesmas)_**



### Coleta de informações



3. Colete as informações abaixo sobre o equipamento (R1 ou R2) e a sua configuração. 

   - Utilize os comandos:
     - `show version`
     - `show interfaces`
     - `show ip interface`
     - `show ip interface brief`


     - Versão do sistema operacional do Cisco IOS;


     - Forma pela qual o roteador foi reinicializado pela última vez;


     - Interfaces de Rede existentes no roteador: Status administrativo e operacional de Camada
       1 (física) e 2 (enlace)


​       



4. O protocolo CDP (Cisco Discovery Protocol) é um protocolo proprietário da Cisco capaz de obter informações diversas sobre equipamentos Cisco que estejam diretamente conectados ao seu equipamento. Verifique se existe algum equipamento vizinho da Cisco e identifique quais informações podem ser obtidas.

   - Utilize os comandos: 

     ```
     show cdp
     
     show cdp neighbors
     ```

     


5. Identifique quais são os protocolos roteados de camada 3 que estão habilitados no roteador. 

   - Utilize o comando:

     ```
     show protocols
     ```




6. Tabelas de roteamento. Visualize a tabela de roteamento dos roteadores R1 e R2. Existem rotas para alguma rede? Caso existam, como essas rotas foram inseridas na tabela?

   - Utilize o comando:

     ```
     show ip route
     ```

  

### Testes de conectividade

1. Realize testes de conectividade entre os dispositivos finais:

   - PC0 para Laptop
   - Utilize o comando `ping` no *Command Prompt* do PC0.
     - Por que o teste não funcionou? O que falta?

   

2. Inserindo rota padrão nos roteadores

   - Utilize os comandos:

     - Em R1:

       ```
       enable
       conf t
       ip route 0.0.0.0 0.0.0.0 192.168.0.2
       ```

       

     - Em R2:

       ```
       enable
       conf t
       ip route 0.0.0.0 0.0.0.0 10.0.0.1
       ```

   

3. Verifique novamente as tabelas de roteamento e comente como as rotas aparecem.

   

4. Refaça os testes novamente e comente os resultados

   

**Capturas de telas devem ser utilizadas em TODAS as respostas das questões**

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
