# Backend — arquitetura e stack

Documentação do submodule `backend/`, alinhada à branch
**`feat/provedores-ia-e-listagem-contas`** (`b529faa`).

README operacional completo: `backend/README.md`. Swagger ao vivo:
`http://127.0.0.1:8000/api/docs/`.

## Snapshot da branch

| Item | Estado |
|------|--------|
| Apps | `core`, `accounts`, `documents`, `ai_providers` |
| Chat / RAG | **Não implementado** (`conversations/` só citado no README antigo) |
| Listagem de contas | `GET /api/auth/accounts/` (CSAdmin) |
| Embeddings | Gerados no upload/reprocess síncrono via `get_embedding_model()` |

## Stack (versões do `requirements/base.txt`)

| Pacote | Faixa | Papel no projeto |
|--------|-------|------------------|
| Django | 5.1.x | Projeto `config/`, apps em `apps/` |
| Django REST Framework | 3.15.x | API; `IsAuthenticated` + `PasswordIsCurrent` no default |
| djangorestframework-simplejwt | 5.3.x | JWT Bearer; access 2h, refresh 7d, `ROTATE_REFRESH_TOKENS=True` |
| django-environ | 0.11.x | `.env` → settings |
| django-cors-headers | 4.4.x | Origens do Vite (`CORS_ALLOWED_ORIGINS`) |
| psycopg + pgvector | 3.2 / 0.3 | Postgres + coluna `VectorField` nos chunks |
| drf-spectacular | 0.27.x | OpenAPI / Swagger / ReDoc |
| LangChain* | ver requirements | Factory em `ai_providers` (chat e embedding separados) |
| pypdf / python-docx / python-pptx | — | Extração de texto |

\* `langchain-core`, `langchain-google-genai`, `langchain-openai`,
`langchain-anthropic`, `langchain-ollama`.

## Convenções (DRF + SimpleJWT)

Com base na documentação atual do [DRF](https://www.django-rest-framework.org/)
e do [Simple JWT](https://django-rest-framework-simplejwt.readthedocs.io/):

1. **Permissão default** exige usuário autenticado (`IsAuthenticated`). Views
   públicas (login/refresh, schema) sobrescrevem `permission_classes`.
2. **`PasswordIsCurrent`** (custom) bloqueia o restante da API enquanto
   `must_change_password` for true — espelha o guard do frontend.
3. **JWT**: header `Authorization: Bearer <access>`. Com
   `ROTATE_REFRESH_TOKENS=True`, cada refresh devolve um par novo; o frontend
   deve persistir o novo refresh (já faz no `api/client.ts`).
4. **Paginação**: `PageNumberPagination`, `PAGE_SIZE=20`.
5. **Throttle de login**: scope `login` = `10/min` (mitiga enumeração de
   identifier).
6. **Schema**: documentação em `apps/*/schema.py`, carregada no
   `AppConfig.ready()` — **não** misturar OpenAPI dentro de `views.py`.

## Camadas por app

```
models.py → serializers.py → views.py → urls.py
                 ↘ services.py / chunking / extraction (regras pesadas)
```

Todo modelo de negócio herda `apps.core.models.TimeStampedModel`.

## Superfície HTTP (resumo)

### `/api/auth/`

| Método | Rota | Quem |
|--------|------|------|
| POST | `login/` | público |
| POST | `login/refresh/` | público |
| GET | `me/` | autenticado |
| POST | `change-password/` | autenticado |
| GET | `courses/` | senha em dia |
| GET | `accounts/` | CSAdmin (`?search`, `?role`) |
| POST | `accounts/students/` | CSAdmin |
| POST | `accounts/students/import/` | CSAdmin (CSV) |
| POST | `accounts/coordinators/` | CSAdmin |
| POST | `accounts/{id}/reset-password/` | CSAdmin |

### `/api/documents/`

| Método | Rota | Quem |
|--------|------|------|
| GET | `/` | autenticado (escopo por papel/curso) |
| POST | `upload/` | admin / coord do curso |
| GET/DELETE | `{id}/` | admin / coord |
| GET | `{id}/chunks/` | admin / coord |
| POST | `{id}/reprocess/` | admin / coord |

## Provedores de IA (`apps/ai_providers`)

Factory (padrão LangChain: classes por provider, escolha por env):

| Função | Env | Providers |
|--------|-----|-----------|
| `get_chat_model()` | `LLM_PROVIDER` / `LLM_MODEL` | gemini, openai, claude, ollama, deepseek*, abacusai* |
| `get_embedding_model()` | `EMBEDDING_PROVIDER` / `EMBEDDING_MODEL` / `EMBEDDING_DIMENSIONS` | **gemini**, **ollama** apenas |

\* DeepSeek e Abacus AI usam `ChatOpenAI` com `base_url` próprio (API
compatível OpenAI).

**Uso real hoje:** embeddings no pipeline de documentos. Chat só exercitado
pelo comando `python manage.py test_ai_provider`.

**Atenção (pgvector):** dimensão do `VectorField` é fixa na migration. Mudar
`EMBEDDING_DIMENSIONS` / modelo exige nova migration + reprocess de todos os
materiais. A extensão `vector` precisa existir no banco (`bootstrap_db` /
`VectorExtension`) — sem isso: `type "vector" does not exist`.

## Gaps conhecidos nesta branch

- App `conversations` (chat RAG) ainda não existe.
- Processamento de documentos é síncrono (sem Celery/RQ).
- README do backend ainda lista `ai_providers` / `conversations` como
  “em construção” na árvore — a factory de providers já está operacional.

## Links oficiais (Context7 / docs)

- DRF permissions & pagination: https://www.django-rest-framework.org/
- Simple JWT settings: https://django-rest-framework-simplejwt.readthedocs.io/
- pgvector Django: https://github.com/pgvector/pgvector-python
- LangChain chat models: https://docs.langchain.com/
