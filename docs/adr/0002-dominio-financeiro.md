# ADR-0002: Vocabulário e regras de negócio do domínio financeiro

Data: 2026-10-05

Status: aceito para modelagem tática (agregados/Prisma); revisão e explicação pelo aluno pendentes (análogo a T-002, a abrir).

## Contexto

ADR-0001 cobriu arquitetura técnica (stack, módulos, validação, contratos, testes), mas não decidiu regras de negócio nem vocabulário de domínio — lacuna identificada em 2026-10-04. T-004 nasceu com escopo restrito (User/Transaction/Category), mas na sessão de 2026-10-05 evoluiu para um modelo DDD mais completo, cobrindo recursos financeiros, movimentações, transferências, reservas, obrigações/recorrências, cartão/parcelamento e posição financeira — documentado em `docs/dominio/01` a `07`.

Esse material foi produzido por uma sessão de IA com explicação conceitual acompanhando (o que é DDD, Entidade, Value Object, como as regras se derivam), não escrito primeiro por Ivo. Por isso, antes de aceitar como compreendido (e não apenas copiado), passou por um checkpoint de active recall em 2026-10-05: 7 perguntas cobrindo Entidade x Value Object, saldo atual x disponível após compromissos, transferência com taxa, e recorrência x obrigação x valor esperado/efetivo. As 3 lacunas críticas identificadas na primeira rodada (critério de Entidade x VO, RN-027 sobre taxa de transferência, e a distinção recorrência/obrigação/valor esperado) foram corrigidas e confirmadas em uma segunda rodada. Registro completo em [docs/aprendizado.md](../aprendizado.md).

## Decisão

Adotar os termos e regras registrados em [docs/dominio/02_linguagem_ubiqua.md](../dominio/02_linguagem_ubiqua.md) e [docs/dominio/03_regras_de_negocio.md](../dominio/03_regras_de_negocio.md) como vocabulário de referência para `schema.prisma` e para a API, com os seguintes pontos centrais:

- Distinguir Entidades (`RecursoFinanceiro`, `Movimentação`, `Transferência`) de Value Objects (`Money`, `Moeda`) pelo critério de identidade: Entidade precisa ser rastreada e distinguida ao longo do tempo mesmo com atributos idênticos; Value Object é definido inteiramente pelo valor que carrega, sem histórico próprio (RN implícita na Seção "Identidade" de cada termo).
- Saldo atual, saldo livre e saldo reservado são três números distintos e nunca devem ser confundidos entre si (RN-001, RN-020, RN-021).
- Movimentações com liquidação imediata (PIX/débito) afetam o saldo atual de imediato; compras no crédito geram compromisso futuro sem afetar o saldo atual (RN-012, RN-013).
- "Disponível após compromissos" é um valor derivado (saldo atual menos compromissos do período) e nunca deve ser persistido de um jeito que permita dupla redução na liquidação (RN-014, RN-016).
- Transferências internas geram movimentações de saída/entrada vinculadas ao mesmo evento e não constituem receita nem despesa (RN-025, RN-026); uma taxa vinculada é registrada como despesa separada, mas ligada ao mesmo evento de transferência, independente do dia em que for cobrada (RN-027).
- Recorrência (regra esperada, ex. "todo dia 15") é distinta de Obrigação (ocorrência concreta de um período), que é distinta de Valor Esperado (o combinado/habitual) e Valor Efetivo (o que de fato ocorreu naquele período) (RN-002, RN-003).
- Ajustes (multa, juros, desconto, abatimento) nunca sobrescrevem silenciosamente o valor anterior da obrigação (RN-006).
- Saldo nunca muda sem movimentação ou fato financeiro explicável; divergências exigem reconciliação, não edição direta (RN-022, RN-023).

### Fora desta decisão (permanece em aberto)

As questões abaixo ficam registradas em [docs/dominio/07_decisoes_em_aberto.md](../dominio/07_decisoes_em_aberto.md) e serão resolvidas durante a modelagem tática dos agregados (Semana 2), não antes:
- estrutura definitiva de `Movimentação` — entidade interna de `RecursoFinanceiro` ou agregado próprio (DA-001);
- persistência de saldo — sempre calculado, armazenado, ou armazenado e reconciliado (DA-002);
- `Reserva` e `Categoria` como entidade própria ou Value Object (DA-003, DA-004);
- modelo definitivo de cartão/parcela/fatura (DA-007).

## Alternativas

Modelar direto em cima das tabelas do Prisma sem documento de domínio prévio — rejeitada, pois esse atalho já gerou a lacuna identificada em 2026-10-04 (regras implícitas, risco de retrabalho de migration).

Tratar o domínio como um CRUD plano de User/Transaction/Category, sem Money/Movimentação/Transferência como conceitos próprios — rejeitada. RN-012, RN-013, RN-014, RN-025, RN-026 e RN-027 exigem distinguir fatos financeiros (compra, pagamento, transferência) que um CRUD genérico de "transações" não capturaria sem ambiguidade (ex.: confundir taxa de transferência com despesa comum, ou contar duas vezes o pagamento de um compromisso já considerado).

## Consequências

O `schema.prisma` da Semana 2 deve refletir ao menos `RecursoFinanceiro`, `Movimentação` e `Transferência` como entidades/tabelas distintas, com `Money` modelado como par (valor, moeda) embutido — não como entidade própria. `Categoria`, `Reserva` e o módulo de Cartão ficam para depois (DA-003/004/007), evitando antecipar modelagem sem necessidade concreta (overengineering).

O CRUD em memória de `transactions` da T-001 continua válido como exercício isolado de NestJS — é descartável e não depende deste vocabulário final, como já registrado na sessão de 2026-10-04.

Uma tarefa análoga a T-002 deve ser aberta para Ivo revisar e explicar este ADR com as próprias palavras antes de iniciar o `schema.prisma`, mantendo o mesmo padrão de verificação usado para ADR-0001.

## Referências

- [docs/dominio/01_visao_do_produto.md](../dominio/01_visao_do_produto.md) a [07_decisoes_em_aberto.md](../dominio/07_decisoes_em_aberto.md).
- RN-001 a RN-027 em [docs/dominio/03_regras_de_negocio.md](../dominio/03_regras_de_negocio.md).
- Checkpoint de active recall de 2026-10-05, registrado em [docs/aprendizado.md](../aprendizado.md).
