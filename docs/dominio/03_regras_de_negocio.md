# Regras de Negócio

## Convenção

Cada regra possui:
- identificador;
- descrição;
- motivo;
- status.

Os identificadores serão preservados durante a evolução do domínio.

---

## RN-001 — Posição atual e posição após compromissos são diferentes

### Descrição

O saldo atual representa dinheiro existente.
O disponível após compromissos considera obrigações pendentes conhecidas.

### Status

**Validada.**

---

## RN-002 — Recorrência e obrigação são conceitos diferentes

### Descrição

Uma recorrência representa aquilo que normalmente se espera acontecer.
Uma obrigação representa uma ocorrência concreta que precisa ser tratada.

### Status

**Validada.**

---

## RN-003 — Valor esperado e valor efetivo são diferentes

### Descrição

Uma obrigação pode possuir valor esperado diferente de seu valor efetivamente cobrado.

### Status

**Validada.**

---

## RN-004 — Alterar recorrência não deve reescrever o passado

### Descrição

Mudanças realizadas em uma recorrência devem valer de acordo com sua vigência sem alterar ocorrências históricas anteriores.

### Status

Validada conceitualmente.

---

## RN-005 — Pagamento registra fato efetivo

### Descrição

Um pagamento deve registrar pelo menos:
- valor efetivamente pago;
- data do pagamento.

### Status

**Validada.**

---

## RN-006 — Ajustes não devem sobrescrever silenciosamente o valor anterior

### Descrição

Multas, juros, descontos, abatimentos e outros fatores devem ser registrados como ajustes da obrigação.

### Motivo

Preservar a explicação de como o valor devido foi alterado.

### Status

**Validada.**

---

## RN-007 — Compra parcelada preserva valor total e parcelas

### Descrição

Uma compra parcelada deve preservar:
- valor total;
- quantidade de parcelas;
- valor e período das parcelas;
- saldo futuro restante.

### Status

Validada conceitualmente.

---

## RN-008 — Compromissos futuros conhecidos não são previsões

### Descrição

Se o usuário já assumiu determinado compromisso, ele deve ser tratado como compromisso conhecido, mesmo que seu pagamento ocorra no futuro.

### Status

**Validada.**

---

## RN-009 — Compromissos parcelados devem possuir horizonte temporal

### Descrição

O sistema deve conseguir determinar o período da última parcela prevista de um compromisso parcelado.

### Status

**Validada.**

---

## RN-010 — Compromissos do período e compromissos futuros são informações diferentes

### Descrição

O valor comprometido no período atual deve ser distinguido do total ainda devido em períodos futuros.

### Status

**Validada.**

---

## RN-011 — Dados adicionais enriquecem o gasto, mas não são obrigatórios

### Descrição

Um gasto deve poder existir com informações mínimas suficientes.
Dados adicionais podem melhorar relatórios e ML futuro sem serem necessários para o funcionamento financeiro básico.

### Status

**Validada.**

---

## RN-012 — Pagamento imediato reduz saldo atual e disponível

### Descrição

Uma compra com liquidação imediata, como PIX ou débito, reduz imediatamente:
- saldo atual;
- disponível após compromissos.

### Status

Validada conceitualmente.

---

## RN-013 — Compra com obrigação futura não reduz imediatamente o saldo atual

### Descrição

Uma compra no crédito não reduz imediatamente o dinheiro existente, mas gera compromisso financeiro.

### Status

**Validada.**

---

## RN-014 — Liquidação de compromisso não pode gerar dupla redução

### Descrição

Quando uma obrigação já foi considerada no disponível após compromissos, seu pagamento reduz o saldo atual e elimina o compromisso correspondente.

Não deve reduzir novamente o disponível como se fosse uma nova despesa.

### Status

**Validada.**

---

## RN-015 — Períodos diferentes não devem ser misturados

### Descrição

Valores pertencentes a horizontes temporais diferentes não devem ser apresentados ou calculados como se pertencessem à mesma posição financeira.

### Status

**Validada.**

---

## RN-016 — Compromissos futuros totais não são subtraídos integralmente do saldo atual

### Descrição

O total de compromissos futuros assumidos não deve ser utilizado diretamente para calcular o disponível do período atual.

### Status

**Validada.**

---

## RN-017 — Transferências próprias não são receita nem despesa

### Descrição

Transferências entre recursos financeiros pertencentes ao mesmo usuário apenas redistribuem patrimônio.

### Status

**Validada.**

---

## RN-018 — Recursos monetários e ativos avaliados devem ser distinguíveis

### Descrição

Recursos cujo saldo é expresso diretamente em moeda devem ser diferenciados de ativos cuja quantidade e avaliação monetária são conceitos distintos.

### Status

Hipótese para arquitetura futura.

---

## RN-019 — Movimentações devem identificar recurso afetado

### Descrição

Toda movimentação que altera dinheiro disponível deve identificar qual recurso financeiro foi afetado.

### Status

**Validada.**

---

## RN-020 — Reservar dinheiro não altera patrimônio

### Descrição

Alocar dinheiro para determinada finalidade não altera o saldo total do recurso.

Altera apenas sua distribuição entre livre e reservado.

### Status

**Validada.**

---

## RN-021 — Reserva possui finalidade

### Descrição

Valores reservados devem poder ser associados a uma finalidade identificável pelo usuário.

### Status

**Validada.**

---

## RN-022 — Saldo não muda sem movimentação explicável

### Descrição

O saldo de um recurso financeiro não pode ser alterado silenciosamente.
Uma mudança precisa ser explicada por uma movimentação ou fato financeiro.

### Status

**Validada.**

---

## RN-023 — Divergência de saldo deve ser reconciliada

### Descrição

Diferenças entre saldo calculado e saldo observado externamente devem ser explicadas por meio de movimentação ou reconciliação.

### Status

Validada conceitualmente.

---

## RN-024 — Erro de cadastro e fato financeiro real são diferentes

### Descrição

Erros de cadastro podem ser corrigidos.

Eventos financeiros efetivamente ocorridos não devem ser reescritos para representar eventos posteriores.

### Exemplo

Erro:

`Compra digitada como R$250 quando era R$25`

Pode ser corrigida.

Evento real:

`Compra R$250 + estorno R$225`

Deve preservar os dois fatos.

### Status

**Validada.**

---

## RN-025 — Transferência interna produz duas movimentações vinculadas

### Descrição

Uma transferência entre recursos próprios deve gerar:

- saída na origem;
- entrada no destino.

As duas movimentações devem permanecer vinculadas à mesma transferência.

### Status

**Validada.**

---

## RN-026 — Movimentações de transferência não constituem renda ou gasto

### Descrição

A entrada no destino não é receita.

A saída na origem não é despesa.

### Status

**Validada.**

---

## RN-027 — Taxa associada à transferência deve ser registrada separadamente e permanecer vinculada ao mesmo evento

### Descrição

Toda taxa gerada em decorrência de uma transferência deve ser registrada como gasto separado do valor efetivamente transferido.

Apesar de possuir natureza financeira própria, a taxa deve permanecer vinculada ao mesmo evento de transferência que a originou.

### Motivo

Preservar rastreabilidade e permitir distinguir:

- valor efetivamente transferido;
- custo da transferência;
- impacto total no recurso de origem.

### Exemplo

Transferência T-001:

- saída Itaú: `-R$500` — natureza: transferência;
- entrada Nubank: `+R$500` — natureza: transferência;
- taxa Itaú: `-R$10` — natureza: taxa/despesa.

As três movimentações pertencem ao mesmo evento `T-001`, mas possuem efeitos financeiros distintos.

### Status

**Validada.**