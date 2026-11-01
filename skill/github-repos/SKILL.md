---
name: github-repos
description: Use when working with GitHub repositories from opencode — clone or download a repo, create and upload a new one, pull, push, branches, commits, pull requests, issues or releases. Trigger on "clona", "baja el repositorio", "sube a GitHub", "haz push", "abre un PR", "mis repos".
---

# GitHub desde opencode

Gestiona repositorios con `git` y `gh`. Verifica antes de actuar; nunca destruyas historial sin confirmación.

## Requisitos

1. `git --version` y `gh --version`.
2. `gh auth status`. Si no está autenticado, **pide al usuario** que ejecute en su terminal:
   ```
   gh auth login
   ```
   Abre el navegador; no intentes autenticarte tú. Con varias cuentas: `gh auth switch --user <cuenta>`.

Si `gh` no está en PATH, usa la ruta donde lo instalaste (por ejemplo `%USERPROFILE%\.local\bin\gh\gh.exe` en Windows).

## Bajar un repositorio

```
gh repo list <usuario> --limit 50          # ver repos
gh repo clone <owner>/<repo>               # clona en ./<repo>
gh repo clone <owner>/<repo> <carpeta>     # en otra carpeta
```

Si ya existe local, no clones otra vez: `git pull --ff-only`.

## Subir un repositorio nuevo

```
git init
git add .
git commit -m "chore: initial commit"
gh repo create <nombre> --private --source=. --remote=origin --push
```

Pregunta público/privado si no lo dijo. Nunca subas `.env`, claves, credenciales ni `node_modules`.

## Trabajar en un repo

```
git status
git pull --ff-only
git checkout -b feat/<descripcion-corta>
# editar
git add -p
git commit -m "feat: <descripcion>"
git push -u origin feat/<descripcion-corta>
```

- Una tarea = una rama.
- Commits atómicos; mensajes `tipo: descripción` (`feat`, `fix`, `refactor`, `docs`, `test`, `chore`).
- Antes de commitear: `git status` y `git diff --staged` para no colar secretos.

## Pull requests

```
gh pr create --title "<título>" --body "<qué y por qué>"
gh pr status
gh pr view <n> --web
gh pr diff <n>
gh pr checkout <n>
```

## Issues

```
gh issue list
gh issue view <n>
gh issue create --title "<título>" --body "<detalle>"
```

## Reglas de seguridad

- **Nunca** `git push --force` a `main`/`master`.
- `--force-with-lease` solo en ramas propias y avisando.
- Pide confirmación antes de: `reset --hard`, `clean -fd`, `branch -D`, `push --force`, `gh repo delete`, `gh release delete`.
- Verifica remoto y rama antes de push: `git remote -v`, `git branch --show-current`.
- No reescribas historial en ramas compartidas.
- Si el repo tiene `AGENTS.md` o `CLAUDE.md`, respeta sus reglas.

## Diagnóstico

- `gh auth status` → sesión.
- `git remote -v` → remoto configurado.
- `git log --oneline -5` → últimos commits.
- `git status` → cambios pendientes.

## Al terminar

Reporta: repo, rama, commit(s), si hiciste push y el enlace del PR si existe.
