# 🚀 Expense Tracker

> Gerenciador financeiro pessoal com API NestJS, web React, mobile Expo e serviço Python de ML.

**Estado em 20/09/2026:** planejamento, sem aplicação implementada. A estrutura, os comandos e os endpoints abaixo são previstos e só poderão ser executados após o setup. O roteiro está em [claude/plano.html](claude/plano.html) e o progresso em [claude/ESTADO.md](claude/ESTADO.md).

**Prioridade inicial:** quatro semanas de NestJS (40–60 horas), com módulos, DI, DTOs, Prisma, autenticação e testes. O prazo de referência é 15/10/2026; revise a disponibilidade semanal ao iniciar.

---

## 📌 Sobre o projeto

**Expense Tracker** é uma aplicação web desenvolvida com uma arquitetura de **monólito modular**, permitindo que diferentes funcionalidades do sistema sejam organizadas em módulos independentes dentro de uma única aplicação.

A proposta é manter o projeto simples de desenvolver, testar e manter inicialmente, sem abrir mão de uma arquitetura preparada para crescimento.

O sistema será disponibilizado online, permitindo que **múltiplos usuários utilizem a plataforma simultaneamente**, com controle adequado de autenticação, dados, permissões e isolamento das informações.

### Principais objetivos

- Criar uma aplicação web organizada e modular.
- Permitir acesso simultâneo de múltiplos usuários.
- Manter baixo acoplamento entre os módulos.
- Facilitar manutenção e evolução do sistema.
- Preparar a aplicação para implantação em ambiente de produção.
- Permitir futura evolução para serviços independentes caso a escala justifique.

---

# 🏗️ Arquitetura

O projeto utiliza inicialmente uma arquitetura de **Monólito Modular**.

Isso significa que o sistema é executado como uma aplicação principal, porém suas responsabilidades são separadas internamente em módulos bem definidos.

```text
                         INTERNET
                            │
                            ▼
                    ┌───────────────┐
                    │    FRONTEND   │
                    └───────┬───────┘
                            │
                         HTTP/API
                            │
                            ▼
                ┌────────────────────────┐
                │       BACKEND          │
                │    MONÓLITO MODULAR    │
                │                        │
                │ ┌────────────────────┐ │
                │ │ Autenticação       │ │
                │ ├────────────────────┤ │
                │ │ Usuários           │ │
                │ ├────────────────────┤ │
                │ │ Módulo Principal   │ │
                │ ├────────────────────┤ │
                │ │ Relatórios         │ │
                │ ├────────────────────┤ │
                │ │ Administração      │ │
                │ └────────────────────┘ │
                └───────────┬────────────┘
                            │
                            ▼
                    ┌───────────────┐
                    │  PostgreSQL   │
                    └───────────────┘
```

A aplicação continua sendo um único sistema para fins de execução e implantação, mas seus componentes possuem responsabilidades bem definidas.

---

# ✨ Funcionalidades

As funcionalidades abaixo representam a estrutura prevista para o sistema e podem ser expandidas conforme o desenvolvimento.

### 👤 Usuários

- Cadastro de usuários.
- Login e logout.
- Gerenciamento de sessão.
- Recuperação de acesso.
- Perfil do usuário.
- Controle de permissões.

### 🔐 Segurança

- Autenticação.
- Autorização baseada em permissões.
- Proteção de endpoints.
- Validação de dados recebidos pela API.
- Variáveis sensíveis armazenadas através de variáveis de ambiente.
- Senhas armazenadas utilizando hashing seguro.

### 📊 Sistema

- Dashboard.
- Processamento das informações da aplicação.
- Consultas ao banco de dados.
- Relatórios.
- Histórico de operações.
- Controle de dados por usuário.

### 🌐 Multiusuário

A aplicação será projetada desde o início para suportar **múltiplos usuários simultaneamente**.

Cada requisição deverá ser tratada de forma independente, evitando que informações ou estados pertencentes a um usuário sejam compartilhados indevidamente com outro.

---

# 🛠️ Tecnologias

A stack inicial proposta para o projeto é:

### Backend (API)

- Node.js
- TypeScript
- NestJS (módulos, controllers, providers e injeção de dependência)
- Prisma, encapsulado em `PrismaModule` / `PrismaService`
- `@nestjs/config` para configuração validada
- `class-validator` + `class-transformer` com `ValidationPipe`
- `@nestjs/jwt` para tokens e guards para proteção de rotas
- `@nestjs/swagger` para o contrato OpenAPI
- `@nestjs/testing` + Vitest + Supertest para testes da API

### Serviço de ML

- Python
- FastAPI
- pandas / scikit-learn

> Serviço separado, acessível só pela rede interna. Nunca fica no caminho crítico: se falhar, a API responde "sem sugestão".

### Banco de dados

- PostgreSQL

### Frontend

- React + TypeScript (web)
- Expo / React Native (mobile)

### Infraestrutura

- pnpm workspaces (monorepo)
- Docker
- Docker Compose
- Git
- GitHub
- GitHub Actions

### Ambiente de produção

A aplicação deverá ser preparada para implantação em um servidor/cloud, utilizando HTTPS e banco de dados PostgreSQL em ambiente de produção.

---

# 📂 Estrutura do projeto

A estrutura sugerida é:

```text
expense-tracker/
│
├── apps/
│   ├── api/                 # NestJS + Prisma
│   │   ├── src/
│   │   │   ├── main.ts
│   │   │   ├── app.module.ts
│   │   │   ├── modules/
│   │   │   │   ├── auth/
│   │   │   │   ├── users/
│   │   │   │   ├── categories/
│   │   │   │   ├── transactions/
│   │   │   │   └── dashboard/
│   │   │   └── shared/      # prisma, env, erros, guards
│   │   ├── prisma/
│   │   │   ├── schema.prisma
│   │   │   └── migrations/
│   │   └── test/
│   ├── web/                 # React + Vite
│   └── mobile/              # Expo
│
├── services/
│   └── ml/                  # Python + FastAPI
│
├── packages/
│   └── shared/              # tipos de contratos e funções puras
│
├── docs/
│   └── adr/                 # registros de decisão arquitetural
│
├── docker-compose.yml
├── pnpm-workspace.yaml
├── .env.example
├── .gitignore
└── README.md
```

---

# 🧩 Organização dos módulos

O código é organizado por domínio, não por tipo de arquivo. Cada domínio é um módulo NestJS.

Por exemplo:

```text
transactions/
│
├── transactions.module.ts
├── transactions.controller.ts
├── transactions.service.ts
└── dto/
```

### Module

Declara o que o módulo fornece e do que depende.

### Controller

Responsável pelos endpoints HTTP.

### Service

Responsável pelas regras de negócio e pelo acesso aos dados via Prisma.

### DTO

Classe que descreve a entrada com decorators de `class-validator`. A validação em execução depende do `ValidationPipe`; tipos TypeScript sozinhos não validam requisições.

Essa separação evita que regras de negócio sejam colocadas diretamente nos endpoints.

### Providers e injeção de dependência

Services recebem suas dependências pelo construtor. Cada módulo declara `providers`, importa os módulos necessários e exporta somente o que outros domínios usam. O `PrismaService` é compartilhado via `PrismaModule`, sem criar um cliente por requisição.

### Validação, autenticação e erros

- `ValidationPipe({ whitelist: true, forbidNonWhitelisted: true, transform: true })` na configuração global; conversões de números e datas devem ser explícitas e testadas.
- `AuthGuard` valida o JWT; `@CurrentUser()` entrega a identidade ao controller. O service recebe `userId` explicitamente e aplica ownership em consultas, alterações e exclusões.
- `HttpException` e subclasses representam erros HTTP; um exception filter padroniza respostas sem expor detalhes internos.
- Interceptors envolvem a execução para medição e transformação de resposta. Middleware fica restrito a tarefas anteriores aos guards, como identificação da requisição.
- DTOs ficam na API. `packages/shared` contém contratos TypeScript e funções puras, sem importar NestJS, Prisma ou decorators de validação. Testes de contrato verificam divergências em execução.

As decisões estão em [ADR-0001](docs/adr/0001-nestjs.md).

---

# 🔄 Fluxo de uma requisição

Um fluxo típico será:

```text
Usuário
   │
   ▼
Frontend
   │
   ▼
API
   │
   ▼
Middleware (quando necessário)
   │
   ▼
Guard → Interceptor (entrada) → Pipe
   │
   ▼
Controller
   │
   ▼
Service
   │
   ▼
Prisma
   │
   ▼
PostgreSQL
   │
   ▼
Service
   │
   ▼
Controller
   │
   ▼
Frontend
   │
   ▼
Usuário
```

Na resposta, os interceptors retornam na ordem inversa. Exceções não tratadas seguem para os exception filters. A configuração global deve ser reutilizada nos testes HTTP.

---

# ⚙️ Configuração prevista

Os comandos desta seção dependem da criação do monorepo e dos scripts de `apps/api/package.json`. O pacote da API deverá se chamar `api` para corresponder aos filtros abaixo.

## 1. Clonar o projeto

```bash
git clone [URL_DO_REPOSITORIO]
cd expense-tracker
```

---

## 2. Pré-requisitos

- Node.js LTS
- pnpm
- Docker (para o PostgreSQL local)
- Python 3 (só para o serviço de ML)

---

## 3. Instalar dependências

```bash
pnpm install
```

---

## 4. Configurar variáveis de ambiente

Copie o arquivo:

```text
.env.example
```

para:

```text
.env
```

Exemplo:

```env
DATABASE_URL=postgresql://usuario:senha@localhost:5432/database
JWT_SECRET=change-me
NODE_ENV=development
```

> O arquivo `.env` não deve ser enviado para o GitHub.

---

# 🐳 Executando com Docker

O projeto também poderá ser executado utilizando Docker.

```bash
docker compose up --build
```

Para executar em segundo plano:

```bash
docker compose up -d --build
```

Para interromper os containers:

```bash
docker compose down
```

---

# ▶️ Executando localmente

```bash
pnpm --filter api start:dev
```

A API ficará disponível localmente em:

```text
http://localhost:3000
```

A documentação da API (Swagger) estará disponível em:

```text
http://localhost:3000/docs
```

---

# 🗄️ Banco de dados

O projeto utiliza PostgreSQL.

As alterações estruturais do banco devem ser controladas através de migrations.

Exemplo:

```text
schema.prisma
   ↓
prisma migrate
   ↓
PostgreSQL
```

Isso permite acompanhar a evolução do banco de dados sem depender de alterações manuais.

---

# 🔐 Autenticação

O sistema deverá utilizar autenticação baseada em tokens.

Fluxo simplificado:

```text
LOGIN
  │
  ▼
Validação das credenciais
  │
  ▼
Token de autenticação
  │
  ▼
Requisições autenticadas
  │
  ▼
Validação do usuário
  │
  ▼
Acesso ao recurso
```

Nenhuma senha deverá ser armazenada diretamente no banco.

---

# 👥 Multiusuário

O sistema foi concebido para funcionar em ambiente online e atender múltiplos usuários simultaneamente.

Para isso, algumas regras arquiteturais são fundamentais:

- Não armazenar estado específico de um usuário em variáveis globais.
- Não utilizar arquivos locais como mecanismo principal de armazenamento de dados dos usuários.
- Utilizar banco de dados para persistência.
- Identificar o usuário através da autenticação.
- Validar autorização antes de acessar recursos.
- Utilizar conexões de banco adequadamente.
- Garantir isolamento entre dados pertencentes a diferentes usuários.

Exemplo:

```text
                 SISTEMA ONLINE
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
       Usuário A    Usuário B    Usuário C
          │            │            │
          └────────────┼────────────┘
                       ▼
                    API
                       │
                       ▼
                  PostgreSQL
```

---

# 🚀 Deploy

A aplicação deverá ser preparada para publicação em ambiente de produção.

Fluxo esperado:

```text
GitHub
   │
   ▼
Build
   │
   ▼
Docker
   │
   ▼
Servidor / Cloud
   │
   ▼
HTTPS
   │
   ▼
Usuários
```

Em produção deverão ser considerados:

- HTTPS.
- Domínio.
- Variáveis de ambiente.
- Banco PostgreSQL de produção.
- Backups.
- Logs.
- Monitoramento.
- Controle de acesso.
- Atualizações de segurança.

---

# 📈 Escalabilidade

A primeira versão não utilizará microserviços sem necessidade.

A estratégia será:

> **Começar simples, mas estruturar corretamente.**

Enquanto o sistema possuir uma escala pequena ou média:

```text
              ┌──────────────┐
              │   Monólito   │
              └──────┬───────┘
                     │
                 PostgreSQL
```

Caso determinado módulo posteriormente necessite de escala independente, ele poderá ser extraído:

```text
                  API
                   │
       ┌───────────┼───────────┐
       ▼           ▼           ▼
    Usuários     Sistema      Relatórios
       │           │           │
       └───────────┴───────────┘
                   │
              PostgreSQL
```

E, somente se necessário:

```text
              API Gateway
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
    Service A  Service B  Service C
```

Dessa maneira, a arquitetura evita a complexidade prematura de microserviços.

---

# 🧪 Testes

Os testes da API usarão `@nestjs/testing`, Vitest e Supertest. Unitários podem substituir providers com `overrideProvider`; testes de integração de ownership devem usar PostgreSQL real e os mesmos pipes, guards e filtros da aplicação.

Os testes devem ser organizados em diferentes níveis:

```text
apps/api/
│
├── src/**/*.spec.ts     # unitários, ao lado do código
└── test/                # integração e e2e (HTTP + banco)
```

Executar:

```bash
pnpm --filter api test
pnpm --filter api test:e2e
```

O objetivo é garantir que alterações em um módulo não introduzam regressões em outros.

---

# 🔒 Segurança

Antes da publicação em produção, o sistema deverá considerar:

- Hashing de senhas.
- Autenticação.
- Autorização.
- HTTPS.
- Validação de entrada.
- Proteção contra SQL Injection.
- Proteção contra acesso indevido a recursos.
- Rate limiting quando necessário.
- CORS configurado corretamente.
- Secrets fora do código-fonte.
- Logs sem exposição de informações sensíveis.
- Backups do banco de dados.

---

# 🗺️ Roadmap

O cronograma detalhado em `claude/plano.html` é a referência. Nas primeiras quatro semanas, entregar uma API NestJS com CRUD, persistência, autenticação básica segura e testes; as fases abaixo agrupam o escopo por tema.

## Fase 1 — Fundação

- [ ] Criar estrutura do projeto.
- [ ] Configurar NestJS.
- [ ] Configurar PostgreSQL.
- [ ] Configurar Prisma.
- [ ] Configurar migrations.
- [ ] Configurar Docker.
- [ ] Criar sistema de configuração.

## Fase 2 — Usuários

- [ ] Cadastro.
- [ ] Login.
- [ ] Logout.
- [ ] Autenticação.
- [ ] Permissões.
- [ ] Perfil.

## Fase 3 — Funcionalidades principais

- [ ] Implementar módulo principal.
- [ ] Criar dashboard.
- [ ] Implementar relatórios.
- [ ] Implementar regras de negócio.
- [ ] Criar frontend.

## Fase 4 — Multiusuário

- [ ] Isolamento dos dados.
- [ ] Controle de permissões.
- [ ] Testes concorrentes.
- [ ] Tratamento de sessões.
- [ ] Testes de carga.

## Fase 5 — Produção

- [ ] Configurar servidor.
- [ ] Configurar domínio.
- [ ] Configurar HTTPS.
- [ ] Configurar banco de produção.
- [ ] Configurar backups.
- [ ] Configurar logs.
- [ ] Monitoramento.

## Fase 6 — Escala

- [ ] Avaliar gargalos.
- [ ] Otimizar banco.
- [ ] Implementar cache quando necessário.
- [ ] Avaliar filas/background workers.
- [ ] Avaliar escalabilidade horizontal.
- [ ] Extrair serviços somente quando houver justificativa técnica.

---

# 🤝 Contribuição

Contribuições são bem-vindas.

Antes de enviar alterações:

1. Crie uma branch.
2. Implemente a alteração.
3. Execute os testes.
4. Verifique possíveis regressões.
5. Faça um commit descritivo.
6. Abra um Pull Request.

Exemplo:

```bash
git checkout -b feature/nova-funcionalidade
```

---

# 📄 Licença

Este projeto está sob a licença:

**[DEFINIR LICENÇA]**

---

# 👨‍💻 Status

🚧 **Em desenvolvimento**

O projeto está em fase de desenvolvimento e sua arquitetura poderá evoluir conforme os requisitos, número de usuários e necessidades de produção.

---

# 💡 Princípio arquitetural

> **"Não construir uma arquitetura complexa porque talvez um dia o sistema cresça. Construir uma arquitetura simples, modular e correta que permita crescer quando houver necessidade."**

O projeto começa como um **monólito modular**, mantendo simplicidade operacional e velocidade de desenvolvimento, mas com separação suficiente entre seus componentes para permitir futuras evoluções arquiteturais.

---

## 📌 Visão resumida

```text
                 ┌─────────────────────┐
                 │       USUÁRIOS      │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │      FRONTEND       │
                 └──────────┬──────────┘
                            │
                            ▼
              ┌──────────────────────────┐
              │     NESTJS BACKEND       │
              │                          │
              │  ┌──────┐ ┌──────────┐  │
              │  │ Auth │ │ Usuários │  │
              │  └──────┘ └──────────┘  │
              │                          │
              │  ┌──────────┐ ┌───────┐ │
              │  │ Principal│ │Report │ │
              │  └──────────┘ └───────┘ │
              │                          │
              └────────────┬─────────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │   PostgreSQL    │
                  └─────────────────┘
```

**Arquitetura inicial:** Monólito Modular
**Backend:** Node.js / NestJS / TypeScript
**Serviço de ML:** Python / FastAPI
**Banco:** PostgreSQL
**Containerização:** Docker
**Deployment:** Cloud / Servidor
**Modelo:** Multiusuário
**Objetivo:** Aplicação web escalável e preparada para produção

## Referências NestJS

- [Módulos](https://docs.nestjs.com/modules) e [providers](https://docs.nestjs.com/providers).
- [Ciclo da requisição](https://docs.nestjs.com/faq/request-lifecycle).
- [Validação](https://docs.nestjs.com/techniques/validation).
- [Autenticação](https://docs.nestjs.com/security/authentication).
- [Testes](https://docs.nestjs.com/fundamentals/testing).
