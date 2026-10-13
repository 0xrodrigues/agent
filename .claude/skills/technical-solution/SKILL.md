---
name: technical-solution
description: Organiza uma proposta técnica inicial, discutível e rastreável a partir de um refinamento aprovado e das anotações do Tech Lead, sem implementar código.
argument-hint: "[caminho ou slug da demand/solution | rascunho técnico]"
disable-model-invocation: true
---

# Technical solution

Use esta skill quando o Tech Lead pedir para iniciar, retomar, discutir ou registrar uma proposta técnica para uma demanda já refinada. O Tech Lead pode fornecer um rascunho com ideias, alternativas e observações; ele é insumo, não decisão automática.

Argumento recebido: `$ARGUMENTS`

## Boundaries

- Não implemente código, altere aplicações, bancos, infraestrutura ou serviços, nem escreva em sistemas externos.
- Inicie uma proposta nova somente a partir de `demand/<demanda>/refinement/spec.md` com `Status: APPROVED`. Se a necessidade for antecipar a discussão, peça indicação explícita do Tech Lead, registre em `Refinement baseline` o status real e a dependência como risco; não trate o refinamento como aprovado.
- Não invente arquitetura existente, contratos, modelo de dados, integrações, métricas, requisitos não funcionais ou decisões. Classifique o que vier do rascunho como `Inference` até haver evidência ou confirmação.
- Leitura técnica é opcional e só ocorre quando o Tech Lead indicar escopo e fonte autorizada. Os repositórios ficam em `project/<serviço>` e só são lidos por meio do `service-researcher`. Registre cada evidência em `exploration.md`; não use leitura de código para implementar ou alterar nada.
- Para leitura técnica ou Oracle, delegue ao `service-researcher` uma pergunta `RES-<n>` delimitada. Não faça varredura ampla na sessão principal. A conta Oracle e as ferramentas disponíveis devem ser read-only; se isso não puder ser confirmado, não consulte dados.
- Não altere `demand/<demanda>/refinement/`, `context/` ou conhecimento durável. Contexto só pode ser consultado pela navegação `context/index.md` e alterações nele exigem autorização explícita do Tech Lead.
- Esta skill nunca aprova a proposta nem autoriza implementação. Ela só registra `APPROVED` mediante pedido explícito do Tech Lead.

## Workflow

1. Leia `CLAUDE.md`, `demand/README.md`, `context/index.md` e o pacote de demanda indicado. Leia todos os Markdown relevantes do pacote antes de perguntar algo já respondido. Confirme que `refinement/spec.md` está `APPROVED` e registre o status lido e a data em `Refinement baseline`. Ao retomar uma proposta, releia o `spec.md`: se o status ou o conteúdo mudou desde o baseline, informe o Tech Lead, liste o que pode afetar a proposta e atualize o baseline somente após a confirmação dele.
2. Resolva o pacote a partir do argumento:
   - Caminho ou slug existente em `demand/<demanda>/solution/`: leia todos os Markdown e retome a proposta, preservando conteúdo e decisões.
   - Caminho ou slug de `demand/`: crie `solution/` dentro do mesmo pacote. Não crie outro pacote, não replique o `refinement/spec.md` e não duplique slug ou data.
   - Rascunho técnico: peça o pacote de demanda com refinement aprovado a que ele se refere. Se houver mais de um candidato, não escolha por conta própria.
   - Sem argumento: pergunte qual demanda com refinement aprovado será a origem e solicite o rascunho inicial, se ainda não foi fornecido.
3. Crie ou atualize `architecture.md` com `Status: UNDER_DESIGN`. Preserve fielmente o rascunho do Tech Lead em `## Initial technical notes`, distinguindo observações, alternativas e propostas de decisão.
4. Construa a proposta a partir de rastreabilidade: conecte cada decisão, componente, alteração de fluxo e requisito não funcional ao requisito, critério, risco, dependência ou evidência que o motivou. Se não houver base, mantenha como hipótese ou pergunta aberta.
5. Discuta a proposta com perguntas apenas quando elas mudarem arquitetura, responsabilidade, contrato, dados, compatibilidade, segurança, operação, risco ou critério de aceite. Registre toda resposta como decisão com origem `Tech Lead` e data. Não repita o que já foi decidido.
6. Compare alternativas viáveis quando houver uma escolha técnica relevante. Declare trade-offs e o que faria a proposta escolhida falhar ou precisar mudar; não crie alternativas artificiais para itens sem decisão real.
7. Quando uma decisão depender do comportamento atual de código, integração ou dados, delegue explicitamente ao `service-researcher`. Confirme com o Tech Lead o serviço (`project/<serviço>`) e a branch/commit se ainda não estiverem definidos. Envie exatamente os campos do contrato do agente: `Research ID` (`RES-<n>`, sequencial dentro de `exploration.md`), `Question`, `Repository`, `Revision`, `Scope`, `Known clues`, `Expected evidence`, `Data limits` e `Budget`. Aguarde o resultado antes de decidir. `ANSWERED`: registre a síntese em `exploration.md`. `PARTIAL`: registre o que foi respondido, mantenha a lacuna como pergunta aberta e só abra `RES-<n+1>` se ela bloquear uma decisão. `BLOCKED`: apresente o motivo ao Tech Lead e não repita o pedido sem corrigi-lo. Pesquisa Oracle depende de ferramenta disponível no ambiente; sem ela, a lacuna fica registrada.
8. Crie `contracts.md`, `data.md` e `integration.md` apenas quando houver impacto correspondente. Eles detalham a proposta, nunca tornam um fato implícito. Crie ou atualize `exploration.md` somente com a síntese e as referências de uma pesquisa que afetem a proposta, uma seção por `RES-<n>` (pergunta, resposta, fonte/escopo, revisão, data, referências, classificação, lacunas).
9. Antes de encerrar, apresente proposta recomendada, decisões pendentes, pressupostos, riscos, lacunas de evidência e a proposta de status. Mude para `READY_FOR_DISCUSSION` somente com confirmação explícita do Tech Lead. Registre `APPROVED` somente quando ele pedir explicitamente, conforme `demand/README.md`.

## Required `architecture.md` structure

```md
# <Título da demanda> — Technical solution

Status: `UNDER_DESIGN`
Created: `YYYY-MM-DD`
Last updated: `YYYY-MM-DD`
Owner: `Tech Lead`
Refinement: [`../refinement/spec.md`](../refinement/spec.md)
Refinement baseline: `<status lido>` em `YYYY-MM-DD`

## Index

- [Context and objective](#context-and-objective)
- [Initial technical notes](#initial-technical-notes)
- [Constraints and assumptions](#constraints-and-assumptions)
- [Alternatives and trade-offs](#alternatives-and-trade-offs)
- [Proposed solution](#proposed-solution)
- [Impacts](#impacts)
- [Decisions](#decisions)
- [Open questions](#open-questions)
- [Risks and validation plan](#risks-and-validation-plan)
- [Delivery and operational considerations](#delivery-and-operational-considerations)
- [Traceability](#traceability)
- [Evidence](#evidence)

## Context and objective
Referência ao problema, escopo e critérios do refinement. Não os copie sem necessidade.

## Initial technical notes
Anotações do Tech Lead preservadas com sua classificação e origem.

## Constraints and assumptions
| ID | Statement | Classification | Origin | Validation / consequence |
| --- | --- | --- | --- | --- |

## Alternatives and trade-offs
| ID | Alternative | Benefits | Costs / risks | Status |
| --- | --- | --- | --- | --- |

## Proposed solution
Fluxo, responsabilidades, limites e mudanças propostas. Use diagrama somente se ele esclarecer uma relação material; o diagrama não substitui a descrição textual.

## Impacts
| Area | Proposed impact | Classification | Detail |
| --- | --- | --- | --- |

## Decisions
| ID | Decision | Rationale | Origin | Date |
| --- | --- | --- | --- | --- |

## Open questions
| ID | Question | Impact if unanswered | Owner | Status |
| --- | --- | --- | --- | --- |

## Risks and validation plan
| ID | Risk / uncertainty | Impact | Mitigation or validation | Owner |
| --- | --- | --- | --- | --- |

## Delivery and operational considerations
Somente o que for pertinente: compatibilidade, rollout, observabilidade, segurança, falhas, reversão e dependências de entrega. Ausência deve ser explícita quando relevante.

## Traceability
| Solution item | Refinement source | Type (requirement/criterion/risk/dependency/evidence) | Notes |
| --- | --- | --- | --- |

## Evidence
| Source | Date | Used for | Classification |
| --- | --- | --- | --- |
```

## Companion artifacts

Use o menor conjunto que mantenha a proposta auditável.

- `contracts.md`: identifique produtor, consumidor, canal, compatibilidade, contrato atual/proposto, erros e estratégia de evolução. Marque o que ainda é hipótese.
- `data.md`: identifique entidades ou conjuntos afetados, alteração proposta, integridade, migração/retrocompatibilidade, acesso e consultas relevantes. Não invente esquema ou query existente.
- `integration.md`: identifique sistemas, direção do fluxo, gatilho, responsabilidade, autenticação quando conhecida, timeout/retry/idempotência quando pertinente e comportamento de falha.
- `exploration.md`: use uma seção por `RES-<n>` com pergunta, resposta, fonte/escopo, data, referências, classificação e lacunas. Mantenha recomendações em `architecture.md`; não copie dumps de código, DDL ou linhas de dados.

## Completion check

Antes de propor `READY_FOR_DISCUSSION`, confirme que `architecture.md` contém: vínculo para refinement aprovado; objetivo e escopo de referência; anotações iniciais preservadas; proposta com responsabilidades e impactos; escolhas relevantes e trade-offs; pressupostos e perguntas abertas; riscos e plano de validação; considerações operacionais aplicáveis; decisões com origem e data; e rastreabilidade para os insumos do refinement. A falta de evidência é aceitável somente se estiver visível como hipótese, pergunta ou bloqueio. Atualize `Last updated` a cada alteração.
