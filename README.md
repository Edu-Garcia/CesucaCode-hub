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
├── .gitmodules
├── skills-lock.json
├── .agents/skills/        # Skills instaladas
├── docs/                  # Documentação do hub
├── frontend/              # submodule → CesucaCode-frontend
└── backend/               # submodule → CesucaCode-backend
```
