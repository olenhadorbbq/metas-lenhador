# ADR — Architecture Decision Records

> Registro das decisões arquiteturais do LenhadorOS. Cada decisão relevante vira um ADR versionado, imutável após aceito (mudanças geram um novo ADR que supersede o anterior).

## O que é um ADR

Um **Architecture Decision Record** documenta uma decisão arquitetural importante, o contexto que a motivou, as alternativas avaliadas e suas consequências. ADRs tornam o raciocínio por trás do sistema rastreável e evitam re-litigar decisões já tomadas.

## Padrão obrigatório

Todo ADR contém as seções:

| Seção | Conteúdo |
|---|---|
| **Contexto** | O problema ou força que motiva a decisão. Situação atual e restrições. |
| **Decisão** | A escolha feita, descrita de forma direta e afirmativa. |
| **Alternativas consideradas** | Outras opções avaliadas e por que foram descartadas. |
| **Consequências** | Efeitos positivos e negativos, trade-offs e impactos futuros. |
| **Status** | `Proposto`, `Aceito`, `Rejeitado`, `Substituído` ou `Obsoleto`. |

## Convenções

- **Nomenclatura:** `ADR-NNNN-titulo-curto-em-kebab-case.md` (ex.: `ADR-0001-postgresql-como-fonte-da-verdade.md`).
- **Numeração:** sequencial e crescente, nunca reutilizada.
- **Imutabilidade:** um ADR aceito não é editado em sua decisão; para mudar, crie um novo ADR que o substitua e atualize o status do antigo para `Substituído`.
- **Status no topo:** o status atual fica visível logo no início do arquivo.

## Índice de ADRs

| ID | Título | Status |
|---|---|---|
| [ADR-0001](./ADR-0001-postgresql-como-fonte-da-verdade.md) | PostgreSQL como fonte da verdade | Aceito |

## Como propor um novo ADR

1. Copie o padrão acima em um novo arquivo `ADR-NNNN-....md`.
2. Preencha as cinco seções.
3. Abra com status `Proposto`.
4. Após discussão e aprovação, atualize para `Aceito` e registre no índice.
