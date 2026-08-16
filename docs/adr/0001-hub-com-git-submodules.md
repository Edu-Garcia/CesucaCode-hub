# ADR 0001 — Hub com Git submodules

## Status

Aceito

## Contexto

O CesucaCode tem frontend e backend em repositórios separados, cada um com seu próprio histórico de commits. Precisamos de um lugar único para desenvolver/documentar o sistema como um todo sem misturar os históricos.

## Decisão

Usar este repositório como **hub** que agrega:

- `frontend` → https://github.com/jvpgjava/CesucaCode-frontend.git
- `backend` → https://github.com/jvpgjava/CesucaCode-backend.git

via Git submodules. O hub versiona apenas o SHA apontado por cada module, mais docs, skills e configuração de agentes.

## Consequências

- Clone do hub exige `--recurse-submodules` (ou `submodule update --init`).
- Commits de produto continuam nos remotes dos modules.
- O hub pode fixar revisões conhecidas dos modules para o time.
- Conteúdo dos modules **não** entra como blob no histórico do hub.
