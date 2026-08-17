# Frontend — arquitetura e stack

Documentação do submodule `frontend/`, alinhada à branch
**`feat/login-contas-e-materiais`** (`36ea274`, inclui migração Biome).

README operacional: `frontend/README.md`.

## Snapshot da branch

| Item | Estado |
|------|--------|
| Features | `auth`, `accounts`, `documents` |
| Tooling | Biome (lint + format); sem ESLint/Prettier |
| Chat UI | Não existe |
| API base | `VITE_API_BASE_URL` (default `http://localhost:8000`) |

## Stack (`package.json`)

| Lib | Versão | Papel |
|-----|--------|-------|
| React + React DOM | 19.2 | UI |
| Vite | 8.2 | Dev server / build; env só com prefixo `VITE_` |
| TypeScript | ~6 | `tsc -b` no build |
| React Router DOM | 7.18 | `createBrowserRouter` + layouts aninhados |
| TanStack Query | 5.101 | Server state (`useQuery` / `useMutation` object API) |
| React Hook Form | 7.85 | Formulários |
| Zod | 4.4 | Schemas de formulário (+ `@hookform/resolvers`) |
| Tailwind CSS | 4.3 | `@import 'tailwindcss'` + `@theme` (Vite plugin) |
| Biome | 2.5 | `biome check` / `biome check --write --unsafe` |
| Radix | dialog, dropdown, tabs | Acessibilidade pontual |
| Lucide | ícones | — |

## Convenções (Context7)

### Vite 8 — env

Só variáveis `VITE_*` chegam ao client via `import.meta.env`. Tipagem em
`src/vite-env.d.ts`. Segredos **nunca** usam esse prefixo.

### React Router 7 — guards como rotas layout

Rotas aninhadas com `children` + componentes que só renderizam `<Outlet />`
(padrão documentado pelo React Router). No projeto:

```
/login                          → público
ProtectedRoute
  RequirePasswordCurrent
    /change-password
    AppShell
      / → redirect /materiais
      /materiais                → todos autenticados
      RequireRole(admin|coord)
        /materiais/:id
      RequireRole(admin)
        /contas
```

Guards de UI **não** substituem autorização do backend.

### TanStack Query v5

API só com objeto: `useQuery({ queryKey, queryFn })`,
`useMutation({ mutationFn, onSuccess })`,
`queryClient.invalidateQueries({ queryKey })`.
`QueryClientProvider` em `src/app/providers.tsx`.

### Zod 4 + RHF

Schemas em `features/*/schemas.ts`. Zod 4 mudou refinements internamente
(`checks` em vez de wrapper `ZodEffects`); a API de schemas usada com
`zodResolver` continua o caminho padrão.

### Tailwind 4

CSS-first: `src/index.css` com `@import 'tailwindcss'` e tokens em `@theme`
(`brand-navy`, `brand-orange`). Sem `tailwind.config.js` clássico.

### Biome

- `npm run lint` → `biome check .`
- `npm run format` → `biome check --write --unsafe .`
- Ordenação de classes: regra nursery `useSortedClasses` (+ função `cn`)
- Preferir pin exato da versão do Biome em upgrades (recomendação oficial)

## Estrutura

```
src/
  app/           router, providers, App
  api/           client (fetch + refresh), endpoints/, types/
  features/
    auth/        login, change-password, AuthContext
    accounts/    listagem/criação/import/reset (CSAdmin)
    documents/   lista, upload, detalhe, chunks
  shared/
    auth/        ProtectedRoute, RequireRole, RequirePasswordCurrent
    layout/      AppShell, Sidebar, UserMenu
    ui/          Button, Input, Dialog, …
    hooks/ lib/
```

## Auth no client

1. Login → tokens em `localStorage` (`cesucacode.access` / `cesucacode.refresh`)
2. `getMe` hidrata o usuário no `AuthProvider`
3. `api/client.ts` envia Bearer; em **401** faz um único refresh concorrente
   em `/api/auth/login/refresh/` e repete a request
4. Com `ROTATE_REFRESH_TOKENS` no backend, o client **precisa** gravar o novo
   refresh (já implementado)

## Escopo por papel (UI × API)

| Papel | `/materiais` | `/materiais/:id` | `/contas` |
|-------|--------------|------------------|-----------|
| CSAdmin | sim | sim | sim |
| CSCoordinator | sim (cursos que coordena na API) | sim | não |
| CSStudent | sim (próprio curso) | não (rota bloqueada) | não |

## Gaps nesta branch

- Sem testes automatizados (verificação manual contra backend).
- Sem telas de chat/RAG.
- Comentário residual `eslint-disable` possível em arquivos antigos — Biome
  é a ferramenta oficial agora.

## Links oficiais (Context7 / docs)

- Vite env: https://vite.dev/guide/env-and-mode.html
- React Router routing: https://reactrouter.com/
- TanStack Query v5: https://tanstack.com/query/latest
- Zod: https://zod.dev/
- Biome: https://biomejs.dev/
- Tailwind v4: https://tailwindcss.com/docs
