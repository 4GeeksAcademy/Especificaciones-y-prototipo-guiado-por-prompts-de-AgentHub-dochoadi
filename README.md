# AgentHub — Prototipo del Panel de Administración

Prototipo HTML (Tailwind CDN + JavaScript vanilla) del panel interno de **AgentHub**, plataforma SaaS de alquiler de agentes de IA. Proyecto guiado por especificación (*Vision to spec*).

## Secciones
Dashboard · Gestión de usuarios · Gestión de agentes · Skills · Contrataciones · Log de errores — con modo claro/oscuro, dropdowns de acciones, modales y listas colapsables. Datos 100 % hardcodeados.

## Flujo de trabajo
1. `SPECS.md` (commit propio, antes de cualquier HTML)
2. Propuesta visual en [Google Stitch](https://stitch.withgoogle.com/) → `docs/stitch/`
3. Construcción de `index.html` adaptando la propuesta a la spec
4. QA y evaluación → `docs/EVALUACION.md`

## Estructura
| Ruta | Contenido |
|---|---|
| `CLAUDE.md` | Reglas del proyecto para Claude |
| `CONTEXT.md` | Producto, stack, dataset maestro, estado |
| `SPECS.md` | Especificación (contrato con el código) |
| `index.html` | Prototipo |
| `.claude/agents/` | spec-writer, stitch-prompter, frontend-builder, ux-reviewer, qa-evaluator |
| `.claude/skills/` | spec-driven, tailwind-vanilla, ui-patterns, phase-logger, error-tracker, git-workflow |
| `docs/FASES.md` | Plan por fases y checklists |
| `docs/GIT.md` | Nomenclatura de commits y tags por fase |
| `docs/CODESPACE.md` | Flujo de trabajo en GitHub Codespaces |
| `docs/RETOMAR.md` | Prompt para reanudar el proyecto en otra sesión |
| `docs/PROMPTS.md` | Bitácora de prompts y salidas |
| `docs/EVALUACION.md` | Matriz de criterios con evidencia |
| `docs/ERRORES.md` | Registro de errores y soluciones |

## Ejecutar
Abrir `index.html` en el navegador (no requiere servidor ni build).
