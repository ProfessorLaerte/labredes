Disciplina: **ENE0011 – Laboratório de Redes**  
Curso: **Engenharia de Redes e Comunicação**  
Instituição: **Universidade de Brasília (UnB)**  
Departamento: **Engenharia Elétrica**
Professor Responsável: **Prof. Dr. Laerte Peotta de Melo**

## Experimento 02 – Analisadores de Protocolos (Sniffers)



## Objetivo

Verificar o acesso ao meio em uma rede Ethernet por meio da utilização de um analisador de protocolos (*sniffer*), analisando o tráfego de rede, os protocolos envolvidos e os mecanismos de encapsulamento entre camadas.

## Introdução Teórica

Este experimento aborda conceitos fundamentais relacionados à análise de tráfego em redes locais, com ênfase em:

- Camada física e camada de enlace em redes locais;
- Redes Ethernet:
  - Topologias físicas e lógicas;
  - Mecanismos de acesso ao meio;
- Analisadores de protocolo (*sniffers*):
  - Princípios de funcionamento;
  - Tipos de sniffers;
  - Aplicações em monitoramento, diagnóstico e segurança de redes.



## Descrição da Atividade

O experimento está dividido em quatro partes.



### Parte 1 – Utilização de Sniffer para Coleta de Estatísticas da Rede

1. Explique, no relatório, como um sniffer pode ser utilizado para a coleta de estatísticas da rede.  
   Evidencie possíveis aplicações desse tipo de ferramenta.

2. Verifique os pacotes capturados dos protocolos **ICMP** e **DNS**:
   - Quantos pacotes DNS e ICMP foram enviados?
   - Quantos pacotes DNS e ICMP foram recebidos?
   - Descreva os pacotes observados.

3. Capture pacotes do protocolo **HTTP** e:
   - Verifique e descreva o **TCP 3-Way Handshake**;
   - Identifique mensagens **GET**, **OK** e a página HTML acessada.

---

### Parte 2 – Arquitetura em Camadas

4. Utilize o sniffer para evidenciar o conceito de **encapsulamento** e a visualização da arquitetura de protocolos em camadas.

5. Identifique os mecanismos de **multiplexação entre camadas**, analisando os seguintes campos:

   - **Camada 2 (Enlace) → Camada 3 (Rede)**  
     - Campo **Type** do protocolo MAC Ethernet;

   - **Camada 3 (Rede) → Camada 3/4 (ICMP, UDP e TCP)**  
     - Campo **Protocol** do protocolo IP;

   - **Camada 4 (Transporte) → Camada 7 (Aplicação)**  
     - Campos **Source Port** e **Destination Port** dos protocolos TCP e UDP;

   - Identifique:
     - Endereços de **nível 2 (MAC)**;
     - Endereços de **nível 3 (IP)**.

---

### Parte 3 – Detecção de Sniffers Maliciosos

6. Utilize o log gerado pelo sniffer para tentar capturar **login e senha** de acesso ao ambiente `[teste](https://www.stealmylogin.com/demo.html)`.

7. Avalie se é possível realizar o mesmo procedimento para a captura de credenciais de acesso ao **email**.

8. Descreva possíveis **mecanismos de detecção** e **mecanismos de proteção** contra sniffers maliciosos em uma rede.

---

### Parte 4 – Analisadores de Protocolos de Camada Física

9. Descreva como seria o funcionamento de um **analisador de protocolos para a Camada 1 (Física)** do modelo de referência.

---

## Equipamentos e Materiais

- Computadores do laboratório;
- Software analisador de protocolos:
  - **Wireshark**.

---

## Orientações para o Relatório

O relatório deve conter, no mínimo:
- Explicações conceituais solicitadas;
- Evidências coletadas por meio do sniffer (capturas, descrições e análises);
- Respostas claras e objetivas para cada item;
- Análise crítica dos resultados observados.

---

## Bibliografia

1. KUROSE, J. F.; ROSS, K. W.  
   *Computer Networks: A Top-Down Approach*. 5ª ed. Pearson Addison-Wesley, 2009.

2. Sites diversos sobre **sniffers** e análise de protocolos na Internet.

---

> Este laboratório tem caráter **didático e acadêmico**, sendo vedado o uso das técnicas apresentadas para fins não autorizados ou que violem políticas institucionais ou legislações vigentes.
