# LAM — LenhadorOS Architecture Method

> A metodologia oficial de desenvolvimento do LenhadorOS. Todo módulo, recurso ou alteração estrutural segue este fluxo. Pular etapas não é permitido.

## Princípio

O LAM transforma uma ideia em software confiável passando por etapas obrigatórias, cada uma com critérios claros de entrada e saída. O fluxo é linear, mas iterativo: descobertas em uma etapa podem exigir retorno à anterior.

## Fluxo oficial

| # | Etapa | O que entrega | Critério de saída |
|---|---|---|---|
| 1 | **Ideia** | Problema, objetivo e escopo descritos. | Necessidade clara e aprovada por Everton. |
| 2 | **Auditoria** | Estado atual do repo/banco mapeado, riscos identificados. | Relatório de auditoria em `audits/`. |
| 3 | **Documento de arquitetura** | Desenho da solução, domínios, contratos, trade-offs. | Documento revisado + ADRs das decisões. |
| 4 | **Modelagem Schema First** | Modelo de dados definido antes do código. | Schema proposto e validado contra o Espelho Vivo. |
| 5 | **Validação no Espelho Vivo** | Conferência do schema real vs. proposto; drift verificado. | Sem drift inesperado; aprovação para alterações. |
| 6 | **Backend** | Implementação da lógica de domínio e APIs. | Código com validação, autorização e logs. |
| 7 | **Frontend** | Interface consumindo as APIs. | UI sem regra de negócio embutida. |
| 8 | **Testes** | Cobertura de testes da entrega. | Testes passando; cenários críticos cobertos. |
| 9 | **Deploy** | Publicação controlada e reversível. | Deploy versionado e monitorável. |
| 10 | **Documentação Viva** | Wiki, dicionário de dados, ADRs atualizados. | Documentação sincronizada com o entregue. |
| 11 | **Observabilidade pelo Vigia** | Monitoramento de drift, score e conformidade. | Vigia acompanhando o módulo em produção. |

## Regras do método

- **Auditoria sempre primeiro.** Nenhuma alteração estrutural começa sem entender o estado atual.
- **Schema First, sempre.** Backend e frontend dependem de um modelo de dados estável.
- **O Espelho Vivo valida.** O banco real é a referência; o modelo proposto é confrontado com ele.
- **Migrations só com aprovação.** `prisma migrate dev/diff` exige autorização explícita (ver `../03-database/ESPELHO_VIVO.md`).
- **Documentação viva fecha o ciclo.** Sem ela, o módulo não está concluído.
- **O Vigia nunca dorme.** A observabilidade é etapa final e contínua, não opcional.

## Diagrama do fluxo

```
Ideia ─► Auditoria ─► Arquitetura ─► Schema First ─► Validação (Espelho Vivo)
   ─► Backend ─► Frontend ─► Testes ─► Deploy ─► Documentação Viva ─► Vigia
                                                                        │
                              feedback contínuo ◄────────────────────────┘
```
