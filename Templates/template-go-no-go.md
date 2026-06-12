# Template: Checklist de Go / No-Go

> **Etapa 6 do Project Readiness Framework (PRF).** Produtor: PM / Tech Lead. Aprovação: **Solicitante + Tech Lead Aprovador + Segurança/DPO**.
> Todos os itens **obrigatórios**, e os **condicionais aplicáveis**, devem estar "Concluído" para autorizar o início do desenvolvimento. Itens recomendados não bloqueiam, mas precisam de prazo.
> **Condicional** = obrigatório quando a dimensão correspondente do BRD se aplica (UI, IA); caso contrário, marque N/A.
> Status: Concluído / Pendente / N/A. Sempre registre a **evidência** (link do artefato).

## Identificação

| Campo | Valor |
| :---- | :---- |
| **Projeto** | `<...>` |
| **Criticidade** | Baixa / Média / Alta / Crítica |
| **PM** | `<...>` |
| **Tech Lead Aprovador** | `<...>` |
| **Segurança / DPO** | `<...>` |
| **Data da avaliação** | `<dd/mm/aaaa>` |

## Checklist

| # | Item | Obrigatoriedade | Status | Evidência (link) |
| :---- | :---- | :---- | :---- | :---- |
| 1 | BRD escrito, revisado e aprovado pelo solicitante | Obrigatório | ☐ | `<...>` |
| 2 | Justificativa de negócio (mín. 1 parágrafo) aprovada | Obrigatório | ☐ | `<...>` |
| 3 | Processo as-is e to-be mapeados | Obrigatório | ☐ | `<...>` |
| 4 | Custo estimado aprovado e reestimado após HLD e LLD (±25%) | Obrigatório | ☐ | `<...>` |
| 5 | Dimensões obrigatórias do BRD definidas (criticidade, usuários, multi-idioma, autoscaling, classificação de dados, compliance, UX, uso de IA) | Obrigatório | ☐ | `<...>` |
| 6 | Avaliação de impacto LGPD concluída e aprovada por Segurança/DPO | Obrigatório | ☐ | `<...>` |
| 7 | HLD elaborado com C4 L1 e L2 | Obrigatório | ☐ | `<...>` |
| 8 | Estratégias de deploy, observabilidade e DR (RTO/RPO) definidas | Obrigatório | ☐ | `<...>` |
| 9 | HLD aprovado pelo Tech Lead Aprovador (produtor ≠ aprovador) | Obrigatório | ☐ | `<...>` |
| 10 | LLD/TDD com modelo de dados e contratos de API | Obrigatório | ☐ | `<...>` |
| 11 | Matriz de rastreabilidade (RF → design → teste) preenchida | Obrigatório | ☐ | `<...>` |
| 12 | SAD consolidado e versionado no Repositório de Documentação | Obrigatório | ☐ | `<...>` |
| 13 | ADRs registrados para decisões relevantes | Obrigatório | ☐ | `<...>` |
| 14 | Registro de Riscos consolidado com mitigações | Obrigatório | ☐ | `<...>` |
| 15 | Repositório criado com estrutura e gitflow definidos | Obrigatório | ☐ | `<...>` |
| 16 | Pipeline CI/CD (build + secrets scan + SAST/SCA) configurado | Obrigatório | ☐ | `<...>` |
| 17 | Gestão de segredos definida (sem segredos no código) | Obrigatório | ☐ | `<...>` |
| 18 | Ambientes (dev/staging/prod) provisionados ou planejados | Obrigatório | ☐ | `<...>` |
| 19 | Estratégia de testes definida (unitário, integração, E2E) | Obrigatório | ☐ | `<...>` |
| 20 | Gate de segurança aprovado por Segurança/DPO | Obrigatório | ☐ | `<...>` |
| 21 | Equipe de desenvolvimento alocada, com papéis e capacidade confirmados | Obrigatório | ☐ | `<...>` |
| 22 | Protótipo/wireframes de UX aprovados pelo Solicitante | Condicional (há UI) | ☐ | `<...>` |
| 23 | Avaliação de risco de IA concluída (dados, viés, conformidade) | Condicional (há IA) | ☐ | `<...>` |
| 24 | Diagramas versionados no repositório Git ou no Repositório de Documentação | Recomendado | ☐ | `<...>` |
| 25 | Estratégia de observabilidade detalhada (dashboards, alertas) | Recomendado | ☐ | `<...>` |
| 26 | Estimativa de prazo alinhada com o solicitante | Recomendado | ☐ | `<...>` |

## Decisão

> Marque a decisão e colete as três assinaturas.

- ☐ **GO**: todos os itens obrigatórios (e condicionais aplicáveis) concluídos; desenvolvimento autorizado.
- ☐ **NO-GO**: itens obrigatórios pendentes (listar abaixo); desenvolvimento bloqueado.

**Itens pendentes / condicionantes:** `<...>`

| Aprovador | Nome | Decisão | Data |
| :---- | :---- | :---- | :---- |
| Solicitante | `<...>` | GO / NO-GO | `<...>` |
| Tech Lead Aprovador | `<...>` | GO / NO-GO | `<...>` |
| Segurança / DPO | `<...>` | GO / NO-GO | `<...>` |
