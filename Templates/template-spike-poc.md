# Template: Spike / PoC Report

> **Etapa 0 do Day Zero Playbook** *(opcional)*. Produtor: Arquiteto / Dev Lead. Aprovação: Tech Lead Aprovador (produtor ≠ aprovador).
> Etapa **timeboxed** de redução de incerteza. Gera aprendizado, não código de produção. **O código do spike é descartável**.
> DoD: pergunta de viabilidade respondida com evidência; recomendação registrada; premissas resultantes prontas para entrar no BRD/HLD.

---

## Identificação

| Campo | Valor |
| :---- | :---- |
| **Projeto / Oportunidade** | `<...>` |
| **Produtor** | `<nome (Arquiteto / Dev Lead)>` |
| **Aprovador (Tech Lead Aprovador)** | `<...>` |
| **Data** | `<dd/mm/aaaa>` |
| **Status** | Rascunho / Em revisão / Aprovado |

## 1. Pergunta de Viabilidade

> *A hipótese a validar, com critério objetivo de sucesso/falha. Formato: "é possível X dentro de Y?".*

**Hipótese:** `<...>`

**Critério de sucesso:** `<mensurável, ex: processar 1.000 req/s com P95 < 200ms usando a stack Z>`

## 2. Timebox

| Campo | Valor |
| :---- | :---- |
| **Esforço máximo alocado** | `<ex: 3-5 dias>` |
| **Esforço efetivamente gasto** | `<...>` |

## 3. Experimento

> *O que foi construído/testado (descartável): abordagem, ferramentas, cenários executados.*

`<...>`

## 4. Resultado

| Campo | Valor |
| :---- | :---- |
| **Veredito** | ☐ Validado / ☐ Invalidado / ☐ Inconclusivo |
| **Evidências** | `<medições, logs, screenshots, links>` |

## 5. Impacto no Projeto

> *Premissas confirmadas/derrubadas que alimentam o BRD e o HLD.*

| Premissa | Confirmada / Derrubada | Impacto (BRD/HLD) |
| :---- | :---- | :---- |
| `<...>` | `<...>` | `<...>` |

## 6. Recomendação

> *Seguir / pivotar / não seguir, com justificativa.*

**Recomendação:** ☐ Seguir / ☐ Pivotar / ☐ Não seguir

**Justificativa:** `<...>`

---

| ⚠️ LEMBRETE | *O código produzido neste spike é descartável e **não vai para produção**. Aprendizados entram no BRD/HLD; código entra no lixo.* |
| :---: | :---- |
