# ESTADO — Expense Tracker

> Arquivo vivo. Leio no início de cada sessão, atualizo no fim das que mudarem algo.
> Permanece em `claude/ESTADO.md` durante o planejamento; mover para `docs/ESTADO.md` no setup, ajustando os links.

---

## PROJECT STATE

**Atualizado em:** 2026-10-05
**Fase:** 0 — Fundamentos
**Módulo:** T-001 em andamento — ambiente e banco prontos; Semana 1 (NestJS) iniciada só na parte conceitual, implementação zero. **T-004 concluída** (checkpoint de active recall passou em 2026-10-05); ADR-0002 redigido, explicação pelo aluno pendente (nova tarefa, análoga a T-002).
**Semana do roadmap:** Semana 01 — Ambiente e primeiro módulo NestJS

**Objetivo atual:** concluir T-001 (falta o NestJS mínimo em `apps/api`: monorepo pnpm, `AppModule`+`TransactionsModule`, CRUD em memória) e seguir o ciclo NestJS de quatro semanas, com DTOs, Prisma, autenticação básica segura e testes. Prazo de referência: 2026-10-15; ajustar a carga de 40–60 h à disponibilidade restante. Domínio (T-004) fechado com ADR-0002; falta Ivo explicar o ADR-0002 com as próprias palavras antes de desenhar `schema.prisma` na Semana 2.

**Perfil informado (2026-09-15):** JS/TS intermediário; 10–15 h/semana. O ciclo completo estima 40–60 h; em 01/10 restam cerca de 14 dias até 15/10 — carga apertada, acompanhar.

**Horas registradas no ciclo:** 4h (2026-09-21, ambiente + banco). Sessão de 2026-10-01 foi conceitual (sem cronometragem de horas de estudo isolada). Total: 4h de 40–60h estimadas.

**Mudança de rota (2026-09-15):** backend passa de Express para **NestJS**, direto, sem a etapa em Express puro (prazo não comporta). Python continua só no ml-service (FastAPI). Revisão de 20/09: README, plano e prompts alinhados ao NestJS; primeiras quatro semanas priorizam a API. A decisão anterior permanece aqui como histórico.

**Conceitos estudados:** mapeamento de portas host/container, imagem x container, volume nomeado x anônimo x bind mount, isolamento de projetos no Docker (nome de projeto, `down` vs `prune`), leitura de log para diagnosticar container em restart loop, risco de imagem sem tag fixa (`latest`); decorators (`@Module`, `@Controller`, `@Injectable`), Nest CLI, dependency injection via constructor, modificadores TypeScript `private`/`readonly`, anotação de tipos em parâmetro.
**Conceitos dominados:** —
**Conceitos a revisar:** distinguir "porta de host ocupada" (erro imediato) de "mapeamento invertido" (sobe sem erro, falha só na conexão) — respondido corretamente após uma tentativa invertida; revisar de novo mais adiante. DI/constructor — explicado em profundidade em 01/10, ainda sem active recall respondido (pergunta pendente: o que precisa estar registrado no `TransactionsModule` para `TransactionsController` receber `TransactionsService` via construtor, e o passo a passo do Nest numa requisição HTTP).
**Conceitos dominados (domínio, 2026-10-05):** Entidade x Value Object (critério de identidade/histórico, não "quantidade de campos"); saldo atual x disponível após compromissos (RN-012/013/016); RN-014 (dupla redução); RN-027 (taxa de transferência é despesa vinculada ao evento, não ao dia); recorrência x obrigação x valor esperado x valor efetivo. Detalhe por pergunta/correção em [docs/aprendizado.md](../docs/aprendizado.md).

**Implementação:** `docker-compose.yml` com Postgres 16 (`db`) e Adminer, ambos validados subindo e respondendo (`psql` conectou, Adminer HTTP 200). Ainda nenhuma linha de NestJS; `apps/api` ainda não existe. Tarefa da semana já entregue a Ivo no formato TAREFA/OBJETIVO/DICAS/CRITÉRIO (scaffold do monorepo + CRUD de transactions em memória via Nest CLI).
**Testes:** verificação manual via `docker compose config`, `docker compose ps` e `psql`; nenhum teste automatizado ainda.
**Débito técnico:** —
**Problemas abertos:** —

**Decisões arquiteturais registradas:** 2 ADRs. [ADR-0001 — NestJS](../docs/adr/0001-nestjs.md): organização, validação, contratos e testes. [ADR-0002 — Domínio financeiro](../docs/adr/0002-dominio-financeiro.md) (2026-10-05): vocabulário e regras de negócio (RN-001 a RN-027) a partir de `docs/dominio/01` a `07`; aceito para modelagem tática, explicação pelo aluno pendente.

**Repositório remoto:** conectado em 2026-10-01 a [github.com/ivosacagami/expensive_tracker](https://github.com/ivosacagami/expensive_tracker) (`origin`), `master` publicado.

**Próxima tarefa:** nova tarefa (análoga a T-002) — Ivo revisa e explica o ADR-0002 com as próprias palavras antes de iniciar o `schema.prisma` na Semana 2. T-001 (scaffold do monorepo + CRUD de `transactions` em memória) segue aberta em paralelo, sem dependência entre as duas.
**Próximo checkpoint:** CP-0 (fim da semana 2) — fundamentos; CP-NEST e CP-2 ao fim da semana 4 — API NestJS.

---

## ESCALA DE DOMÍNIO

| Nível | Nome | Significa |
|---|---|---|
| 1 | Funciona | O código roda. Não conta como aprendizado. |
| 2 | Entendo | Sei o que cada parte faz quando leio. |
| 3 | Explico | Explico sem olhar o código, incluindo o porquê. |
| 4 | Modifico | Mudo o comportamento e corrijo um bug relacionado. |
| 5 | Refaço | Reimplemento do zero, sem consultar a solução original. |

**Regra de avanço:** semana fecha com nível 3 nos conceitos críticos dela.
Fase fecha com nível 4 no conceito central e nível 5 em pelo menos um.

---

## CONCEITOS — RASTREAMENTO

| Conceito | Introduzido | Nível | Última revisão | Próxima revisão |
|---|---|---|---|---|
| Injeção de dependência (motivação) | 2026-09-15 | 2 | 2026-09-15 | ao ver o primeiro módulo Nest |

---

## BACKLOG

### TODO
- **T-001** Ambiente e primeiro repositório (semana 1)
- **T-002** Ivo revisa e explica o ADR-0001 com as próprias palavras: módulos/DI, DTOs com class-validator versus Zod e contratos independentes do framework. O documento está escrito; o domínio desses conceitos ainda precisa ser verificado.
- **T-005** Ivo revisa e explica o ADR-0002 com as próprias palavras (vocabulário de domínio, RN-001 a RN-027), antes de iniciar `schema.prisma` na Semana 2.

### IN PROGRESS
- —

### LEARNING (a verificar)
- Injeção de dependência → testabilidade: resposta inicial correta (nível 2). Aprofundar: por que trocar provider no módulo de teste é diferente de mockar um import diretamente.

### BLOCKED
- —

### DONE
- **T-004** Vocabulário e regras de negócio do domínio financeiro (`docs/dominio/01` a `07`, RN-001 a RN-027), checkpoint de active recall com 3 gaps críticos corrigidos em retestes, formalizado em [ADR-0002](../docs/adr/0002-dominio-financeiro.md) (2026-10-05).
- Commits divididos por efeito (infra Docker vs. documentação/diário) e publicados: `origin` configurado para github.com/ivosacagami/expensive_tracker, `master` com push feito (2026-10-01).
- Ambiente Linux (WSL Ubuntu) pronto: Node 24 via nvm, pnpm 12 via corepack, Docker Desktop com integração WSL ativa (2026-09-21).
- `docker-compose.yml` do Postgres + Adminer, isolado do projeto sistemaescola por nome de projeto e portas próprias (2026-09-21). Depurado um bug real (imagem `postgres` sem tag puxando a 18+, incompatível com o layout de volume usado); corrigido fixando `postgres:16-alpine`.
- **T-003** Plano, README e prompts alinhados ao NestJS; ciclo de quatro semanas, CP-NEST e ADR-0001 registrados (2026-09-20). Documentação concluída, implementação e aprendizado ainda pendentes.
- Plano de 42 semanas definido e publicado (2026-09-04)
- Protocolo de mentoria acordado (2026-09-04)

### DÉBITO TÉCNICO
_Formato: O QUE / POR QUE deixamos / RISCO / QUANDO REVISAR_
- —

### LEARNING
_Lacunas identificadas que precisam de exercício dedicado._
- —

### BUGS
- —

---

## DIÁRIO DE APRENDIZADO

> Registro por data. Registro por tópico técnico (pergunta/resposta/correção): [docs/aprendizado.md](../docs/aprendizado.md).

### Erros recorrentes
_(três ocorrências do mesmo erro viram um item de LEARNING)_
- —

### Dúvidas importantes em aberto
- —

### Sessões
- **2026-10-05** — Revisão do material de domínio gerado em `docs/dominio/01` a `07` (não escrito primeiro por Ivo; gerado por IA com explicação conceitual acompanhando). Checkpoint de active recall com 7 perguntas cobrindo Entidade x Value Object, saldo atual x disponível após compromissos, transferência com taxa (RN-027) e recorrência x obrigação x valor esperado/efetivo. 3 gaps críticos na primeira rodada, todos corrigidos em retestes. T-004 encerrada; formalizado [ADR-0002](../docs/adr/0002-dominio-financeiro.md). Aberta T-005 (explicar ADR-0002, mesmo padrão de T-002 para ADR-0001). Detalhe por pergunta em [docs/aprendizado.md](../docs/aprendizado.md).
- **2026-10-04** — Ivo apontou que arquitetura, regras de negócio e DDD não tinham sido decididos como um pacote fundacional antes do código. Revisão confirmou: ADR-0001 cobre arquitetura técnica (stack, módulos, validação, auth), mas regras de negócio e modelagem de domínio ficaram implícitas/adiadas. Combinado tratar isso com o mesmo rigor do ADR-0001, antes do Prisma (Semana 2), sem bloquear T-001 (CRUD em memória é descartável/prático, não depende do domínio final). Aberta **T-004**: exercício entregue a Ivo para definir vocabulário e invariantes de User/Transaction/Category antes de formalizar ADR-0002. Também corrigido aviso do VSCode (interpretador Python apontando para caminho Windows); criado `.vscode/settings.json` com `python.defaultInterpreterPath` para `/usr/bin/python3` — vale só para acesso via WSL, não para VSCode nativo do Windows.
- **2026-10-01** — Sessão de retomada e conceitos. Commits do trabalho de 21/09 divididos por efeito (infra vs. docs) e publicados no GitHub recém-conectado (`origin`). Iniciada a Semana 1 (NestJS) do lado conceitual: decorators, `@Module`/`@Controller`/`@Injectable`, Nest CLI, dependency injection via constructor, modificadores TS `private`/`readonly`, anotação de tipo. Tarefa da semana entregue (CRUD de transactions em memória, scaffold via Nest CLI) — implementação ainda não iniciada por Ivo. Pedido de ajuste de estilo: quebrar toda linha de código nova token por token, distinguindo sintaxe da linguagem de classes do próprio projeto. Pendente: active recall sobre DI/ciclo de requisição.
- **2026-09-21** — 4h. Ambiente configurado (Node/pnpm nativos no WSL, Docker com integração ativa). T-001 avançou: `docker-compose.yml` do Postgres+Adminer escrito, revisado e corrigido em várias rodadas (sintaxe YAML de listas, segredos fora do código, versão de imagem fixa). Um bug real diagnosticado e corrigido (container em restart loop por imagem `latest`/Postgres 18). Registro detalhado por tópico iniciado em [docs/aprendizado.md](../docs/aprendizado.md). Falta o NestJS mínimo em `apps/api` para fechar T-001.
- **2026-09-20** — Revisão documental solicitada para NestJS. Definidos DTOs com class-validator/ValidationPipe, contratos puros em packages/shared, guards, filters e @nestjs/testing + Vitest + Supertest. Semanas 1–4 priorizam a API; 5–9 consolidam e integram web. Não houve implementação nem avaliação de domínio nesta sessão.
- **2026-09-15** — Ivo não tinha feito nada da semana 1. Decidido: NestJS no lugar de Express (projeto da FIAP, em paralelo, exige Nest em ~30 dias), Python só no ML. README atualizado. Pergunta de DI respondida no nível 2. Próxima sessão: fazer T-003 (reordenar semanas para Nest cabendo em ~45–60 h) e iniciar T-001.

### Retrospectivas de fase
- —

---

## ÍNDICE DE ADRs

| # | Decisão | Status | Semana |
|---|---|---|---|
| [0001](../docs/adr/0001-nestjs.md) | NestJS, módulos, validação, contratos e testes | Aceito para implementação; explicação pelo aluno pendente | Pré-semana 1 |
| [0002](../docs/adr/0002-dominio-financeiro.md) | Vocabulário e regras de negócio do domínio financeiro (RN-001 a RN-027) | Aceito para modelagem tática; explicação pelo aluno pendente (T-005) | Pré-semana 2 |
