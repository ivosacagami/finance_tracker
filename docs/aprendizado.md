# Aprendizado — perguntas e respostas por tópico

> Registro técnico, por tópico, não por sessão. Cada entrada: pergunta (P), minha(s) tentativa(s) de resposta (R), correção quando houve erro, conclusão final. Complementa o diário por data em [ESTADO.md](../claude/ESTADO.md).

---

## Docker: porta host x porta do container
- P: o que significa cada lado de `HOST:CONTAINER`?
- R1: host = onde roda o processo do projeto; container = onde o processo "vira uma imagem".
- Correção: host = porta da sua máquina, livre pra escolher. Container = porta interna fixa que o programa já usa (Postgres sempre 5432 lá dentro); a imagem já vem com tudo instalado, subir o container não instala nada.
- Conclusão: `5433:5432` → host 5433 (livre, já que 5432 está ocupada pelo sistemaescola) mapeado para container 5432 (padrão do Postgres).

## Docker: volume anônimo órfão
- P: um volume anônimo deixado por um `down` sem volume nomeado fica ocupando o quê?
- R1: "memória, tipo um alocamento que nunca mais é usado".
- Correção: ocupa disco, não RAM. RAM é liberada ao parar o container; o volume órfão persiste em disco, sem nome reconhecível (hash), até um `prune`.
- Conclusão: usar volume nomeado (`pgdata`) evita esse acúmulo e facilita saber do que é.

## Docker: isolar dois projetos no mesmo Docker
- P: por que `docker compose down` na pasta do Expense Tracker não afeta o sistemaescola?
- R: correta de primeira — o Compose rotula os recursos pelo nome do projeto (`name:`) e só age sobre o que tem esse rótulo.
- Conclusão: comandos globais (`system prune`, `volume prune`) não filtram por projeto e devem ser evitados; sempre usar `docker compose` dentro da pasta certa.

## Docker Compose: por que sem Redis por enquanto
- P: por que tirar o serviço `redis` do compose?
- R1: "porque não vamos salvar o processo na RAM, e sim na memória do projeto".
- Correção: Redis é cache em RAM, útil quando uma consulta já é lenta/repetida. O motivo real é que esse problema ainda não existe — nem há código rodando. Adicionar por antecipação é overengineering.
- Conclusão: Redis fica de fora até haver gargalo real medido.

## Docker: para onde aponta o caminho do volume (`/var/lib/postgresql/data`)
- P: esse caminho existe dentro do container ou em outro lugar?
- R: em aberto.
- Explicação dada: é um caminho fixo *dentro* do container, definido pela imagem do Postgres (onde ele grava dados por padrão). O volume nomeado (lado esquerdo do mapeamento) é o que o Docker mantém de verdade em disco e "gruda" nesse caminho. Diferente de `.:/code`, que monta o projeto inteiro para um app cujo *código* roda dentro do container — sem relação com onde o Postgres guarda dados.
- Conclusão: pendente confirmação.

## Docker: bind mount (`./pasta:...`) x volume nomeado (`nome:...`)
- P: `./dados-postgres:/var/lib/postgresql/data` ainda seria um volume nomeado?
- R1: "estaria subindo uma pasta no sistema, criando o volume num nível abaixo?".
- Correção: não é hierarquia, é mecanismo diferente. `./pasta` = bind mount — o Docker usa exatamente aquela pasta do seu disco, sem gerenciar nada. `nome` (sem `/` ou `.`) = volume nomeado — o Docker decide e controla onde guardar. No projeto (em `/mnt/d`, caminho Windows via WSL), bind mount do diretório de dados do Postgres pode falhar por permissão; volume nomeado evita isso.
- Conclusão: usar sempre volume nomeado para dados do Postgres neste ambiente.

## Docker: como decidir qual porta usar
- P: como saber a porta interna e a porta de host de um serviço?
- R: não houve tentativa prévia — pergunta direta.
- Explicação: porta interna não se escolhe, vem documentada na página da imagem no Docker Hub (Postgres = 5432, Adminer = 8080, sempre). Porta de host se escolhe livremente, mas precisa estar livre — verificar com `docker ps` (containers já rodando) ou `ss -ltn` (todas as portas do sistema).
- Conclusão: Postgres → `5433:5432` (5432 do host já ocupada pelo sistemaescola); Adminer → `8080:8080` (livre).

## Docker: por que porta interna repetida entre containers não conflita
- P: dois containers com Postgres, cada um escutando 5432 por dentro, entram em conflito?
- R1: "não, porque a porta leva à imagem, não ao container; as informações vêm do host para criar um container novo".
- Correção: a imagem é a receita, parada, sem porta aberta — quem escuta é o **container**, e cada container tem rede isolada própria, como uma casa com endereço só dele. Duas portas 5432 internas nunca se encontram porque são redes diferentes, não por serem "da imagem". O conflito só existe no host, que é uma máquina única e compartilhada.
- Conclusão: porta interna pode repetir entre containers à vontade; porta de host precisa ser única.

## Docker: dois tipos de erro de porta (host ocupado x mapeamento invertido)
- P: porta de host já em uso e porta invertida (`5432:5433`) dão o mesmo tipo de problema?
- R1: achou que o mapeamento invertido faria o container subir "sem nada dentro" e o direcionamento "não iria à imagem".
- Correção: o container sobe completo, com o Postgres rodando normal por dentro — o mapeamento de porta nunca muda o que roda dentro do container. O que acontece é o encaminhamento apontar pra uma porta interna vazia (5433, onde ninguém escuta) em vez da certa (5432). Demonstrado com `docker run -p 5432:5432 hello-world` contra o sistemaescola: erro real foi `Bind for 0.0.0.0:5432 failed: port is already allocated`, com exit code 125 — isso só acontece quando a porta de **host** já está em uso.
- Conclusão: host ocupado → Docker recusa subir na hora, erro explícito no terminal. Mapeamento invertido → sobe sem erro nenhum, e só falha depois, quando algo tenta de fato conectar (conexão recusada). O segundo é mais perigoso por parecer que funcionou.

## Docker: por que fixar a versão da imagem (`postgres` sem tag quebrou o container)
- P: container `db` entrando em loop de restart — diagnosticado ao vivo.
- Sintoma: `docker compose ps` mostrava `Restarting (1)`.
- Evidência (log): Postgres 18+ mudou a convenção de onde monta o volume de dados (`/var/lib/postgresql` em vez de `/var/lib/postgresql/data`); a imagem recusa iniciar se detecta o layout antigo.
- Causa raiz: `image: postgres` sem tag baixa `latest`, que hoje é a 18+. Ambiente muda sem o compose ter mudado.
- Correção: fixar `image: postgres:16-alpine` (versão estável, layout compatível com a documentação usada) e recriar com `docker compose down -v && docker compose up -d`.
- Conclusão/prevenção: nunca usar imagem sem tag fixa — `latest` pode mudar comportamento entre uma execução e outra, inclusive em máquinas diferentes. Confirmado no fim: Postgres aceitando conexão com `expense`/`expense_tracker`, Adminer respondendo HTTP 200.

## Docker: dois projetos, dois VSCode, mesma máquina
- P: dá pra rodar dois VSCode com Docker aberto (Expense Tracker e sistemaescola) ao mesmo tempo, sem conflito?
- R: não perguntou por engano — dúvida direta.
- Explicação: VSCode não tem relação com o Docker; é só editor, todos conversam com o mesmo Docker Desktop por trás. O isolamento vem do `name:` do compose e das portas de host diferentes — já garantidos nos dois projetos.
- Conclusão: sem conflito, único limite real é CPU/RAM compartilhada entre tudo que roda ao mesmo tempo.

## NestJS/TypeScript: constructor e dependency injection (DI)
- P: o que é um constructor? O que é DI?
- Explicação: constructor é o método especial que roda quando a classe é instanciada (`new`), onde as dependências recebidas por parâmetro são guardadas em propriedades. DI é o Nest resolver essas dependências sozinho a partir do que o constructor pede — a classe nunca faz `new` da própria dependência, só declara que precisa dela.
- Dúvida levantada: no exemplo `private readonly prisma: PrismaService`, não ficou claro se `readonly` era sintaxe válida (escrito como "randoly" na pergunta) nem se `PrismaService` vinha pronto de alguma lib.
- Correção/ajuste: `readonly` é palavra-chave real do TypeScript (impede reatribuição da propriedade fora do constructor). `PrismaService` não é built-in de NestJS nem do Prisma — é uma classe que o próprio projeto vai escrever, prevista só pra Semana 2; citá-la na Semana 1 (ainda sem Prisma) estava fora de lugar. Exemplo corrigido pra esta semana: `private readonly transactionsService: TransactionsService`.
- Conclusão: ao mostrar uma linha de código nova, quebrar token por token (modificador, identificador, tipo) e indicar se é sintaxe da linguagem, classe do próprio projeto ou algo de uma lib — pedido explícito do Ivo (2026-10-01), vale daqui pra frente.

## Domínio: Entidade x Value Object (checkpoint T-004)
- P: por que `Money` é Value Object e `RecursoFinanceiro` é Entidade? Dê um exemplo de cada.
- R1: "Money carrega informação usada por entities porém não precisa ser constante, não necessariamente tem regras." Exemplo dado foi de duas movimentações de R$200 serem distintas — respondeu ao lado de Entidade, não ao de Value Object.
- Correção: o critério não é "ter ou não regras". É igualdade por valor: dois `Money(200, BRL)` são o mesmo Money, intercambiáveis. Uma Entidade (`RecursoFinanceiro`, `Movimentação`) precisa manter identidade e histórico ao longo do tempo, mesmo com atributos idênticos a outra.
- R2 (retest): "ele lembra de onde veio, mas quem lembra não é o Money, e sim o sistema/Movimentação que registra a origem para manter o histórico." Correto — isolou que Value Object não carrega proveniência; isso é responsabilidade da Entidade que o referencia.
- Conclusão: Entidade = preciso distinguir e acompanhar ESTE objeto específico ao longo do tempo. Value Object = só importa o valor que carrega agora. Mesmo critério fechou a pergunta sobre `Reserva` (DA-003): o sinal de precisar virar Entidade é precisar de histórico/identidade própria ao longo do tempo, não só "ter mais campos".

## Domínio: saldo atual x disponível após compromissos (checkpoint T-004)
- P: compra de R$200 no PIX e R$200 no crédito, mesmo dia, mesmo recurso, saldo inicial R$1000. O que muda em cada caso?
- R: saldo atual cai para R$800 só com o PIX (débito imediato); o crédito não afeta o saldo atual, mas entra como compromisso do período, deixando o disponível após compromissos em R$600 (800 − 200).
- Conclusão: correto de primeira — RN-012, RN-013 e RN-016 aplicadas corretamente sem confundir os dois números.

## Domínio: dupla redução na liquidação (checkpoint T-004)
- P: identificar o erro em "ao pagar a fatura, reduzo o saldo atual E reduzo de novo o disponível após compromissos".
- R: identificado como errado de primeira, citando RN-014 e explicando que o compromisso já tinha sido descontado do disponível antes.
- Conclusão: correto de primeira.

## Domínio: transferência com taxa vinculada (checkpoint T-004)
- P: transferência de R$500 Itaú → Nubank com taxa de R$10 no Itaú. O que é despesa? Quantas movimentações?
- R1: "nada é considerado despesa, a taxa é taxa de funcionamento, tudo dentro da mesma transação." Invertido — RN-027 diz que a taxa É despesa (vinculada ao evento, mas com natureza própria); as movimentações da transferência em si (saída/entrada) é que não são despesa/receita (RN-026).
- Correção: três movimentações distintas, mesmo evento `T-001` — saída R$500 (natureza: transferência), entrada R$500 (natureza: transferência), taxa R$10 (natureza: taxa/despesa).
- R2 (retest): sem taxa seriam só 2 movimentações (saída/entrada); se a taxa fosse cobrada em outro dia, ainda ficaria vinculada ao mesmo `T-001` "por conta do evento, não do dia". Correto nos dois pontos.
- Conclusão: o vínculo em RN-027 é por identidade do evento, não por proximidade temporal — ponto que não estava claro na primeira tentativa.

## Domínio: recorrência x obrigação x valor esperado x valor efetivo (checkpoint T-004)
- P: academia com recorrência de R$100 todo dia 10; em outubro saiu R$115. O que é a Recorrência, a Obrigação, o Valor Esperado e o Valor Efetivo? Como registrar um desconto de R$10 em novembro?
- R1: confundiu Recorrência com "o pagamento em si" e Obrigação com a regra geral recorrente, em vez da ocorrência concreta de um mês; também inventou um valor esperado (R$110) sem base no enunciado. Acertou a parte do ajuste (preservar o valor anterior, registrar o desconto à parte, sem sobrescrever — RN-006).
- Correção: Recorrência é a regra esperada ("todo dia 10, ~R$100"); Obrigação é a cobrança concreta de um mês específico (ex.: "outubro — R$115 — vencimento 10/10"); Valor Esperado vem da recorrência (R$100, não R$110); Valor Efetivo é o que de fato ocorreu (R$115).
- R2 (retest): "recorrência é a regra de cobrança todo mês; isso se torna obrigação a partir do momento que vira cobrança real, quando ocorre de fato; valor esperado é o valor acordado/habitual; valor efetivo pode divergir naquele mês." Os quatro termos corretamente separados, sem sobreposição.
- Conclusão: os quatro conceitos fecharam depois da segunda explicação com exemplo reaproveitado (mesmo cenário da academia).

## Domínio: saldo livre (checkpoint T-004, nota menor)
- P: R$3000 na poupança, R$1000 reservado para viagem. Saldo atual/livre/reservado? Depois de sacar R$200 fora da reserva?
- R: números finais corretos (2800 atual / 1000 reservado / 1800 livre), mas rotulou o estado inicial como "saldo livre é 0" e tratou os R$2000 livres como uma categoria à parte ("investimento").
- Correção: saldo livre = saldo atual − reservado; os R$2000 não alocados SÃO o saldo livre, não uma terceira categoria. "Investimento" é um módulo à parte (Patrimônio e Investimentos, fora do MVP) e não deve se misturar com os `RecursoFinanceiro` comuns do domínio atual.
- Conclusão: ponto de vocabulário, não de cálculo — os números já estavam certos.

**T-004 encerrada em 2026-10-05** — checkpoint de active recall com 7 perguntas, 3 gaps críticos identificados e corrigidos em retestes, registrado em [ADR-0002](adr/0002-dominio-financeiro.md).

---

## Como este arquivo é mantido
Atualizado ao final de cada tópico fechado durante as sessões, por adição no fim do arquivo — sem reescrever entradas antigas.
