# Casos de Uso

Este documento registra casos de uso conhecidos até o momento.

Eles representam intenções da aplicação e não substituem regras de domínio.

---

# CU-001 — Cadastrar recurso financeiro

## Objetivo

Permitir ao usuário registrar onde mantém dinheiro.

## Exemplos

- Conta Nubank;
- Conta Itaú;
- Poupança;
- Dinheiro.

## Status

Hipótese forte.

---

# CU-002 — Informar saldo inicial

## Objetivo

Definir o ponto inicial conhecido de determinado recurso financeiro.

## Observação

Depois da inicialização, alterações normais de saldo devem ser explicadas por movimentações.

## Status

Hipótese forte.

---

# CU-003 — Registrar entrada

## Objetivo

Registrar entrada financeira em determinado recurso.

## Exemplos

- salário;
- recebimento externo;
- outra receita.

## Status

Em evolução.

---

# CU-004 — Registrar gasto com liquidação imediata

## Objetivo

Registrar gasto que reduz imediatamente determinado recurso financeiro.

## Exemplos

- PIX;
- débito;
- dinheiro.

## Dados mínimos

- valor;
- descrição;
- forma de pagamento;
- recurso financeiro afetado.

## Status

Hipótese forte.

---

# CU-005 — Registrar compra no crédito

## Objetivo

Registrar gasto sem saída imediata de dinheiro e criar compromisso futuro correspondente.

## Status

Ainda em modelagem.

---

# CU-006 — Transferir dinheiro entre recursos

## Objetivo

Mover determinado valor entre dois recursos do usuário.

## Fluxo conceitual

1. identificar recurso de origem;
2. identificar recurso de destino;
3. validar transferência;
4. criar movimentação de saída;
5. criar movimentação de entrada;
6. vincular as movimentações;
7. persistir de forma consistente.

## Status

Hipótese forte.

---

# CU-007 — Registrar taxa de transferência

## Objetivo

Registrar gasto adicional associado a uma transferência.

## Observação

Taxa não deve alterar o valor transferido.

## Status

Validado conceitualmente.

---

# CU-008 — Criar reserva

## Objetivo

Destinar parte do saldo de um recurso para determinada finalidade.

## Exemplo

`R$1.000 → Reserva de emergência`

## Status

Em modelagem.

---

# CU-009 — Liberar valor reservado

## Objetivo

Retornar determinado valor reservado para condição livre ou utilizá-lo conforme regras futuras.

## Status

Em modelagem.

---

# CU-010 — Reconciliar saldo

## Objetivo

Explicar divergência entre saldo calculado e saldo observado externamente.

## Princípio

Não sobrescrever silenciosamente o saldo.

## Status

Hipótese forte.

---

# CU-011 — Cadastrar conta recorrente

## Objetivo

Registrar compromisso que normalmente ocorre de forma repetida.

## Dados conhecidos

- descrição;
- periodicidade;
- vencimento;
- valor esperado.

## Status

Ainda será aprofundado.

---

# CU-012 — Registrar pagamento de obrigação

## Objetivo

Registrar valor efetivamente pago e sua data.

## Status

Ainda será aprofundado.

---

# CU-013 — Registrar ajuste de obrigação

## Objetivo

Registrar acréscimos ou reduções no valor devido.

## Exemplos

- multa;
- juros;
- desconto;
- abatimento.

## Status

Ainda será aprofundado.

---

# CU-014 — Consultar posição financeira

## Objetivo

Exibir uma visão consolidada da situação financeira.

## Informações candidatas

- saldo atual;
- saldo reservado;
- saldo livre;
- compromissos do período;
- disponível após compromissos;
- compromissos futuros.

## Status

Hipótese forte.