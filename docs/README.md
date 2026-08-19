# Docs — CesucaCode Hub

Documentação agregada (não substitui os READMEs operacionais dos modules).

## Índice

| Doc | Conteúdo |
|-----|----------|
| [backend.md](./backend.md) | Arquitetura, API, chat RAG/SSE, AI providers, DRF/JWT/pgvector |
| [frontend.md](./frontend.md) | Arquitetura SPA, rotas, chat SSE, auth client, React/Vite/Query/Biome |
| [agents/](./agents/) | Config das skills (issue tracker, domain) |
| [adr/](./adr/) | Architecture Decision Records |
| [../CONTEXT.md](../CONTEXT.md) | Glossário e modelo de domínio |

## Branches de referência

Documentação de produto abaixo reflete a **`main`** dos submodules:

| Module | Commit | Foco |
|--------|--------|------|
| `backend/` | `c4384b3` | Auth, documents, **conversations (chat RAG + SSE)**, AI providers |
| `frontend/` | `550f3de` | Login, **chat**, materiais, contas + Biome |

Os READMEs dentro de cada submodule continuam sendo a fonte do “como rodar”.
Estas fichas do hub são a fonte do “como o sistema está desenhado” para agentes
e o time.

## Submodules

- Frontend: https://github.com/jvpgjava/CesucaCode-frontend
- Backend: https://github.com/jvpgjava/CesucaCode-backend

Clone do hub: `--recurse-submodules` (ver README da raiz).
