# Exemplo Preenchido: Architecture Decision Record (ADR)

> **Projeto fictício** para servir de referência de preenchimento do [`template-adr.md`](../Templates/template-adr.md). Mesmo projeto de referência dos demais exemplos (SAP-Peças).
> No projeto real, cada ADR é um arquivo próprio em `docs/adr/` no repositório Git (ex: `ADR-002-cache-redis-disponibilidade.md`).

---

# ADR-002: Cache Redis para catálogo e disponibilidade de estoque

| Campo | Valor |
| :---- | :---- |
| **Status** | Aceita |
| **Data** | 16/06/2026 |
| **Autor** | Paula Andrade (Arquiteta) |
| **Decisores** | Paula Andrade, Tiago Mota, Carlos Menezes (Tech Lead Aprovador) |

## Contexto

O spike da Etapa 0 demonstrou que a API de estoque do ERP sustenta no máximo **~60 req/s** (acima disso, CPU do ERP > 80% e P95 de 2,4s). A projeção de carga do portal com 480 concessionários é de até 200 req/s de consultas de disponibilidade em pico de safra. O RNF-001 exige busca com P95 < 800ms e a restrição do BRD proíbe alterações no ERP nesta fase. É preciso desacoplar o volume de leitura do portal da capacidade do ERP, sem abrir mão da precisão no momento da confirmação do pedido.

## Decisão

Adotar **Redis (AWS ElastiCache)** como camada de cache com duas políticas distintas:

- **Catálogo de peças:** TTL 24h, recarregado por job noturno a partir do ERP, com alerta de staleness acima de 26h.
- **Disponibilidade de estoque:** TTL 5 min, populado sob demanda (cache-aside).
- **Exceção:** na confirmação do pedido, a disponibilidade é validada por **consulta direta ao ERP** (bypass do cache), garantindo precisão onde importa.

## Alternativas Consideradas

| Alternativa | Prós | Contras | Por que não foi escolhida |
| :---- | :---- | :---- | :---- |
| Consulta direta ao ERP em todas as operações | Dado sempre preciso; sem infra extra | ERP não suporta a carga (evidência do spike); P95 estoura o RNF-001 | Inviabilizada pelo spike |
| Réplica de leitura do banco do ERP | Carga sai da API do ERP | Exige mudança no ERP (restrição do BRD); acoplamento ao schema interno | Viola restrição do projeto |
| Cache em memória da própria aplicação (sem Redis) | Sem custo de ElastiCache | Inconsistência entre as 2-8 tasks do ECS; perda total a cada deploy | Não funciona com autoscaling horizontal |

## Consequências

**Positivas:**
- Portal suporta 200 req/s de pico consumindo do ERP apenas o miss do cache (~5%) + confirmações.
- RNF-001 atendido com folga (P95 de busca ~110ms no miss, menor no hit).
- ERP protegido de sobrecarga (mitiga o RISK-001).

**Negativas / Trade-offs:**
- Disponibilidade exibida pode estar até 5 min defasada; aceito pelo negócio, pois a confirmação valida direto no ERP (registrado com o Solicitante).
- Custo recorrente do ElastiCache (~R$ 500/mês, dentro do TCO aprovado).
- Novo componente para operar e monitorar (staleness, hit ratio).
