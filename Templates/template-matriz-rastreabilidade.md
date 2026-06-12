# Template: Matriz de Rastreabilidade

> **Artefato transversal (Capítulo 6 do framework).** Garante que todo requisito foi desenhado e será testado (Princípio P3).
> Preenchida ao longo do **HLD/LLD** e validada no **Go/No-Go** (item 11).
> Regra: **nenhum RF/RNF pode ficar sem componente nem sem caso de teste.**

| RF/RNF | Descrição | Componente (HLD/LLD) | Endpoint / API | Caso de Teste | Critério de Aceite / SLO |
| :---- | :---- | :---- | :---- | :---- | :---- |
| RF-001 | `<...>` | `<componente responsável>` | `<endpoint, se houver>` | TC-001 | CA-001 |
| RF-002 | `<...>` | `<...>` | `<...>` | TC-002 | CA-002 |
| RNF-001 | `<ex: performance>` | `<...>` | - | TC-0NN | SLO-001 |

> **Cobertura:** ao final, toda linha de RF deve ter as colunas Componente e Caso de Teste preenchidas. Lacunas indicam requisito não desenhado ou não testável.
