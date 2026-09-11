# Roteiro do Experimento 05:

## Topologia Redundante com VLANs, EtherChannel, Roteamento Inter-VLAN e Hardening de Camada 2



### OBJETIVO

Implementar uma topologia redundante em laboratório utilizando **VLANs**, **EtherChannel**, **roteamento inter-VLAN** e mecanismos de **segurança de camada 2**, de forma a garantir maior disponibilidade, desempenho e proteção da rede.

## Conceitos complementares

- **Agregação de links  - EtherChannel**

  - **Agregação de Links**   É a técnica de combinar múltiplas interfaces físicas em um único enlace lógico, aumentando a largura de banda e oferecendo redundância. Na Cisco, isso é implementado por meio do **EtherChannel**, que garante maior desempenho e resiliência da rede.

  - O **EtherChannel** é uma tecnologia de agregação de links utilizada em switches Cisco que permite combinar várias interfaces físicas em um único enlace lógico.

  - Ele cria um **Port-Channel**, que é visto pelo sistema como uma única interface.

  - Isso aumenta a **largura de banda** disponível e fornece **redundância**, já que, se uma porta falhar, as demais continuam funcionando.

  - Simplifica a configuração de VLANs e trunking, pois todas as portas agregadas compartilham os mesmos parâmetros.

  - Modos de operação

    - **On**: configuração estática, sem protocolo de negociação.

    - **PAgP**: protocolo proprietário da Cisco para negociação automática.

    - **LACP**: protocolo padrão IEEE 802.3ad, usado para interoperabilidade entre diferentes fabricantes.

  - Benefícios

    - Melhor utilização dos recursos físicos.
    - Redução de loops e simplificação da topologia.
    - Maior resiliência e desempenho em redes corporativas.

- **DHCP Snooping**

  - É um recurso de segurança que protege contra servidores DHCP não autorizados. Ele monitora as mensagens DHCP na rede e só permite respostas vindas de portas confiáveis, evitando que dispositivos maliciosos forneçam endereços IP falsos.

  - Ele atua como um **firewall para mensagens DHCP**, permitindo apenas respostas vindas de portas confiáveis.

  - Cria uma **tabela de ligações IP–MAC–Porta** baseada nas concessões legítimas do servidor DHCP.
  - Essa tabela é usada por outros recursos de segurança, como o **Dynamic ARP Inspection (DAI)**.

  - **Portas confiáveis (trusted)**: normalmente conectadas ao servidor DHCP ou ao uplink para o core.

  - **Portas não confiáveis (untrusted)**: conectadas a dispositivos finais, onde não se espera que haja servidores DHCP.

  - Pacotes DHCP vindos de portas não confiáveis são bloqueados, evitando ataques como *DHCP spoofing*.

  - Benefícios

    - Garante que apenas servidores DHCP legítimos distribuam endereços IP.

    - Evita que atacantes forneçam configurações falsas de rede.

    - Serve de base para outras proteções, como validação de ARP.

- **Dynamic ARP Inspection (DAI)** 

  - Funciona em conjunto com o DHCP Snooping, validando pacotes ARP com base na tabela de ligações IP-MAC criada pelo DHCP. Isso impede ataques de **ARP Spoofing**, garantindo que apenas associações legítimas sejam aceitas na rede.

  - O DAI valida pacotes ARP recebidos na rede, garantindo que apenas associações legítimas entre **IP e MAC** sejam aceitas.

  - Ele utiliza a tabela de ligações criada pelo **DHCP Snooping** como referência para verificar se o dispositivo realmente possui o endereço IP que afirma ter.

  - **Portas confiáveis (trusted)**: normalmente conectadas a switches ou servidores legítimos.

  - **Portas não confiáveis (untrusted)**: conectadas a dispositivos finais, onde o risco de ataques é maior.

  - Pacotes ARP inválidos ou não correspondentes à tabela de ligações são descartados.

  - Benefícios

    - Impede ataques de **ARP spoofing**, que poderiam redirecionar tráfego para dispositivos maliciosos.

    - Garante maior integridade na comunicação entre hosts dentro da LAN.

    - Trabalha em conjunto com o DHCP Snooping para formar uma camada de defesa contra ataques de falsificação de identidade na rede.



### DESCRIÇÃO DA ATIVIDADE

1. Cenário:
   1. Switch0 Core - vlan100
   2. Switch1 Acesso - Vlan10
   3. Switch2 Acesso - Vlan20
   4. Roteador Router1 - GW das vlans
   5. Dispositivos finais as vlans 10 e 20
2. Escolher os blocos de redes que serão utilizados
3. Configurar agregação de links (EtherChannels) entre os switches de acesso e o core
4. Configurar sub-interfaces do roteador para servirem de gateway para as vlans
5. Configurar DHCP server nos swicthes de acesso excluindo os endereços do 1 ao 49
6. Implementar DHCP snooping e ARP inspection
7. Configurar SVI para gerenciamento das vlans
8. Desligar portas não ativas
9. Configurar SSH.

<img width="819" height="457" alt="Captura de tela de 2026-09-07 19-31-38" src="https://github.com/user-attachments/assets/0ee450c5-b149-4766-94eb-67e6c008fa7d" />






### Instruções

#### Etapa 1:   Configuração do Switch1 (VLAN 10, DHCP, SSH, EtherChannels, Snooping e DAI)

1. Nome do dispositivo

   ```
   enable
   configure terminal
   hostname Switch1
   ```

2. Configuração do Acesso SSH e Usuário e segurança

   ```
   ip domain-name labredes.com
   service password-encryption
   enable secret sua_senha
   no ip domain-lookup
   username seu_usuario secret sua_senha
   crypto key generate rsa
   How many bits in the modulus [512]: 1024
   line vty 0 15
   transport input ssh
   login local
   exec-timeout 6
   banner motd #
   ATENÇÃO: Acesso restrito!
   Somente usuários autorizados podem entrar.
   Tentativas não autorizadas serao registradas.#
   ```

   

3. Configuração da VLAN e SVI de Gerenciamento

   ```
   vlan 10
   name VLAN10
   vlan 20
   name VLAN20
   vlan 100
   name VLAN100
   ```

   ```
   interface vlan 10
   ip address x.x.x.x x.x.x.x
   no shutdown
   exit
   ip default-gateway x.x.x.x
   ```

   Configuração das portas de acesso dos PCs (Fa0/3 a Fa0/13)

   ```
   interface range FastEthernet0/x - x
   switchport mode access
   switchport access vlan 10
   ```

4. Configuração do Servidor DHCP (Excluindo IPs .1 a .49)

   ```
   ip dhcp excluded-address x.x.x.1 x.x.x.49
   ip dhcp pool POOL_VLAN10
   network x.x.x.x x.x.x.x
   default-router x.x.x.x
   dns-server x.x.x.x
   ```

5. SEGURANÇA: DHCP Snooping e Dynamic ARP Inspection (DAI) 

   Desativa a Opção 82 (necessário no Packet Tracer para evitar rejeição de pacotes)

   ```
   no ip dhcp snooping information option
   ```

   Ativa DHCP Snooping globalmente e nas VLANs

   ```
   ip dhcp snooping
   ip dhcp snooping vlan 10,20,100
   ```

   Ativa Dynamic ARP Inspection (DAI) nas VLANs

   ```
   ip arp inspection vlan 10,20,100
   ```

6. Configuração EtherChannel 1 (com Switch1 Fa0/1 e Fa0/2 com Switch0: Fa0/1 e Fa0/2)

   ```
   interface range FastEthernet0/1 - 2
   ip dhcp snooping trust
   ip arp inspection trust
   channel-group 1 mode active
   exit
   interface Port-channel 1
   switchport mode trunk
   exit
   ```

   O DAI utiliza a tabela do DHCP Snooping para validar pacotes ARP.

   Em portas do tipo *Trunk* ou *EtherChannel* que ligam switches e roteadores, **é obrigatório** manter o comando `ip arp inspection trust`.

   Se o EtherChannel não estiver marcado como confiável para o DAI (`ip arp inspection trust`), o switch começará a descartar as requisições ARP que vêm do roteador e dos outros switches, derrubando a comunicação e o ping entre os computadores das VLANs 10 e 20.

   

   Vericando configuração etherchannel

   ```
   show etherchannel summary
   !ou
   show etherchannel port-channel
   ```

   

7. Desligando portas não utilizadas

   ```
   int range fa0/5-24, g0/1-2
   shut
   ```

   

   **SALVAR AS CONFIGURAÇÕES**

   **Repita as mesmas configurações no switch2 e no switch0, adaptando os comandos para os parâmetros corretos.**

   **Obs.:**

   * **switch0 não tem DHCP Server ativo nem portas de acesso**

   * **cada etherchannel deve receber um número próprio que deve ser o mesmo nas duas pontas**

   

   Conexão com o Switch0 (Trunk na porta g0/1) <---> Roteador 

   ```
   interface g0/1
    switchport mode trunk
    ip dhcp snooping trust
    ip arp inspection trust
   ```

   

#### Etapa 2: Configuração do Router1 (Roteamento Inter-VLAN / Router-on-a-Stick)

1. Nome do dispositivo

   ```
   enable
   configure terminal
   hostname Router1
   ```

2. Sub-interface VLAN 10

   ```
   interface g0/0.10
   encapsulation dot1Q 10
   ip address x.x.x.x x.x.x.x
   ```

3. Sub-interface VLAN 20

   ```
   interface g0/0.20
   encapsulation dot1Q 20
   ip address x.x.x.x x.x.x.x
   ```

4. Sub-interface VLAN 100

   ```
   interface g0/0.100
   encapsulation dot1Q 100
   ip address x.x.x.x x.x.x.x
   ```

5. Ativação da interface física

   ```
   interface g0/0
   no shutdown
   ```

   

   


# Questões

1. **EtherChannel em funcionamento**   Configure o EtherChannel entre o Switch1 e o Switch0. Em seguida, execute o comando `show etherchannel summary`.
   - Pergunta: O Port-Channel aparece como ativo?
   - Evidência: Capture a tela mostrando o resultado do comando.
   - Verifique a Bandwidth/Largura de Banda do enlace com o comando `show interfaces port-channel 1`
     
2. **DHCP Snooping aplicado**   Após a configuração do DHCP Snooping. Conecte um servidor DHCP falso em uma porta não confiável e observe o comportamento.
   - Pergunta: O switch bloqueou as mensagens DHCP vindas da porta não confiável?
   - Evidência: Capture a tela mostrando a tabela de bindings (`show ip dhcp snooping binding`) e o teste com o servidor falso.

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

