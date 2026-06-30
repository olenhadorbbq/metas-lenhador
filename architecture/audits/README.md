# Auditorias

> Relatórios de auditoria do LenhadorOS: repositório, banco (Espelho Vivo) e módulos. Auditar é a etapa 2 do LAM e antecede qualquer alteração estrutural.

## Propósito

Registrar, de forma versionada e datada, o estado real do sistema em cada momento de auditoria: estrutura, stack, módulos, riscos e recomendações. Auditorias são a base factual sobre a qual decisões de arquitetura são tomadas.

## Tipos de auditoria

| Tipo | Escopo |
|---|---|
| **Repositório** | Estrutura de pastas, stack, módulos, riscos de código, documentação. |
| **Banco (Espelho Vivo)** | Schema real, drift Prisma × banco, score, tabelas órfãs (ver `../03-database/ESPELHO_VIVO.md`). |
| **Módulo** | Auditoria focada em um módulo antes de evoluí-lo. |
| **Segurança** | Autenticação, autorização, segredos, exposição de endpoints. |

## Convenções

- **Nomenclatura:** `AAAA-MM-DD-escopo.md` ou nome canônico (ex.: `REPO_AUDIT.md`).
- **Somente leitura.** Auditorias usam apenas comandos seguros de leitura; nada de migrations ou comandos destrutivos.
- **Classificação de risco:** P0 (crítico), P1 (alto), P2 (médio), P3 (baixo).
- **Datadas e versionadas.** Cada auditoria é commitada para permitir comparação histórica.

## Índice

| Relatório | Escopo | Data |
|---|---|---|
| `REPO_AUDIT.md` (raiz do repositório) | Auditoria inicial do repositório | 2026-06-30 |

> A primeira auditoria do repositório (`REPO_AUDIT.md`) foi gerada na raiz do projeto. Proposta de organização futura: mover/replicar auditorias para esta pasta como `audits/AAAA-MM-DD-repo.md`.
