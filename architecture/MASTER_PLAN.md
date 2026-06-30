# LenhadorOS Architecture & Engineering

> Plano-mestre da engenharia do LenhadorOS. Descreve as fases que levam de uma fundação documental até a evolução contínua governada. Cada fase tem entregáveis versionados neste diretório.

## Visão geral

O LenhadorOS é construído em fases incrementais. Cada fase entrega artefatos concretos e habilita a próxima. O fio condutor de todas elas é o **Espelho Vivo** (o banco PostgreSQL real como fonte de verdade) e o **LAM** (a metodologia oficial de desenvolvimento).

---

## Fase 0 — Fundação

Manifesto, princípios, visão e estrutura inicial.

- Estrutura `/architecture` criada e versionada.
- `MANIFESTO.md` com princípios de engenharia.
- Papéis definidos (Everton, ChatGPT, Claude Code, Vigia).
- **Entregáveis:** `00-manifesto/MANIFESTO.md`, `README.md`, este `MASTER_PLAN.md`.

## Fase 1 — Architecture Book

Biblioteca oficial de arquitetura.

- Documentação de referência de padrões arquiteturais do sistema.
- Decisões macro de organização (módulos, domínios, camadas).
- **Entregáveis:** `01-architecture-book/`.

## Fase 2 — LAM

LenhadorOS Architecture Method: metodologia oficial de desenvolvimento.

- Fluxo obrigatório de criação de módulos (ideia → observabilidade).
- Critérios de entrada e saída de cada etapa.
- **Entregáveis:** `02-lam/LAM.md`.

## Fase 3 — Espelho Vivo

Banco vivo, snapshot, ERD, auditoria, score e catálogo.

- PostgreSQL real como única fonte de verdade.
- `schema_snapshot.json`, ERD por domínio, `AUDIT.md`, `score.json`, catálogo e dicionário de dados.
- Detecção de drift Prisma × banco.
- **Entregáveis:** `03-database/ESPELHO_VIVO.md` e artefatos derivados.

## Fase 4 — Engineering Standards

Padrões de backend, frontend, API, segurança, DevOps e banco.

- Convenções de código, contratos de API, padrões de segurança e CI/CD.
- **Entregáveis:** `04-backend/`, `05-frontend/`, `06-apis/`, `08-security/`, `10-devops/`, `standards/`.

## Fase 5 — Documentação Viva

Wiki, dicionário de dados, ADR, glossário e geração automática.

- Documentação que se mantém sincronizada com o código e o banco.
- **Entregáveis:** `12-wiki-viva/`, `14-glossary/GLOSSARY.md`, `11-adr/`.

## Fase 6 — Observabilidade

Vigia, dashboards, alertas, drift e auditoria contínua.

- Monitoramento de saúde do schema, score e conformidade.
- **Entregáveis:** integração do Vigia, dashboards e alertas.

## Fase 7 — Automação

Prompts oficiais, pipelines, checklists e integração com IA.

- Prompts versionados, pipelines automatizados e checklists de qualidade.
- **Entregáveis:** `prompts/`, `checklists/`, pipelines em `10-devops/`.

## Fase 8 — Evolução Contínua

Roadmap, revisões, governança e melhoria permanente.

- Cadência de revisão, governança de decisões e melhoria contínua.
- **Entregáveis:** `13-roadmap/ROADMAP.md` e ritos de governança.

---

## Princípios transversais

- **O banco é a verdade.** Diagramas e Prisma são projeções derivadas.
- **Nada de migration sem aprovação explícita.** `prisma migrate dev/diff` exige autorização.
- **Documentação é entrega.** Código sem documentação viva não está pronto.
- **Tudo versionado.** Decisões, padrões e auditorias vivem no Git.
