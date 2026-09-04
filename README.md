# 🚀 [NOME DO PROJETO]

> Sistema web modular desenvolvido para oferecer uma aplicação escalável, segura e preparada para utilização simultânea por múltiplos usuários.

---

## 📌 Sobre o projeto

**[NOME DO PROJETO]** é uma aplicação web desenvolvida com uma arquitetura de **monólito modular**, permitindo que diferentes funcionalidades do sistema sejam organizadas em módulos independentes dentro de uma única aplicação.

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

### Backend

- Python
- FastAPI
- Pydantic
- SQLAlchemy

### Banco de dados

- PostgreSQL

### Frontend

- HTML
- CSS
- JavaScript

> O frontend poderá posteriormente ser substituído ou expandido por um framework moderno caso a complexidade da interface justifique.

### Infraestrutura

- Docker
- Docker Compose
- Git
- GitHub

### Ambiente de produção

A aplicação deverá ser preparada para implantação em um servidor/cloud, utilizando HTTPS e banco de dados PostgreSQL em ambiente de produção.

---

# 📂 Estrutura do projeto

A estrutura sugerida é:

```text
project/
│
├── app/
│   │
│   ├── main.py
│   │
│   ├── core/
│   │   ├── config.py
│   │   ├── security.py
│   │   └── database.py
│   │
│   ├── auth/
│   │   ├── router.py
│   │   ├── service.py
│   │   ├── schemas.py
│   │   └── models.py
│   │
│   ├── users/
│   │   ├── router.py
│   │   ├── service.py
│   │   ├── schemas.py
│   │   └── models.py
│   │
│   ├── dashboard/
│   │   ├── router.py
│   │   ├── service.py
│   │   └── schemas.py
│   │
│   ├── reports/
│   │   ├── router.py
│   │   ├── service.py
│   │   └── schemas.py
│   │
│   └── admin/
│       ├── router.py
│       ├── service.py
│       └── schemas.py
│
├── tests/
│
├── migrations/
│
├── frontend/
│
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── .env.example
├── .gitignore
└── README.md
```

---

# 🧩 Organização dos módulos

Cada módulo deve seguir uma separação clara de responsabilidades.

Por exemplo:

```text
users/
│
├── router.py
├── service.py
├── repository.py
├── schemas.py
└── models.py
```

### Router

Responsável pelos endpoints HTTP.

### Service

Responsável pelas regras de negócio.

### Repository

Responsável pelo acesso aos dados.

### Schemas

Responsável pela validação e estrutura dos dados.

### Models

Responsável pela representação das entidades no banco.

Essa separação evita que regras de negócio sejam colocadas diretamente nos endpoints.

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
Router
   │
   ▼
Service
   │
   ▼
Repository
   │
   ▼
PostgreSQL
   │
   ▼
Repository
   │
   ▼
Service
   │
   ▼
Router
   │
   ▼
Frontend
   │
   ▼
Usuário
```

Isso mantém as responsabilidades separadas e facilita testes e manutenção.

---

# ⚙️ Configuração

## 1. Clonar o projeto

```bash
git clone [URL_DO_REPOSITORIO]
cd [NOME_DO_PROJETO]
```

---

## 2. Criar ambiente virtual

### Windows

```bash
python -m venv .venv
.venv\Scripts\activate
```

### Linux/macOS

```bash
python3 -m venv .venv
source .venv/bin/activate
```

---

## 3. Instalar dependências

```bash
pip install -r requirements.txt
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
SECRET_KEY=change-me
ENVIRONMENT=development
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

Com o ambiente virtual ativado:

```bash
uvicorn app.main:app --reload
```

A API ficará disponível localmente em:

```text
http://localhost:8000
```

A documentação automática do FastAPI estará disponível em:

```text
http://localhost:8000/docs
```

---

# 🗄️ Banco de dados

O projeto utiliza PostgreSQL.

As alterações estruturais do banco devem ser controladas através de migrations.

Exemplo:

```text
models
   ↓
migration
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

Os testes devem ser organizados em diferentes níveis:

```text
tests/
│
├── unit/
├── integration/
└── api/
```

Executar:

```bash
pytest
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

## Fase 1 — Fundação

- [ ] Criar estrutura do projeto.
- [ ] Configurar FastAPI.
- [ ] Configurar PostgreSQL.
- [ ] Configurar SQLAlchemy.
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
              │     FASTAPI BACKEND      │
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
**Backend:** Python / FastAPI
**Banco:** PostgreSQL
**Containerização:** Docker
**Deployment:** Cloud / Servidor
**Modelo:** Multiusuário
**Objetivo:** Aplicação web escalável e preparada para produção
