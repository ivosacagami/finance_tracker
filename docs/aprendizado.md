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

---

## Como este arquivo é mantido
Atualizado ao final de cada tópico fechado durante as sessões, por adição no fim do arquivo — sem reescrever entradas antigas.
