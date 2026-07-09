# Exemplo Preenchido: Checklist de Bootstrap Técnico

> **Projeto fictício** para servir de referência de preenchimento do [`template-bootstrap.md`](../Templates/template-bootstrap.md). Mesmo projeto de referência dos demais exemplos (SAP-Peças).
> Note que a etapa iniciou logo após a aprovação do HLD (17/06), **em paralelo** ao LLD, como o playbook permite.

## Identificação

| Campo | Valor |
| :---- | :---- |
| **Projeto** | Portal de Autoatendimento de Solicitação de Peças (SAP-Peças) |
| **Dev Lead / DevOps** | Tiago Mota / Equipe DevOps |
| **Aprovador (Tech Lead Aprovador)** | Carlos Menezes |
| **HLD de origem** | [exemplo-hld-preenchido.md](./exemplo-hld-preenchido.md) |
| **Data** | 10/07/2026 |

## Checklist

| # | Item | Status | Evidência (link) |
| :---- | :---- | :---- | :---- |
| 1 | Repositório criado com README, estrutura de pastas e CODEOWNERS | ☑ Concluído | `git.empresa.com/plataformas/sap-pecas` |
| 2 | Estratégia de branches definida conforme o documento de Gitflow da organização | ☑ Concluído | README §Branching (link para o Gitflow corporativo) |
| 3 | Pipeline CI/CD com build automatizado | ☑ Concluído | `.github/workflows/ci.yml` (run #42) |
| 4 | Secrets scan no pipeline | ☑ Concluído | Gitleaks no estágio `security` (run #42) |
| 5 | Lint e execução de testes no pipeline | ☑ Concluído | ESLint + Jest no estágio `quality` (run #42) |
| 6 | SAST configurado no pipeline | ☑ Concluído | Semgrep, relatório no run #42 |
| 7 | SCA (análise de dependências) configurado no pipeline | ☑ Concluído | Dependabot + osv-scanner, painel de vulnerabilidades |
| 8 | DAST configurado *(quando há superfície web exposta; senão, N/A)* | ☑ Concluído | OWASP ZAP baseline contra staging (job semanal) |
| 9 | Cofre de segredos definido (ex: AWS Secrets Manager / Parameter Store) | ☑ Concluído | AWS Secrets Manager: `sap-pecas/*` |
| 10 | Verificado: **nenhum segredo em repositório, log ou imagem** | ☑ Concluído | Gitleaks histórico completo limpo (run #42) |
| 11 | Ambientes dev/staging/prod provisionados ou com plano de provisionamento | ☑ Concluído | Terraform `infra/`: dev e staging ativos; prod planejado para a semana do piloto |
| 12 | Tagging de custo (FinOps) aplicado aos recursos | ☑ Concluído | Tags `projeto=sap-pecas`, `centro-custo=pos-vendas` em todos os módulos Terraform |
| 13 | Convenções de código e padrão de issues da organização referenciados no README | ☑ Concluído | README §Contribuindo |

## Aprovação

| Aprovador | Nome | Decisão | Data |
| :---- | :---- | :---- | :---- |
| Tech Lead Aprovador | Carlos Menezes | Aprovado | 10/07/2026 |

**Observações / pendências com prazo:** provisionamento de prod agendado para 22/09/2026 (uma semana antes do piloto), via mesmo módulo Terraform já validado em staging.
