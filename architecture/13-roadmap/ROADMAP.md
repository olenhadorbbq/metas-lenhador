# Roadmap — LenhadorOS

> Direção de evolução do LenhadorOS, alinhada às fases do `../MASTER_PLAN.md`. Documento vivo: revisado periodicamente e ajustado conforme prioridades de produto.

## Como ler

- **Agora:** em execução ou próximo item.
- **Próximo:** planejado para o curto prazo.
- **Depois:** desejável, sem data definida.

## Estado atual (2026-06-30)

O repositório contém hoje apenas um protótipo estático de gestão de metas (ver `../audits/REPO_AUDIT.md`). A estrutura de engenharia (`/architecture`) acaba de ser fundada. O foco imediato é consolidar a fundação e decidir a stack-alvo do LenhadorOS "real".

## Agora — Fase 0 (Fundação)

- [x] Criar estrutura `/architecture`.
- [x] Manifesto, MASTER_PLAN, LAM, Espelho Vivo, ADR-0001.
- [ ] Definir stack-alvo do backend/frontend (decisão via ADR).
- [ ] Esclarecer escopo: este repo evolui para o LenhadorOS ou ele nasce em repo próprio.

## Próximo — Fases 1 a 3

- [ ] **Fase 1 — Architecture Book:** consolidar padrões macro de arquitetura.
- [ ] **Fase 2 — LAM:** detalhar critérios de entrada/saída e checklists por etapa.
- [ ] **Fase 3 — Espelho Vivo:** conectar PostgreSQL, gerar `schema_snapshot.json`, ERD por domínio, `score.json` e `AUDIT.md`.

## Depois — Fases 4 a 8

- [ ] **Fase 4 — Engineering Standards:** padrões de backend, frontend, API, segurança, DevOps e banco.
- [ ] **Fase 5 — Documentação Viva:** wiki, dicionário de dados, geração automática.
- [ ] **Fase 6 — Observabilidade:** Vigia, dashboards, alertas e drift contínuo.
- [ ] **Fase 7 — Automação:** prompts oficiais, pipelines e checklists integrados à IA.
- [ ] **Fase 8 — Evolução Contínua:** governança, revisões e melhoria permanente.

## Revisão

Este roadmap é revisado a cada ciclo de planejamento. Mudanças de direção significativas devem ser registradas como ADR em `../11-adr/`.
