# Demand packages

Cada demanda ocupa um único pacote em `demand/YYYY-MM-DD-SHORT-SLUG/`. As etapas compartilham o mesmo slug, histórico e evidências, mas cada artefato preserva seu próprio status e responsabilidade.

```text
demand/YYYY-MM-DD-SHORT-SLUG/
├── refinement/
│   ├── spec.md
│   ├── requirements.md        # opcional
│   ├── risks.md               # opcional
│   ├── dependencies.md        # opcional
│   └── review.md              # produzido por /review-refinement
└── solution/
    ├── architecture.md
    ├── contracts.md           # opcional
    ├── data.md                # opcional
    ├── integration.md         # opcional
    └── exploration.md         # opcional; índice de pesquisas RES-* e evidência técnica autorizada
```

Não crie diretórios de etapa vazios: `refinement/` nasce com `spec.md`; `solution/` só é criado pela `/technical-solution` quando houver refinement aprovado.

## Refinement

`refinement/spec.md` é obrigatório e é o ponto de entrada humano e técnico da etapa de refinement. Ele começa com um índice navegável e registra o status de produto da demanda.

Artefatos auxiliares só existem quando melhorarem materialmente a leitura ou separarem responsabilidade: `requirements.md`, `risks.md`, `dependencies.md` e `review.md`. A revisão nunca reescreve os artefatos revisados.

### Refinement status

| Status | Meaning |
| --- | --- |
| `UNDER_REFINEMENT` | Conversa e descoberta em curso. |
| `BLOCKED` | Falta uma informação, decisão ou acesso externo identificável. |
| `READY_FOR_REVIEW` | Refinamento encerrado pelo Tech Lead e elegível para auditoria estrutural. |
| `APPROVED` | Aceito explicitamente pelo Tech Lead; pode orientar a solução técnica, mas não implementa por si só. |
| `CANCELED` | A demanda não seguirá. Preserve o motivo. |
| `SUPERSEDED` | Substituído por outro pacote; aponte o sucessor. |

`DONE` não é usado: aprovação humana é o marco relevante nesta etapa.

### Refinement status transitions

| From | To | Condição | Quem registra |
| --- | --- | --- | --- |
| `UNDER_REFINEMENT` | `READY_FOR_REVIEW` | Completion check da skill `/refinement` satisfeito e confirmação explícita do Tech Lead. | `/refinement` |
| `READY_FOR_REVIEW` | `UNDER_REFINEMENT` | Apontamentos de `review.md` ou novas informações exigem ajuste. | `/refinement` |
| `READY_FOR_REVIEW` | `APPROVED` | Decisão explícita do Tech Lead, que pode usar `review.md` como insumo. | Tech Lead decide; `/refinement` apenas registra a pedido explícito. |
| qualquer | `BLOCKED` | Falta informação, decisão ou acesso externo identificável; registre o bloqueio e quem o resolve. | `/refinement` |
| `BLOCKED` | `UNDER_REFINEMENT` | Bloqueio resolvido, com confirmação do Tech Lead. | `/refinement` |
| qualquer | `CANCELED` | Tech Lead decide não seguir; registre o motivo. | `/refinement` |
| qualquer | `SUPERSEDED` | Outro pacote substitui este; aponte o sucessor. | `/refinement` |

Toda mudança atualiza `Status` e `Last updated` em `refinement/spec.md`.

## Technical solution

`solution/architecture.md` é a proposta técnica inicial e só é criada após `refinement/spec.md` estar `APPROVED`. Ela referencia o refinement, preserva as anotações iniciais do Tech Lead e indexa os detalhes de contrato, dados, integração ou exploração que forem necessários.

Quando a proposta precisar confirmar comportamento legado, `/technical-solution` pode delegar uma pergunta delimitada ao `service-researcher`. Cada rodada recebe um ID `RES-<n>` e devolve evidência concisa sobre fluxo, validações, integrações e linhagem de dados. A solução principal registra em `exploration.md` apenas o resultado que afetar proposta, risco ou decisão; a saída do agente não substitui julgamento técnico nem cria conhecimento durável automaticamente.

### Technical solution status

| Status | Meaning |
| --- | --- |
| `UNDER_DESIGN` | Proposta técnica em elaboração e discussão. |
| `READY_FOR_DISCUSSION` | Proposta coerente para discussão com o time; ainda não é aprovada. |
| `APPROVED` | Aceita explicitamente pelo Tech Lead como direção técnica. Não autoriza implementação por si só. |
| `BLOCKED` | Falta evidência, decisão, acesso ou definição identificável. |
| `CANCELED` | A proposta não seguirá; preserve o motivo. |
| `SUPERSEDED` | Substituída por outra proposta; aponte a sucessora. |

### Technical solution status transitions

| From | To | Condição | Quem registra |
| --- | --- | --- | --- |
| `UNDER_DESIGN` | `READY_FOR_DISCUSSION` | Completion check de `/technical-solution` satisfeito e confirmação explícita do Tech Lead. | `/technical-solution` |
| `READY_FOR_DISCUSSION` | `UNDER_DESIGN` | Discussão ou mudança no refinement exige ajuste. | `/technical-solution` |
| `READY_FOR_DISCUSSION` | `APPROVED` | Decisão explícita do Tech Lead. | Tech Lead decide; `/technical-solution` registra a pedido explícito. |
| qualquer | `BLOCKED` | Falta evidência, decisão ou acesso identificável; registre o bloqueio e quem o resolve. | `/technical-solution` |
| `BLOCKED` | `UNDER_DESIGN` | Bloqueio resolvido, com confirmação do Tech Lead. | `/technical-solution` |
| qualquer | `CANCELED` | Tech Lead decide não seguir; registre o motivo. | `/technical-solution` |
| qualquer | `SUPERSEDED` | Outra proposta substitui esta; aponte a sucessora. | `/technical-solution` |

Toda mudança atualiza `Status` e `Last updated` em `solution/architecture.md`. A aprovação técnica não autoriza implementação.

### Changes after approval

Alterar um `refinement/spec.md` `APPROVED` é decisão do Tech Lead: o pacote volta a `UNDER_REFINEMENT` via `/refinement` e precisa ser aprovado de novo. Se já existir `solution/`, a `architecture.md` mantém o `Refinement baseline` anterior até o Tech Lead confirmar a releitura via `/technical-solution`.

## Service repositories

Os repositórios dos serviços ficam em `project/<serviço>` na raiz do workspace. Somente o `service-researcher` os lê, sem alterá-los. Ele roda sem interação: as permissões estão pré-aprovadas em `.claude/settings.json` e o disparo é anunciado por hook. Oracle depende de ferramenta read-only configurada no ambiente; sem ela, a pesquisa de banco fica como lacuna.

## Classification of statements

Afirmações relevantes usam `Confirmed` (origem e data registradas), `Inference` (derivada, ainda não validada) ou `Open question` (sem resposta). Nenhuma inferência é promovida a fato sem confirmação do Tech Lead.
