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

## Atualizar os modules

Puxa o commit apontado pelo hub:

```bash
git submodule update --remote --merge
```

Depois, se quiser fixar novas revisões no hub:

```bash
git add frontend backend
git commit -m "chore: atualiza ponteiros dos submodules"
```

## Desenvolvimento

- Trabalho de **UI/app**: faça commits dentro de `frontend/` (remote do frontend).
- Trabalho de **API/serviços**: faça commits dentro de `backend/` (remote do backend).
- Trabalho de **docs/skills/hub**: faça commits na raiz deste repositório.

Não misture o conteúdo dos modules em commits do hub — o hub só registra o SHA apontado por cada submodule.

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
