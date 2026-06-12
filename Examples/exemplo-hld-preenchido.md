# Exemplo Preenchido: High-Level Design (HLD)

> **Projeto fictício** para servir de referência de preenchimento do [`template-hld.md`](../Templates/template-hld.md). Os números são ilustrativos. Mesmo projeto de referência dos demais exemplos (SAP-Peças).

## Identificação

| Campo | Valor |
| :---- | :---- |
| **Projeto** | Portal de Autoatendimento de Solicitação de Peças (SAP-Peças) |
| **Arquiteto / Tech Lead** | Paula Andrade (Arquiteta) |
| **Aprovador (Tech Lead Aprovador)** | Carlos Menezes |
| **Versão / Data** | 1.0 / 17/06/2026 |
| **BRD de origem** | [exemplo-brd-preenchido.md](./exemplo-brd-preenchido.md) |

## 1. Visão Geral da Arquitetura

**Monolito modular** (módulos: Catálogo, Pedidos, Integração-ERP) atrás de um frontend SPA. Justificativa: time de 1 squad, domínio coeso e tráfego previsível não justificam microsserviços; módulos com fronteiras explícitas preservam a opção de extração futura. O cache de disponibilidade (decisão derivada do spike; ver ADR-002) é o único componente de infraestrutura adicional.

## 2. Diagrama de Contexto (C4 L1)

*Concessionário (operador) → SAP-Peças → ERP de Pedidos (consulta de estoque + criação de pedido) e Diretório corporativo (SSO/OIDC).*

`[link do diagrama no Confluence: C4 L1 · SAP-Peças]`

## 3. Diagrama de Contêineres (C4 L2)

*SPA (React) → API (NestJS, monolito modular) → PostgreSQL (RDS), Redis (ElastiCache, cache de catálogo/disponibilidade) → ERP (REST) / IdP corporativo (OIDC).*

`[link do diagrama no Confluence: C4 L2 · SAP-Peças]`

## 4. Stack Tecnológica

| Camada | Tecnologia | Justificativa |
| :---- | :---- | :---- |
| Frontend | React 19 + TypeScript | Padrão da organização; componentes do design system corporativo |
| Backend | NestJS (Node 22) | Experiência do time; estrutura modular nativa alinhada ao estilo arquitetural |
| Banco de dados | PostgreSQL 16 (AWS RDS) | Ver ADR-001 |
| Cache | Redis 7 (AWS ElastiCache) | Ver ADR-002 (mitigação do limite de carga do ERP) |
| Nuvem / Serviços | AWS (ECS Fargate, RDS, ElastiCache, CloudFront, SES) | Contrato corporativo vigente; Fargate elimina gestão de instâncias |

## 5. Análise Build vs Buy vs Open Source

| Componente | Opções avaliadas | Decisão | Critério decisivo | ADR |
| :---- | :---- | :---- | :---- | :---- |
| Autenticação | Build / Keycloak / IdP corporativo existente | **IdP corporativo (OIDC)** | TCO zero incremental; concessionários já têm conta no diretório | ADR-003 |
| Busca de catálogo | PostgreSQL full-text / Elasticsearch / SaaS de busca | **PostgreSQL full-text** | 5k itens não justificam cluster dedicado; evita lock-in e custo recorrente | ADR-004 |
| Notificação de status | Build (SMTP) / AWS SES / SaaS de e-mail | **AWS SES** | Volume baixo (~200 e-mails/dia); custo marginal; já homologado na organização | ADR-005 |

## 6. Topologia de Infraestrutura

VPC dedicada com 2 AZs (sa-east-1): subnets públicas (ALB) e privadas (ECS Fargate, RDS, ElastiCache). CloudFront na frente da SPA. Saída para o ERP via VPN site-to-site existente. Sem exposição direta do banco ou do Redis.

## 7. Fluxo de Dados Principal

Busca: SPA → API → cache Redis (hit ~95%) → fallback PostgreSQL (catálogo replicado do ERP via carga noturna). Disponibilidade: cache com TTL 5 min; na confirmação do pedido, consulta direta ao ERP (validação final). Pedido: API → fila interna (tabela outbox) → integração ERP com retry; status retorna por polling a cada 10 min.

## 8. Estratégia de Segurança

Autenticação via OIDC no IdP corporativo (sem senha local). Autorização por papel (operador / gestor do concessionário) com escopo por CNPJ do concessionário. TLS em todas as conexões; dados em repouso criptografados (RDS/ElastiCache nativos). Threat modeling STRIDE realizado nos fluxos de login, busca e envio de pedido. Principal ameaça identificada: *tampering* de escopo de concessionário (operador acessar pedidos de outro CNPJ); controle: claim de CNPJ no token + filtro obrigatório na camada de dados.

## 9. Estratégia de Escalabilidade

Horizontal no ECS (2-8 tasks, gatilho: CPU > 60% ou P95 > 600ms). RDS com read replica opcional (decisão adiada; gatilho documentado: consultas > 70% da capacidade). Redis dimensionado para o catálogo completo em memória.

## 10. Estratégia de Deploy (preliminar)

**Rolling** no ECS (substituição gradual de tasks com health check). Blue/green descartado nesta fase: monolito sem migração de tráfego complexa; rollback é o redeploy da imagem anterior.

## 11. Estratégia de Observabilidade (preliminar)

| SLI candidato | SLO alvo |
| :---- | :---- |
| Latência de busca (P95) | < 800ms (RNF-001) |
| Disponibilidade da API (6h-22h) | ≥ 99,5% (RNF-002) |
| Tempo pedido-portal → pedido-ERP | < 1 min (CA-002) |
| Taxa de erro na integração ERP | < 0,5% com retry automático |

## 12. Requisitos de DR

| Parâmetro | Valor |
| :---- | :---- |
| RTO | 4h |
| RPO | 1h |
| Estratégia macro | Multi-AZ no RDS; backup automatizado (PITR); infraestrutura recriável via IaC (Terraform); cache é reconstruível (não exige DR) |

## 13. Riscos Técnicos

| ID | Descrição | Prob. | Impacto | Mitigação |
| :---- | :---- | :---- | :---- | :---- |
| RISK-003 | Janela de carga noturna do catálogo falhar e servir dados defasados | Média | Médio | Alerta de staleness (> 26h); fallback de consulta direta com degradação anunciada |
| RISK-004 | VPN site-to-site com o ERP indisponível | Baixa | Alto | Fila outbox segura pedidos até 24h; runbook de reprocessamento |

## 14. Pontos de Decisão Técnica (ADR)

| ADR | Decisão | Status |
| :---- | :---- | :---- |
| ADR-001 | PostgreSQL 16 (RDS) como banco principal | Aceita |
| ADR-002 | Cache Redis para catálogo e disponibilidade (mitigação do limite do ERP) | Aceita |
| ADR-003 | Autenticação via IdP corporativo (OIDC), sem identidade local | Aceita |
| ADR-004 | Busca com PostgreSQL full-text (sem Elasticsearch) | Aceita |
| ADR-005 | Notificações por AWS SES | Aceita |

## 15. Reestimativa (pós-HLD)

| Item | Estimativa atualizada | Variação vs. BRD |
| :---- | :---- | :---- |
| Construção (infra + desenvolvimento) | ~3,2 squads-mês + R$ 20k de infra de projeto | +8% |
| Custo recorrente anual (TCO de operação) | ~R$ 78k/ano (inclui ElastiCache) | +8% |
