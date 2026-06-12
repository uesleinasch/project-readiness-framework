# Exemplo Preenchido: Low-Level Design / TDD

> **Projeto fictício** para servir de referência de preenchimento do [`template-lld-tdd.md`](../Templates/template-lld-tdd.md). Os números são ilustrativos. Mesmo projeto de referência dos demais exemplos (SAP-Peças).

## Identificação

| Campo | Valor |
| :---- | :---- |
| **Projeto** | Portal de Autoatendimento de Solicitação de Peças (SAP-Peças) |
| **Dev Lead / Arquiteto** | Tiago Mota (Dev Lead) |
| **Aprovador (Tech Lead Aprovador)** | Carlos Menezes |
| **Versão / Data** | 1.0 / 01/07/2026 |
| **HLD de origem** | [exemplo-hld-preenchido.md](./exemplo-hld-preenchido.md) |

## 1. Diagrama de Componentes (C4 L3)

*Contêiner API (NestJS), módulos: `CatalogoModule` (busca, cache de catálogo), `PedidosModule` (carrinho, criação, status), `IntegracaoErpModule` (cliente REST, outbox, poller de status), `AuthModule` (OIDC, escopo por CNPJ).*

`[link do diagrama no Confluence: C4 L3 · SAP-Peças]`

## 2. Modelo de Dados

`[link do diagrama ER (dbdiagram.io)]`. Naming: snake_case, tabelas no singular, PKs `id` (UUID v7).

| Tabela/Entidade | Campos-chave | Índices | Constraints |
| :---- | :---- | :---- | :---- |
| `peca` | id, codigo, descricao, modelo_equipamento, ts_busca (tsvector) | GIN em `ts_busca`; UNIQUE em `codigo` | codigo NOT NULL |
| `pedido` | id, cnpj_concessionario, status, criado_em, erp_numero | B-tree em `(cnpj_concessionario, criado_em)` | status ∈ (rascunho, enviado, registrado, em_separacao, faturado, erro) |
| `pedido_item` | id, pedido_id, peca_id, quantidade | FK composta | quantidade > 0 |
| `outbox_erp` | id, pedido_id, payload, tentativas, proximo_retry_em | B-tree em `proximo_retry_em` | tentativas ≤ 10 |

## 3. Definição de APIs e Contratos

`[repo: docs/api/openapi.yaml]`

| Endpoint | Método | Auth | Payload | Respostas / Erros |
| :---- | :---- | :---- | :---- | :---- |
| `/pecas?q=&modelo=` | GET | OIDC (operador) | - | 200 lista paginada / 401 |
| `/pecas/{codigo}/disponibilidade` | GET | OIDC (operador) | - | 200 {qtd, atualizado_em} / 404 / 401 |
| `/pedidos` | POST | OIDC (operador) | {itens: [{codigo, qtd}]} | 201 / 409 sem-estoque / 422 / 401 |
| `/pedidos?status=` | GET | OIDC (operador) | - | 200 lista do próprio CNPJ / 401 |
| `/pedidos/{id}/export` | GET | OIDC (gestor) | - | 200 CSV / 403 / 401 |

## 4. Fluxos de Processo Detalhados

`[Confluence: diagramas de sequência]`: (a) busca com cache hit/miss; (b) confirmação de pedido com validação direta no ERP e gravação na outbox; (c) retry da outbox com backoff exponencial; (d) poller de status (10 min) atualizando `pedido.status` e disparando e-mail (SES).

## 5. Estratégia de Tratamento de Erros

Exceções de domínio mapeadas para HTTP via filtro global (RFC 9457, problem+json). Erros de integração ERP **não** vazam ao usuário: pedido fica `enviado` e a outbox faz retry; após 10 tentativas → status `erro` + alerta ao time e e-mail ao concessionário. Todos os erros logados com `trace_id` e `cnpj` (sem dados pessoais no log).

## 6. Estratégia de Cache

| O que | TTL | Invalidação | Ferramenta |
| :---- | :---- | :---- | :---- |
| Catálogo de peças | 24h | Recarga noturna (job 2h) + alerta staleness > 26h | Redis |
| Disponibilidade de estoque | 5 min | Expiração natural; bypass na confirmação do pedido | Redis |
| Assets da SPA | 7 dias | Hash no nome do arquivo a cada deploy | CloudFront |

## 7. Instrumentação de Observabilidade

| Componente | Logs | Métricas | Traces | SLI/SLO associado |
| :---- | :---- | :---- | :---- | :---- |
| CatalogoModule | Busca (termo, hits) | `busca_p95`, `cache_hit_ratio` | OpenTelemetry | Latência busca P95 < 800ms |
| PedidosModule | Ciclo de vida do pedido | `pedido_para_erp_segundos` | OpenTelemetry | Pedido→ERP < 1 min |
| IntegracaoErpModule | Tentativas/falhas outbox | `erp_erro_ratio`, `outbox_pendentes` | OpenTelemetry | Taxa de erro < 0,5% |
| API (global) | Acesso estruturado | `disponibilidade_api` | - | ≥ 99,5% (6h-22h) |

## 8. Pontos de Rollback / Feature Flags

Deploy rolling com rollback = redeploy da imagem anterior (mantidas as 5 últimas). Feature flags (variável de ambiente + tabela `feature_flag`): `exportacao_csv`, `notificacao_email`; permitem desligar funções secundárias sem deploy. Migrations sempre retrocompatíveis com a versão anterior da aplicação (expand/contract).

## 9. Migrations e Versionamento de Schema

| Item | Definição |
| :---- | :---- |
| Ferramenta | Flyway (SQL versionado em `db/migrations/`) |
| Estratégia de rollback | Expand/contract: nunca dropar coluna em uso; script de down apenas para dev; produção reverte por nova migration |

## 10. Estratégia de Testes

Massa de teste: catálogo sintético de 5k peças + CNPJs fictícios (sem dados reais). Ambientes: integração roda contra ERP *mock* (WireMock); staging usa homologação do ERP.

| Tipo | Cobertura mínima | Ferramenta | Responsável |
| :---- | :---- | :---- | :---- |
| Unitário | 80% (módulos de domínio) | Jest | Devs |
| Integração | Fluxos críticos (busca, pedido, outbox) | Jest + Testcontainers + WireMock | Devs |
| E2E | CA-001 e CA-002 + smoke por release | Playwright | QA |

## 11. Dependências e Bibliotecas

| Biblioteca | Versão | Licença | Justificativa |
| :---- | :---- | :---- | :---- |
| NestJS | 11.x | MIT | Framework base (HLD §4) |
| ioredis | 5.x | MIT | Cliente Redis |
| openid-client | 6.x | MIT | OIDC com IdP corporativo |
| @opentelemetry/sdk-node | 0.5x | Apache-2.0 | Traces/métricas (§7) |

## 12. Considerações de Performance

Ponto crítico: busca de catálogo (RNF-001). Benchmark alvo: 200 req/s com P95 < 800ms, atendido com cache (hit ~95%, ver spike). Full-text do PostgreSQL com GIN testado até 5k itens (P95 110ms no miss). Confirmação de pedido depende do ERP (P95 1,8s), comunicada na UI como etapa de "validação final".

## 13. Matriz de Rastreabilidade

> Versão consolidada em [exemplo-matriz-rastreabilidade-preenchida.md](./exemplo-matriz-rastreabilidade-preenchida.md).

| RF/RNF | Componente | Endpoint/API | Caso de Teste | Critério de Aceite |
| :---- | :---- | :---- | :---- | :---- |
| RF-001 | CatalogoModule | GET /pecas | TC-001 | CA-001 |
| RF-002 | CatalogoModule + Redis | GET /pecas/{codigo}/disponibilidade | TC-002 | CA-001 |
| RF-003 | PedidosModule + IntegracaoErpModule | POST /pedidos | TC-003 | CA-002 |

## 14. Reestimativa (pós-LLD)

| Item | Esforço estimado | Variação acumulada vs. BRD |
| :---- | :---- | :---- |
| Desenvolvimento | 3,5 squads-mês | +17% (dentro de ±25%; Solicitante ciente) |
