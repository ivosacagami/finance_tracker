# Decisões em Aberto

Este documento registra pontos ainda não fechados.

Uma questão não deve ser tratada como regra definitiva enquanto permanecer neste arquivo.

---

# DA-001 — Estrutura definitiva de Movimentação

## Questão

Movimentação será entidade interna de RecursoFinanceiro, agregado próprio ou outro modelo?

## Conhecido

- possui identidade;
- afeta um recurso específico;
- precisa permanecer rastreável;
- pode se relacionar a transferências, pagamentos e outros fatos.

## Status

Revisar.

---

# DA-002 — Persistência de saldo

## Questão

Saldo será:

- sempre calculado;
- armazenado;
- armazenado e reconciliado;
- materializado para performance.

## Regra já conhecida

Saldo não pode mudar sem fato financeiro explicável.

## Status

Decisão técnica futura.

---

# DA-003 — Reserva como entidade

## Questão

Reserva precisa de identidade própria?

## Possíveis necessidades

- renomear;
- acompanhar histórico;
- aumentar e diminuir;
- registrar objetivo;
- possuir datas;
- analisar retirada de reservas.

## Status

Em análise.

---

# DA-004 — Categoria como entidade ou Value Object

## Questão

Categoria será apenas um valor ou possuirá identidade e ciclo de vida?

## Indício atual

Se o usuário puder:

- criar;
- renomear;
- reorganizar;
- manter histórico;

provavelmente será entidade.

## Status

Em análise.

---

# DA-005 — Pagamento parcial

## Questão

Como representar obrigações parcialmente pagas?

## Conhecido

O sistema deve poder representar:

- valor original;
- pagamentos realizados;
- saldo restante.

O sistema não deve inventar automaticamente:

- juros;
- multa;
- renegociação.

Essas alterações poderão ser informadas por ajustes.

## Status

Aprofundar em Obrigações.

---

# DA-006 — Pagamento superior ao saldo devido

## Questão

Como representar pagamento maior que o valor conhecido da obrigação?

Possibilidades:

- erro de cadastro;
- crédito futuro;
- valor efetivo da obrigação diferente;
- outro evento.

## Status

Aprofundar.

---

# DA-007 — Modelo definitivo de Cartão

## Questão

Como organizar:

- cartão;
- compra;
- parcela;
- fatura;
- pagamento da fatura.

## Hipótese atual

Compra → parcelas → faturas → pagamento.

Ainda não validada como estrutura definitiva.

## Status

Aprofundar.

---

# DA-008 — Criptomoedas e investimentos

## Decisão provisória

Fora do MVP.

## Diretriz

A arquitetura não deve assumir que todo patrimônio é simples saldo em moeda.

## Status

Adiado.

---

# DA-009 — Integração bancária

## Decisão provisória

Fora do MVP.

## Diretriz

O domínio não deve depender da origem manual dos dados.

## Status

Adiado.

---

# DA-010 — Machine Learning

## Decisão provisória

Fora do MVP.

## Diretriz

O sistema deve registrar fatos financeiros suficientemente estruturados para permitir análise futura.

## Status

Adiado.

---

# DA-011 — Data financeira

## Questão

Será necessário um Value Object específico para representar data financeira, período ou competência?

## Status

Ainda não decidido.

---

# DA-012 — Moedas diferentes em transferência

## Questão

Como tratar transferência entre recursos em moedas diferentes?

Isso poderá envolver:

- câmbio;
- cotação;
- taxa;
- diferença de valor.

## Status

Fora do escopo atual.

---

# DA-013 — Recursos encerrados

## Questão

O que acontece quando uma conta deixa de ser utilizada?

Provavelmente deverá ser encerrada/inativada sem apagar histórico.

## Status

Revisar futuramente.

---

# DA-014 — Bounded Contexts definitivos

## Questão

Os módulos atualmente identificados realmente formarão Bounded Contexts separados?

## Situação atual

A divisão é conceitual e ainda não deve ser tratada como arquitetura definitiva.

## Status

Revisar após amadurecimento do domínio.