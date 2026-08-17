# Docs — CesucaCode Hub

Documentação agregada (não substitui os READMEs operacionais dos modules).

## Índice

| Doc | Conteúdo |
|-----|----------|
| [backend.md](./backend.md) | Arquitetura, API, AI providers, convenções DRF/JWT/pgvector (branch atual) |
| [frontend.md](./frontend.md) | Arquitetura SPA, rotas, auth client, convenções React/Vite/Query/Biome |
| [agents/](./agents/) | Config das skills (issue tracker, domain) |
| [adr/](./adr/) | Architecture Decision Records |
| [../CONTEXT.md](../CONTEXT.md) | Glossário e modelo de domínio |

## Branches de referência

Documentação de produto abaixo reflete o estado local tipicamente usado no hub:

| Module | Branch | Foco |
|--------|--------|------|
| `backend/` | `feat/provedores-ia-e-listagem-contas` | Auth, documents, AI providers, listagem de contas |
| `frontend/` | `feat/login-contas-e-materiais` | Login, materiais, contas + Biome |

Os READMEs dentro de cada submodule continuam sendo a fonte do “como rodar”.
Estas fichas do hub são a fonte do “como o sistema está desenhado” para agentes
e o time.

## Submodules

- Frontend: https://github.com/jvpgjava/CesucaCode-frontend
- Backend: https://github.com/jvpgjava/CesucaCode-backend

Clone do hub: `--recurse-submodules` (ver README da raiz).
