# LenhadorOS Architecture & Engineering

> **Documento estratégico oficial de engenharia do LenhadorOS.**
> Esta é a referência canônica para a visão, as fases, a governança e o método de trabalho do projeto. Onde houver conflito entre este documento e práticas informais, este documento prevalece. Decisões que o contrariem devem ser registradas como ADR (`11-adr/`).

---

## 1. Visão executiva

O LenhadorOS é o sistema operacional de gestão do negócio — uma plataforma que unifica operação, dados e decisão em um único ecossistema governado. Este documento não descreve apenas *o que* será construído, mas *como* será construído: com método, rastreabilidade e qualidade verificável.

O projeto **LenhadorOS Architecture & Engineering** existe para garantir que cada parte do sistema seja **conhecida, documentada, auditável e evolutiva**. O fio condutor é duplo:

- **O Espelho Vivo** — o banco PostgreSQL real é a única fonte de verdade do schema.
- **O LAM** — o LenhadorOS Architecture Method, o fluxo obrigatório que transforma ideia em software confiável.

A engenharia é construída em **9 fases incrementais (0 a 8)**, cada uma com objetivo, entregáveis e critérios de conclusão explícitos. Nenhuma fase é decorativa: cada uma habilita a seguinte e reduz risco acumulado.

**Estado atual:** o repositório contém hoje um protótipo estático de gestão de metas (ver `audits/`/`REPO_AUDIT.md`) e a fundação de engenharia recém-criada (`/architecture`). O foco imediato é consolidar a Fase 0 e decidir a stack-alvo do LenhadorOS "real".

---

## 2. Por que o LenhadorOS precisa de uma engenharia própria

Sistemas de gestão crescem por acreção: módulos são adicionados sob pressão de operação, decisões são tomadas implicitamente e o conhecimento vive na cabeça de poucas pessoas. O resultado previsível é **dívida arquitetural**: duplicação, drift entre código e banco, regras de negócio espalhadas, ausência de testes e de auditoria, e medo de mudar.

Uma engenharia própria existe para inverter essa entropia:

- **Conhecimento explícito.** Nada é "óbvio" ou implícito; tudo relevante está versionado.
- **Verdade única.** O banco real define o schema — não diagramas que envelhecem.
- **Mudança segura.** Auditar antes de alterar; nada destrutivo sem aprovação.
- **Qualidade verificável.** Definição de Pronto, checklists e observabilidade contínua.
- **Escala sustentável.** Novos módulos nascem por um processo repetível, não por improviso.

Sem isso, cada novo módulo aumenta o risco do todo. Com isso, cada módulo nasce íntegro e o sistema fica mais forte a cada entrega.

---

## 3. ERP, plataforma e ecossistema — a diferença

O LenhadorOS é deliberadamente mais do que um ERP. Entender a distinção orienta as decisões de arquitetura.

| Conceito | O que é | Limite | Papel no LenhadorOS |
|---|---|---|---|
| **ERP** | Conjunto de módulos de gestão (financeiro, estoque, fiscal, RH) integrados sobre um banco comum. | Fechado e monolítico; integrações são exceção. | É a **camada de gestão** — necessária, mas não suficiente. |
| **Plataforma** | Base sobre a qual módulos e terceiros constroem, com contratos estáveis (APIs, eventos, autenticação, dados). | Extensível por contratos; o núcleo é estável e versionado. | É a **espinha dorsal**: APIs, identidade, permissões, Espelho Vivo. |
| **Ecossistema** | Rede de módulos próprios, integrações externas, automações e agentes de IA que evoluem de forma coordenada sob governança. | Aberto e evolutivo; governado, não caótico. | É a **ambição**: operação + integrações (WhatsApp, automações) + IA, tudo governado pelo LAM e observado pelo Vigia. |

> **Em uma frase:** o ERP entrega gestão, a plataforma entrega contratos estáveis, e o ecossistema entrega evolução coordenada. O LenhadorOS persegue os três — nessa ordem de maturidade.

---

## 4. Papéis

O projeto opera com quatro papéis, cada um com autoridade e responsabilidade claras. A separação evita que decisão, execução e fiscalização se confundam.

| Papel | Quem | Autoridade | Responsabilidade |
|---|---|---|---|
| **Product Owner** | **Everton** | Decide prioridades e aprova decisões estruturais (incluindo migrations e mudanças no Espelho Vivo). | Define o *quê* e o *porquê*; valida requisitos de negócio; é a palavra final de produto. |
| **Chief Architect** | **ChatGPT** | Propõe arquitetura, padrões e modelagem conceitual. | Desenha a solução, redige documentos e ADRs, gera prompts oficiais; garante coerência arquitetural. |
| **Engineering Executor** | **Claude Code** | Implementa diretamente no repositório seguindo o LAM. | Audita, modela, escreve código, testes e documentação viva; executa apenas o aprovado e nunca operações destrutivas sem autorização. |
| **Architecture Guardian** | **Vigia** | Monitora e sinaliza; pode bloquear via alertas de conformidade. | Observabilidade contínua: drift Prisma×banco, score de schema, qualidade e conformidade ao longo do tempo. |

### Fluxo entre os papéis

```
Everton (Product Owner) ── define o quê e por quê ──►
   ChatGPT (Chief Architect) ── desenha, documenta, decide arquitetura ──►
      Claude Code (Engineering Executor) ── audita, implementa, testa, documenta ──►
         Vigia (Architecture Guardian) ── observa drift, score, conformidade ──►
            feedback ──► volta a Everton/ChatGPT (ciclo fechado)
```

Nenhum papel atropela o outro: produto não escreve schema sem arquitetura, execução não altera banco sem aprovação, e o Vigia não decide produto — ele informa.

---

## 5 e 6. Fases oficiais do projeto

As fases são incrementais. Cada uma traz **objetivo**, **entregáveis**, **critérios de conclusão**, **riscos** e **próximos passos**.

### Fase 0 — Fundação
- **Objetivo:** estabelecer princípios, visão, papéis e a estrutura documental do projeto.
- **Entregáveis:** estrutura `/architecture` versionada; `MANIFESTO.md`; `README.md`; este `MASTER_PLAN.md`; papéis definidos.
- **Critérios de conclusão:** estrutura criada e no Git; manifesto e plano-mestre aprovados; papéis acordados.
- **Riscos:** documentação virar letra morta; escopo do LenhadorOS ainda ambíguo (este repo vs. repo próprio).
- **Próximos passos:** decidir escopo e stack-alvo via ADR; iniciar o Architecture Book.

### Fase 1 — Architecture Book
- **Objetivo:** consolidar a biblioteca oficial de arquitetura (padrões macro: módulos, domínios, camadas).
- **Entregáveis:** `01-architecture-book/` com padrões de organização, fronteiras de domínio e princípios de design.
- **Critérios de conclusão:** existe referência clara de "como organizamos um módulo e suas camadas".
- **Riscos:** abstração excessiva sem casos reais; divergência do que será de fato implementado.
- **Próximos passos:** derivar do Architecture Book os critérios concretos do LAM.

### Fase 2 — LAM (LenhadorOS Architecture Method)
- **Objetivo:** formalizar a metodologia oficial de desenvolvimento (ideia → observabilidade).
- **Entregáveis:** `02-lam/LAM.md` com as 11 etapas, critérios de entrada/saída e checklists associadas.
- **Critérios de conclusão:** qualquer módulo pode ser conduzido fim-a-fim só com o LAM.
- **Riscos:** processo pesado demais e ignorado na prática; etapas sem critérios objetivos.
- **Próximos passos:** ligar cada etapa do LAM a uma checklist em `checklists/`.

### Fase 3 — Espelho Vivo
- **Objetivo:** estabelecer o PostgreSQL real como única fonte de verdade e gerar seus artefatos.
- **Entregáveis:** `schema_snapshot.json`; ERD por domínio; `AUDIT.md`; `score.json`; catálogo de tabelas; dicionário de dados; relatório de drift Prisma×banco (ver `03-database/ESPELHO_VIVO.md`).
- **Critérios de conclusão:** snapshot gerado do banco real, versionado; drift visível e auditável.
- **Riscos:** ausência de banco ainda; execução acidental de migrations; confiar em diagrama manual.
- **Próximos passos:** conectar banco (quando existir) e automatizar a geração de snapshot read-only.

### Fase 4 — Engineering Standards
- **Objetivo:** padronizar backend, frontend, API, segurança, DevOps e banco.
- **Entregáveis:** `04-backend/`, `05-frontend/`, `06-apis/`, `08-security/`, `10-devops/`, `standards/`.
- **Critérios de conclusão:** padrões claros e aplicáveis para cada área; convenções de código e contrato.
- **Riscos:** padrões genéricos demais; padrões que ninguém adota por falta de tooling.
- **Próximos passos:** transformar padrões em lint/CI sempre que possível.

### Fase 5 — Documentação Viva
- **Objetivo:** manter documentação sincronizada com código e banco, parte dela gerada automaticamente.
- **Entregáveis:** `12-wiki-viva/`; dicionário de dados; ADRs atualizados; `14-glossary/GLOSSARY.md`.
- **Critérios de conclusão:** documentação reflete o sistema real; geração automática a partir do Espelho Vivo.
- **Riscos:** documentação manual envelhecer; duplicação entre wiki e código.
- **Próximos passos:** automatizar geração de catálogo/dicionário a partir do snapshot.

### Fase 6 — Observabilidade
- **Objetivo:** colocar o Vigia em operação: drift, score, dashboards e alertas contínuos.
- **Entregáveis:** integração do Vigia; dashboards de saúde de schema e qualidade; alertas de conformidade.
- **Critérios de conclusão:** drift e queda de score geram alerta automático e rastreável.
- **Riscos:** alertas ruidosos e ignorados; cobertura parcial dando falsa sensação de segurança.
- **Próximos passos:** definir limiares de score e políticas de alerta.

### Fase 7 — Automação
- **Objetivo:** automatizar prompts oficiais, pipelines, checklists e integração com IA.
- **Entregáveis:** `prompts/` populado; pipelines em `10-devops/`; checklists integradas ao fluxo.
- **Critérios de conclusão:** etapas repetíveis são automatizadas; prompts versionados e reutilizáveis.
- **Riscos:** automação frágil; prompts sem versionamento ou sem dono.
- **Próximos passos:** integrar checklists do LAM ao CI.

### Fase 8 — Evolução Contínua
- **Objetivo:** sustentar governança, revisões periódicas e melhoria permanente.
- **Entregáveis:** `13-roadmap/ROADMAP.md` atualizado; ritos de governança; cadência de revisão de ADRs.
- **Critérios de conclusão:** o sistema melhora de forma previsível e governada, sem grandes saltos arriscados.
- **Riscos:** estagnação; governança virar burocracia.
- **Próximos passos:** revisar roadmap e ADRs a cada ciclo de planejamento.

---

## Sequência Oficial de Trabalho

A ordem em que o projeto avança — fases acumulam, não se substituem:

1. **Fundação** estabelece princípios e estrutura.
2. **Architecture Book** define como organizamos arquitetura.
3. **LAM** define como executamos qualquer módulo.
4. **Espelho Vivo** garante a verdade dos dados.
5. **Standards** padronizam a execução.
6. **Documentação Viva** mantém o conhecimento sincronizado.
7. **Observabilidade** vigia a saúde contínua.
8. **Automação** torna o repetível barato e confiável.
9. **Evolução Contínua** governa a melhoria permanente.

> Regra de sequência: não se inicia uma fase dependente sem que a anterior tenha atingido seus critérios mínimos de conclusão — ou sem uma exceção aprovada e registrada como ADR.

---

## Como cada novo módulo deve nascer

Todo módulo segue o LAM (`02-lam/LAM.md`), nesta ordem inegociável:

1. **Ideia** — problema, objetivo e escopo aprovados por Everton.
2. **Auditoria** — estado atual de repo/banco mapeado; riscos catalogados em `audits/`.
3. **Documento de arquitetura** — solução, domínios e contratos desenhados; decisões viram ADR.
4. **Modelagem Schema First** — modelo de dados definido antes do código.
5. **Validação no Espelho Vivo** — schema proposto confrontado com o banco real; drift verificado.
6. **Backend** — lógica de domínio e APIs com validação, autorização e logs.
7. **Frontend** — UI consumindo APIs, sem regra de negócio embutida.
8. **Testes** — cenários críticos cobertos e passando.
9. **Deploy** — publicação reversível, versionada e monitorável.
10. **Documentação Viva** — wiki, dicionário e ADRs sincronizados.
11. **Observabilidade pelo Vigia** — módulo acompanhado em produção.

> **Um módulo que pula uma etapa não é considerado entregue.** A regra de ouro: *auditoria → arquitetura → banco → implementação → teste → deploy → documentação viva*.

---

## Relação entre Architecture Book, LAM, Espelho Vivo, Wiki Viva e Vigia

Estes cinco pilares operam juntos; cada um responde a uma pergunta distinta:

| Pilar | Pergunta que responde | Papel |
|---|---|---|
| **Architecture Book** | *Como organizamos a arquitetura?* | Define os padrões macro (módulos, domínios, camadas). É a teoria. |
| **LAM** | *Como executamos um módulo?* | Define o processo passo a passo. É o método. |
| **Espelho Vivo** | *Qual é a verdade dos dados?* | Define a fonte canônica do schema (o banco real). É o chão de fábrica. |
| **Wiki Viva** | *O que o sistema é, hoje?* | Mantém o conhecimento sincronizado com código e banco. É a memória. |
| **Vigia** | *O sistema continua íntegro?* | Observa drift, score e conformidade ao longo do tempo. É a sentinela. |

### Como se encadeiam

```
Architecture Book ──(padrões)──► LAM ──(execução)──► Espelho Vivo (verdade)
        │                                                   │
        └────────────► Wiki Viva ◄──(reflete código+banco)──┘
                            │
                            ▼
                          Vigia ──(drift/score/alertas)──► feedback ao LAM/Architecture Book
```

O Architecture Book informa o LAM; o LAM produz mudanças validadas contra o Espelho Vivo; a Wiki Viva reflete o resultado real; o Vigia observa e realimenta o ciclo. Nenhum pilar funciona isolado.

---

## Governança

A governança garante que decisões sejam tomadas pelas pessoas certas, registradas e revisáveis.

- **Decisões estruturais** (schema, stack, fronteiras de domínio, segurança) exigem aprovação do Product Owner e registro como ADR.
- **ADRs são imutáveis após aceitos.** Mudar uma decisão cria um novo ADR que substitui o anterior.
- **Operações sensíveis** — migrations, `prisma migrate dev/diff`, qualquer comando destrutivo — exigem **aprovação explícita** e nunca são executadas por padrão.
- **O banco é a verdade.** Diagramas e Prisma são projeções; em conflito, o PostgreSQL real vence.
- **Tudo versionado.** Se não está no Git, não é oficial.
- **Separação de papéis.** Produto decide, arquitetura desenha, execução implementa, Vigia fiscaliza — sem sobreposição.
- **Exceções são explícitas.** Pular uma etapa do LAM ou um critério de Pronto exige exceção aprovada e documentada.

---

## Roadmap de Execução

Alinhado ao `13-roadmap/ROADMAP.md`. Horizontes: **Agora**, **Próximo**, **Depois**.

### Agora — Fase 0 (Fundação)
- [x] Criar estrutura `/architecture`.
- [x] Manifesto, MASTER_PLAN (este documento), LAM, Espelho Vivo, ADR-0001.
- [ ] **ADR-0002:** definir escopo (este repo evolui para o LenhadorOS ou nasce em repo próprio).
- [ ] **ADR-0003:** definir stack-alvo (backend/frontend/ORM).

### Próximo — Fases 1 a 3
- [ ] **Fase 1:** consolidar o Architecture Book.
- [ ] **Fase 2:** detalhar critérios de entrada/saída e checklists do LAM.
- [ ] **Fase 3:** conectar PostgreSQL e gerar `schema_snapshot.json`, ERD por domínio, `score.json`, `AUDIT.md`.

### Depois — Fases 4 a 8
- [ ] **Fase 4:** Engineering Standards por área.
- [ ] **Fase 5:** Documentação Viva com geração automática.
- [ ] **Fase 6:** Vigia em operação (drift, score, dashboards, alertas).
- [ ] **Fase 7:** Automação (prompts, pipelines, checklists no CI).
- [ ] **Fase 8:** governança contínua e revisão de roadmap/ADRs.

---

## Definição de Pronto

Uma entrega só é considerada **Pronta** quando **todos** os critérios abaixo são satisfeitos (ou têm exceção aprovada e registrada):

- [ ] **Auditada** — estado atual mapeado antes da mudança.
- [ ] **Arquitetada** — documento de arquitetura e ADRs das decisões relevantes.
- [ ] **Modelada (Schema First)** — modelo de dados definido e **validado no Espelho Vivo**, sem drift inesperado.
- [ ] **Implementada** — backend com validação, autorização e logs; frontend sem regra de negócio.
- [ ] **Testada** — cenários críticos cobertos e passando.
- [ ] **Segura** — autenticação, autorização, sem segredos versionados em texto claro.
- [ ] **Deployada** — publicação reversível, versionada e monitorável.
- [ ] **Documentada (viva)** — wiki, dicionário de dados e ADRs sincronizados com o entregue.
- [ ] **Observada** — Vigia acompanhando o módulo (drift/score/conformidade).
- [ ] **Versionada** — tudo no Git.

> Faltou um item sem exceção aprovada? Então **não está pronto**.

---

## Próximos Passos

1. **Decisão de escopo (bloqueante):** registrar **ADR-0002** definindo se o LenhadorOS nasce neste repositório ou em um repositório próprio.
2. **Decisão de stack (bloqueante):** registrar **ADR-0003** com a stack-alvo (backend, frontend, ORM, banco).
3. **Fase 1 — Architecture Book:** iniciar os padrões macro de arquitetura em `01-architecture-book/`.
4. **Fase 2 — LAM:** ligar cada etapa do LAM a uma checklist objetiva em `checklists/`.
5. **Preparar Fase 3:** especificar o processo read-only de geração do `schema_snapshot.json` para quando o banco existir.

> **Próximo prompt recomendado:** *PROMPT 02 — Architecture Book*, para consolidar os padrões macro de arquitetura (módulos, domínios e camadas) que alimentarão o LAM e os Engineering Standards.
