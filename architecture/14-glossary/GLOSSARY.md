# Glossário — LenhadorOS

> Vocabulário oficial do projeto. Termos definidos aqui têm significado único e canônico em toda a documentação e código. Em caso de ambiguidade, este glossário decide.

## Convenções

- Termos em ordem alfabética.
- Cada termo: definição curta e, quando útil, referência ao documento que o aprofunda.
- Novos termos relevantes devem ser adicionados junto com a entrega que os introduz.

## Termos

### ADR (Architecture Decision Record)
Registro versionado de uma decisão arquitetural, contendo contexto, decisão, alternativas, consequências e status. Ver `../11-adr/`.

### Architecture Book
Biblioteca oficial de padrões e referências de arquitetura do LenhadorOS. Ver `../01-architecture-book/`.

### Catálogo de tabelas
Lista canônica de todas as tabelas do banco, com propósito, domínio e status. Artefato do Espelho Vivo.

### ChatGPT
Papel de co-arquiteto e redator: apoia planejamento, modelagem conceitual e geração de documentos/prompts.

### Claude Code
Papel de executor técnico: audita, implementa, testa e documenta diretamente no repositório, seguindo o LAM.

### Dicionário de dados
Documento que descreve o significado de cada tabela e coluna, regras de negócio e domínios de valores. Artefato do Espelho Vivo.

### Drift
Divergência entre artefatos que deveriam estar sincronizados — tipicamente entre o `schema.prisma` e o banco PostgreSQL real. O banco é sempre a referência.

### ERD (Entity-Relationship Diagram)
Diagrama entidade-relacionamento. No LenhadorOS, ERDs confiáveis são **gerados** a partir do `schema_snapshot.json`, não desenhados à mão.

### Espelho Vivo
Conjunto de artefatos que refletem fielmente o estado do banco PostgreSQL real, considerado a única fonte de verdade do schema. Ver `../03-database/ESPELHO_VIVO.md`.

### Everton
Dono do produto e da visão. Decide prioridades e aprova decisões estruturais (incluindo migrations e mudanças no Espelho Vivo).

### LAM (LenhadorOS Architecture Method)
Metodologia oficial de desenvolvimento: fluxo obrigatório de ideia até observabilidade. Ver `../02-lam/LAM.md`.

### LenhadorOS
Sistema operacional de gestão objeto deste projeto de engenharia.

### Manifesto
Documento com os princípios inegociáveis de engenharia do LenhadorOS. Ver `../00-manifesto/MANIFESTO.md`.

### Migration
Alteração estrutural aplicada ao banco. Operações como `prisma migrate dev/diff` exigem **aprovação explícita** antes de execução.

### Schema First
Princípio de modelar os dados antes de implementar backend e frontend. Etapa 4 do LAM.

### schema_snapshot.json
Snapshot estruturado e versionado do schema real do banco, base para gerar ERDs, catálogo e dicionário de dados.

### score.json
Pontuação de saúde do schema (FKs, nomenclatura, índices, normalização). Artefato do Espelho Vivo.

### Vigia
Papel de observabilidade e auditoria contínua: monitora drift, score, qualidade e conformidade ao longo do tempo.

### Wiki Viva
Documentação que se mantém sincronizada com o código e o banco, em parte gerada automaticamente. Ver `../12-wiki-viva/`.
