# Registro de errores

Cómo registrar: skill `.claude/skills/error-tracker/SKILL.md`. Enlazar con la entrada de `PROMPTS.md`.

| ID | Fecha | Fase | Síntoma | Causa raíz | Solución | Verificación | Estado |
|---|---|---|---|---|---|---|---|
| ERR-000 | 2026-11-18 | F0 | `git clone` en carpeta no vacía no es posible | La carpeta de trabajo ya existía | Clonar a `_repo` temporal y mover `.git` | `git status` muestra la plantilla borrada | resuelto |
| ERR-001 | 2026-11-18 | F2 | Los agentes referencian skills no instaladas en el Codespace (`frontend-design`, `ui-ux-pro-max`, `impeccable`, `design:accessibility-review`, `design:design-critique`, `webapp-testing`, `ponytail`) | Esas skills estaban instaladas solo en el entorno local donde se planificó, no en el Codespace | Referencias condicionales ("si está instalada") + reglas de respaldo en cada agente (`stitch-prompter`, `frontend-builder`, `ux-reviewer`, `qa-evaluator`) · commit: `211eabf` | `grep -n 'si está instalada' .claude/agents/*.md` | resuelto |
| ERR-002 | 2026-11-18 | F1 | Los 5 agentes del proyecto no aparecían como subagentes | La sesión de Claude Code se abrió antes de crear `.claude/agents/` | Developer: Reload Window y chat nuevo; mientras tanto, leer el `.md` del agente y seguirlo · commit: `211eabf` | El agente aparece en el chat nuevo (o se siguió su `.md`) | resuelto |
| ERR-003 | 2026-11-18 | F2 | Los prompts de Stitch no incluían la restricción sin `style`/CSS externo y la exportación trae `style` inline, hojas externas y tokens Material | El prompt global no declaraba la restricción (R3) | No copiar su código; adaptar con `docs/stitch/adaptacion.md` (paleta Tailwind estándar, fuente del sistema, sprite SVG inline, JS con `data-*`) · commit: `a5be32d` | `grep -nE 'style=|<style|rel="stylesheet"' index.html` → 0 en el futuro `index.html` | resuelto |

## Lecciones
- Registrar el error al detectarlo, con la causa raíz (no solo el síntoma).
