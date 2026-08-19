# CONTEXT — CesucaCode

Glossário e modelo de domínio compartilhados. Atualizado a partir da `main`
dos submodules (`backend`: `c4384b3`, `frontend`: `550f3de`).

## Produto

**CesucaCode** — assistente de estudo/tutoria com RAG sobre materiais dos cursos
de Tecnologia do Centro Universitário Cesuca (CC e ADS). A plataforma cobre
autenticação, gestão de contas, upload/processamento de materiais com embeddings
e **chat com RAG** (assistente **S.O.F.I.**) sobre os materiais visíveis ao
usuário.

## Contextos

| Contexto | Repo / pasta | Responsabilidade |
|----------|--------------|------------------|
| Frontend | `frontend/` | SPA React: login, chat, materiais, contas |
| Backend | `backend/` | API Django/DRF: auth, documents, conversations, ai_providers |
| Hub | este repo | Agregação, docs, skills, ADRs |

## Entidades

| Entidade | Onde | Notas |
|----------|------|-------|
| Course | backend `accounts` | Seed: `cc`, `ads` |
| User | backend `accounts` | Papéis CSAdmin / CSCoordinator / CSStudent |
| Document | backend `documents` | PDF/DOCX/PPTX/TXT; status `ready`/`failed` |
| DocumentChunk | backend `documents` | Texto + embedding `pgvector` (dimensão fixa) |
| Conversation | backend `conversations` | Por usuário; título auto na 1ª mensagem |
| Message | backend `conversations` | Papel `user` ou `assistant`; histórico do chat |

## Papéis

| Papel | Código API/FE | Login | Poderes principais |
|-------|---------------|-------|-------------------|
| CSAdmin | `cs_admin` | e-mail | Tudo; cria contas; só nasce via `createsuperuser` |
| CSCoordinator | `cs_coordinator` | e-mail | Materiais dos cursos em `coordinated_courses` |
| CSStudent | `cs_student` | RGM (ou e-mail) | Lista materiais do próprio curso; chat no escopo do curso |

## Glossário

| Termo | Significado |
|-------|-------------|
| Hub | Repo agregador via git submodules; sem código de produto |
| Submodule | Ponteiro Git para um commit de outro repositório |
| identifier | Campo único de login: e-mail ou RGM (detectado por `@`) |
| must_change_password | Flag que bloqueia API/UI até troca de senha |
| Embedding | Vetor numérico do chunk; dimensão travada em migration + `EMBEDDING_DIMENSIONS` |
| RAG | Retrieval-Augmented Generation — busca vetorial de chunks + prompt com contexto |
| SSE | Server-Sent Events — streaming da resposta do chat (`text/event-stream`) |
| S.O.F.I. | Persona da assistente acadêmica; definida em `system_prompt.md` |
| SYSTEM_PROMPT_PATH | Env com caminho do system prompt do chat (Markdown/texto puro) |
| Provider | Backend de LLM/embedding escolhido por env (`LLM_PROVIDER` / `EMBEDDING_PROVIDER`) |

## Decisões

Ver `docs/adr/` e as fichas `docs/backend.md` / `docs/frontend.md`.
