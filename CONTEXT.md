# CONTEXT — CesucaCode

Glossário e modelo de domínio compartilhados. Atualizado a partir das branches
`frontend`: `feat/login-contas-e-materiais` e `backend`: `feat/provedores-ia-e-listagem-contas`.

## Produto

**CesucaCode** — assistente de estudo/tutoria com RAG sobre materiais dos cursos
de Tecnologia do Centro Universitário Cesuca (CC e ADS). Hoje a plataforma cobre
autenticação, gestão de contas, upload/processamento de materiais com embeddings;
o chat com RAG ainda não está implementado.

## Contextos

| Contexto | Repo / pasta | Responsabilidade |
|----------|--------------|------------------|
| Frontend | `frontend/` | SPA React: login, materiais, contas |
| Backend | `backend/` | API Django/DRF: auth, documents, ai_providers |
| Hub | este repo | Agregação, docs, skills, ADRs |

## Entidades

| Entidade | Onde | Notas |
|----------|------|-------|
| Course | backend `accounts` | Seed: `cc`, `ads` |
| User | backend `accounts` | Papéis CSAdmin / CSCoordinator / CSStudent |
| Document | backend `documents` | PDF/DOCX/PPTX/TXT; status `ready`/`failed` |
| DocumentChunk | backend `documents` | Texto + embedding `pgvector` (dimensão fixa) |

## Papéis

| Papel | Código API/FE | Login | Poderes principais |
|-------|---------------|-------|-------------------|
| CSAdmin | `cs_admin` | e-mail | Tudo; cria contas; só nasce via `createsuperuser` |
| CSCoordinator | `cs_coordinator` | e-mail | Materiais dos cursos em `coordinated_courses` |
| CSStudent | `cs_student` | RGM (ou e-mail) | Lista materiais do próprio curso; sem gestão |

## Glossário

| Termo | Significado |
|-------|-------------|
| Hub | Repo agregador via git submodules; sem código de produto |
| Submodule | Ponteiro Git para um commit de outro repositório |
| identifier | Campo único de login: e-mail ou RGM (detectado por `@`) |
| must_change_password | Flag que bloqueia API/UI até troca de senha |
| Embedding | Vetor numérico do chunk; dimensão travada em migration + `EMBEDDING_DIMENSIONS` |
| RAG | Retrieval-Augmented Generation — planejado; app `conversations` ainda não existe |
| Provider | Backend de LLM/embedding escolhido por env (`LLM_PROVIDER` / `EMBEDDING_PROVIDER`) |

## Decisões

Ver `docs/adr/` e as fichas `docs/backend.md` / `docs/frontend.md`.
