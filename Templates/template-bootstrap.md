# Template: Checklist de Bootstrap Técnico

> **Etapa 5 do Day Zero Playbook.** Produtor: Dev Lead / DevOps. Aprovação: Tech Lead Aprovador (produtor ≠ aprovador).
> **Pode iniciar assim que o HLD (Etapa 2) for aprovado**, em paralelo às Etapas 3 e 4.
> DoD: repositório e gitflow operacionais; pipeline executa build + secrets scan + SAST/SCA; segredos fora do código; ambientes provisionados ou planejados com tagging de custo.

---

## Identificação

| Campo | Valor |
| :---- | :---- |
| **Projeto** | `<...>` |
| **Dev Lead / DevOps** | `<...>` |
| **Aprovador (Tech Lead Aprovador)** | `<...>` |
| **HLD de origem** | `<link>` |
| **Data** | `<dd/mm/aaaa>` |

## Checklist

| # | Item | Status | Evidência (link) |
| :---- | :---- | :---- | :---- |
| 1 | Repositório criado com README, estrutura de pastas e CODEOWNERS | ☐ | `<link do repo>` |
| 2 | Estratégia de branches definida e documentada no README (ex.: Gitflow, trunk-based) | ☐ | `<...>` |
| 3 | Pipeline CI/CD com build automatizado | ☐ | `<link do pipeline>` |
| 4 | Secrets scan no pipeline | ☐ | `<...>` |
| 5 | Lint e execução de testes no pipeline | ☐ | `<...>` |
| 6 | SAST configurado no pipeline | ☐ | `<...>` |
| 7 | SCA (análise de dependências) configurado no pipeline | ☐ | `<...>` |
| 8 | DAST configurado *(quando há superfície web exposta; senão, N/A)* | ☐ | `<...>` |
| 9 | Cofre de segredos definido (ex: AWS Secrets Manager / Parameter Store) | ☐ | `<...>` |
| 10 | Verificado: **nenhum segredo em repositório, log ou imagem** | ☐ | `<...>` |
| 11 | Ambientes dev/staging/prod provisionados ou com plano de provisionamento | ☐ | `<...>` |
| 12 | Tagging de custo (FinOps) aplicado aos recursos | ☐ | `<...>` |
| 13 | Convenções de código e formato de issues do projeto referenciados no README | ☐ | `<...>` |

## Aprovação

| Aprovador | Nome | Decisão | Data |
| :---- | :---- | :---- | :---- |
| Tech Lead Aprovador | `<...>` | Aprovado / Reprovado | `<...>` |

**Observações / pendências com prazo:** `<...>`
