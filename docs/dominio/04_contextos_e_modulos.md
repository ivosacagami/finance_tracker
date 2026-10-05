# Contextos e Módulos

Este documento registra uma organização inicial do domínio.
Ainda não representa uma definição final de Bounded Contexts.

---

# 1. Recursos Financeiros

Responsável por representar onde o dinheiro do usuário está.

Conceitos associados:
- Recurso Financeiro;
- Saldo;
- Movimentação;
- Reserva;
- Alocação;
- Reconciliação.

Exemplos:

- Conta Nubank;
- Conta Itaú;
- Poupança;
- Dinheiro em espécie.

### Status

Em modelagem.

---

# 2. Transferências

Responsável por representar movimentação de dinheiro entre recursos financeiros próprios.

Conceitos associados:

- Transferência;
- Movimentação de saída;
- Movimentação de entrada;
- Taxa vinculada.

### Status

Em modelagem.

---

# 3. Gastos e Receitas

Responsável por representar fatos financeiros de consumo e entrada externa.

Conceitos associados:

- Compra;
- Gasto;
- Receita;
- Forma de pagamento;
- Categoria.

### Status

Ainda não aprofundado.

---

# 4. Obrigações e Recorrências

Responsável por representar compromissos financeiros conhecidos.

Conceitos associados:
- Recorrência;
- Obrigação;
- Vencimento;
- Pagamento;
- Quitação;
- Ajuste;
- Saldo devido.

### Status

Próximo bloco a ser aprofundado.

---

# 5. Cartões

Responsável pelos fenômenos associados ao crédito.

Conceitos associados:
- Cartão;
- Compra no crédito;
- Parcela;
- Fatura;
- Pagamento da fatura;
- compromisso futuro.

### Status

Parcialmente explorado.

---

# 6. Posição Financeira

Responsável por apresentar visão derivada da situação financeira do usuário.

Possíveis informações:
- saldo atual;
- saldo livre;
- saldo reservado;
- compromissos do período;
- disponível após compromissos;
- compromissos futuros;
- horizonte dos compromissos.

### Observação

Provavelmente representa projeções e consultas derivadas dos outros módulos em vez de possuir fatos financeiros próprios.

### Status

Hipótese forte.

---

# 7. Patrimônio e Investimentos

Responsável futuramente por ativos que não se comportam como simples saldo monetário.

Exemplos:
- criptomoedas;
- ações;
- fundos;
- renda fixa;
- outros investimentos.

Deve distinguir:
- tipo do ativo ou investimento;
- ativo específico;
- quantidade possuída;
- moeda de referência ou moeda em que o valor está alocado;
- valor de aquisição, quando aplicável;
- avaliação monetária atual;
- variação de mercado;
- movimentações reais;
- eventuais taxas ou custos associados.

### Exemplos

Bitcoin:
- tipo: criptomoeda;
- ativo: BTC;
- quantidade: 0,01 BTC;
- moeda de avaliação: BRL;
- valor atual estimado: R$ X.

Ação:
- tipo: ação;
- ativo: PETR4;
- quantidade: 100 unidades;
- moeda de negociação/avaliação: BRL.

Investimento internacional:
- tipo: ação;
- ativo: AAPL;
- quantidade: 5 unidades;
- moeda de negociação: USD;
- avaliação consolidada do usuário: poderá ser apresentada também em BRL, caso exista mecanismo de conversão.

### Observação

A moeda em que o ativo é negociado ou mantido não deve ser confundida com a moeda utilizada para exibir seu valor consolidado ao usuário.

Por exemplo, um ativo pode estar denominado em USD e ainda assim o sistema futuramente apresentar seu valor equivalente em BRL.

A quantidade do ativo também deve permanecer separada de sua avaliação monetária, pois o valor pode variar sem que exista compra, venda ou qualquer outra movimentação real.

### Status

Fora do MVP.

---
# 8. Inteligência Financeira

Responsável futuramente por análise automatizada e Machine Learning sobre os dados financeiros e comportamentais registrados pelo usuário.

Possíveis responsabilidades:
- identificação de padrões de consumo;
- detecção de recorrências;
- estimativas de gastos futuros com base no histórico;
- classificação automática de lançamentos;
- reconhecimento de estabelecimentos ou descrições recorrentes;
- identificação de comportamentos repetitivos nos dados;
- antecipação de eventos recorrentes com base em padrões históricos;
- geração de alertas contextuais baseados em comportamento observado;
- apoio à organização e interpretação do histórico financeiro.

### Exemplos

O sistema pode identificar:

`Nos últimos 6 meses, seus gastos com supermercado ficaram entre R$700 e R$850 por mês.`

Pode também estimar:

`Com base no histórico, o gasto esperado com supermercado para o próximo mês é de aproximadamente R$790.`

E, caso identifique um padrão recorrente:

`Você costuma realizar a compra principal de supermercado por volta do dia 15. Esse período está se aproximando.`

Futuramente, caso o domínio seja expandido para controle doméstico ou lista de compras, esse padrão poderá ser combinado com informações adicionais para alertas como:

`Sua compra principal costuma acontecer em 5 dias e alguns itens recorrentes podem precisar de reposição.`

Esse tipo de alerta deve ser fundamentado em dados ou padrões observados, e não apresentado como fato quando houver apenas estimativa.

## Possível expansão futura — Lista de Compras

Uma futura funcionalidade poderá permitir ao usuário manter uma lista de mercado ou itens domésticos.

Essa funcionalidade poderá se relacionar com:
- compras anteriores;
- frequência de aquisição;
- quantidade ou duração estimada de determinados itens;
- histórico de preços;
- estabelecimento;
- lista planejada para próxima compra.

Com dados suficientes, o sistema poderá futuramente sugerir itens possivelmente próximos da reposição.

Essa expansão ainda não faz parte do domínio financeiro principal e deverá ser modelada separadamente antes de ser implementada.

## Limites

A Inteligência Financeira não deve:
- recomendar investimentos;
- sugerir compra ou venda de ativos;
- indicar como o usuário deve gastar seu dinheiro;
- emitir aconselhamento financeiro;
- definir metas financeiras pelo usuário;
- tomar decisões financeiras em nome do usuário.

A Inteligência Financeira deve trabalhar sobre fatos existentes no domínio e produzir análises descritivas, preditivas ou alertas contextuais, sem alterar o significado básico desses fatos.

### Status

Fora do MVP.