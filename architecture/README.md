# LenhadorOS — Architecture & Engineering

> **Fonte oficial de engenharia do LenhadorOS.**
> Este diretório é a referência canônica para arquitetura, banco de dados, padrões, decisões e documentação viva do sistema. Tudo aqui é versionado no Git e evolui junto com o produto.

---

## Propósito

Centralizar, padronizar e governar a engenharia do LenhadorOS. Nenhuma decisão arquitetural, modelo de dados ou padrão de código é considerado oficial se não estiver documentado aqui. O objetivo é eliminar conhecimento implícito, reduzir retrabalho e garantir que cada módulo nasça auditado, modelado, testado e documentado.

## Como usar

- **Leia antes de codar.** Todo novo módulo começa pela leitura do Manifesto, do LAM e dos Standards.
- **Documente enquanto constrói.** A documentação viva é parte da entrega, não um passo posterior.
- **Decisões viram ADR.** Qualquer escolha arquitetural relevante é registrada em `11-adr/`.
- **O banco manda.** O Espelho Vivo (PostgreSQL real) é a única fonte de verdade — ver `03-database/ESPELHO_VIVO.md`.

## Ordem de leitura recomendada

1. `00-manifesto/MANIFESTO.md` — princípios e visão.
2. `MASTER_PLAN.md` — o plano completo por fases.
3. `02-lam/LAM.md` — a metodologia oficial de desenvolvimento.
4. `03-database/ESPELHO_VIVO.md` — o modelo de fonte de verdade.
5. `11-adr/` — decisões arquiteturais.
6. `standards/` e pastas `04`–`10` — padrões por área.
7. `13-roadmap/ROADMAP.md` e `14-glossary/GLOSSARY.md` — direção e vocabulário.

## Estrutura do diretório

| Pasta | Conteúdo |
|---|---|
| `00-manifesto/` | Princípios, visão e valores de engenharia |
| `01-architecture-book/` | Biblioteca oficial de arquitetura |
| `02-lam/` | LenhadorOS Architecture Method (metodologia) |
| `03-database/` | Espelho Vivo, snapshots, ERD, dicionário de dados |
| `04-backend/` | Padrões e arquitetura de backend |
| `05-frontend/` | Padrões e arquitetura de frontend |
| `06-apis/` | Contratos e padrões de API |
| `07-ai-engineering/` | Engenharia de IA, prompts de produto, agentes |
| `08-security/` | Segurança, autenticação, autorização, LGPD |
| `09-infrastructure/` | Infraestrutura, ambientes, redes |
| `10-devops/` | CI/CD, pipelines, automação operacional |
| `11-adr/` | Architecture Decision Records |
| `12-wiki-viva/` | Documentação viva gerada e curada |
| `13-roadmap/` | Roadmap e planejamento |
| `14-glossary/` | Glossário e dicionário de termos |
| `audits/` | Auditorias do repositório e de módulos |
| `prompts/` | Prompts oficiais de engenharia e IA |
| `checklists/` | Checklists de qualidade por etapa |
| `standards/` | Padrões transversais de engenharia |
| `diagrams/` | Diagramas (fonte versionável) |

## Papéis da equipe

| Papel | Responsabilidade |
|---|---|
| **Everton** | Dono do produto e da visão. Decide prioridades, valida requisitos de negócio e aprova decisões estruturais (incluindo migrations e mudanças no Espelho Vivo). |
| **ChatGPT** | Co-arquiteto e redator. Apoia no planejamento, na escrita de documentos, na modelagem conceitual e na geração de prompts. |
| **Claude Code** | Executor técnico. Audita, implementa, escreve código, testes e documentação viva diretamente no repositório, sempre seguindo o LAM. |
| **Vigia** | Observabilidade e auditoria contínua. Monitora drift, score do banco, qualidade e conformidade ao longo do tempo. |

### Relação entre os papéis

```
Everton (visão/decisão)
   │  define o quê e por quê
   ▼
ChatGPT (arquitetura/redação) ──► gera documentos, prompts e modelos
   │
   ▼
Claude Code (execução) ──► audita, implementa, testa e documenta no repo
   │
   ▼
Vigia (observabilidade) ──► verifica drift, score e conformidade contínua
   │
   └──► feedback volta para Everton/ChatGPT, fechando o ciclo
```

## Regra de ouro

> **Todo novo módulo deve obrigatoriamente passar por: auditoria → arquitetura → banco → implementação → teste → deploy → documentação viva.**

Nenhuma etapa é opcional. Um módulo que pula uma etapa não é considerado entregue. Essa sequência é detalhada no LAM (`02-lam/LAM.md`).
