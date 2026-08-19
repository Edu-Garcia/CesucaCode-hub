# Domain docs

**Layout:** single-context

| Artefato | Caminho |
|----------|---------|
| Contexto / glossário | `CONTEXT.md` (raiz do hub) |
| Backend (arquitetura) | `docs/backend.md` |
| Frontend (arquitetura) | `docs/frontend.md` |
| ADRs | `docs/adr/` |

## Regras para consumidores (skills / agentes)

1. Leia `CONTEXT.md` antes de nomear entidades de domínio ou propor mudanças de modelo.
2. Ao decidir algo arquitetural relevante, registre um ADR em `docs/adr/`.
3. Não invente termos que contradigam o glossário; se o glossário estiver incompleto, atualize-o (ex.: via `/grill-with-docs`).
4. Código de produto continua nos submodules; o domínio compartilhado e decisões de integração frontend↔backend podem ficar documentados aqui.
5. **Git:** alterar docs não implica commit nem PR. Só versionar quando o usuário pedir explicitamente (ver regra 7 em `AGENTS.md`).
