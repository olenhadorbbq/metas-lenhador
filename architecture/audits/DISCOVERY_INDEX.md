# DISCOVERY INDEX — LenhadorOS

> Índice de navegação das descobertas da Fase 0 (Discovery & Assessment). Ponto de entrada rápido para os achados; o detalhamento vive em `DISCOVERY_MASTER.md`.
>
> **Data:** 2026-06-30 · **Branch:** `claude/lenhador-os-audit-yb4h9y` · **Modo:** somente leitura.

## Declaração de escopo (resumo)

> **Este repositório NÃO é o LenhadorOS completo.** Contém um protótipo de gestão de metas (`index.html`) + a fundação documental `/architecture`. Nenhum módulo foi inventado; o que segue reflete apenas o que existe.

## Documentos de discovery

| Documento | Conteúdo |
|---|---|
| [`DISCOVERY_MASTER.md`](./DISCOVERY_MASTER.md) | Auditoria completa das 20 áreas, matriz de maturidade, mapa real e inventário executivo. |
| [`REPO_AUDIT.md`](../../REPO_AUDIT.md) | Auditoria inicial do repositório (estrutura, stack, riscos P0–P3). |
| [`README.md`](./README.md) | Índice geral da pasta de auditorias. |

## Status por área (visão rápida)

Legenda: 🟢 Confirmado · 🟡 Precisa validar · 🔵 Hipótese · 🔴 Problema · ⚫ Não encontrado

| # | Área | Status | Nota |
|---|---|:---:|---:|
| 1 | Repositório | 🟢 | 25 |
| 2 | Banco de dados | ⚫ | 5 |
| 3 | Backend | ⚫ / 🔵 | 5 |
| 4 | Frontend | 🟢 / 🔴 | 20 |
| 5 | APIs | 🟡 | 10 |
| 6 | Segurança | 🔴 | 5 |
| 7 | Permissões | 🔴 | — |
| 8 | Autenticação | ⚫ | — |
| 9 | Infraestrutura | ⚫ | 0 |
| 10 | Deploy | ⚫ | — |
| 11 | Integrações | 🟢 / ⚫ | 15 |
| 12 | IA | ⚫ | 0 |
| 13 | N8N | ⚫ | — |
| 14 | WhatsApp | ⚫ | — |
| 15 | Observabilidade | ⚫ | 0 |
| 16 | Documentação | 🟢 / ⚫ | 45 |
| 17 | Testes | ⚫ | 0 |
| 18 | Performance | 🟡 | — |
| 19 | Módulos funcionais | 🟢 / ⚫ | — |
| 20 | Dívida técnica | 🔴 | — |

> **Média de maturidade geral: ~11/100** — estágio inicial (protótipo + fundação de engenharia).

## Achados de maior severidade

| Severidade | Achado | Onde |
|---|---|---|
| 🔴 P0 | Escopo: este repo não é o LenhadorOS completo | repo inteiro |
| 🔴 P0 | URL do backend quebrada (`https://https://`) — app provavelmente não funciona | `index.html:75` |
| 🔴 P0 | `?admin=true` concede admin sem autenticação | `index.html:78,109` |
| 🔴 P0 | Endpoint de escrita (Apps Script) exposto no HTML público | `index.html:75` |
| 🔴 P1 | XSS via `innerHTML` sem escape | `index.html:143-154,131` |

## Decisões pendentes (bloqueantes)

1. **ADR-0002** — Onde nasce o LenhadorOS real: este repo ou repo próprio?
2. **ADR-0003** — Stack-alvo (backend, frontend, ORM, banco).
3. Destino do protótipo de Metas: corrigir, migrar ou descartar.

## Próximo passo recomendado

- **PROMPT 03** — Definir e registrar **ADR-0002 (escopo/repositório)** e **ADR-0003 (stack-alvo)**, desbloqueando a Fase 1 (Architecture Book) e a Fase 3 (Espelho Vivo).
