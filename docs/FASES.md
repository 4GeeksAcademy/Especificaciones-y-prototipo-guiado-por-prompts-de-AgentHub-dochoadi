# Fases del proyecto

Cada fase: ejecutar prompt(s) → registrar en `PROMPTS.md` → revisar contra `EVALUACION.md` → errores a `ERRORES.md` → cerrar.

| Fase | Agente | Entregable | Commit | Puerta de salida |
|---|---|---|---|---|
| F0 Estructura | — | `.claude/`, `docs/`, README/CONTEXT/CLAUDE | `chore: estructura…` | Árbol correcto, plantilla limpiada |
| F1 Spec | spec-writer | `SPECS.md` | `docs: add SPECS.md` (solo ese fichero) | Checklist de `spec-driven` completa; E1, E2 ✅ |
| F2 Stitch | stitch-prompter | `docs/stitch/*` | `docs: propuesta Stitch` | Prompt + capturas + tabla de adaptación |
| F3 Build | frontend-builder | `index.html` | `feat: prototipo…` (puede dividirse por sección) | Las 6 secciones navegables |
| F4 QA | ux-reviewer + qa-evaluator | `EVALUACION.md` completa | `docs: evaluación` | E1–E12 con evidencia ✅ |
| F5 Entrega | — | repo público en GitHub | push | `git log` spec < HTML; enlace verificado |

Mensajes de commit y tags canónicos: `docs/GIT.md`. Trabajo y commits desde Codespaces (`docs/CODESPACE.md`).

## Checklist por fase (resumen)
- **F1**: 6 secciones × ≥3 specs · inventario (7 componentes) · criterios numerados (≥1 por dropdown, modal, colapsable, dark) · dataset coherente.
- **F2**: prompt guardado · capturas light/dark · decisiones de adaptación.
- **F3**: Tailwind CDN · sin `style`/CSS · JS vanilla · sidebar con activo · datos mínimos (5/4/4/4/6).
- **F4**: pruebas en 1440 y 768 px · claro y oscuro · evidencia por criterio.
- **F5**: repo público · `SPECS.md` e `index.html` visibles.
