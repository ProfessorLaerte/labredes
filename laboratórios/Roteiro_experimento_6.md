# Roteiro do Experimento 06

## Desafio de resolução de problemas.

## Objetivo

O estudante deve **diagnosticar e corrigir falhas de configuração** em uma rede previamente montada no Cisco Packet Tracer.
O foco é desenvolver habilidades práticas de:

- Identificação de problemas de conectividade.
  
- Testes e validação de hipóteses.
  
- Aplicação de soluções técnicas em VLANs, DHCP, EtherChannel e acesso remoto.
  
- Documentação clara do processo de troubleshooting.

## Cenário
<img width="1767" height="685" alt="image" src="https://github.com/user-attachments/assets/3723416e-7a83-4e60-83c4-e18352e46fea" />

## Descrição da Atividade

O arquivo `.pkt` fornecido contém uma rede dividida em três andares, com computadores, impressoras, switches e um roteador configurados.
Apesar da configuração inicial, **existem falhas de acesso** que precisam ser analisadas e corrigidas.

O estudante deve:

1. Realizar testes de conectividade.
  
2. Identificar os problemas.
  
3. Implementar soluções.
  
4. Documentar cada etapa com evidências (prints de tela e comandos utilizados).
  
5. Entregar o relatório junto com o arquivo `.pkt` corrigido.
  

Para auxiliar no trabalho este roteiro possui uma breve documentação de como a rede deveria está configurada e uma lista dos problemas encontrados.

## Documentação de Referência da Rede

### VLANs e Sub-redes

- VLAN 10 - TI → 192.168.10.0/24
  
- VLAN 20 - GER → 192.168.20.0/24
  
- VLAN 30 - ADM → 192.168.30.0/24
  
- VLAN 40 - IMP → 192.168.40.0/24
  

### Switches

- **Switch0**
  
  - Gerência: VLAN 10 → IP 192.168.10.2/24, GW 192.168.10.1
    
  - Portas:
    
    - VLAN 10 → g0/1
      
    - VLAN 20 → g0/2
      
    - VLAN 30 → fa0/16
      
    - VLAN 40 → -
      
- **Switch1**
  
  - Gerência: VLAN 10 → IP 192.168.10.3/24
    
  - Portas:
    
    - VLAN 10 → fa0/1-10
      
    - VLAN 20 → fa0/11-15
      
    - VLAN 30 → fa0/16-20
      
    - VLAN 40 → fa0/21-24
      
- **Switch2**
  
  - Gerência: VLAN 10 → IP 192.168.10.4/24
    
  - Portas:
    
    - VLAN 10 → fa0/1-3, fa0/6-10
      
    - VLAN 20 → fa0/11-15
      
    - VLAN 30 → fa0/16-20
      
    - VLAN 40 → fa0/21-24
      
- **Switch3**
  
  - Gerência: VLAN 10 → IP 192.168.10.5/24
    
  - Portas:
    
    - VLAN 10 → -
    - VLAN 20 → fa0/11-15
    - VLAN 30 → fa0/16-20
    - VLAN 40 → fa0/21-24

### Link Aggregation / EtherChannel

- Channel 1 → sw0 fa0/1-2 ↔ sw1 fa0/1-2
  
- Channel 2 → sw0 fa0/4-5 ↔ sw2 fa0/4-5
  
- Channel 3 → sw0 fa0/6-7 ↔ sw3 fa0/6-7
  

### DHCP

- VLAN 10 - TI → Range: 192.168.10.10–254 | GW: 192.168.10.1
  
- VLAN 20 - GER → Range: 192.168.20.10–254 | GW: 192.168.20.1
  
- VLAN 30 - ADM → Range: 192.168.30.10–254 | GW: 192.168.30.1
  

### Sem DHCP

- VLAN 40 - IMP → GW: 192.168.40.1
  
  - Impressora Térreo → 192.168.40.10
    
  - Impressora 1º Andar → 192.168.40.11
    
  - Impressora 2º Andar → 192.168.40.12
    

### Router 0 (Router-on-a-Stick)

- fa0/0.10 → 192.168.10.1
  
- fa0/0.20 → 192.168.20.1
  
- fa0/0.30 → 192.168.30.1
  
- fa0/0.40 → 192.168.40.1
  

### Acesso SSH aos switches

- Usuário: admin
  
- Senha: labredes
  

## Problemas Identificados

1. **Computadores da TI não navegam na rede**
  
  - Teste: renovar IP e verificar se o endereço obtido pertence à VLAN 10.
2. **Impressoras inacessíveis**
  
  - Teste: ping para os endereços das impressoras.
3. **EtherChannel entre Switch0 e Switch3 não funciona**
  
  - Teste: `show interfaces port-channel 3` → deve mostrar `Port-channel3 is up`.
4. **Switch3 inacessível via SSH ou ping**
  
  - Teste: ping para IP de gerência ou `ssh -l [usuário] [end. IP]`.

## Relatório Final

O estudante deve apresentar para cada problema:

- **Teste inicial** (evidência do erro).
  
- **Descrição do problema**.
  
- **Possível causa**.
  
- **Solução implementada** (com comentários).
  
- **Teste final** (evidência da correção).
  
- Enviar junto ao relatório o arquivo `.pkt` com o cenário corrigido e funcionando.
  

## Comandos úteis

- `show vlan brief` → Verificar VLANs criadas e portas associadas.
  
- `show ip interface brief` → Conferir IPs e status das interfaces.
  
- `show running-config` → Revisar configuração atual.
  
- `show etherchannel summary` → Verificar estado dos canais.
  
- `show interfaces port-channel [numero]` → Verificar o estado de um canal específico
  
- `ping [IP]` → Testar conectividade.
  
- `ssh -l admin [IP]` → Testar acesso remoto.
  

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
