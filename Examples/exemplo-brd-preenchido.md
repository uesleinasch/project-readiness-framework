# Exemplo Preenchido: BRD

> **Projeto fictício** para servir de referência de preenchimento do [`template-brd.md`](../Templates/template-brd.md). Os números são ilustrativos. Mesmo projeto de referência dos demais exemplos (SAP-Peças).

## Identificação

| Campo | Valor |
| :---- | :---- |
| **Projeto** | Portal de Autoatendimento de Solicitação de Peças (SAP-Peças) |
| **Solicitante (Business Owner)** | Marina Couto (Diretoria de Pós-Vendas) |
| **PM** | Rafael Lima |
| **Data** | 05/06/2026 |
| **Versão** | 1.0 |
| **Status** | Aprovado |
| **Repositório de Documentação** | Confluence, espaço SAP-Peças (ferramenta acordada entre as partes) |

## 1. Contexto e Problema

Hoje a solicitação de peças de reposição pelos concessionários é feita por telefone e e-mail. Um atendente registra o pedido manualmente no ERP, o que gera fila, erros de digitação de código de peça e tempo médio de registro de **3h20** por pedido. Em picos de safra, a fila chega a 2 dias úteis, gerando insatisfação e perda de vendas para o mercado paralelo.

## 2. Objetivo de Negócio

Reduzir o tempo médio de registro de pedido de peças de **3h20 para menos de 15 minutos** (-92%) e eliminar a fila de digitação manual até o fim do 4º trimestre de 2026, permitindo autoatendimento 24/7 pelos concessionários.

## 3. Escopo

**Dentro do escopo:**
- Catálogo de peças com busca por código e por equipamento.
- Carrinho, conferência de disponibilidade e envio do pedido ao ERP.
- Acompanhamento de status do pedido pelo concessionário.

**Fora do escopo (exclusões explícitas):**
- Pagamento online (mantém-se o faturamento atual via ERP).
- Logística/transportadora (sistema existente, fora deste projeto).

**Sistemas impactados / integrações:**
- ERP de Pedidos (API de criação de pedido e consulta de estoque).
- Diretório corporativo (SSO dos concessionários).

## 4. Stakeholders

| Papel | Nome | Responsabilidade |
| :---- | :---- | :---- |
| Solicitante | Marina Couto | Aprova negócio e custo |
| Usuários finais | Concessionários (≈ 480) | Usam o portal |
| Responsável técnico | Equipe de Plataformas Digitais | Sustentação |
| Aprovadores | Marina Couto + Tech Lead Aprovador | Go/No-Go |

## 5. Requisitos Funcionais

| ID | Descrição | Prioridade |
| :---- | :---- | :---- |
| RF-001 | Buscar peça por código ou por modelo de equipamento | Must |
| RF-002 | Exibir disponibilidade de estoque em tempo real (via ERP) | Must |
| RF-003 | Montar carrinho e enviar pedido ao ERP | Must |
| RF-004 | Acompanhar status do pedido (registrado, em separação, faturado) | Should |
| RF-005 | Exportar histórico de pedidos em CSV | Could |

## 6. Requisitos Não-Funcionais

| ID | Categoria | Métrica/Alvo |
| :---- | :---- | :---- |
| RNF-001 | Performance | Busca de peça com P95 < 800ms |
| RNF-002 | Disponibilidade | 99,5% no horário comercial estendido (6h-22h) |
| RNF-003 | Segurança | SSO corporativo; sem dados de cartão |
| RNF-004 | Acessibilidade / i18n | WCAG 2.1 AA; PT-BR e ES |

## 7. Critérios de Aceite (de negócio)

| ID | Critério |
| :---- | :---- |
| CA-001 | Concessionário cria um pedido completo em < 15 min sem ajuda de atendente |
| CA-002 | Pedido criado no portal aparece no ERP em < 1 min |

## 8. Processo As-Is / To-Be

**As-Is:** concessionário liga/e-mail → atendente digita no ERP → confirma por e-mail. ~3h20.
**To-Be:** concessionário acessa o portal (SSO) → busca, monta carrinho, envia → pedido entra automaticamente no ERP. <15 min.

## 9. Restrições e Premissas

**Restrições:** integração limitada à API atual do ERP (sem alterações no ERP nesta fase); orçamento aprovado de infra até R$ 8k/mês.
**Premissas:** a API de estoque do ERP suporta a carga de consultas; concessionários já possuem conta no diretório corporativo.

## 10. Riscos de Negócio (inicial)

| ID | Descrição | Probabilidade | Impacto | Mitigação |
| :---- | :---- | :---- | :---- | :---- |
| RISK-001 | API de estoque do ERP não suportar o volume de consultas | Média | Alto | Spike de carga na Etapa 0; cache de disponibilidade |
| RISK-002 | Baixa adoção pelos concessionários | Média | Médio | Piloto com 20 concessionários antes do rollout |

## 11. Custo Estimado (ordem de grandeza)

| Item | Estimativa |
| :---- | :---- |
| Construção: desenvolvimento | ~3 squads-mês (será refinado após LLD) |
| Construção: infraestrutura de projeto | ~R$ 18k (ambientes de dev/staging durante o projeto) |
| Custo recorrente anual (TCO de operação: infra, licenças, suporte) | ~R$ 72k/ano (~R$ 6k/mês; será refinado após HLD) |

## 12. Justificativa de Negócio

A automação elimina ~480h/mês de digitação manual e reduz a fila de safra, recuperando vendas hoje perdidas para o mercado paralelo (estimativa de R$ 1,2M/ano). O payback do investimento de infra+desenvolvimento ocorre em menos de 8 meses.

_Assinatura do Solicitante: **Marina Couto** · Data: 05/06/2026_

---

## Definições Obrigatórias

| Dimensão | Definição |
| :---- | :---- |
| **Criticidade** | Alta → impacta receita de pós-vendas e operação de concessionários |
| **Nº de Usuários** | Inicial: 480 concessionários / Pico (safra): ~150 simultâneos |
| **Multi-idioma** | Sim → PT-BR e ES |
| **Autoscaling** | Necessário (picos de safra) |
| **Dados Pessoais (LGPD)** | Coleta (dados de contato do operador do concessionário) |
| **Classificação de Dados** | Interno |
| **Requisitos Regulatórios / Compliance** | LGPD |
| **Integrações Externas** | ERP de Pedidos (API REST); Diretório corporativo (SSO/OIDC) |
| **Disponibilidade / DR** | RTO: 4h / RPO: 1h |
| **Interface de Usuário (UX)** | Possui UI → portal web para concessionários; wireframes a aprovar antes do LLD |
| **Uso de IA** | Não usa |
