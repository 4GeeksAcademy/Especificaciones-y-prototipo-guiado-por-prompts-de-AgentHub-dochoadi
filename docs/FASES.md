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

## Estado de cierre (2026-11-19)
| Fase | Estado | Commit(s) | Tag |
|---|---|---|---|
| F0 | cerrada | `ffc7cab` | `F0-estructura` |
| F1 | cerrada | `b3b4d64` (solo `SPECS.md`), `c2e937a` | `F1-spec` |
| F2 | cerrada | `3ebe8f3`, `43c4828`, `a5be32d` | `F2-stitch` |
| F3 | cerrada | `b91ee39` … `ea5fd3d`, cierre `32504d5` | `F3-build` |
| F4 | completa | `d830e18` … `bad921d` (ERR-005…012), `eddb4ad` | `F4-qa` (pendiente del usuario) |
| F5 | lista | `docs(F5): README final y enlace de entrega` | `F5-entrega` (pendiente del usuario) |

Puertas de salida: F3 con las 6 secciones navegables y verificadas por el usuario en navegador; F4 con E1–E12 ✅ y evidencia en `docs/EVALUACION.md`; F5 con repo público, `SPECS.md` e `index.html` en la raíz de `main` y `git log` con SPECS antes del HTML del prototipo.

