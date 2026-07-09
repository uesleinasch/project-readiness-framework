# Template: High-Level Design (HLD)

> **Etapa 2 do Day Zero Playbook.** Produtor: Arquiteto / Tech Lead. Aprovação: Tech Lead Aprovador (produtor ≠ aprovador).
> Obrigatórios os diagramas C4 **L1 (Context)** e **L2 (Containers)**.
> DoD: C4 L1/L2 presentes, stack justificada, análise build vs buy registrada, integrações mapeadas, estratégias de segurança/deploy/observabilidade/DR definidas em nível macro, ADRs registrados, riscos técnicos atualizados, reestimativa registrada.

---

## Identificação

| Campo | Valor |
| :---- | :---- |
| **Projeto** | `<...>` |
| **Arquiteto / Tech Lead** | `<...>` |
| **Aprovador (Tech Lead Aprovador)** | `<...>` |
| **Versão / Data** | `1.0` / `<dd/mm/aaaa>` |
| **BRD de origem** | `<link>` |

## 1. Visão Geral da Arquitetura

> *Estilo arquitetural (microsserviços, monolito modular, serverless…) com justificativa.*

`<...>`

## 2. Diagrama de Contexto (C4 L1)

> *O sistema como caixa preta, usuários e sistemas externos.*

`<imagem/link (Draw.io / Miro / Structurizr)>`

## 3. Diagrama de Contêineres (C4 L2)

> *Apps, APIs, bancos de dados, filas.*

`<imagem/link>`

## 4. Stack Tecnológica

| Camada | Tecnologia | Justificativa |
| :---- | :---- | :---- |
| Frontend | `<...>` | `<...>` |
| Backend | `<...>` | `<...>` |
| Banco de dados | `<...>` | `<...>` |
| Nuvem / Serviços | `<...>` | `<...>` |

## 5. Análise Build vs Buy vs Open Source

> *Para cada componente principal: construir, comprar (SaaS/COTS) ou adotar open source. Critérios: TCO, prazo, aderência ao requisito, lock-in, maturidade. Registre a decisão em ADR.*

| Componente | Opções avaliadas | Decisão | Critério decisivo | ADR |
| :---- | :---- | :---- | :---- | :---- |
| `<ex: autenticação>` | `<build / Keycloak / Auth0>` | `<...>` | `<...>` | ADR-0NN |

## 6. Topologia de Infraestrutura

> *VPC, subnets, load balancers, regiões AWS, estratégia de rede.*

`<...>`

## 7. Fluxo de Dados Principal

`<descrição + diagrama>`

## 8. Integrações e APIs Externas

| Integração | Protocolo | Contrato esperado | Dependência / Homologação |
| :---- | :---- | :---- | :---- |
| `<...>` | `<REST/gRPC/fila>` | `<...>` | `<...>` |

## 9. Estratégia de Segurança

> *Autenticação, autorização, criptografia, pontos de entrada. Inclui threat modeling inicial (STRIDE); ver baseline de segurança.*

`<...>`

## 10. Estratégia de Escalabilidade

`<horizontal/vertical, auto-scaling, gatilhos>`

## 11. Estratégia de Deploy (preliminar)

> *Blue/green, canary ou rolling; a decisão impacta a infra.*

`<...>`

## 12. Estratégia de Observabilidade (preliminar)

> *O que será monitorado; SLIs/SLOs candidatos.*

| SLI candidato | SLO alvo |
| :---- | :---- |
| `<ex: latência P95>` | `<ex: < 500ms>` |

## 13. Requisitos de DR

| Parâmetro | Valor |
| :---- | :---- |
| RTO | `<...>` |
| RPO | `<...>` |
| Estratégia macro | `<backup, redundância, multi-AZ…>` |

## 14. Riscos Técnicos

| ID | Descrição | Prob. | Impacto | Mitigação |
| :---- | :---- | :---- | :---- | :---- |
| RISK-0NN | `<...>` | B/M/A | B/M/A | `<...>` |

## 15. Pontos de Decisão Técnica (ADR)

> *Registre cada decisão relevante como ADR no momento em que é tomada; ver template-adr.*

| ADR | Decisão | Status |
| :---- | :---- | :---- |
| ADR-001 | `<...>` | Aceita |

## 16. Reestimativa (pós-HLD)

| Item | Estimativa atualizada | Variação vs. BRD |
| :---- | :---- | :---- |
| Construção (infra + desenvolvimento) | `<...>` | `<±%>` |
| Custo recorrente anual (TCO de operação) | `<...>` | `<±%>` |
