# Template: Architecture Decision Record (ADR)

> **Usado nas Etapas 2 a 4.** Um ADR por decisão arquitetural relevante, registrado **no momento em que a decisão é tomada**.
> O ADR é **curto e imutável**: decisões não são deletadas, apenas marcadas como *Supercedida por ADR-NNN*.
> Convenção de arquivo: `ADR-001-titulo-curto.md`, versionado no **repositório Git** (pasta `docs/adr/`).

---

# ADR-NNN: `<descrição curta da decisão>`

| Campo | Valor |
| :---- | :---- |
| **Status** | Proposta / Aceita / Deprecada / Supercedida por ADR-NNN |
| **Data** | `<dd/mm/aaaa>` |
| **Autor** | `<...>` |
| **Decisores** | `<...>` |

## Contexto

> *Qual problema ou situação motivou esta decisão? Quais forças/restrições estão em jogo?*

`<...>`

## Decisão

> *O que foi decidido, de forma clara e direta.*

`<...>`

## Alternativas Consideradas

| Alternativa | Prós | Contras | Por que não foi escolhida |
| :---- | :---- | :---- | :---- |
| `<...>` | `<...>` | `<...>` | `<...>` |

## Consequências

**Positivas:**
- `<...>`

**Negativas / Trade-offs:**
- `<...>`

---

### Exemplo preenchido

> **ADR-001: Uso de PostgreSQL como banco principal**
> **Status:** Aceita · **Data:** 11/06/2026
> **Contexto:** Precisamos de um banco relacional com suporte a transações ACID e consultas complexas; o time já tem experiência operacional.
> **Decisão:** Adotar PostgreSQL 16 gerenciado (AWS RDS) como banco principal.
> **Alternativas:** MySQL (menos recursos de JSON/CTE), DynamoDB (não relacional, modelagem inadequada ao domínio).
> **Consequências:** (+) Recursos avançados de SQL e JSONB; (+) ecossistema maduro. (−) Custo de RDS superior a instância única; (−) necessidade de tuning de conexões.
