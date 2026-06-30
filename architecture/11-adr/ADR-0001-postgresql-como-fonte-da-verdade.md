# ADR-0001 — PostgreSQL como fonte da verdade

- **Status:** Aceito
- **Data:** 2026-06-30
- **Decisores:** Everton (produto), ChatGPT (arquitetura), Claude Code (execução)

## Contexto

O LenhadorOS precisa de uma referência única e inequívoca para o schema de dados. Em projetos onde diagramas, arquivos ORM e documentação competem como "verdade", surgem divergências (drift): o diagrama diz uma coisa, o `schema.prisma` diz outra, e o banco em produção diz uma terceira. Isso gera bugs, retrabalho e decisões baseadas em informação incorreta.

É necessário definir, de forma explícita, qual artefato é canônico quando houver conflito.

## Decisão

O **banco PostgreSQL real** (o Espelho Vivo) é a **fonte canônica do schema** do LenhadorOS.

Prisma (`schema.prisma`), ERDs, diagramas e documentação são **projeções derivadas** do banco — nunca a origem da verdade. Em qualquer divergência, o estado real do PostgreSQL prevalece, e os artefatos derivados devem ser regenerados/corrigidos para refletir o banco.

## Alternativas consideradas

1. **Prisma `schema.prisma` como fonte da verdade.** Rejeitada: o arquivo Prisma pode divergir do banco aplicado (migrations parciais, alterações manuais no banco, ambientes dessincronizados), e tratá-lo como verdade mascararia o estado real de produção.
2. **Diagramas/ERD manuais como fonte da verdade.** Rejeitada: diagramas manuais envelhecem rápido e raramente refletem o banco real; são úteis para comunicação, não como referência canônica.
3. **Documentação textual como fonte da verdade.** Rejeitada: ainda mais sujeita a desatualização que o ORM ou diagramas.

## Consequências

**Positivas:**
- Uma única referência inequívoca elimina ambiguidade sobre "qual é o schema certo".
- Drift entre Prisma e banco torna-se um sinal detectável e auditável, não uma surpresa.
- ERDs e dicionário de dados passam a ser gerados a partir do snapshot real, ganhando confiabilidade.

**Negativas / trade-offs:**
- Exige tooling para introspecção do banco e geração de `schema_snapshot.json` e artefatos derivados.
- Requer disciplina: alterações no banco precisam ser refletidas nos artefatos versionados.
- Operações que tocam o banco (`prisma migrate dev/diff`) passam a exigir aprovação explícita, adicionando uma etapa de governança.

## Relacionados

- `../03-database/ESPELHO_VIVO.md` — define os artefatos e regras do Espelho Vivo.
- `../00-manifesto/MANIFESTO.md` — princípio 1 (o banco vivo é a fonte de verdade).
