# Bitácora de prompts y salidas

Formato de entrada: ver skill `.claude/skills/phase-logger/SKILL.md`. Orden cronológico, sin borrar.

## F0 — Estructura

### P0.1 — Planificación de estructura Claude (2026-11-18)
- **Agente/skill**: sesión principal (modo plan)
- **Prompt**:
  > Planificar plataforma de alquiler de agentes IA (panel admin). Actuar como experto Backend/Frontend/UX/TS/CSS. Crear estructura claude (CONTEXT, README, Agents, Skills) con las skills instaladas. Limpiar el repo de plantilla (4Geeks), documentar cada prompt por fases, documento de evaluación según criterios (imagen 3) y fichero de errores.
- **Salida**: `CLAUDE.md`, `CONTEXT.md`, `README.md`, 5 agentes, 5 skills, `docs/` (FASES, PROMPTS, EVALUACION, ERRORES); eliminados `.vscode/`, `README.cn.md`, `README.es.md`, `learn.json`, `server.py`.
- **Revisión**: estructura lista; EVALUACION.md contiene los 12 criterios (todos pendientes).
- **Decisión**: aceptada.
- **Commit**: `chore(F0): estructura Claude y limpieza de plantilla` · _hash: completar desde Codespace_
- **Tag**: `F0-estructura`
- **Errores**: ninguno.

### P0.2 — Adaptación a Codespaces y nomenclatura Git (2026-11-18)
- **Agente/skill**: sesión principal
- **Prompt**:
  > El proyecto se correrá en Codespaces: generar aquí los prompts, copiarlos al Codespace y hacer commit/push desde allí con nomenclatura por cambio y fase para documentar todo.
- **Salida**: `docs/GIT.md`, `docs/CODESPACE.md`, `.devcontainer/devcontainer.json`, skill `git-workflow`, `bootstrap-f0.sh` (recrea toda la estructura en el Codespace); `CLAUDE.md`, `phase-logger` y `error-tracker` actualizados.
- **Revisión**: soporta E1 (orden de commits verificable) y trazabilidad prompt→commit.
- **Decisión**: aceptada.
- **Commit**: incluido en `chore(F0)…` · _hash: completar_
- **Errores**: ninguno.

## F1 — Especificación
_(pendiente)_

## F2 — Google Stitch
_(pendiente)_

## F3 — Construcción
_(pendiente)_

## F4 — QA
_(pendiente)_

## F5 — Entrega
_(pendiente)_
