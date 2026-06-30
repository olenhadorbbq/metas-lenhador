# DISCOVERY MASTER — LenhadorOS

> **Fase 0 — Discovery & Assessment.** Descoberta da realidade atual do projeto, baseada exclusivamente no que existe no repositório. Documento de discovery (não define padrões nem arquitetura-alvo).
>
> **Repositório:** `olenhadorbbq/metas-lenhador` · **Branch:** `claude/lenhador-os-audit-yb4h9y` · **Data:** 2026-06-30 · **Modo:** somente leitura (nenhum código/banco alterado).

---

## ⚠️ Declaração de realidade (ler primeiro)

> **Este repositório NÃO é o "LenhadorOS completo".**

O projeto descrito nos prompts (ERP/plataforma com backend, frontend, Prisma, PostgreSQL, RH, Fiscal, Financeiro, Delivery, Caixa, IA, WhatsApp, N8N, Vigia, etc.) **não está presente neste repositório**. O que existe de fato:

```
metas-lenhador/
├── index.html        ← protótipo estático de Gestão de Metas (a ÚNICA aplicação)
├── REPO_AUDIT.md     ← auditoria inicial (gerada nesta iniciativa)
└── architecture/     ← fundação documental de engenharia (gerada nesta iniciativa)
```

**Inventário objetivo de arquivos versionados:** 25 arquivos no total — `index.html` (1 app), `REPO_AUDIT.md` (1 auditoria) e 23 documentos em `/architecture`. **Nenhum** arquivo de código de aplicação (`.js/.ts/.py/.sql/.prisma`), **nenhum** `package.json`, **nenhum** Dockerfile, **nenhum** `.env`, **nenhum** banco.

Este documento descreve a realidade — não a expectativa. Nenhum módulo foi inventado.

---

## Síntese das perguntas-chave

| Pergunta | Resposta baseada em evidência |
|---|---|
| **O que existe?** | Um protótipo SPA estático (`index.html`) de gestão de metas, com backend em Google Apps Script + Google Sheets. A fundação documental `/architecture`. |
| **O que não existe?** | Backend próprio, banco relacional, Prisma, APIs próprias, autenticação real, testes, CI/CD, infra, integrações (N8N/WhatsApp), IA, observabilidade. |
| **O que funciona?** | A UI renderiza (formulário, tabela, gráfico Chart.js). **A integração de rede provavelmente NÃO funciona** por bug de URL (ver 🔴 abaixo). |
| **O que está incompleto?** | Praticamente todo o "LenhadorOS" idealizado: ele não foi iniciado neste repo. |
| **O que é legado?** | O `index.html` tem cara de protótipo/legado de upload manual (commit único "Add files via upload"). |
| **O que é protótipo?** | `index.html` — explicitamente um MVP. |
| **O que é dívida técnica?** | URL quebrada, ausência de auth, XSS, regra de negócio no cliente, zero testes, segredo de endpoint exposto. |
| **O que está bom?** | A iniciativa de engenharia (`/architecture`) é sólida e versionada; o protótipo é simples e legível. |
| **O que está perigoso?** | `?admin=true` libera admin sem auth; endpoint de escrita público; XSS via `innerHTML`. |
| **O que precisa ser investigado?** | Onde vive o "LenhadorOS real" (outro repo? não existe ainda?); o que é o Apps Script por trás; o que são Vigia/Espelho Vivo na prática. |

---

## Auditoria por área (1–20)

Legenda de status: 🟢 Confirmado · 🟡 Precisa validar · 🔵 Hipótese · 🔴 Problema encontrado · ⚫ Não encontrado

### 1. Repositório
- **Estado atual:** mínimo. 1 app + docs de engenharia.
- **Evidências:** `git ls-files` → 25 arquivos; `git log` → 1 commit de aplicação (`653d9e7 Add files via upload`).
- **Existe:** `index.html`, `REPO_AUDIT.md`, `/architecture`.
- **Não existe:** estrutura de código, monorepo, `package.json`, configs.
- **Riscos:** confundir este repo com o "LenhadorOS completo".
- **Pendências:** decidir se o sistema nasce aqui ou em repo próprio.
- **Hipóteses:** existe (ou existirá) outro repositório com o sistema real.
- **Próximas auditorias:** localizar/confirmar o repo do sistema real.
- **Status:** 🟢 Confirmado (repo é um protótipo + fundação documental).

### 2. Banco de dados
- **Estado atual:** inexistente no repo. Persistência real = Google Sheets (externa).
- **Evidências:** nenhum `.sql`/`.prisma`/conexão; `index.html:75` aponta para Apps Script.
- **Existe:** "tabela" lógica na planilha (colunas: ID, Meta, Responsável, Prioridade, Prazo, Status, Criada por).
- **Não existe:** PostgreSQL, schema, migrations, Espelho Vivo real.
- **Riscos:** o "Espelho Vivo" (fonte de verdade) não tem banco para espelhar ainda.
- **Pendências:** provisionar/conectar PostgreSQL quando o sistema real existir.
- **Hipóteses:** o banco do LenhadorOS vive fora deste repo (ou ainda não existe).
- **Próximas auditorias:** introspecção read-only do banco real, quando disponível.
- **Status:** ⚫ Não encontrado (banco relacional) / 🔵 Hipótese (Sheets como store atual).

### 3. Backend
- **Estado atual:** não há backend próprio. Papel de "API" é cumprido por Google Apps Script.
- **Evidências:** `fetch(scriptURL, ...)` (`index.html:96,109,161`).
- **Existe:** um endpoint `/exec` do Apps Script (externo, não versionado aqui).
- **Não existe:** servidor próprio, camada de domínio, serviços, validação server-side.
- **Riscos:** regra de negócio toda no cliente; lógica do Apps Script é caixa-preta.
- **Pendências:** obter/auditar o código do Apps Script.
- **Hipóteses:** o Apps Script faz CRUD direto na planilha.
- **Próximas auditorias:** revisar o script do Apps Script (fora do repo).
- **Status:** ⚫ Não encontrado (backend próprio) / 🔵 Hipótese (Apps Script).

### 4. Frontend
- **Estado atual:** SPA estática de arquivo único, vanilla JS.
- **Evidências:** `index.html` (HTML+CSS+JS inline, 206 linhas); Chart.js via CDN (`:7`).
- **Existe:** formulário de meta, tabela, filtros, gráfico doughnut.
- **Não existe:** framework, build, componentização, separação de camadas.
- **Riscos:** regra de negócio e papéis no front; XSS via `innerHTML`.
- **Pendências:** definir stack-alvo de frontend.
- **Hipóteses:** servido como página estática (ex.: GitHub Pages).
- **Próximas auditorias:** confirmar hospedagem.
- **Status:** 🟢 Confirmado (protótipo) / 🔴 Problema (XSS, regra no cliente).

### 5. APIs
- **Estado atual:** uma integração HTTP com Apps Script; sem API própria documentada.
- **Evidências:** GET `?admin=true`/`?user=` (`:109`), POST `{acao: criar|editar}` (`:85-100,161`).
- **Existe:** contrato implícito (ações `criar`/`editar`) com a planilha.
- **Não existe:** versionamento, especificação (OpenAPI), autenticação de API.
- **Riscos:** contrato não documentado; mudança na planilha quebra o cliente.
- **Pendências:** documentar o contrato do Apps Script.
- **Hipóteses:** respostas são JSON de linhas da planilha.
- **Próximas auditorias:** mapear formato exato de request/response.
- **Status:** 🟡 Precisa validar.

### 6. Segurança
- **Estado atual:** frágil/inexistente.
- **Evidências:** `?admin=true` vira admin (`:78,109`); URL do Apps Script (`AKfycby61...`) exposta no HTML público (`:75`); `innerHTML` sem escape (`:143-154,131-133`).
- **Existe:** nada de proteção real.
- **Não existe:** autenticação, autorização server-side, sanitização, gestão de segredos.
- **Riscos:** **P0** controle de acesso nulo; endpoint de escrita público; **P1** XSS armazenado.
- **Pendências:** modelo de auth real; proteção do endpoint.
- **Hipóteses:** o Apps Script não valida origem/permissão.
- **Próximas auditorias:** auditoria de segurança dedicada.
- **Status:** 🔴 Problema encontrado.

### 7. Permissões
- **Estado atual:** baseadas em query string; sem verificação.
- **Evidências:** `admin = urlParams.get("admin")` (`:78`).
- **Existe:** distinção lógica admin/user só no front.
- **Não existe:** RBAC, papéis no servidor, enforcement.
- **Riscos:** qualquer um eleva privilégio mudando a URL.
- **Pendências:** desenhar modelo de permissões real.
- **Hipóteses:** Apps Script trata `admin=true` literalmente.
- **Próximas auditorias:** confirmar enforcement no Apps Script.
- **Status:** 🔴 Problema encontrado.

### 8. Autenticação
- **Estado atual:** inexistente.
- **Evidências:** sem login/sessão/token em todo o `index.html`.
- **Existe:** identificação por `?user=` (string livre, não autenticada).
- **Não existe:** login, sessão, tokens, IdP.
- **Riscos:** identidade não confiável; impersonação trivial.
- **Pendências:** escolher mecanismo de auth.
- **Hipóteses:** nunca houve auth real neste protótipo.
- **Próximas auditorias:** —.
- **Status:** ⚫ Não encontrado.

### 9. Infraestrutura
- **Estado atual:** não descrita no repo.
- **Evidências:** sem IaC, sem configs de ambiente.
- **Existe:** nada versionado.
- **Não existe:** definição de ambientes, redes, provisionamento.
- **Riscos:** ambiente implícito e não reproduzível.
- **Pendências:** definir infra-alvo.
- **Hipóteses:** hospedagem estática simples para o protótipo.
- **Próximas auditorias:** confirmar onde `index.html` é servido.
- **Status:** ⚫ Não encontrado.

### 10. Deploy
- **Estado atual:** indefinido.
- **Evidências:** sem pipeline, sem workflow, sem scripts de deploy.
- **Existe:** nada versionado.
- **Não existe:** CI/CD, ambientes, releases.
- **Riscos:** deploy manual e não rastreável.
- **Pendências:** definir processo de deploy.
- **Hipóteses:** upload manual (commit "Add files via upload").
- **Próximas auditorias:** —.
- **Status:** ⚫ Não encontrado.

### 11. Integrações
- **Estado atual:** uma integração externa real (Apps Script) + CDN.
- **Evidências:** `script.google.com/macros/...` (`:75`), Chart.js CDN (`:7`).
- **Existe:** integração com Google Apps Script/Sheets; dependência de CDN.
- **Não existe:** N8N, WhatsApp, gateways, webhooks próprios.
- **Riscos:** acoplamento ao Google; CDN sem SRI.
- **Pendências:** catalogar integrações pretendidas.
- **Hipóteses:** Sheets é o "banco" atual.
- **Próximas auditorias:** auditar o projeto Apps Script.
- **Status:** 🟢 Confirmado (Google) / ⚫ Não encontrado (demais).

### 12. IA
- **Estado atual:** ausente no produto.
- **Evidências:** nenhuma chamada a modelos no código.
- **Existe:** IA apenas no *processo de engenharia* (ChatGPT/Claude Code), não no produto.
- **Não existe:** agentes, prompts de produto, integração com LLM no app.
- **Riscos:** nenhum no app (porque não há IA embarcada).
- **Pendências:** definir onde IA entra no produto (Fase 7).
- **Hipóteses:** —.
- **Próximas auditorias:** —.
- **Status:** ⚫ Não encontrado (no produto).

### 13. N8N
- **Estado atual:** inexistente.
- **Evidências:** nenhuma referência a N8N no repo.
- **Existe:** nada.
- **Não existe:** workflows, instância, credenciais.
- **Riscos:** —.
- **Pendências:** confirmar se existe N8N fora do repo.
- **Hipóteses:** se existir, é externo e não versionado.
- **Próximas auditorias:** verificar infra externa.
- **Status:** ⚫ Não encontrado.

### 14. WhatsApp
- **Estado atual:** inexistente.
- **Evidências:** nenhuma referência (API, webhook, número) no repo.
- **Existe:** nada.
- **Não existe:** integração de mensageria.
- **Riscos:** —.
- **Pendências:** confirmar se há integração externa pretendida.
- **Hipóteses:** —.
- **Próximas auditorias:** —.
- **Status:** ⚫ Não encontrado.

### 15. Observabilidade
- **Estado atual:** inexistente.
- **Evidências:** sem logs, métricas, alertas; sem `try/catch` (`:96,108,161`).
- **Existe:** nada (nem o "Vigia").
- **Não existe:** logging, monitoramento, drift, score, dashboards.
- **Riscos:** falhas silenciosas (ex.: `alert("Meta salva!")` mesmo se POST falhar — `:102`).
- **Pendências:** definir Vigia e instrumentação.
- **Hipóteses:** —.
- **Próximas auditorias:** —.
- **Status:** ⚫ Não encontrado.

### 16. Documentação
- **Estado atual:** boa para engenharia (recém-criada), inexistente para o app.
- **Evidências:** `/architecture` (23 arquivos) + `REPO_AUDIT.md`; nenhum README do `index.html`.
- **Existe:** Manifesto, MASTER_PLAN, LAM, Espelho Vivo, ADR-0001, etc.
- **Não existe:** README do produto, contrato do Apps Script, dicionário de dados real.
- **Riscos:** docs de engenharia descreverem um sistema que ainda não existe.
- **Pendências:** documentar o que de fato existe (o protótipo).
- **Hipóteses:** —.
- **Próximas auditorias:** —.
- **Status:** 🟢 Confirmado (engenharia) / ⚫ Não encontrado (produto).

### 17. Testes
- **Estado atual:** inexistentes.
- **Evidências:** nenhum arquivo de teste; sem framework.
- **Existe:** nada.
- **Não existe:** unit, integração, e2e.
- **Riscos:** nenhuma rede de segurança para mudanças.
- **Pendências:** definir estratégia de testes (Fase 4/LAM etapa 8).
- **Hipóteses:** —.
- **Próximas auditorias:** —.
- **Status:** ⚫ Não encontrado.

### 18. Performance
- **Estado atual:** não mensurável de forma significativa (app trivial).
- **Evidências:** SPA pequena; recarrega tudo via `fetchMetas()` a cada ação.
- **Existe:** padrão simples de re-render total.
- **Não existe:** métricas, budget de performance, otimização.
- **Riscos:** baixos hoje; re-render total não escala com volume.
- **Pendências:** reavaliar quando houver sistema real.
- **Hipóteses:** —.
- **Próximas auditorias:** —.
- **Status:** 🟡 Precisa validar (baixa relevância atual).

### 19. Módulos funcionais
- **Estado atual:** um único módulo — **Metas**.
- **Evidências:** todo o `index.html` gira em torno de CRUD de metas.
- **Existe:** módulo **Metas** (criar/editar/filtrar/visualizar status).
- **Não existe:** os ~17 módulos citados (Financeiro, Fiscal, RH, Delivery, Caixa, etc.).
- **Riscos:** assumir que módulos citados existem.
- **Pendências:** ver "Mapa Real Encontrado".
- **Hipóteses:** módulos vivem em outro repo (se existirem).
- **Próximas auditorias:** localizar repo do sistema.
- **Status:** 🟢 Confirmado (Metas) / ⚫ Não encontrado (demais).

### 20. Dívida técnica
- **Estado atual:** concentrada no protótipo.
- **Evidências:** URL quebrada (`:75`), sem auth (`:78`), XSS (`:143-154`), regra no cliente, zero testes/logs.
- **Existe:** dívida de segurança, de qualidade e de arquitetura no `index.html`.
- **Não existe:** dívida em sistema maior (porque o sistema maior não está aqui).
- **Riscos:** P0 (URL/auth/endpoint), P1 (XSS), P2/P3 (validação, CDN, testes).
- **Pendências:** decidir se corrige o protótipo ou o substitui.
- **Hipóteses:** o protótipo será substituído pelo sistema real.
- **Próximas auditorias:** ver `REPO_AUDIT.md` (classificação P0–P3 detalhada).
- **Status:** 🔴 Problema encontrado.

---

## Matriz de Maturidade (0–100)

Notas baseadas no que existe **neste repositório**, não na ambição do projeto.

| Área | Nota | Justificativa |
|---|---:|---|
| Repositório | 25 | Existe e está versionado, mas é só protótipo + docs; sem estrutura de sistema. |
| Banco | 5 | Nenhum banco relacional; persistência externa em planilha. |
| Backend | 5 | Sem backend próprio; lógica em Apps Script (caixa-preta, externo). |
| Frontend | 20 | Funciona como protótipo, mas monolítico, sem framework e com XSS. |
| Segurança | 5 | Sem auth; `?admin=true`; endpoint exposto; XSS. |
| Infra | 0 | Nada descrito/versionado. |
| APIs | 10 | Contrato implícito com Apps Script, não documentado. |
| Integrações | 15 | Uma integração real (Google), sem padrão; sem N8N/WhatsApp. |
| IA | 0 | Ausente no produto. |
| Documentação | 45 | Engenharia bem documentada (recente); produto sem doc. |
| Testes | 0 | Inexistentes. |
| Observabilidade | 0 | Inexistente. |
| **Média geral** | **~11** | Estágio inicial: protótipo + fundação de engenharia. |

> A nota relativamente alta de Documentação reflete a fundação `/architecture` criada nesta iniciativa — não documentação do sistema operacional real (que não existe aqui).

---

## Mapa Real Encontrado

### Módulos confirmados (existem no repo)
- **Metas** — `index.html`. CRUD de metas via Apps Script/Sheets. 🟢

### Módulos não encontrados (citados e ausentes no repo)
- Core/OS, Usuários, Permissões, Empresas, Produtos, Estoque, Financeiro, Fiscal, RH, Delivery, Garçom, Caixa, Painel Gerencial. ⚫

### Módulos citados mas inexistentes no repositório atual
- IA, WhatsApp, N8N, Integrações (genéricas), Observabilidade/Vigia, Espelho Vivo (banco). ⚫

### Módulos que parecem externos
- **Google Apps Script + Google Sheets** — atua como backend/banco do protótipo; código **não** está neste repo. 🔵
- **Chart.js** — biblioteca de terceiros via CDN. 🟢

### Módulos que precisam de outro repositório
- Todo o "LenhadorOS" (backend próprio, PostgreSQL, módulos de gestão) **provavelmente vive — ou viverá — em outro repositório**. Precisa ser localizado ou criado. 🔵

---

## Inventário Executivo

### Resumo da situação atual
O repositório `metas-lenhador` é um **protótipo de página única para gestão de metas** (vanilla JS + Google Sheets via Apps Script), acrescido de uma **fundação documental de engenharia** (`/architecture`) criada nesta iniciativa. **O "LenhadorOS" como ERP/plataforma/ecossistema não existe aqui.** A descoberta mais importante é essa lacuna entre a ambição documentada e a realidade do código.

### Principais riscos
1. **P0 — Confusão de escopo:** tratar este repo como o sistema completo.
2. **P0 — Segurança do protótipo:** `?admin=true` sem auth; endpoint de escrita público; URL quebrada (`https://https://`) que provavelmente impede o app de funcionar.
3. **P1 — XSS** via `innerHTML` com dados não escapados.
4. **P1 — Caixa-preta externa:** lógica e dados no Apps Script/Sheets, fora de controle de versão.

### Principais oportunidades
1. **Começar limpo:** sem sistema legado pesado, o LenhadorOS pode nascer já sob o LAM e o Espelho Vivo.
2. **Fundação pronta:** `/architecture` já oferece método, governança e padrões iniciais.
3. **Caso de teste real:** o módulo Metas é um piloto pequeno para validar o LAM ponta a ponta.

### Próximos passos obrigatórios
1. **Decidir o repositório do sistema real** (ADR-0002): este repo evolui ou nasce um novo.
2. **Decidir a stack-alvo** (ADR-0003): backend, frontend, ORM, banco.
3. **Auditar o Apps Script** (fora do repo) para entender o contrato e os dados reais.
4. **Tratar os P0 do protótipo** caso ele continue em uso enquanto o sistema real não existe.

### Decisões pendentes
- O LenhadorOS nasce **aqui** ou em **repositório próprio**?
- O protótipo de Metas será **corrigido**, **migrado** ou **descartado**?
- Onde vivem (ou viverão) Espelho Vivo, Vigia, N8N e WhatsApp?

---

## Próximas auditorias necessárias

| Auditoria | Objetivo | Prioridade |
|---|---|---|
| Localização do sistema real | Achar/criar o repo do LenhadorOS de fato | Alta |
| Apps Script / Sheets | Contrato de API e "schema" real da planilha | Alta |
| Segurança do protótipo | Detalhar e propor correção dos P0/P1 | Alta |
| Infra externa | Confirmar hospedagem, N8N, WhatsApp (se existirem) | Média |

---

*Discovery não-destrutivo concluído. Nenhum código funcional, banco ou migration foi tocado. Realidade > expectativa.*
