# Atividade Prática  
## Modelagem de Sistema de Matrícula com Casos de Uso e Misuse Cases  
Disciplina: CIC0105 Engenharia de Software  
Professor: Doutor Laerte Peotta de Melo  laerte.melo@unb.br

---

## Objetivo

Modelar funcionalmente um sistema de matrícula em disciplina e expandir o modelo para incluir comportamentos indevidos, identificando ameaças e propondo mecanismos de mitigação.

---

## Cenário

Considere o desenvolvimento de um:

Sistema de Matrícula em Disciplina de Engenharia de Software.

O sistema deve permitir:

- Autenticação do aluno  
- Consulta de vagas  
- Solicitação de matrícula  
- Cancelamento de matrícula  
- Consulta de lista de alunos (para o professor)  

Atores envolvidos:

- Aluno  
- Professor  
- Coordenador do curso  
- Sistema Acadêmico Externo  

---

## Parte 1 -  Modelagem Funcional

Em grupos de 3 a 4 alunos:

1. Identifique os atores legítimos.  
2. Defina os principais casos de uso.  
3. Construa o diagrama de casos de uso UML.  
4. Identifique relações de `<<include>>` ou `<<extend>>`, se existirem.  

Pergunta orientadora:

O modelo cobre completamente as interações legítimas?

---

## Parte 2 -  Identificação de Comportamentos Indevidos

Amplie o modelo considerando:

1. Introdução de um ator malicioso (ex.: Usuário Não Autorizado).  
2. Identificação de pelo menos três misuse cases, por exemplo:
   - Realizar matrícula sem autenticação  
   - Alterar lista de alunos indevidamente  
   - Acessar dados acadêmicos de outro aluno  
3. Modelagem dos misuse cases no diagrama.  

Pergunta orientadora:

Quais partes do sistema são mais sensíveis?

---

## Parte 3 -  Mitigações e Integração ao SDLC

Para cada misuse case:

1. Proponha uma medida de mitigação (ex.: autenticação forte, controle de acesso por perfil, registro de logs).  
2. Modele a relação de mitigação no diagrama.  
3. Indique em qual fase do SDLC a mitigação deveria ser tratada:
   - Planejamento  
   - Análise de Requisitos  
   - Design  
   - Codificação e Testagem  
   - Implantação  
   - Manutenção  

---

## Entregável

Cada grupo deverá apresentar:

- Diagrama completo (uso, misuse e mitigação)  
- Lista das ameaças identificadas  
- Justificativa técnica das mitigações  
- Indicação da fase do SDLC correspondente  

---

## Critérios de Avaliação

| Critério | Peso |
|----------|------|
| Correção da modelagem funcional | 25% |
| Qualidade dos misuse cases | 25% |
| Coerência das mitigações | 25% |
| Integração com SDLC | 15% |
| Clareza da argumentação | 10% |

---

## Discussão Final

Se o sistema estivesse funcionalmente correto, mas sem os misuse cases modelados, quais riscos permaneceriam invisíveis?
