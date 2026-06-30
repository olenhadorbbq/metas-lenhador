# Espelho Vivo — O Banco como Fonte de Verdade

> O **PostgreSQL vivo** (o banco real em execução) é a **única fonte de verdade** do schema do LenhadorOS. Prisma, ERDs e documentação são projeções derivadas do banco — nunca o oposto.

## Princípio central

> **Nunca confie em diagramas manuais.** O diagrama pode estar errado; o banco não. Toda dúvida sobre estrutura se resolve consultando o PostgreSQL real (em modo leitura).

O "Espelho Vivo" é o conjunto de artefatos que refletem fielmente o estado do banco em um dado momento, gerados a partir dele e versionados no Git para histórico e auditoria.

## Artefatos do Espelho Vivo

| Artefato | Descrição |
|---|---|
| `schema_snapshot.json` | Snapshot completo e estruturado do schema real (tabelas, colunas, tipos, índices, constraints, FKs) extraído do banco vivo. |
| **ERD por domínio** | Diagramas entidade-relacionamento gerados a partir do snapshot, organizados por domínio (não um único diagrama monolítico). |
| `AUDIT.md` | Relatório de auditoria do banco: achados, inconsistências, tabelas órfãs, colunas sem uso, riscos. |
| `score.json` | Pontuação de saúde do schema (cobertura de FKs, nomenclatura, índices, normalização, etc.). |
| **Catálogo de tabelas** | Lista canônica de todas as tabelas com propósito, domínio e status. |
| **Dicionário de dados** | Significado de cada tabela/coluna, regras de negócio e domínios de valores. |
| **Drift Prisma × banco** | Relatório das divergências entre o `schema.prisma` e o banco real. |

## Regras inegociáveis

1. **O banco real é a verdade.** Prisma, ERDs e docs são projeções. Em caso de conflito, o banco vence.
2. **Não confiar em diagrama manual.** Diagramas só são confiáveis se gerados a partir do `schema_snapshot.json`.
3. **Nada de `prisma migrate dev` sem aprovação explícita.** Pode alterar o banco de forma irreversível.
4. **Nada de `prisma migrate diff` sem aprovação explícita.** Mesmo sendo leitura, faz parte do protocolo de controle de mudanças e pode ser usado indevidamente como base para migrations não autorizadas.
5. **Snapshots são versionados.** Cada `schema_snapshot.json` é commitado para permitir comparação histórica e detecção de drift.
6. **Auditar antes de alterar.** Qualquer mudança de schema começa por gerar/atualizar o snapshot e o `AUDIT.md`.
7. **Acesso de leitura por padrão.** A geração de artefatos usa apenas comandos seguros de leitura sobre o banco.

## Fluxo de sincronização

```
[ PostgreSQL vivo ] ──(introspecção read-only)──► schema_snapshot.json
        │                                              │
        │                                              ├─► ERD por domínio
        │                                              ├─► catálogo de tabelas
        │                                              ├─► dicionário de dados
        │                                              └─► score.json
        │
        └──(comparação)──► drift Prisma × banco ──► AUDIT.md
```

## Relação com o LAM

A etapa 5 do LAM (**Validação no Espelho Vivo**) usa estes artefatos para confrontar qualquer schema proposto com a realidade do banco antes de qualquer implementação. Ver `../02-lam/LAM.md`.

## Status atual

> Ainda **não há** banco PostgreSQL neste repositório (ver auditoria em `../audits/`). Este documento define o modelo-alvo. Os artefatos (`schema_snapshot.json`, `score.json`, etc.) serão gerados quando o banco existir e for conectado.
