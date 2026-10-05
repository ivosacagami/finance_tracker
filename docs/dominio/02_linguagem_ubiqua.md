# Linguagem Ubíqua

Este documento registra os termos usados pelo domínio.
Os nomes ainda podem evoluir conforme o domínio for aprofundado.

---

# Recurso Financeiro

## O que é?
Local ou instrumento onde o usuário mantém valor monetário.

## Por que existe?
Permite identificar onde o dinheiro está e controlar separadamente o saldo de cada origem.

## Identidade
**Entidade.**
Cada recurso possui identidade própria mesmo que tenha o mesmo nome, tipo ou saldo de outro recurso.

### Exemplos
- Conta Itaú
- Conta Nubank
- Poupança
- Dinheiro em espécie

### Relações
Pode:
- possuir saldo;
- receber movimentações;
- realizar movimentações;
- participar de transferências;
- possuir valores reservados.

## Status
**Validado.**

---

# Money / Valor Monetário

## O que é?

Representação de determinado valor em determinada moeda.
Exemplo:
`R$ 250,00 BRL`

## Identidade

**Value Object.**
Sua identidade não importa. O que importa é a combinação de valor e moeda.

## Regra conceitual

Money deve preferencialmente ser imutável.
`Money(250, BRL)` e `Money(300, BRL)` representam valores diferentes.

## Status
**Validado.**

---

# Moeda

## O que é?

Unidade monetária associada a um valor financeiro.

Exemplos:
- BRL
- USD
- EUR

## Identidade
Candidata a **Value Object**.

## Status

**Hipótese forte.**

---

# Movimentação

## O que é?

Alteração financeira ocorrida em um recurso financeiro.
Uma movimentação pode representar entrada ou saída.

## Exemplos
- entrada de salário;
- saída por PIX;
- entrada de transferência;
- saída de transferência;
- taxa bancária;
- estorno;
- pagamento.

## Identidade

**Entidade candidata.**

Uma movimentação específica precisa ser distinguível de outra, mesmo quando possuem valores semelhantes.

## Relações

Relaciona-se com:
- Recurso Financeiro;
- Transferência;
- Pagamento;
- Receita;
- Despesa;
- ajustes.

## Status

**Hipótese forte.**

---

# Transferência

## O que é?

Fato financeiro que representa movimentação de valor entre dois recursos financeiros pertencentes ao usuário.

## Característica

Uma transferência interna gera:
- uma movimentação de saída;
- uma movimentação de entrada.

Essas movimentações permanecem vinculadas ao mesmo fato de transferência.

## Identidade

**Entidade.**

Cada transferência possui identidade própria.

## Observação

Transferência não representa receita nem despesa.

## Status

**Validado.**

---

# Reserva

## O que é?

Parte de um recurso financeiro que o usuário decidiu separar para determinada finalidade.

## Exemplos

- Reserva de emergência;
- Viagem;
- Curso;
- Compra futura.

## Característica

Uma reserva não remove dinheiro do patrimônio.

Ela altera apenas a destinação daquele valor.

## Identidade

Ainda em análise.

Pode tornar-se entidade caso precise:

- identidade própria;
- histórico;
- renomeação;
- movimentações;
- acompanhamento de finalidade.

## Status

**Revisar durante modelagem do agregado RecursoFinanceiro.**

---

# Saldo Atual

## O que é?

Quantidade de dinheiro efetivamente existente no recurso ou conjunto de recursos financeiros naquele momento.

## Característica

Deve ser explicável pelas movimentações registradas.

## Status

**Validado.**

---

# Saldo Livre

## O que é?

Parte do saldo que não está reservada ou comprometida conforme as regras do domínio.

## Status

**Hipótese validada conceitualmente.**

---

# Saldo Reservado

## O que é?

Parte do patrimônio que continua pertencendo ao usuário, mas foi destinada a determinada finalidade.

## Status

**Validado conceitualmente.**

---

# Compromisso Financeiro

## O que é?

Condição financeira em que determinado valor já foi assumido pelo usuário
e ainda produzirá desembolso futuro.

Pode resultar de:
- obrigação pendente;
- parcela futura;
- fatura;
- outro débito conhecido.

## Observação

Compromisso não representa necessariamente uma entidade própria.
Pode ser uma visão derivada de outros conceitos do domínio.

Compromisso não é previsão.

## Status

Hipótese conceitual — estrutura ainda não definida.

---

# Obrigação

## O que é?

Ocorrência financeira concreta que precisa ser liquidada.

## Exemplo

A recorrência:

`Internet — aproximadamente R$120 todo dia 15`

pode originar:

`Internet — outubro — R$127,43 — vencimento 15/10`

## Status

**Hipótese forte.**

---

# Recorrência

## O que é?

Regra que representa um evento financeiro esperado de forma repetida.

## Exemplo

Internet:

- recorrência mensal;
- vencimento no dia 15;
- valor esperado de R$120.

## Observação

Recorrência não é a obrigação concreta.

## Status

**Validado conceitualmente.**

---

# Valor Esperado

## O que é?

Valor conhecido ou estimado para uma obrigação antes do valor efetivo ser conhecido.

## Exemplo

Internet esperada:

`R$120`

Conta efetiva:

`R$127,43`

## Status

**Validado.**

---

# Valor Efetivo

## O que é?

Valor real conhecido da obrigação ou pagamento.

## Status

**Validado.**

---

# Pagamento

## O que é?

Fato financeiro que representa valor efetivamente pago em determinada data.

## Características

Deve registrar:

- valor pago;
- data;
- obrigação relacionada quando aplicável;
- recurso financeiro utilizado.

## Observação

Pagamento é um fato.

Quitação é uma condição da obrigação.

## Status

**Validado conceitualmente.**

---

# Quitação

## O que é?

Estado em que uma obrigação não possui saldo devido restante conforme as regras do domínio.

## Status

**Hipótese forte.**

---

# Ajuste de Obrigação

## O que é?

Alteração explícita do valor devido decorrente de evento externo.

## Exemplos positivos

- multa;
- juros;
- acréscimo.

## Exemplos negativos

- desconto;
- abatimento.

## Característica

Ajustes devem preservar o valor previamente conhecido em vez de simplesmente sobrescrevê-lo.

## Status

**Validado conceitualmente.**

---

# Compra

## O que é?

Fato que representa aquisição ou consumo realizado pelo usuário.

## Dados mínimos

- valor;
- descrição/motivo/local/intenção;
- forma de pagamento.

## Status

**Hipótese forte.**

---

# Parcela

## O que é?

Parte de um compromisso financeiro distribuído ao longo do tempo.

## Exemplo

Compra:

`R$2.400 em 12 × R$200`

Cada R$200 representa uma parcela associada a determinado período.

## Status

**Hipótese forte.**

---

# Fatura

## O que é?

Conjunto financeiro associado ao cartão de crédito que reúne valores devidos em determinado ciclo.

## Observação

Compra, parcela, fatura e pagamento da fatura representam conceitos diferentes.

## Status

**Ainda em modelagem.**

---

# Gasto

## O que é?

Valor associado a consumo ou despesa realizada pelo usuário.

## Observação

Gasto não é necessariamente saída imediata de dinheiro.

Uma compra no cartão gera gasto sem reduzir imediatamente o saldo bancário.

## Status

**Validado conceitualmente.**

---

# Receita

## O que é?

Entrada financeira externa que aumenta o patrimônio do usuário.

## Observação

Transferência recebida entre recursos próprios não constitui receita.

## Status

**Hipótese forte.**

---

# Forma de Pagamento

## O que é?

Mecanismo pelo qual um gasto ou obrigação é liquidado ou assume compromisso futuro.

## Exemplos

- PIX;
- débito;
- dinheiro;
- cartão de crédito;
- boleto.

## Observação

Diferentes formas de pagamento podem ter efeitos financeiros diferentes.

## Status

**Em evolução.**

---

# Posição Financeira

## O que é?

Visão derivada que resume a situação financeira do usuário em determinado contexto temporal.

Pode envolver:

- saldo atual;
- valores reservados;
- compromissos do período;
- disponível após compromissos;
- compromissos futuros.

## Identidade

Provavelmente não é entidade persistente.

Pode ser projeção ou cálculo derivado.

## Status

**Hipótese forte.**

---

# Reconciliação

## O que é?

Processo de identificar e explicar divergências entre o saldo calculado pelo sistema e o saldo observado externamente.

## Princípio

Uma divergência não deve simplesmente desaparecer por edição de saldo.

Deve existir movimentação ou fato que explique a diferença.

## Status

**Hipótese forte.**