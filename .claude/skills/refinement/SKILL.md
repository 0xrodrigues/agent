---
name: refinement
description: Conduz um refinamento conversacional de demanda e cria ou atualiza um pacote de especificação sem explorar ou implementar código.
argument-hint: "[caminho ou slug do pacote | descrição da demanda]"
disable-model-invocation: true
---

# Refinement

Use esta skill somente quando o Tech Lead pedir para iniciar, retomar, concluir ou mudar o status do refinamento de uma demanda. Ela é deliberadamente interativa: não avance sobre lacunas relevantes sem registrar a incerteza ou consultar o Tech Lead.

Argumento recebido: `$ARGUMENTS`

## Boundaries

- Não explore código, não edite aplicações e não execute implementação. Hipóteses de abordagem podem ser registradas apenas como `Inference`, em nível conceitual, sem leitura de código.
- Não invente regras de negócio, respostas, integrações, responsáveis ou datas.
- Não escreva em sistemas externos. MCPs só podem ser usados para leitura autorizada de evidências e devem ter origem e data registradas.
- Não altere conhecimento durável em `context/` sem autorização explícita do Tech Lead.

## Workflow

1. Leia `CLAUDE.md`, `demand/README.md` e `context/index.md`.
2. Resolva o pacote a partir do argumento:
   - Caminho ou slug de pacote existente em `demand/`: leia todos os Markdown dele antes de perguntar algo já respondido. Se o slug casar com mais de um pacote, pergunte qual.
   - Descrição de demanda nova: crie `demand/YYYY-MM-DD-SHORT-SLUG/refinement/`, com data local corrente e slug curto em maiúsculas, separado por hífens. Confirme o slug se houver ambiguidade.
   - Sem argumento: pergunte se é uma demanda nova ou qual pacote retomar.
   - Arquivos existentes vazios ou sem a estrutura abaixo: trate como rascunho e reconstrua a estrutura sem apagar conteúdo.
3. Crie ou atualize `spec.md` com `Status: UNDER_REFINEMENT` (ao retomar pacote `BLOCKED`, informe o Tech Lead e volte a `UNDER_REFINEMENT` apenas com a confirmação dele). Comece por um índice com links internos.
4. Extraia fatos iniciais sem reinterpretá-los como decisão. Pergunte usando `AskUserQuestion` apenas sobre lacunas que mudem escopo, comportamento, risco, dependência, prioridade ou critério de aceite: problema, usuários afetados, comportamento atual e esperado, exclusões, serviços conhecidos, dependências, riscos, critérios de entrada e de aceite. Agrupe perguntas relacionadas e evite repetir rodadas sobre o mesmo ponto.
5. Registre respostas como decisões, com origem `Tech Lead` e data. Respostas livres ("Other") entram com o texto fiel do Tech Lead. Mantenha perguntas não respondidas visíveis; não as converta em conclusões.
6. Classifique cada afirmação relevante como `Confirmed`, `Inference` ou `Open question`, com a origem.
7. Estruture `spec.md` para que uma pessoa consiga navegar e entender a demanda sem a conversa original. Crie arquivos auxiliares (`requirements.md`, `risks.md`, `dependencies.md`) somente quando a densidade justificar e referencie-os a partir do índice.
8. Antes de encerrar, apresente um resumo de escopo, exclusões, dúvidas abertas, bloqueios e a proposta de status. Só mude o status após confirmação explícita do Tech Lead, conforme a seção "Refinement status transitions" de `demand/README.md`. Esta skill nunca decide a aprovação: só registra `APPROVED` (status, data e origem `Tech Lead` em `Decisions`) quando o Tech Lead a pedir explicitamente no chat e o pacote estiver em `READY_FOR_REVIEW`.

## Required `spec.md` structure

```md
# <Título da demanda>

Status: `UNDER_REFINEMENT`
Created: `YYYY-MM-DD`
Last updated: `YYYY-MM-DD`
Owner: `Tech Lead`

## Index

- [Overview](#overview)
- [Scope](#scope)
- [Current and expected behavior](#current-and-expected-behavior)
- [Decisions](#decisions)
- [Open questions](#open-questions)
- [Risks and dependencies](#risks-and-dependencies)
- [Approach hypotheses](#approach-hypotheses) (opcional)
- [Definition of Ready](#definition-of-ready)
- [Acceptance criteria](#acceptance-criteria)
- [Evidence](#evidence)

## Overview
Objetivo, problema e usuários afetados.

## Scope
In scope / Out of scope, em listas separadas.

## Current and expected behavior
Cada afirmação marcada `Confirmed`, `Inference` ou `Open question`.

## Decisions
| ID | Decision | Origin | Date |
| --- | --- | --- | --- |
| D1 | ... | Tech Lead | YYYY-MM-DD |

## Open questions
| ID | Question | Impact | Owner | Status |
| --- | --- | --- | --- | --- |
| Q1 | ... | ... | `Unknown` ou nome | Open |

## Risks and dependencies
| ID | Type (risk/dependency) | Description | Owner | Mitigation / next step |
| --- | --- | --- | --- | --- |

## Approach hypotheses
Opcional. Somente `Inference`, sem leitura de código e sem compromisso de solução.

## Definition of Ready
Checklist verificável.

## Acceptance criteria
Critérios verificáveis.

## Evidence
| Source | Date | Used for |
| --- | --- | --- |
```

Use tabelas concisas quando elas tornarem as relações mais fáceis de auditar. Toda decisão, risco, dependência e pergunta aberta deve ser acionável ou estar explicitamente marcada como desconhecida.

## Completion check

Antes de propor `READY_FOR_REVIEW`, verifique que `spec.md` contém: objetivo da demanda, escopo e exclusões explícitos, comportamento atual e esperado conhecidos, decisões e perguntas não resolvidas, riscos e dependências, Definition of Ready, critérios de aceite e evidência/origem. A ausência de um fato só é válida quando registrada como pergunta aberta ou bloqueio. Atualize `Last updated` a cada alteração.
