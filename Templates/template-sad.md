# Template: Solution Architecture Document (SAD)

> **Etapa 4 do Day Zero Playbook.** Produtor: Arquiteto. Aprovação: Tech Lead Aprovador (produtor ≠ aprovador).
> O SAD é um **documento vivo**: mantido ao longo das Etapas 2-4 e consolidado aqui. **Não é retrabalho**: cada seção é montada a partir dos artefatos anteriores.
> DoD: 12 seções consolidadas e consistentes com HLD/LLD; ADRs e Registro de Riscos integrados; versionado no Repositório de Documentação (ferramenta acordada entre as partes, registrada no BRD); sem contradições entre seções.

---

## Identificação

| Campo | Valor |
| :---- | :---- |
| **Projeto** | `<...>` |
| **Arquiteto** | `<...>` |
| **Aprovador (Tech Lead Aprovador)** | `<...>` |
| **Versão / Data** | `1.0` / `<dd/mm/aaaa>` |

## 1. Visão Geral
> *Origem: BRD.* Sumário executivo, objetivo, escopo e contexto de negócio.

`<...>`

## 2. Requisitos e Restrições
> *Origem: BRD.* RFs/RNFs relevantes à arquitetura; restrições técnicas e de negócio.

`<...>`

## 3. Decisões Arquiteturais (ADR)
> *Origem: HLD/LLD.* Consolidação dos ADRs registrados.

| ADR | Decisão | Status |
| :---- | :---- | :---- |
| ADR-001 | `<...>` | `<...>` |

## 4. Visão Arquitetural
> *Origem: HLD/LLD.* C4 L1, L2 e L3; componentes, responsabilidades e interações.

`<diagramas + descrição>`

## 5. Infraestrutura e Deploy
> *Origem: HLD/Bootstrap.* Topologia AWS, pipeline CI/CD, estratégia de deploy (blue/green, canary, rolling).

`<...>`

## 6. Segurança
> *Origem: HLD/LLD.* Modelo de ameaças e controles aplicados (baseline de segurança / companheiro de Segurança).

`<...>`

## 7. Observabilidade
> *Origem: HLD/LLD.* Logs, métricas, traces, alertas, dashboards e SLIs/SLOs.

| SLI | SLO | Alerta |
| :---- | :---- | :---- |
| `<...>` | `<...>` | `<...>` |

## 8. Estratégia de Dados
> *Origem: BRD/LLD.* Modelo de dados, classificação, retenção, backup, compliance LGPD.

`<...>`

## 9. Riscos e Mitigações
> *Origem: todas as etapas.* Consolidação do Registro de Riscos.

| ID | Descrição | Prob. | Impacto | Mitigação | Responsável |
| :---- | :---- | :---- | :---- | :---- | :---- |
| RISK-001 | `<...>` | B/M/A | B/M/A | `<...>` | `<...>` |

## 10. Cronograma e Marcos
> *Origem: PM.*

| Marco | Data prevista |
| :---- | :---- |
| `<...>` | `<...>` |

## 11. Glossário

| Termo | Definição |
| :---- | :---- |
| `<...>` | `<...>` |

## 12. Histórico de Revisões

| Versão | Data | Autor | Mudanças |
| :---- | :---- | :---- | :---- |
| 1.0 | `<dd/mm/aaaa>` | `<...>` | Versão inicial |
