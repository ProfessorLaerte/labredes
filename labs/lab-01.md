Disciplina: **ENE0011 – Laboratório de Redes**  
Curso: **Engenharia de Redes de Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica** 

Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**

---
## Experimento 01 – Introdução aos Dispositivos de Redes

## Objetivo

Conhecer os dispositivos básicos de rede e realizar operações básicas em um roteador Cisco.

## Introdução Teórica

- Hub  
- Switch  
- Roteador  
- Comparação entre os dispositivos de rede  
- Roteadores Cisco  
- Sistema Operacional Cisco IOS (*Internetworking Operating System*)

---

## Descrição da Atividade

1. Utilize o cliente de conexão remota **PuTTY** e acesse o roteador.

2. Entre no **modo SETUP** e configure o roteador conforme os parâmetros abaixo:

   ***Obs.:** Caso o equipamento não possua uma configuração inicial, ele entrará em **modo setup** logo após a inicialização. Caso contrário ultima configuração será carregada e a reconfiguração pode ser realizada com o comando `#setup`.*

   1. **Nome do roteador**: `lab_1`  
   2. **Senha criptografada para acesso ao modo EXEC privilegiado**: `apr`  
   3. **Senha de acesso via Telnet**: `apr`  
   4. **Endereço IP da interface FastEthernet0/0**: `192.168.0.10`  
   5. Salve as configurações no arquivo de inicialização do roteador (**NVRAM** ou `startup-config`)

3. Colete as seguintes informações sobre o equipamento e sua configuração.  
   Utilize os comandos:
   - `show version`
   - `show interfaces`
   - `show ip interface`

   Informações a serem coletadas:
   - Versão do sistema operacional Cisco IOS  
   - Tempo em que o sistema está ativo e em execução  
   - Forma pela qual o roteador foi reinicializado pela última vez  
   - Interfaces de rede existentes no roteador:
     - Status administrativo  
     - Status operacional de:
       - Camada 1 (física)
       - Camada 2 (enlace)

4. O **CDP (Cisco Discovery Protocol)** é um protocolo proprietário da Cisco que permite obter informações sobre equipamentos Cisco diretamente conectados.

   - Verifique se existe algum equipamento Cisco vizinho  
   - Identifique:
     - Tempo padrão de envio das mensagens
     - Tempo de validade das informações

   Utilize os comandos:
   - `show cdp`
   - `show cdp neighbors`

5. Identifique quais **protocolos roteados de camada 3** estão habilitados no roteador.

   - Utilize o comando `show protocols`
   - Verifique o campo **Global values**

6. Salve toda a configuração ativa do roteador da **RAM para a NVRAM**:

```bash
   copy running-config startup-config
```

> Laboratório desenvolvido para a disciplina **ENE0011 – Laboratório de Redes**  
> **Prof. Dr. Laerte Peotta de Melo**
