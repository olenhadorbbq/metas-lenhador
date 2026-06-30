# Manifesto LenhadorOS

> Os princípios inegociáveis de engenharia do LenhadorOS. Quando houver dúvida, este documento decide.

## Visão

Construir um sistema operacional de gestão (LenhadorOS) confiável, auditável e evolutivo, no qual cada parte é conhecida, documentada e governada — sem caixas-pretas, sem conhecimento implícito e sem decisões não rastreáveis.

## Princípios

1. **O banco vivo é a fonte de verdade.** O PostgreSQL real define o schema. Prisma, ERDs e documentação são projeções derivadas — nunca o contrário.
2. **Auditar antes de alterar.** Toda mudança estrutural começa por uma auditoria do estado atual.
3. **Schema First.** A modelagem de dados precede backend e frontend.
4. **Documentação é parte da entrega.** Um módulo sem documentação viva não está pronto.
5. **Decisões viram ADR.** Escolhas arquiteturais relevantes são registradas, com contexto, alternativas e consequências.
6. **Segurança não é opcional.** Autenticação, autorização, validação e auditoria fazem parte do mínimo de qualquer módulo.
7. **Nada destrutivo sem aprovação explícita.** Migrations, `prisma migrate dev/diff` e operações irreversíveis exigem autorização clara.
8. **Tudo versionado no Git.** Se não está no repositório, não é oficial.
9. **Observabilidade contínua.** O Vigia acompanha drift, score e conformidade ao longo do tempo.
10. **Evolução incremental.** Entregas pequenas, testadas e reversíveis sobre grandes saltos arriscados.

## Antipadrões a evitar

- Confiar em diagramas manuais como fonte de verdade.
- Misturar regra de negócio com UI.
- Endpoints sem autenticação ou autorização.
- Credenciais e segredos versionados em texto claro.
- Código sem testes, sem validação e sem logs.
- Módulos duplicados ou nomes inconsistentes.

## Compromisso

Toda pessoa ou agente que contribui com o LenhadorOS se compromete a seguir este manifesto e a metodologia LAM (`../02-lam/LAM.md`). Divergências devem ser propostas como ADR, discutidas e aprovadas — nunca aplicadas silenciosamente.
