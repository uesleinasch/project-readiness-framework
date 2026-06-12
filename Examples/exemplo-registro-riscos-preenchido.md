# Exemplo Preenchido: Registro de Riscos

> **Projeto fictício** para servir de referência de preenchimento do [`template-registro-riscos.md`](../Templates/template-registro-riscos.md). Mesmo projeto de referência dos demais exemplos (SAP-Peças).
> Repare na evolução do artefato vivo: RISK-001/002 nasceram no **BRD**, RISK-003/004 entraram no **HLD**, RISK-005/006 no **LLD**: e o status muda conforme as mitigações se confirmam.

| ID | Descrição | Categoria | Prob. | Impacto | Mitigação | Responsável | Status |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| RISK-001 | API de estoque do ERP não suportar o volume de consultas (limite ~60 req/s medido no spike) | Técnico | Média | Alto | Cache de catálogo e disponibilidade (ADR-002); consulta direta só na confirmação do pedido | Paula Andrade | Mitigado |
| RISK-002 | Baixa adoção pelos concessionários | Negócio | Média | Médio | Piloto com 20 concessionários antes do rollout; treinamento e material de apoio | Marina Couto | Aberto |
| RISK-003 | Janela de carga noturna do catálogo falhar e servir dados defasados | Técnico | Média | Médio | Alerta de staleness (> 26h); fallback de consulta direta com degradação anunciada | Tiago Mota | Aberto |
| RISK-004 | VPN site-to-site com o ERP indisponível | Dependência | Baixa | Alto | Fila outbox segura pedidos por até 24h; runbook de reprocessamento; alerta de `outbox_pendentes` | DevOps | Aberto |
| RISK-005 | Operador de um concessionário acessar pedidos de outro CNPJ (tampering de escopo, STRIDE) | Segurança | Baixa | Alto | Claim de CNPJ no token OIDC + filtro obrigatório na camada de dados; teste de integração dedicado (TC-010) | Tiago Mota | Mitigado |
| RISK-006 | Homologação do SSO com o time do IdP corporativo atrasar o piloto | Cronograma | Média | Médio | Solicitação de homologação aberta já no Bootstrap; ambiente de staging usa realm de testes do IdP | Rafael Lima | Aberto |

> **Dica aplicada:** nenhum risco Alta × Alta permanece sem mitigação. RISK-001 e RISK-005 (impacto Alto) foram mitigados **antes** do Go/No-Go, como exige o framework.
