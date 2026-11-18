---
name: git-workflow
description: Nomenclatura de commits, tags por fase y comandos de commit/push en Codespaces para AgentHub. Usar al preparar cualquier commit, cerrar una fase o verificar que SPECS.md precede al HTML.
---

# Git workflow (Codespaces)

Fuente de verdad: `docs/GIT.md`. Resumen:

- Formato: `<tipo>(F<n>[-ámbito]): <resumen imperativo en español>`; tipos `chore docs feat fix style test refactor`.
- `fix` cita `ERR-00X`; commits de prompt citan `P<fase>.<n>` en el cuerpo.
- Rama `main`; push tras cada commit; `git add` de ficheros concretos, tras `git status && git diff --stat`.
- Cierre de fase: `git tag -a F<n>-<nombre> -m "…" && git push origin F<n>-<nombre>` (`F0-estructura`, `F1-spec`, `F2-stitch`, `F3-build`, `F4-qa`, `F5-entrega`).
- `docs(F1): añade SPECS.md` es un commit propio, anterior a cualquier `.html`. Verificar: `git log --reverse --name-status --format='%h %s'`.
- Nunca `--force`, nunca reescribir historial ya pusheado.
- Tras cada commit, anotar mensaje + hash (`git log -1 --format=%h`) en `docs/PROMPTS.md` (skill `phase-logger`).
