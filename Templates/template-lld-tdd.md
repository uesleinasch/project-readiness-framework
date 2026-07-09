# Template: Low-Level Design / Technical Design Document (LLD/TDD)

> **Etapa 3 do Day Zero Playbook.** Produtor: Dev Lead / Arquiteto. Aprovação: Tech Lead Aprovador (produtor ≠ aprovador).
> Obrigatório o diagrama C4 **L3 (Components)**.
> DoD: C4 L3, modelo de dados e contratos OpenAPI completos; observabilidade e rollback por componente; estratégia de testes; matriz de rastreabilidade preenchida; reestimativa de esforço registrada. Handoff: um desenvolvedor implementa só com este documento.

---

## Identificação

| Campo | Valor |
| :---- | :---- |
| **Projeto** | `<...>` |
| **Dev Lead / Arquiteto** | `<...>` |
| **Aprovador (Tech Lead Aprovador)** | `<...>` |
| **Versão / Data** | `1.0` / `<dd/mm/aaaa>` |
| **HLD de origem** | `<link>` |

## 1. Diagrama de Componentes (C4 L3)

> *Módulos internos de cada contêiner: responsabilidades e interfaces.*

`<imagem/link>`

## 2. Modelo de Dados

> *Diagrama ER, esquema, naming conventions, índices e constraints.*

`<diagrama ER + DDL/descrição>`

| Tabela/Entidade | Campos-chave | Índices | Constraints |
| :---- | :---- | :---- | :---- |
| `<...>` | `<...>` | `<...>` | `<...>` |

## 3. Definição de APIs e Contratos

> *Especificação OpenAPI/Swagger de todos os endpoints.*

`<link para o arquivo openapi.yaml no repositório>`

| Endpoint | Método | Auth | Payload | Respostas / Erros |
| :---- | :---- | :---- | :---- | :---- |
| `<...>` | `<GET/POST…>` | `<...>` | `<...>` | `<200 / 4xx / 5xx>` |

## 4. Fluxos de Processo Detalhados

> *Diagramas de sequência/UML dos principais casos de uso e fluxos alternativos.*

`<diagramas>`

## 5. Estratégia de Tratamento de Erros

> *Como erros são capturados, logados, retornados ao cliente e notificados.*

`<...>`

## 6. Estratégia de Cache

| O que | TTL | Invalidação | Ferramenta |
| :---- | :---- | :---- | :---- |
| `<...>` | `<...>` | `<...>` | `<Redis/CloudFront…>` |

## 7. Instrumentação de Observabilidade

> *Logs, métricas e traces por componente; mapeamento até os SLIs/SLOs do HLD.*

| Componente | Logs | Métricas | Traces | SLI/SLO associado |
| :---- | :---- | :---- | :---- | :---- |
| `<...>` | `<...>` | `<...>` | `<...>` | `<...>` |

## 8. Pontos de Rollback / Feature Flags

> *Como cada mudança pode ser revertida com segurança.*

`<...>`

## 9. Migrations e Versionamento de Schema

| Item | Definição |
| :---- | :---- |
| Ferramenta | `<Flyway/Liquibase/Alembic>` |
| Estratégia de rollback | `<...>` |

## 10. Estratégia de Testes

> *Inclua também: gestão de dados de teste (massa/anonimização) e ambientes de teste necessários.*

| Tipo | Cobertura mínima | Ferramenta | Responsável |
| :---- | :---- | :---- | :---- |
| Unitário | `<ex: 80%>` | `<...>` | `<...>` |
| Integração | `<...>` | `<...>` | `<...>` |
| E2E | `<...>` | `<...>` | `<...>` |

## 11. Dependências e Bibliotecas

| Biblioteca | Versão | Licença | Justificativa |
| :---- | :---- | :---- | :---- |
| `<...>` | `<...>` | `<...>` | `<...>` |

## 12. Considerações de Performance

> *Pontos críticos, benchmarks esperados e estratégias de otimização.*

`<...>`

## 13. Matriz de Rastreabilidade

> *Ver template-matriz-rastreabilidade. Deve estar completa para o Go/No-Go.*

| RF/RNF | Componente | Endpoint/API | Caso de Teste | Critério de Aceite |
| :---- | :---- | :---- | :---- | :---- |
| RF-001 | `<...>` | `<...>` | TC-001 | CA-001 |

## 14. Reestimativa (pós-LLD)

| Item | Esforço estimado | Variação acumulada vs. BRD |
| :---- | :---- | :---- |
| Desenvolvimento | `<...>` | `<±%>` |
