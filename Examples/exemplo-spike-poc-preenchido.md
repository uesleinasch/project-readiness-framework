# Exemplo Preenchido: Spike / PoC Report

> **Projeto fictício** para servir de referência de preenchimento do [`template-spike-poc.md`](../Templates/template-spike-poc.md). Os números são ilustrativos. Mesmo projeto de referência dos demais exemplos (SAP-Peças).

## Identificação

| Campo | Valor |
| :---- | :---- |
| **Projeto / Oportunidade** | Portal de Autoatendimento de Solicitação de Peças (SAP-Peças) |
| **Produtor** | Paula Andrade (Arquiteta) |
| **Aprovador (Tech Lead Aprovador)** | Carlos Menezes |
| **Data** | 26/05/2026 |
| **Status** | Aprovado |

## 1. Pergunta de Viabilidade

**Hipótese:** a API de consulta de estoque do ERP suporta a carga de consultas geradas por 480 concessionários em autoatendimento, sem degradar o ERP.

**Critério de sucesso:** sustentar **200 req/s** de consulta de disponibilidade com P95 < 800ms por 30 minutos, sem elevar o consumo de CPU do ERP acima de 70%.

## 2. Timebox

| Campo | Valor |
| :---- | :---- |
| **Esforço máximo alocado** | 5 dias |
| **Esforço efetivamente gasto** | 4 dias |

## 3. Experimento

Script de carga (k6) disparando consultas de disponibilidade contra o ambiente de homologação do ERP, com massa de 5.000 códigos de peça reais anonimizados. Três cenários: 50, 100 e 200 req/s, com monitoramento de CPU/memória do ERP pelo time de sustentação. Código do experimento em repositório descartável (`spike-sap-pecas-carga`).

## 4. Resultado

| Campo | Valor |
| :---- | :---- |
| **Veredito** | ☑ Invalidado *(parcialmente; viável com mitigação)* |
| **Evidências** | Relatório k6 anexo no Confluence; gráficos de CPU do ERP. A API sustentou **60 req/s** com P95 de 650ms; acima disso o ERP passou de 80% de CPU e o P95 estourou para 2,4s. |

## 5. Impacto no Projeto

| Premissa | Confirmada / Derrubada | Impacto (BRD/HLD) |
| :---- | :---- | :---- |
| API de estoque suporta consulta direta em tempo real | **Derrubada** (limite ~60 req/s) | HLD deve prever **cache de disponibilidade** com TTL curto; consulta direta só no fechamento do pedido |
| Massa de códigos de peça é estável (catálogo muda pouco no dia) | Confirmada | Viabiliza cache de catálogo com TTL longo |
| ERP aceita criação de pedido via API na carga esperada | Confirmada (pedidos são ~2% do volume de consultas) | Sem mudança no fluxo de envio de pedido |

## 6. Recomendação

**Recomendação:** ☑ Seguir *(com mitigação obrigatória no HLD)*

**Justificativa:** a viabilidade do autoatendimento se mantém desde que a arquitetura adote camada de cache para disponibilidade (TTL curto) e catálogo (TTL longo), reservando a consulta direta ao ERP para o momento de confirmação do pedido. A mitigação entra como decisão registrada em ADR na Etapa 2 (ver ADR-002) e o RISK-001 do BRD passa a ter mitigação validada por evidência.

---

| ⚠️ LEMBRETE | *O código produzido neste spike é descartável e **não vai para produção**. Aprendizados entram no BRD/HLD; código entra no lixo.* |
| :---: | :---- |
