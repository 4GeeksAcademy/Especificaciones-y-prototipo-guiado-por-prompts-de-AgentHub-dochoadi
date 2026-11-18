# Registro de errores

Cómo registrar: skill `.claude/skills/error-tracker/SKILL.md`. Enlazar con la entrada de `PROMPTS.md`.

| ID | Fecha | Fase | Síntoma | Causa raíz | Solución | Verificación | Estado |
|---|---|---|---|---|---|---|---|
| ERR-000 | 2026-11-18 | F0 | `git clone` en carpeta no vacía no es posible | La carpeta de trabajo ya existía | Clonar a `_repo` temporal y mover `.git` | `git status` muestra la plantilla borrada | resuelto |

## Lecciones
- Registrar el error al detectarlo, con la causa raíz (no solo el síntoma).
