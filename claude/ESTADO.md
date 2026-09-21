# ESTADO — Expense Tracker

> Arquivo vivo. Leio no início de cada sessão, atualizo no fim das que mudarem algo.
> Permanece em `claude/ESTADO.md` durante o planejamento; mover para `docs/ESTADO.md` no setup, ajustando os links.

---

## PROJECT STATE

**Atualizado em:** 2026-09-20
**Fase:** 0 — Fundamentos
**Módulo:** nenhum iniciado
**Semana do roadmap:** pré-semana 1

**Objetivo atual:** iniciar T-001 e executar o primeiro ciclo NestJS de quatro semanas, com CRUD, DTOs, Prisma, autenticação básica segura e testes. Prazo de referência: 2026-10-15; ajustar a carga de 40–60 h à disponibilidade restante.

**Perfil informado (2026-09-15):** JS/TS intermediário; 10–15 h/semana. O ciclo completo estima 40–60 h; em 20/09 restam cerca de 25 dias até 15/10, portanto validar disponibilidade ao iniciar.

**Mudança de rota (2026-09-15):** backend passa de Express para **NestJS**, direto, sem a etapa em Express puro (prazo não comporta). Python continua só no ml-service (FastAPI). Revisão de 20/09: README, plano e prompts alinhados ao NestJS; primeiras quatro semanas priorizam a API. A decisão anterior permanece aqui como histórico.

**Conceitos estudados:** —
**Conceitos dominados:** —
**Conceitos a revisar:** —

**Implementação:** nenhuma linha de código do projeto escrita.
**Testes:** —
**Débito técnico:** —
**Problemas abertos:** —

**Decisões arquiteturais registradas:** 1 ADR: [ADR-0001 — NestJS](../docs/adr/0001-nestjs.md), incluindo organização, validação, contratos e testes. As decisões D1–D9 da Seção 2 continuam no plano; formalizar as demais a partir de ADR-0002, sem duplicar o registro existente.

**Próxima tarefa:** T-001 — Ambiente e primeiro repositório.
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

### LEARNING (a verificar)
- Injeção de dependência → testabilidade: resposta inicial correta (nível 2). Aprofundar: por que trocar provider no módulo de teste é diferente de mockar um import diretamente.

### IN PROGRESS
- —

### BLOCKED
- —

### DONE
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

### Erros recorrentes
_(três ocorrências do mesmo erro viram um item de LEARNING)_
- —

### Dúvidas importantes em aberto
- —

### Sessões
- **2026-09-20** — Revisão documental solicitada para NestJS. Definidos DTOs com class-validator/ValidationPipe, contratos puros em packages/shared, guards, filters e @nestjs/testing + Vitest + Supertest. Semanas 1–4 priorizam a API; 5–9 consolidam e integram web. Não houve implementação nem avaliação de domínio nesta sessão.
- **2026-09-15** — Ivo não tinha feito nada da semana 1. Decidido: NestJS no lugar de Express (outro projeto exige Nest em ~30 dias), Python só no ML. README atualizado. Pergunta de DI respondida no nível 2. Próxima sessão: fazer T-003 (reordenar semanas para Nest cabendo em ~45–60 h) e iniciar T-001.

### Retrospectivas de fase
- —

---

## ÍNDICE DE ADRs

| # | Decisão | Status | Semana |
|---|---|---|---|
| [0001](../docs/adr/0001-nestjs.md) | NestJS, módulos, validação, contratos e testes | Aceito para implementação; explicação pelo aluno pendente | Pré-semana 1 |
