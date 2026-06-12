# Template: Business Requirements Document (BRD)

> **Etapa 1 do Project Readiness Framework (PRF).** Produtor: Solicitante (+ PM). Aprovação: PM (+ Solicitante para negócio).
> Preencha os campos `<...>`. Remova as instruções em *itálico* antes de finalizar.
> Critério de conclusão (DoD): todas as seções preenchidas, RFs/RNFs codificados e mensuráveis, criticidade e classificação de dados definidas, justificativa de negócio assinada.

---

## Identificação

| Campo | Valor |
| :---- | :---- |
| **Projeto** | `<nome do projeto>` |
| **Solicitante (Business Owner)** | `<nome / área>` |
| **PM** | `<nome>` |
| **Data** | `<dd/mm/aaaa>` |
| **Versão** | `1.0` |
| **Status** | Rascunho / Em revisão / Aprovado |
| **Repositório de Documentação** | `<ferramenta acordada entre as partes, ex: Confluence, SharePoint, wiki do Git>` |

## 1. Contexto e Problema

> *Situação atual (as-is), a dor de negócio e por que o projeto é necessário agora.*

`<...>`

## 2. Objetivo de Negócio

> *O que se espera alcançar. Deve ser mensurável.*

`<ex: reduzir o tempo do processo de X de 4h para 30min (-87%) até o 2º trimestre.>`

## 3. Escopo

**Dentro do escopo:**
- `<...>`

**Fora do escopo (exclusões explícitas):**
- `<...>`

**Sistemas impactados / integrações:**
- `<...>`

## 4. Stakeholders

| Papel | Nome | Responsabilidade |
| :---- | :---- | :---- |
| Solicitante | `<...>` | Aprova negócio e custo |
| Usuários finais | `<...>` | - |
| Responsável técnico | `<...>` | - |
| Aprovadores | `<...>` | - |

## 5. Requisitos Funcionais

| ID | Descrição | Prioridade (Must/Should/Could) |
| :---- | :---- | :---- |
| RF-001 | `<...>` | `<...>` |
| RF-002 | `<...>` | `<...>` |

## 6. Requisitos Não-Funcionais

> *Cada RNF deve ser mensurável.*

| ID | Categoria | Métrica/Alvo |
| :---- | :---- | :---- |
| RNF-001 | Performance | `<ex: P95 < 500ms>` |
| RNF-002 | Disponibilidade | `<ex: 99,9%>` |
| RNF-003 | Segurança | `<...>` |
| RNF-004 | Acessibilidade / i18n | `<ex: WCAG 2.1 AA>` |

## 7. Critérios de Aceite (de negócio)

| ID | Critério |
| :---- | :---- |
| CA-001 | `<...>` |

## 8. Processo As-Is / To-Be

**As-Is (atual):** `<descrição ou link do diagrama>`
**To-Be (proposto):** `<descrição ou link do diagrama>`

## 9. Restrições e Premissas

**Restrições:** `<tecnologia, prazo, orçamento>`
**Premissas:** `<...>`

## 10. Riscos de Negócio (inicial)

> *Primeira versão do Registro de Riscos; ver template-registro-riscos.*

| ID | Descrição | Probabilidade | Impacto | Mitigação |
| :---- | :---- | :---- | :---- | :---- |
| RISK-001 | `<...>` | B/M/A | B/M/A | `<...>` |

## 11. Custo Estimado (ordem de grandeza)

| Item | Estimativa |
| :---- | :---- |
| Construção: desenvolvimento | `<...>` |
| Construção: infraestrutura de projeto | `<...>` |
| Custo recorrente anual (TCO de operação: infra, licenças, suporte) | `<...>` |

> *Será refinado após HLD e LLD (variação > ±25% exige ciência do Solicitante).*

## 12. Justificativa de Negócio

> *Parágrafo obrigatório com ROI ou motivação estratégica, assinado pelo solicitante.*

`<...>`

_Assinatura do Solicitante: __________________________ Data: ____/____/_____

---

## Definições Obrigatórias

| Dimensão | Definição |
| :---- | :---- |
| **Criticidade** | Baixa / Média / Alta / Crítica → `<...>` |
| **Nº de Usuários** | Inicial: `<...>` / Pico: `<...>` |
| **Multi-idioma** | Sim / Não → idiomas: `<PT, ES, EN…>` |
| **Autoscaling** | Necessário / Não necessário |
| **Dados Pessoais (LGPD)** | Coleta / Não coleta |
| **Classificação de Dados** | Público / Interno / Confidencial / Restrito |
| **Requisitos Regulatórios / Compliance** | `<LGPD, PCI-DSS, SOC 2, setoriais…>` |
| **Integrações Externas** | `<lista de sistemas/APIs>` |
| **Disponibilidade / DR** | RTO: `<...>` / RPO: `<...>` |
| **Interface de Usuário (UX)** | Possui UI / Não possui → *se possui, protótipo/wireframes aprovados antes do LLD* |
| **Uso de IA** | Usa IA/LLM / Não usa → *se usa, avaliação de risco de IA antes do Go/No-Go* |
