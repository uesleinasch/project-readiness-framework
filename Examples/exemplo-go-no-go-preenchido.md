# Exemplo Preenchido: Checklist de Go / No-Go

> **Projeto fictício** para servir de referência de preenchimento do [`template-go-no-go.md`](../Templates/template-go-no-go.md). Mesmo projeto de referência dos demais exemplos (SAP-Peças).
> Repare: toda linha tem **evidência** (link do artefato). O item 23 é Condicional e está N/A porque o BRD define "Uso de IA: Não usa".

## Identificação

| Campo | Valor |
| :---- | :---- |
| **Projeto** | Portal de Autoatendimento de Solicitação de Peças (SAP-Peças) |
| **Criticidade** | Alta |
| **PM** | Rafael Lima |
| **Tech Lead Aprovador** | Carlos Menezes |
| **Segurança / DPO** | Fernanda Ribeiro |
| **Data da avaliação** | 15/07/2026 |

## Checklist

| # | Item | Obrigatoriedade | Status | Evidência (link) |
| :---- | :---- | :---- | :---- | :---- |
| 1 | BRD escrito, revisado e aprovado pelo solicitante | Obrigatório | ☑ Concluído | [exemplo-brd-preenchido.md](./exemplo-brd-preenchido.md) |
| 2 | Justificativa de negócio (mín. 1 parágrafo) aprovada | Obrigatório | ☑ Concluído | BRD §12 (assinado em 05/06/2026) |
| 3 | Processo as-is e to-be mapeados | Obrigatório | ☑ Concluído | BRD §8 + diagrama no Confluence |
| 4 | Custo estimado aprovado e reestimado após HLD e LLD (±25%) | Obrigatório | ☑ Concluído | BRD §11 → HLD §15 (+8%) → LLD §14 (+17%, Solicitante ciente) |
| 5 | Dimensões obrigatórias do BRD definidas (criticidade, usuários, multi-idioma, autoscaling, classificação de dados, compliance, UX, uso de IA) | Obrigatório | ☑ Concluído | BRD §Definições Obrigatórias |
| 6 | Avaliação de impacto LGPD concluída e aprovada por Segurança/DPO | Obrigatório | ☑ Concluído | RIPD no Confluence, aprovado por Fernanda Ribeiro em 03/07/2026 |
| 7 | HLD elaborado com C4 L1 e L2 | Obrigatório | ☑ Concluído | [exemplo-hld-preenchido.md](./exemplo-hld-preenchido.md) §2-3 |
| 8 | Estratégias de deploy, observabilidade e DR (RTO/RPO) definidas | Obrigatório | ☑ Concluído | HLD §10-12 |
| 9 | HLD aprovado pelo Tech Lead Aprovador (produtor ≠ aprovador) | Obrigatório | ☑ Concluído | Aprovação no Confluence em 17/06/2026 |
| 10 | LLD/TDD com modelo de dados e contratos de API | Obrigatório | ☑ Concluído | [exemplo-lld-tdd-preenchido.md](./exemplo-lld-tdd-preenchido.md) §2-3 + openapi.yaml no repo |
| 11 | Matriz de rastreabilidade (RF → design → teste) preenchida | Obrigatório | ☑ Concluído | [exemplo-matriz-rastreabilidade-preenchida.md](./exemplo-matriz-rastreabilidade-preenchida.md): 9/9 linhas completas |
| 12 | SAD consolidado e versionado no Repositório de Documentação | Obrigatório | ☑ Concluído | [exemplo-sad-preenchido.md](./exemplo-sad-preenchido.md) v1.0, no Confluence (ferramenta acordada no BRD) |
| 13 | ADRs registrados para decisões relevantes | Obrigatório | ☑ Concluído | ADR-001 a ADR-005 em `docs/adr/` ([exemplo](./exemplo-adr-preenchido.md)) |
| 14 | Registro de Riscos consolidado com mitigações | Obrigatório | ☑ Concluído | [exemplo-registro-riscos-preenchido.md](./exemplo-registro-riscos-preenchido.md); riscos de impacto Alto mitigados |
| 15 | Repositório criado com estrutura e gitflow definidos | Obrigatório | ☑ Concluído | Bootstrap itens 1-2 |
| 16 | Pipeline CI/CD (build + secrets scan + SAST/SCA) configurado | Obrigatório | ☑ Concluído | Bootstrap itens 3-7 ([exemplo](./exemplo-bootstrap-preenchido.md)) |
| 17 | Gestão de segredos definida (sem segredos no código) | Obrigatório | ☑ Concluído | Bootstrap itens 9-10 |
| 18 | Ambientes (dev/staging/prod) provisionados ou planejados | Obrigatório | ☑ Concluído | Bootstrap item 11 (prod planejado: 22/09/2026) |
| 19 | Estratégia de testes definida (unitário, integração, E2E) | Obrigatório | ☑ Concluído | LLD §10 |
| 20 | Gate de segurança aprovado por Segurança/DPO | Obrigatório | ☑ Concluído | Parecer de Fernanda Ribeiro no Confluence, 14/07/2026 |
| 21 | Equipe de desenvolvimento alocada, com papéis e capacidade confirmados | Obrigatório | ☑ Concluído | Squad Plataformas confirmada (4 devs + QA) a partir de 20/07/2026 |
| 22 | Protótipo/wireframes de UX aprovados pelo Solicitante | Condicional (há UI) | ☑ Concluído | Figma, aprovado por Marina Couto em 25/06/2026 |
| 23 | Avaliação de risco de IA concluída (dados, viés, conformidade) | Condicional (há IA) | ☑ N/A | BRD: "Uso de IA: não usa" |
| 24 | Diagramas versionados no repositório Git ou no Repositório de Documentação | Recomendado | ☑ Concluído | Confluence, espaço SAP-Peças |
| 25 | Estratégia de observabilidade detalhada (dashboards, alertas) | Recomendado | ☐ Pendente *(prazo: Sprint 1)* | LLD §7 define instrumentação; dashboard será montado na Sprint 1 |
| 26 | Estimativa de prazo alinhada com o solicitante | Recomendado | ☑ Concluído | SAD §10; marcos validados com Marina Couto |

## Decisão

- ☑ **GO**: todos os itens obrigatórios (e condicionais aplicáveis) concluídos; desenvolvimento autorizado.
- ☐ **NO-GO**: itens obrigatórios pendentes (listar abaixo); desenvolvimento bloqueado.

**Itens pendentes / condicionantes:** item 25 (recomendado) com prazo na Sprint 1; não bloqueia.

| Aprovador | Nome | Decisão | Data |
| :---- | :---- | :---- | :---- |
| Solicitante | Marina Couto | GO | 15/07/2026 |
| Tech Lead Aprovador | Carlos Menezes | GO | 15/07/2026 |
| Segurança / DPO | Fernanda Ribeiro | GO | 15/07/2026 |
