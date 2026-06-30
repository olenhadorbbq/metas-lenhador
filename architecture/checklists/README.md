# Checklists de Qualidade

> Checklists objetivos por etapa do LAM. Servem como porta de qualidade: um item não marcado é um bloqueio, não uma sugestão.

## Propósito

Tornar a qualidade verificável e repetível. Cada etapa do LAM (`../02-lam/LAM.md`) tem uma checklist própria que precisa estar satisfeita antes de avançar.

## Checklists previstas

| Checklist | Etapa do LAM | Objetivo |
|---|---|---|
| `auditoria.md` | 2 | Garantir que o estado atual foi mapeado e os riscos catalogados. |
| `arquitetura.md` | 3 | Garantir documento de arquitetura e ADRs das decisões. |
| `schema.md` | 4–5 | Garantir modelagem Schema First validada no Espelho Vivo. |
| `backend.md` | 6 | Validação, autorização, logs e tratamento de erro presentes. |
| `frontend.md` | 7 | UI sem regra de negócio, acessível e sem dados sensíveis expostos. |
| `testes.md` | 8 | Cobertura mínima e cenários críticos testados. |
| `deploy.md` | 9 | Deploy reversível, versionado e monitorável. |
| `documentacao.md` | 10 | Documentação viva sincronizada com a entrega. |
| `seguranca.md` | transversal | Autenticação, autorização, segredos e LGPD. |

## Modelo de item

Cada checklist usa itens objetivos e verificáveis:

```
- [ ] Endpoints exigem autenticação e autorização.
- [ ] Entradas validadas no backend (não só no front).
- [ ] Sem segredos versionados em texto claro.
- [ ] Logs de auditoria para ações sensíveis.
```

## Regra

> Uma entrega só avança de etapa quando a checklist da etapa atual está **100% satisfeita** ou tem exceções aprovadas e registradas (idealmente como ADR).
