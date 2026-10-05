# Visão do Produto # 

## 1. Objetivo

O sistema tem como objetivo permitir que uma pessoa física acompanhe e compreenda sua vida financeira de maneira simples, sem exigir conhecimento contábil ou financeiro avançado.

O foco inicial é permitir que o usuário saiba:
- quanto dinheiro possui atualmente;
- onde esse dinheiro está;
- quais valores estão livres;
- quais valores estão reservados;
- quais compromissos financeiros ainda precisa pagar;
- quais compromissos já foram pagos;
- quando cada obrigação vence;
- quanto do dinheiro atual permanece disponível após os compromissos conhecidos;
- quais compromissos futuros já foram assumidos.

***O sistema deve priorizar simplicidade de uso sem sacrificar a qualidade estrutural dos dados.***


## 2. Público inicial

O produto será desenvolvido inicialmente para pessoa física e uso financeiro pessoal.

Nesta fase, não tem como objetivo representar:
- contabilidade empresarial;
- ERP financeiro;
- escrituração contábil;
- gestão fiscal;
- folha de pagamento;
- contabilidade bancária.

## 3. Princípio central

O sistema deve distinguir claramente:
- Saldo atual;
- Quanto dinheiro existe efetivamente nos recursos financeiros do usuário neste momento;
- Compromissos do período;
- Valores conhecidos que precisam ser pagos dentro do período analisado;
Disponível após compromissos;
- Quanto do saldo atual permanece disponível depois de considerados os compromissos - conhecidos daquele período;
- Compromissos futuros assumidos;
- Valores que já foram contratados ou assumidos para períodos posteriores, como compras parceladas;
- Compromissos futuros não devem ser integralmente subtraídos do saldo presente como se pertencessem ao mesmo período.


## 4. Entrada de dados

No MVP, a principal forma de entrada será manual.
O usuário poderá cadastrar informações como:
- saldo inicial;
- salário;
- contas recorrentes;
- compras;
- pagamentos;
- transferências;
- despesas inesperadas;
- reservas;
- compromissos parcelados.

O sistema deve funcionar corretamente mesmo com dados mínimos.

***Informações adicionais podem enriquecer os registros, mas sua ausência não deve impedir o funcionamento financeiro básico.***


## 5. Dados mínimos para registro de gasto

Um gasto deve poder ser registrado inicialmente com:
- valor;
- descrição, motivo, local ou intenção;
- forma de pagamento.

Outras informações poderão ser opcionais, como:
- categoria;
- estabelecimento;
- recurso financeiro específico;
- cartão utilizado;
- parcelamento;
- observação;
- finalidade detalhada.

## 6. Futuro: inteligência e Machine Learning

Existe intenção futura de adicionar mecanismos de análise e Machine Learning.

Possíveis usos:
- identificação de padrões de gastos;
- estimativa de gastos recorrentes;
- previsão de gastos de supermercado;
- identificação de comportamentos financeiros;
- enriquecimento automático de categorias;
- geração de insights.

*O ML não faz parte do MVP.*

O domínio financeiro deve ser construído de maneira que produza dados históricos confiáveis para permitir essa evolução no futuro.

## 7. Futuro: integração bancária

Existe intenção futura de permitir importação automática de informações financeiras por integrações bancárias ou mecanismos equivalentes.

Essa integração não faz parte do MVP.
O domínio não deve depender da origem do dado.

Um fato financeiro poderá futuramente ter origem:
- manual;
- importação bancária;
- integração externa;
- outro mecanismo.

*A origem do dado não deve alterar seu significado financeiro.*


## 8. Escopo inicial

O MVP deve se concentrar em:
recursos financeiros;
- saldos;
- movimentações;
- transferências;
- reservas;
- gastos;
- receitas;
- obrigações;
- contas recorrentes;
- pagamentos;
- cartão de crédito;
- parcelamentos;
- compromissos atuais e futuros;
- posição financeira.

9. Fora do escopo inicial

Inicialmente ficam fora do MVP:
Machine Learning;
- previsões automáticas avançadas;
- integração bancária automática;
- cálculo automático de juros contratuais;
- cálculo automático de multas;
- investimentos complexos;
- cotação automática de ativos;
- criptomoedas com valorização automática;
- ações;
- fundos;
- apuração de ganho de capital.

*A arquitetura não deve impedir suporte futuro a essas funcionalidades.*

## 10. Princípios de produto

O sistema deve seguir os seguintes princípios:

- O usuário deve conseguir registrar dados de forma simples.
- Dados enriquecidos melhoram análise, mas não são obrigatórios para o funcionamento básico.
- O sistema não deve inventar fatos financeiros desconhecidos.
- Valores derivados devem ser distinguíveis de fatos informados.
- O histórico financeiro real deve ser preservado.
- Erros de cadastro podem ser corrigidos.

Eventos financeiros efetivamente ocorridos não devem ser apagados para representar acontecimentos posteriores.

**O saldo deve ser explicável por movimentações.**

Informações de períodos diferentes não devem ser combinadas de forma enganosa.

*O sistema deve privilegiar compreensão financeira em vez de gerar números alarmantes sem contexto.*


# Status
Versão: V0.1
Status: Em evolução
Última classificação: visão inicial validada, com funcionalidades futuras ainda em aberto.