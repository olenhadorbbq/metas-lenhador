# Prompts Oficiais

> Biblioteca de prompts versionados do LenhadorOS. Prompts usados para auditoria, arquitetura, geração de código, documentação e operação de IA ficam aqui — versionados, revisáveis e reutilizáveis.

## Propósito

Garantir que as interações com agentes de IA (ChatGPT, Claude Code, Vigia) sejam consistentes, auditáveis e melhoráveis ao longo do tempo. Um prompt oficial é tratado como artefato de engenharia: tem dono, versão e histórico.

## Convenções

- **Nomenclatura:** `NN-nome-do-prompt.md` (numeração opcional para ordenar fluxos).
- **Conteúdo mínimo de cada prompt:** objetivo, contexto esperado, regras/restrições, formato de saída e exemplo de uso.
- **Regras destrutivas explícitas.** Prompts que tocam banco ou arquivos devem reafirmar as regras de segurança (sem migrations/destrutivos sem aprovação).
- **Versionável.** Toda alteração relevante de um prompt é commitada com justificativa.

## Categorias sugeridas

| Categoria | Uso |
|---|---|
| Auditoria | Varredura de repositório, banco e módulos. |
| Arquitetura | Desenho de solução, ADRs, modelagem. |
| Implementação | Geração de backend, frontend e testes. |
| Documentação | Wiki viva, dicionário de dados, glossário. |
| Observabilidade | Vigia, drift, score e alertas. |

## Índice

| Arquivo | Propósito | Status |
|---|---|---|
| _(a popular)_ | — | — |
