# Exemplo Preenchido: Solution Architecture Document (SAD)

> **Projeto fictício** para servir de referência de preenchimento do [`template-sad.md`](../Templates/template-sad.md). Os números são ilustrativos. Mesmo projeto de referência dos demais exemplos (SAP-Peças).
> Repare que cada seção **consolida** conteúdo dos artefatos anteriores (coluna "Origem" do template): o SAD não reescreve nada do zero.

## Identificação

| Campo | Valor |
| :---- | :---- |
| **Projeto** | Portal de Autoatendimento de Solicitação de Peças (SAP-Peças) |
| **Arquiteto** | Paula Andrade |
| **Aprovador (Tech Lead Aprovador)** | Carlos Menezes |
| **Versão / Data** | 1.0 / 08/07/2026 |

## 1. Visão Geral

Portal web de autoatendimento para 480 concessionários solicitarem peças de reposição, substituindo o fluxo manual por telefone/e-mail (3h20 por pedido) por autoatendimento 24/7 (< 15 min). Integra-se ao ERP de Pedidos (estoque e criação de pedido) e ao IdP corporativo (SSO). Fora do escopo: pagamento online e logística. *(Origem: BRD §1-3.)*

## 2. Requisitos e Restrições

Requisitos com impacto arquitetural: RNF-001 (busca P95 < 800ms, que motiva o cache), RNF-002 (99,5% em 6h-22h, que motiva multi-AZ), RF-002 (disponibilidade em tempo real, limitada pelo ERP, ver spike), CA-002 (pedido no ERP < 1 min, que motiva a outbox com retry). Restrições: API atual do ERP imutável nesta fase (limite ~60 req/s, evidência do spike); orçamento de infra ≤ R$ 8k/mês. *(Origem: BRD §5-6, §9; Spike.)*

## 3. Decisões Arquiteturais (ADR)

| ADR | Decisão | Status |
| :---- | :---- | :---- |
| ADR-001 | PostgreSQL 16 (RDS) como banco principal | Aceita |
| ADR-002 | Cache Redis para catálogo e disponibilidade (mitigação do limite do ERP) | Aceita |
| ADR-003 | Autenticação via IdP corporativo (OIDC), sem identidade local | Aceita |
| ADR-004 | Busca com PostgreSQL full-text (sem Elasticsearch) | Aceita |
| ADR-005 | Notificações por AWS SES | Aceita |

## 4. Visão Arquitetural

Monolito modular (Catálogo, Pedidos, Integração-ERP, Auth) servido por SPA React. Diagramas: C4 L1 e L2 no HLD; C4 L3 no LLD `[links no Confluence]`. Interação crítica: busca atende do cache (hit ~95%); confirmação de pedido valida estoque direto no ERP e grava na outbox para entrega garantida. *(Origem: HLD §1-3, LLD §1.)*

## 5. Infraestrutura e Deploy

VPC em 2 AZs (sa-east-1); ALB → ECS Fargate (2-8 tasks) → RDS multi-AZ + ElastiCache; CloudFront na SPA; VPN site-to-site para o ERP. IaC em Terraform. Pipeline CI/CD: build + lint + testes + secrets scan + SAST/SCA → deploy rolling com health check; rollback por redeploy da imagem anterior. *(Origem: HLD §6, §10; Bootstrap.)*

## 6. Segurança

OIDC no IdP corporativo; autorização por papel com escopo por CNPJ (principal ameaça do STRIDE: acesso entre concessionários; controle: claim no token + filtro na camada de dados). TLS em trânsito, criptografia em repouso (RDS/ElastiCache). Segredos no AWS Secrets Manager. LGPD: coleta apenas contato do operador; RIPD aprovado pela DPO. *(Origem: HLD §8; LLD §5; baseline de segurança.)*

## 7. Observabilidade

| SLI | SLO | Alerta |
| :---- | :---- | :---- |
| Latência de busca P95 | < 800ms | P95 > 800ms por 10 min |
| Disponibilidade API (6h-22h) | ≥ 99,5% | Error budget 50% consumido |
| Pedido portal → ERP | < 1 min | `outbox_pendentes` > 50 ou pedido > 5 min |
| Erro integração ERP | < 0,5% | Taxa > 0,5% em 15 min |

Logs estruturados com `trace_id`; traces OpenTelemetry; dashboard único do serviço. *(Origem: HLD §11; LLD §7.)*

## 8. Estratégia de Dados

Catálogo replicado do ERP (carga noturna, staleness máx. 26h); pedidos como fonte de verdade local até confirmação no ERP (`erp_numero`). Classificação: Interno. Retenção: pedidos 5 anos (fiscal); logs 90 dias. Backup RDS com PITR (RPO 1h). Dados pessoais limitados a nome/e-mail do operador (minimização LGPD). *(Origem: BRD definições; LLD §2.)*

## 9. Riscos e Mitigações

> Consolidação; registro completo em [exemplo-registro-riscos-preenchido.md](./exemplo-registro-riscos-preenchido.md).

| ID | Descrição | Prob. | Impacto | Mitigação | Responsável |
| :---- | :---- | :---- | :---- | :---- | :---- |
| RISK-001 | ERP não suporta volume de consultas | Média | Alto | Cache (ADR-002), validado por spike | Paula Andrade |
| RISK-002 | Baixa adoção pelos concessionários | Média | Médio | Piloto com 20 concessionários | Marina Couto |
| RISK-003 | Carga noturna falha, catálogo defasado | Média | Médio | Alerta staleness + fallback | Tiago Mota |
| RISK-004 | VPN com ERP indisponível | Baixa | Alto | Outbox 24h + runbook | DevOps |

## 10. Cronograma e Marcos

| Marco | Data prevista |
| :---- | :---- |
| Go/No-Go | 15/07/2026 |
| Sprint 1 (fundação + catálogo) | 31/07/2026 |
| Piloto (20 concessionários) | 30/09/2026 |
| Rollout geral (480) | 30/11/2026 |

## 11. Glossário

| Termo | Definição |
| :---- | :---- |
| Concessionário | Revenda autorizada que solicita peças de reposição |
| Outbox | Tabela de saída transacional que garante entrega do pedido ao ERP com retry |
| Staleness | Idade do dado em cache desde a última atualização da origem |

## 12. Histórico de Revisões

| Versão | Data | Autor | Mudanças |
| :---- | :---- | :---- | :---- |
| 0.1 | 17/06/2026 | Paula Andrade | Esqueleto criado junto com o HLD (documento vivo) |
| 0.2 | 01/07/2026 | Paula Andrade | Seções 3, 4, 7 e 8 atualizadas com o LLD |
| 1.0 | 08/07/2026 | Paula Andrade | Consolidação final para o Go/No-Go |
