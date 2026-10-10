Disciplina: **ENE0011 - Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica**  
Professor: **Prof. Dr. Laerte Peotta de Melo**

---

# Roteiro do Experimento 06 - Desafio de Resolução de Problemas (Troubleshooting)

## Objetivo

O estudante deve **diagnosticar e corrigir falhas de configuração** em uma rede previamente montada no Cisco Packet Tracer.
O foco é desenvolver habilidades práticas de:

- Identificação de problemas de conectividade.
  
- Testes e validação de hipóteses.
  
- Aplicação de soluções técnicas em VLANs, DHCP, EtherChannel e acesso remoto.
  
- Documentação clara do processo de troubleshooting.

## Cenário
<img width="1767" height="685" alt="Topologia do Experimento 06 no Packet Tracer" src="https://github.com/user-attachments/assets/3723416e-7a83-4e60-83c4-e18352e46fea" />

## Descrição da Atividade

O arquivo `.pkt` fornecido ([Baixar arquivo do laboratório](./topologias/experimento_06_nao_resolvido.pkt)) contém uma rede dividida em três andares, com computadores, impressoras, switches e um roteador configurados.
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

- VLAN 10 - TI → Range: 192.168.10.10-254 | GW: 192.168.10.1
  
- VLAN 20 - GER → Range: 192.168.20.10-254 | GW: 192.168.20.1
  
- VLAN 30 - ADM → Range: 192.168.30.10-254 | GW: 192.168.30.1
  

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
4. **Switch 1 inacessível via SSH ou ping**
  
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

## Uso de Inteligência Artificial

Esta atividade segue a Política de Integridade Acadêmica e Uso Responsável da Inteligência Artificial da Faculdade de Tecnologia (FT/UnB), aprovada pelo Conselho da FT em 30/09/2026.

**Regime desta atividade:** uso restrito.

Ferramentas de IA podem ser usadas apenas para consultar a sintaxe e o significado dos comandos de verificação. É vedado usá-las para identificar os problemas, formular o diagnóstico ou definir as correções registradas na seção "Problemas Identificados" e no relatório final.

Justificativa: a atividade avalia a capacidade do estudante de diagnosticar falhas a partir das evidências coletadas na própria topologia, competência que o uso de IA nessas etapas impediria de aferir.

**Responsabilidade:** o estudante responde integralmente pelo conteúdo entregue.

**Condutas vedadas:**

- fabricar ou alterar saídas de comandos, capturas, dados ou referências;
- apresentar como própria uma resposta substancialmente elaborada por IA sem contribuição intelectual compatível;
- omitir o uso relevante de IA;
- inserir em plataformas externas dados pessoais, senhas, chaves ou capturas de redes reais.

**Declaração:** obrigatória quando a IA tiver influência relevante sobre o conteúdo, a análise, a interpretação ou as conclusões do relatório. Anexar ao relatório:

| Campo | Preenchimento |
| :--- | :--- |
| Ferramenta e versão | |
| Finalidade | |
| Etapas do trabalho em que foi empregada | |
| Natureza da contribuição | |
| Validação humana realizada | |

---

[← Anterior: Experimento 05](05_roteiro.md) | [Índice Geral (README)](../README.md) | [Próximo: Experimento 07 →](07_roteiro.md)
