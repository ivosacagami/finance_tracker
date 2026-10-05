# Agregados

Este documento registra hipóteses atuais de modelagem DDD tática.

As decisões ainda poderão ser revistas.

---

# Agregado RecursoFinanceiro

## Aggregate Root

`RecursoFinanceiro`

## Objetivo

Proteger a consistência de um recurso onde o usuário mantém dinheiro.

## Entidades candidatas

- RecursoFinanceiro
- Movimentação
- Reserva — em avaliação

## Value Objects candidatos

- Money
- Moeda
- talvez DataFinanceira
- talvez Descrição

## Invariantes conhecidas

- o recurso deve possuir identidade;
- o saldo não deve mudar sem movimentação correspondente;
- movimentações devem indicar o recurso afetado;
- valores reservados continuam compondo o patrimônio;
- reservas não devem criar dinheiro;
- alocações não constituem receita ou despesa;
- operações devem respeitar a moeda do recurso;
- regras futuras poderão impedir movimentações em recursos encerrados.

## Operações candidatas

- registrar entrada;
- registrar saída;
- reservar valor;
- liberar reserva;
- reconciliar diferença;
- consultar saldo;
- consultar saldo livre;
- consultar saldo reservado.

## Status

**Hipótese forte / em modelagem.**

---

# Agregado Transferencia

## Aggregate Root

`Transferencia`

## Objetivo

Representar e proteger o fato de movimentar valor entre dois recursos próprios.

## Entidades candidatas

- Transferencia
- Movimentação de saída
- Movimentação de entrada

## Value Objects

- Money
- Moeda
- DataFinanceira — candidata

## Invariantes conhecidas

- origem e destino devem ser diferentes;
- valor deve ser positivo;
- saída e entrada devem representar o mesmo valor transferido;
- as duas movimentações devem permanecer vinculadas;
- transferência interna não constitui receita;
- transferência interna não constitui despesa;
- eventual taxa deve ser registrada separadamente;
- eventual taxa pode manter vínculo com a transferência;
- operação não deve permanecer pela metade.

## Exemplo

Transferência:

`R$500 Itaú → Nubank`

Movimentações:

- Itaú: `-R$500`
- Nubank: `+R$500`

Taxa:

- Itaú: `-R$10` — despesa vinculada

## Status

**Hipótese forte.**

---

# Agregados futuros candidatos

Ainda não formalizados:

- Obrigação;
- Recorrência;
- Fatura;
- Cartão;
- Compra;
- Reserva, caso se demonstre independente;
- Categoria, caso possua identidade e ciclo de vida próprio.

Esses agregados só devem ser definidos após aprofundamento do respectivo domínio.