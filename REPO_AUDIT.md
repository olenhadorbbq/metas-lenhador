# REPO_AUDIT.md — Auditoria Inicial do Repositório

> **Projeto auditado:** `olenhadorbbq/metas-lenhador`
> **Branch:** `claude/lenhador-os-audit-yb4h9y`
> **Data da auditoria:** 2026-06-30
> **Auditor:** Arquitetura de Software (somente leitura — nenhum arquivo do sistema foi alterado)
> **Escopo:** Varredura completa e não-destrutiva. Nenhuma migration executada, nenhum comando destrutivo, nenhum `prisma migrate`.

---

## ⚠️ Achado Crítico Inicial — Divergência entre o Prompt e o Repositório Real

O prompt de auditoria descreve um sistema chamado **LenhadorOS**: uma plataforma multi-módulo (backend, frontend, `apps/`, `packages/`, Prisma, banco de dados, RH, Fiscal, Financeiro, Delivery, Caixa, IA, WhatsApp, N8N, etc.).

**Nenhuma dessas estruturas existe neste repositório.**

O repositório `metas-lenhador`, em **todos os branches** (`main` e `claude/lenhador-os-audit-yb4h9y`), contém **um único arquivo versionado**:

```
index.html        (≈ 6,7 KB / 206 linhas)
```

Histórico Git completo:

```
653d9e7  Add files via upload      (commit único, upload manual)
```

Não há `package.json`, `node_modules`, `prisma/`, `src/`, `backend/`, `frontend/`, `apps/`, `packages/`, `docker-compose`, CI, testes, scripts ou `.env`.

**Conclusão honesta:** este repositório **não é** o "LenhadorOS". É um **protótipo de página única (SPA estática)** para gestão de metas do restaurante *O Lenhador BBQ*, cujo "backend" é uma planilha Google (Google Sheets) acessada via Google Apps Script.

Toda a auditoria abaixo reflete **o que de fato existe**. As seções exigidas pelo prompt (Mapa de Módulos, Stack, Riscos, etc.) são preenchidas com a realidade encontrada, e a divergência é tratada explicitamente em **Próximos Passos**.

---

## Estrutura do Repositório

```
metas-lenhador/
├── .git/
└── index.html        ← única aplicação; SPA estática + chamadas fetch a Apps Script
```

| Item esperado pelo prompt | Encontrado? | Observação |
|---|---|---|
| backend | ❌ | Não existe. O "backend" é um Google Apps Script externo (Google Sheets). |
| frontend | ⚠️ Parcial | Apenas `index.html` (HTML + CSS inline + JS inline). Sem framework. |
| apps/ | ❌ | Não existe (não é monorepo). |
| packages/ | ❌ | Não existe. |
| prisma/ | ❌ | Não existe. Não há ORM. |
| banco de dados | ⚠️ Externo | Persistência em Google Sheets, não em banco relacional. |
| scripts | ❌ | Nenhum script de build/deploy/seed. |
| documentação | ❌ | Nenhum README, ADR ou doc. (Este arquivo é o primeiro.) |
| integrações | ⚠️ | Uma única: Google Apps Script (`script.google.com/macros/...`). |
| módulos existentes | ⚠️ | Um único "módulo" lógico: **Metas**. |
| arquivos de configuração | ❌ | Nenhum (`package.json`, `tsconfig`, `.env`, lint, etc. ausentes). |
| variáveis de ambiente | ❌ | Nenhuma. URL do backend está **hardcoded** no HTML. |
| pontos de entrada | ✅ | `index.html` (executa `fetchMetas()` no load). |

---

## Stack Técnica Encontrada

| Camada | Tecnologia real |
|---|---|
| Linguagem principal | JavaScript (ES inline no HTML) + HTML5 + CSS inline |
| Framework backend | **Nenhum** — Google Apps Script (serverless do Google) atua como API |
| Framework frontend | **Nenhum** — JS puro (vanilla), manipulação direta do DOM |
| ORM | **Nenhum** (sem Prisma, sem banco relacional) |
| Banco de dados | **Google Sheets** (planilha) via Apps Script |
| Bibliotecas principais | **Chart.js** via CDN (`cdn.jsdelivr.net`) — gráfico doughnut de status |
| Autenticação | **Inexistente / frágil** — papéis passados por query string (`?user=`, `?admin=true`) |
| Permissões | **Inexistente** — `admin=true` na URL libera modo admin sem qualquer verificação |
| Build | **Nenhum** — arquivo servido estaticamente |
| Scripts (npm/pnpm/yarn) | **Nenhum** — não há `package.json` |
| Deploy | Indefinido — provavelmente hospedagem estática (ex.: GitHub Pages) |
| Docker | **Não** |
| Filas | **Não** |
| Cron/Scheduler | **Não** |
| Webhooks | **Não** |
| Integrações externas | Google Apps Script (CRUD em planilha) + CDN do Chart.js |

---

## Mapa de Módulos

Existe **um único módulo funcional**. Os demais módulos citados no prompt (Usuários, Permissões, Empresas, Produtos, Estoque, Financeiro, Fiscal, RH, Delivery, Garçom, Caixa, Integrações, IA, WhatsApp, N8N, Painel gerencial) **não existem** neste repositório.

### Módulo: **Metas (Gestão de Metas)** — ÚNICO existente

| Atributo | Detalhe |
|---|---|
| Caminho | `index.html` |
| Arquivos principais | `index.html` (tudo: UI, lógica, chamadas de rede) |
| "Tabelas" relacionadas | Uma aba/planilha no Google Sheets, com colunas: `ID`, `Meta`, `Responsável`, `Prioridade`, `Prazo`, `Status`, `Criada por` |
| Rotas/API | Endpoint único do Apps Script (`/exec`), via `POST` (`acao: criar/editar`) e `GET` (`?admin=true` ou `?user=`) |
| Telas | Uma: formulário de meta, tabela de metas, gráfico de status, filtros (responsável/status/prioridade) |
| Status aparente | **Protótipo incompleto / legado** — funcional como MVP, mas com bug na URL (ver Riscos P0) |

### Módulos esperados pelo prompt e NÃO encontrados

`Core/OS`, `Usuários`, `Permissões`, `Empresas`, `Produtos`, `Estoque`, `Financeiro`, `Fiscal`, `RH`, `Delivery`, `Garçom`, `Caixa`, `Integrações`, `IA`, `WhatsApp`, `N8N`, `Painel gerencial` → **status: inexistentes neste repositório.**

---

## Riscos Arquiteturais

Classificação: **P0 (crítico)** · **P1 (alto)** · **P2 (médio)** · **P3 (baixo)**.

### P0 — Críticos

| # | Risco | Evidência | Impacto |
|---|---|---|---|
| P0-1 | **URL do backend quebrada (bug de digitação)** | `index.html:75` → `"https://https://script.google.com/..."` (duplo `https://`) | Aplicação provavelmente **não funciona** em produção; todos os `fetch` falham. |
| P0-2 | **Autenticação/autorização inexistente** | `index.html:78,109` → `admin = urlParams.get("admin")`; basta `?admin=true` na URL | Qualquer pessoa vira admin. Sem login, sem token, sem sessão. **Controle de acesso nulo.** |
| P0-3 | **Endpoint de dados público e exposto no cliente** | `index.html:75` URL do Apps Script `/exec` embutida no HTML público | Qualquer um pode ler/escrever/editar metas chamando o endpoint diretamente. Sem chave/segredo. |

### P1 — Altos

| # | Risco | Evidência | Impacto |
|---|---|---|---|
| P1-1 | **XSS armazenado (stored XSS)** | `index.html:143-154` e `:131-133` → `innerHTML` com dados não-escapados (`m.Meta`, `m.Responsável`, `nome`) | Conteúdo da planilha é injetado direto no DOM; campos maliciosos executam script. |
| P1-2 | **Injeção via handler inline** | `index.html:154` → `onclick='editarMeta(${JSON.stringify(m)})'` | `JSON.stringify` interpolado em atributo HTML permite quebra de aspas/injeção. |
| P1-3 | **Regras de negócio 100% no cliente** | Todo o JS em `index.html` | Validação, filtros e papéis no front; trivialmente contornáveis. |

### P2 — Médios

| # | Risco | Evidência | Impacto |
|---|---|---|---|
| P2-1 | **Dependência de CDN sem SRI** | `index.html:7` → Chart.js via `cdn.jsdelivr.net` sem `integrity`/`crossorigin` | Risco de supply chain; quebra se a CDN cair. |
| P2-2 | **Ausência total de validação de entrada** | Formulário aceita qualquer string; só `required` de HTML | Dados inconsistentes na planilha. |
| P2-3 | **Sem tratamento de erro de rede** | `await fetch(...)` sem `try/catch` (`:96,161,108`) | Falha silenciosa; `alert("Meta salva!")` mesmo se o POST falhar (`:102`). |
| P2-4 | **Acoplamento ao formato da planilha** | `m.Prazo.split("T")[0]` (`:146,174`) assume formato ISO | Quebra se a coluna mudar de tipo/formato. |

### P3 — Baixos

| # | Risco | Evidência | Impacto |
|---|---|---|---|
| P3-1 | **Ausência de testes** | Nenhum arquivo de teste no repo | Sem rede de segurança para mudanças. |
| P3-2 | **Ausência de logs/auditoria** | Nenhum logging | Impossível rastrear quem alterou metas. |
| P3-3 | **Sem build/lint/formatação** | Sem `package.json`/config | Padronização e qualidade não garantidas. |
| P3-4 | **Status `Pendente` forçado no submit** | `index.html:92` sempre envia `status:"Pendente"` ao editar pelo form | Edição via formulário sobrescreve o status atual. |
| P3-5 | **Tudo em um único arquivo monolítico** | `index.html` mistura HTML/CSS/JS | Manutenção difícil; sem separação de domínio. |

> **Variáveis sensíveis no repositório:** a URL do Apps Script (`AKfycby61...`) está commitada em texto claro. Não é uma credencial secreta no sentido clássico, mas é um **endpoint de escrita exposto** — deve ser tratado como sensível.

---

## Documentação Existente

| Tipo | Existe? | Local | Atualizado? | Falta |
|---|---|---|---|---|
| README | ❌ | — | — | Criar README com propósito, deploy e configuração do Apps Script |
| docs/ | ❌ | — | — | Criar estrutura de documentação |
| architecture/ | ❌ | — | — | Pasta proposta (ver abaixo) |
| ADR | ❌ | — | — | Nenhuma decisão arquitetural registrada |
| Schema/banco | ❌ | — | — | Documentar colunas da planilha como "schema" |
| Manual | ❌ | — | — | Manual de uso (significado de `?admin`/`?user`) |
| Prompts/Planejamento/Roadmap | ❌ | — | — | Inexistentes |

**Documentação atual do projeto: zero.** Este `REPO_AUDIT.md` é o primeiro artefato de documentação.

> **Nota sobre o destino do relatório:** o prompt pediu `/architecture/audits/REPO_AUDIT.md`. Como a pasta `/architecture/audits/` **não existe**, e conforme a regra "se a pasta não existir, apenas proponha a criação — não crie ainda", o relatório foi gerado na **raiz** como `REPO_AUDIT.md`. **Proposta:** criar futuramente `architecture/audits/` e mover este arquivo para lá.

---

## Visão Arquitetural Inicial

**Como o sistema está organizado hoje:**

```
[ Navegador do usuário ]
        │  carrega index.html (HTML+CSS+JS inline)
        │
        ▼
[ index.html / Vanilla JS ]
        │  fetch (GET/POST JSON)
        │  papel definido por query string (?admin / ?user)
        ▼
[ Google Apps Script  /exec ]  ← endpoint público, sem auth
        │
        ▼
[ Google Sheets (planilha) ]  ← persistência (ID, Meta, Responsável, Prioridade, Prazo, Status, Criada por)
```

* **Principais blocos:** (1) UI estática única; (2) camada de rede embutida; (3) Apps Script como API; (4) Sheets como "banco".
* **Pontos de acoplamento:** URL hardcoded do Apps Script; nomes das colunas da planilha referenciados literalmente no JS (`m.Responsável`, `m["Criada por"]`); papel do usuário acoplado à query string.
* **Pontos frágeis:** ausência de autenticação real (P0-2), URL quebrada (P0-1), XSS (P1-1/P1-2), nenhuma validação/log/teste, monólito de arquivo único.
* **Áreas que precisam de documentação primeiro:** (a) contrato da API do Apps Script (ações, parâmetros, formato de resposta); (b) "schema" da planilha; (c) modelo de autenticação pretendido; (d) como o repositório se relaciona — se é que se relaciona — com o "LenhadorOS" descrito no prompt.

---

## Próximos Passos Recomendados

> Nenhuma alteração de código foi feita. As ações abaixo são **recomendações** para aprovação.

### Ações imediatas
1. **Esclarecer a divergência de escopo:** este repo (`metas-lenhador`) é apenas o módulo de Metas, **não** o "LenhadorOS". Confirmar se a auditoria deveria apontar para **outro repositório**.
2. **Corrigir o bug P0-1** (`https://https://` na linha 75) — sem isso a aplicação não funciona.

### Ações antes de novos módulos
3. Definir se o futuro "LenhadorOS" nasce **neste** repo (migrando para um stack real — ex.: backend Node/Prisma + frontend SPA) ou em repositório próprio.
4. Estabelecer stack-alvo, padrão de monorepo (`apps/`+`packages/`) ou multi-repo, e separação por domínio **antes** de codar módulos novos.

### Ações de organização
5. Adicionar `package.json`, linter, formatador e estrutura de pastas mínima.
6. Separar HTML/CSS/JS; extrair a camada de acesso a dados; remover lógica do `index.html` monolítico.

### Ações de segurança
7. **P0-2/P0-3:** implementar autenticação/autorização reais (não confiar em query string); proteger o endpoint de escrita.
8. **P1-1/P1-2:** sanitizar/escapar toda saída no DOM; substituir `innerHTML`+handlers inline por criação segura de nós.
9. **P2-1:** adicionar SRI ao Chart.js ou auto-hospedar.
10. Mover a URL do Apps Script para configuração (não hardcoded) e revisar exposição do endpoint.

### Ações de documentação
11. Criar `README.md` (propósito, deploy, configuração do Apps Script, significado de `?admin`/`?user`).
12. Criar `architecture/` + `architecture/audits/` e mover este relatório para `architecture/audits/REPO_AUDIT.md`.
13. Documentar o contrato da API do Apps Script e o "schema" da planilha; iniciar um log de ADRs.

### Ações para integração com o Espelho Vivo
14. O "Espelho Vivo" **não foi encontrado** no repositório (nenhuma referência em código). Antes de qualquer integração: definir o que é, onde vive e qual o contrato de dados.
15. Mapear quais entidades de Metas (status, responsável, prazo) alimentariam o Espelho Vivo e por qual mecanismo (webhook, fila, sync) — nenhum desses mecanismos existe hoje e precisariam ser introduzidos junto com o backend real.

---

### Apêndice — Inventário de evidências (somente leitura)

| Verificação | Resultado |
|---|---|
| `git ls-files` | `index.html` (1 arquivo) |
| `git log` | 1 commit: `653d9e7 Add files via upload` |
| `git ls-tree origin/main` | `index.html` |
| `git ls-tree origin/claude/lenhador-os-audit-yb4h9y` | `index.html` |
| `git stash list` | vazio |
| Tamanho | `index.html` ≈ 8 KB; `.git` ≈ 236 KB |

*Fim do relatório. Auditoria não-destrutiva concluída — sistema não modificado.*
