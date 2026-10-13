---
name: review-refinement
description: Audita a composição, rastreabilidade e completude estrutural de um pacote de refinamento pronto para revisão, sem alterar seus artefatos ou decidir negócio.
argument-hint: "[caminho ou slug do pacote]"
disable-model-invocation: true
---

# Review refinement

Use esta skill somente quando o Tech Lead indicar um pacote de `demand/` cujo `refinement/spec.md` está em `READY_FOR_REVIEW`. O propósito é auditoria estrutural, não refinamento de produto.

Argumento recebido: `$ARGUMENTS`

## Boundaries

- Não altere `refinement/spec.md`, `refinement/requirements.md`, `refinement/risks.md`, `refinement/dependencies.md` ou qualquer artefato que esteja sendo revisado. O único arquivo escrito é `refinement/review.md`.
- Não responda perguntas abertas, não crie regras de negócio, não avalie mérito de produto e não proponha solução técnica.
- Não revise pacote em `UNDER_REFINEMENT`, `BLOCKED`, `APPROVED`, `CANCELED` ou `SUPERSEDED`.
- Não use código, MCPs ou fontes externas para completar lacunas. A revisão é interna ao pacote.
- Não altere o status do pacote.

## Workflow

1. Leia `CLAUDE.md`, `demand/README.md` e todos os arquivos Markdown de `refinement/` do pacote indicado (ignore `solution/`). Resolva o pacote pelo argumento (caminho ou slug); se estiver ausente, inexistente ou ambíguo, pergunte ao Tech Lead.
2. Confirme que `refinement/spec.md` existe e que o status é `READY_FOR_REVIEW`. Caso contrário, informe a condição e não crie `refinement/review.md`.
3. Audite somente:
   - presença da estrutura obrigatória de `spec.md` (definida em `.claude/skills/refinement/SKILL.md`);
   - links e referências entre artefatos;
   - consistência de status, datas (`Created` ≤ `Last updated`) e nomes (pasta `YYYY-MM-DD-SHORT-SLUG`);
   - origem e data de decisões e evidências;
   - classificação `Confirmed` / `Inference` / `Open question` das afirmações relevantes;
   - decisões versus perguntas abertas (nada aberto tratado como decidido);
   - Definition of Ready e critérios de aceite verificáveis;
   - riscos e dependências com responsável ou lacuna explícita.
4. Crie ou substitua apenas `refinement/review.md` no mesmo pacote. Esse arquivo é o registro da rodada de auditoria e não altera o resultado do refinamento.
5. Classifique cada apontamento como `BLOCKER`, `WARNING` ou `NOTE`, com arquivo/seção, evidência objetiva e ação necessária. Se não houver apontamentos, registre isso explicitamente.
6. Termine informando que a revisão não aprova o pacote. A mudança para `APPROVED` continua sendo decisão explícita do Tech Lead, e o retorno a `UNDER_REFINEMENT` para corrigir apontamentos é feito via `/refinement`.

## Required `review.md` structure

```md
# Refinement review — <Título da demanda>

Demand package: `<path>`
Reviewed: `YYYY-MM-DD`
Reviewer: `review-refinement`
Package status at review: `READY_FOR_REVIEW`

## Summary

## Findings

| Severity | Location | Finding | Evidence | Required action |
| --- | --- | --- | --- | --- |

## Structural checks

| Check | Result | Notes |
| --- | --- | --- |

## Limitations

Esta revisão não validou regras de negócio, decisões de produto, código, integrações ou fontes externas.
```

Se `refinement/review.md` já existir, substitua-o apenas como a rodada de revisão atual e informe a data da revisão anterior no resumo, quando disponível.
