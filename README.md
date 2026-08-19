# CesucaCode Hub

Hub de agregação do **CesucaCode**. Este repositório **não** versiona o código-fonte do frontend nem do backend — apenas os aponta via [Git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules), além de manter documentação, skills de agentes e configuração compartilhada.

| Caminho | Repositório | Commits |
|---------|-------------|---------|
| `frontend/` | [CesucaCode-frontend](https://github.com/jvpgjava/CesucaCode-frontend) | Independentes (no repo do frontend) |
| `backend/` | [CesucaCode-backend](https://github.com/jvpgjava/CesucaCode-backend) | Independentes (no repo do backend) |
| *(raiz do hub)* | este repositório | Só docs, skills, `AGENTS.md`, etc. |

## Clone

```bash
git clone --recurse-submodules <url-do-hub>
```

Se já clonou sem submodules:

```bash
git submodule update --init --recursive
```

Isso coloca cada submodule no **commit fixado pelo hub** (em geral um ponto de `main`). Em seguida, no dia a dia:

```bash
cd frontend && git checkout main && git pull
cd ../backend && git checkout main && git pull
```

A partir daí você cria branches, commita e faz push **dentro** de `frontend/` ou `backend/` — isso **não** precisa (e não deve) gerar commit no hub.

## Desenvolvimento

- Trabalho de **UI/app**: branches/commits/PRs em `frontend/` → remote do frontend.
- Trabalho de **API/serviços**: branches/commits/PRs em `backend/` → remote do backend.
- Trabalho de **docs/skills/hub**: commits na raiz deste repositório.

Trocar de branch localmente no submodule é esperado. O hub está configurado com `ignore = all` nos submodules para o `git status` da raiz **não** ficar sujo por causa disso.

## Quando atualizar o ponteiro no hub

Só quando o time quiser mudar o **default** que um clone novo recebe (ex.: avançar o pin de `main`):

```bash
git submodule update --remote --merge
git add frontend backend
git commit -m "chore: atualiza ponteiros dos submodules para main"
```

Não rode isso só porque você entrou numa feature branch no submodule.

## Rodar localmente com Docker

Pré-requisito: [Docker Desktop](https://docs.docker.com/get-started/get-docker/) (ou Docker Engine + Compose v2), com os submodules já inicializados.

```bash
cp .env.example .env   # opcional — só se for preencher chaves de IA ou trocar senhas
docker compose up --build
```

| Serviço | URL |
|---------|-----|
| Frontend (Vite) | http://localhost:5173 |
| API Django | http://localhost:8000 |
| Swagger | http://localhost:8000/api/docs/ |
| Admin | http://localhost:8000/admin/ |

Na primeira vez, crie o CSAdmin (é interativo; o login é por e-mail):

```bash
docker compose exec backend python manage.py createsuperuser
```

E-mails de senha inicial em desenvolvimento saem no log do backend (`docker compose logs -f backend`). Chaves de LLM/embedding vão no `.env` da raiz do hub; o Compose injeta no container. Ollama no host usa `http://host.docker.internal:11434`.

**Chat RAG:** exige `LLM_*`, `EMBEDDING_*` e chaves válidas no `.env`, além de materiais processados (`status=ready`) com embeddings no banco. Sem isso, upload ou respostas do chat podem falhar.

A URL da API no frontend é `http://localhost:8000` de propósito: o navegador roda na sua máquina, não na rede interna do Compose.

Para parar: `docker compose down`. Para zerar o banco: `docker compose down -v`.

## Skills de agentes

Instaladas a partir de [AI Skills for Real Engineers](https://www.aihero.dev/skills) (`mattpocock/skills`):

| Skill | Uso |
|-------|-----|
| `/grill-with-docs` | Entrevista rigorosa + ADRs/glossário |
| `/to-spec` | Transforma a conversa em spec |
| `/to-tickets` | Quebra plano/spec em tickets |
| `/implement` | Implementa a partir de spec/tickets |
| `/code-review` | Review Standards + Spec |
| `/prototype` | Protótipo descartável para validar design |

Atualizar:

```bash
npx skills update
```

## Estrutura

```
CesucaCode-hub/
├── AGENTS.md              # Orientações para agentes de IA
├── README.md
├── docker-compose.yml     # Sobe db + API + frontend localmente
├── .gitmodules
├── skills-lock.json
├── .agents/skills/        # Skills instaladas
├── docker/postgres/       # Init do pgvector
├── docs/                  # Documentação do hub
├── frontend/              # submodule → CesucaCode-frontend
└── backend/               # submodule → CesucaCode-backend
```
