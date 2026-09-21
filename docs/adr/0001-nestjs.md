# ADR-0001: API em NestJS

Data: 2026-09-20

Status: aceito para implementação. Projeto ainda sem aplicação; revisão e explicação pelo aluno pendentes (T-002).

## Contexto

O Expense Tracker prioriza aprendizado e precisa preparar Ivo para outro projeto com NestJS até aproximadamente 15/10/2026. O perfil informado é JS/TS intermediário, com 10–15 horas semanais. O roteiro anterior usava Express diretamente e adiava conceitos necessários de NestJS.

## Decisão

Construir a API diretamente em NestJS com TypeScript e Prisma, mantendo o monólito modular, PostgreSQL e o serviço Python/FastAPI exclusivo de ML. O primeiro ciclo terá quatro semanas estimadas em 40–60 horas; o prazo deve ser confrontado com a disponibilidade restante, sem registrar aprendizado como concluído apenas pela passagem do tempo.

- Organizar por domínio: Module, Controller, Service e dto/. Controllers tratam HTTP; services recebem dependências pelo construtor e concentram regras de negócio.
- Declarar providers no módulo responsável e exportar somente os necessários. PrismaModule exporta PrismaService; não instanciar um cliente por requisição nem armazenar identidade de usuário em providers singleton.
- Validar entradas com classes DTO, class-validator/class-transformer e ValidationPipe global com whitelist, forbidNonWhitelisted e transform. Definir e testar conversões de entrada, inclusive números, datas e valores opcionais. Interfaces TypeScript não validam JSON em execução.
- Manter em packages/shared tipos de contratos e funções puras. DTOs, Prisma e dependências NestJS permanecem na API. Testes de contrato verificam se respostas e entradas respeitam os tipos publicados. O pacote compartilhado não elimina sozinho divergências de validação.
- Usar AuthGuard e @nestjs/jwt para autenticação; @CurrentUser entrega a identidade ao controller, que passa userId ao service. Consultas e mutações filtram por ownership; categorias também precisam pertencer ao usuário.
- Usar HttpException e subclasses, com exception filter para respostas padronizadas. Interceptors envolvem a execução para medição e transformação; middleware fica para tarefas que precisam ocorrer antes dos guards.
- Configurar ambiente com @nestjs/config e validação na inicialização. Reutilizar configureApp em main.ts e nos testes HTTP para aplicar os mesmos pipes, filtros e políticas relevantes.
- Testar com @nestjs/testing, Vitest e Supertest. Unitários podem usar overrideProvider; integração de autenticação e ownership usa PostgreSQL real isolado e encerra aplicação/conexões no teardown.
- Publicar contrato OpenAPI com @nestjs/swagger. Hash de senha e ownership entram no primeiro ciclo; refresh rotativo e endurecimento da sessão vêm nas semanas 10–13.

## Alternativas

Express diretamente permitiria estudar composição de middleware, mas acrescentaria uma etapa anterior ao framework exigido pelo prazo. Express pode existir como adaptador HTTP do NestJS; isso não exige manter uma segunda API nem uma trilha prévia em Express.

Zod compartilhado continua sendo uma alternativa válida de validação. Para este ciclo, DTOs e ValidationPipe foram escolhidos para aprender o fluxo declarativo do NestJS. O custo é manter os contratos puros e a validação da API alinhados por revisão e testes. Não adicionar duas validações concorrentes no backend.

Uma camada de repository não será obrigatória enquanto apenas repetir o Prisma. Jest também é compatível com @nestjs/testing; o roteiro mantém Vitest, já previsto, sem ensinar dois runners no mesmo ciclo.

## Consequências

Classes, decorators, módulos e DI precisam ser estudados logo na primeira semana. Services passam a existir desde o primeiro módulo, substituindo a regra antiga de criá-los somente com dois consumidores. O aluno deve conseguir explicar resolução de dependências, imports/exports e o ciclo HTTP, além de implementar CRUD.

O primeiro ciclo entrega uma API local. Não promete MVP web, sessão completa, deploy ou domínio comprovado em quatro semanas. As semanas 5–9 consolidam persistência e integram React; mobile, ML e produção continuam no roteiro de 42 semanas.

O access token inicial tem expiração curta. Descartá-lo no cliente não revoga um token já emitido; o plano não apresenta esse logout como revogação imediata no servidor.

## Referências

- [Módulos](https://docs.nestjs.com/modules) e [providers](https://docs.nestjs.com/providers).
- [Ciclo HTTP](https://docs.nestjs.com/faq/request-lifecycle).
- [Validação](https://docs.nestjs.com/techniques/validation).
- [Autenticação](https://docs.nestjs.com/security/authentication).
- [Testes](https://docs.nestjs.com/fundamentals/testing).
