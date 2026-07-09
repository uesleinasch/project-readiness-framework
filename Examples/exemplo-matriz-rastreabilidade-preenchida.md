# Exemplo Preenchido: Matriz de Rastreabilidade

> **Projeto fictício** para servir de referência de preenchimento do [`template-matriz-rastreabilidade.md`](../Templates/template-matriz-rastreabilidade.md). Mesmo projeto de referência dos demais exemplos (SAP-Peças).
> Regra do playbook: **nenhum RF/RNF sem componente nem sem caso de teste.** Toda linha abaixo está completa. É isso que o item 11 do Go/No-Go verifica.

| RF/RNF | Descrição | Componente (HLD/LLD) | Endpoint / API | Caso de Teste | Critério de Aceite / SLO |
| :---- | :---- | :---- | :---- | :---- | :---- |
| RF-001 | Buscar peça por código ou modelo de equipamento | CatalogoModule | GET /pecas | TC-001 | CA-001 |
| RF-002 | Exibir disponibilidade de estoque em tempo real | CatalogoModule + Redis (ADR-002) | GET /pecas/{codigo}/disponibilidade | TC-002 | CA-001 |
| RF-003 | Montar carrinho e enviar pedido ao ERP | PedidosModule + IntegracaoErpModule (outbox) | POST /pedidos | TC-003 | CA-002 |
| RF-004 | Acompanhar status do pedido | PedidosModule (poller de status + SES) | GET /pedidos?status= | TC-004 | CA-001 |
| RF-005 | Exportar histórico de pedidos em CSV | PedidosModule (feature flag `exportacao_csv`) | GET /pedidos/{id}/export | TC-005 | CA-001 |
| RNF-001 | Performance: busca P95 < 800ms | CatalogoModule + Redis | - | TC-006 (teste de carga k6) | SLO: P95 < 800ms |
| RNF-002 | Disponibilidade 99,5% (6h-22h) | Infra (ECS multi-AZ + RDS multi-AZ) | - | TC-007 (game day / falha de AZ) | SLO: ≥ 99,5% |
| RNF-003 | Segurança: SSO corporativo; escopo por CNPJ | AuthModule (OIDC) | - | TC-008 (E2E login) + TC-010 (isolamento de CNPJ) | SLO: 0 acessos entre CNPJs |
| RNF-004 | Acessibilidade WCAG 2.1 AA; PT-BR e ES | SPA (design system + i18n) | - | TC-009 (axe-core + revisão manual) | WCAG 2.1 AA sem violações críticas |

> **Cobertura final:** 5/5 RFs e 4/4 RNFs com componente e caso de teste: matriz apta para o Go/No-Go (item 11).
