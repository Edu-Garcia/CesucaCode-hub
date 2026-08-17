# AGENTS.md — CesucaCode Hub

Este repositório é um **hub de agregação**. O código de produto vive nos submodules; o hub guarda contexto compartilhado, docs e skills.

## Mapa do workspace

| Caminho | O que é | Onde commitar |
|---------|---------|---------------|
| `frontend/` | App frontend (submodule) | Repo [CesucaCode-frontend](https://github.com/jvpgjava/CesucaCode-frontend) |
| `backend/` | API/backend (submodule) | Repo [CesucaCode-backend](https://github.com/jvpgjava/CesucaCode-backend) |
| Raiz (`docs/`, `.agents/`, `AGENTS.md`, …) | Hub | Este repositório |

## Regras para agentes

1. **Não trate os submodules como pastas normais do hub.** Alterações de código em `frontend/` ou `backend/` devem ser commitadas **dentro** do respectivo submodule, no remote correto.
2. **Commits do hub** só devem incluir: docs, skills, `AGENTS.md`, `README.md`, `.gitmodules`, config do hub — e **ponteiros de submodule apenas quando o time pedir** para atualizar o pin default de `main`.
3. **Nunca** copie o conteúdo dos modules para a árvore do hub como arquivos comuns — o hub apenas agrega.
4. Checkout de feature branch / commits locais nos submodules **não** devem ser commitados no hub. Os submodules usam `ignore = all` + `branch = main` no `.gitmodules`.
5. Ao trabalhar em feature full-stack, trabalhe e faça push **dentro** de cada submodule; mantenha históricos separados.
6. Prefira as skills em `.agents/skills/` para o fluxo idea → ship.
## Agent skills

### Issue tracker

Issues do hub: markdown local em `.scratch/issues/` (ver `docs/agents/issue-tracker.md`). Issues de produto podem viver nos remotes do frontend/backend conforme o time definir.

### Domain docs

Layout single-context: `CONTEXT.md` na raiz + fichas `docs/backend.md` /
`docs/frontend.md` + ADRs em `docs/adr/`. Ver `docs/agents/domain.md`.

### Skills instaladas

- `grill-with-docs` — afiar plano/design e gerar ADRs/glossário
- `to-spec` — sintetizar a conversa em spec
- `to-tickets` — quebrar em tickets com dependências
- `implement` — implementar a partir de spec/tickets
- `code-review` — review Standards + Spec
- `prototype` — protótipo descartável

Fonte: [aihero.dev/skills](https://www.aihero.dev/skills) / [mattpocock/skills](https://github.com/mattpocock/skills).
