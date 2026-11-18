# CLAUDE.md — AgentHub Admin Panel

Lee primero `CONTEXT.md`. Este proyecto es **spec-first**: nada de HTML hasta que `SPECS.md` esté commiteado.

## Reglas duras
1. **Orden Git**: commit de `SPECS.md` ANTES de cualquier `.html`. Commit separado, sin otros ficheros.
2. **Stack**: HTML + Tailwind CSS vía CDN + JavaScript vanilla. Sin frameworks, sin jQuery, sin build, sin backend.
3. **Sin CSS propio ni atributos `style`**. Solo utilidades Tailwind. Modo oscuro con `dark:` (`darkMode: 'class'` en `tailwind.config` inline del CDN).
4. **Un único `index.html`** con las 6 secciones (Dashboard, Usuarios, Agentes, Skills, Contrataciones, Log de errores) y sidebar persistente.
5. **Datos hardcodeados y coherentes**: el mismo nombre de agente/cliente en Agentes, Contrataciones y Errores (ver `CONTEXT.md` → dataset maestro).
6. **HTML semántico**: `header`, `nav`, `main`, `section`, `table`/`thead`/`tbody`, `button`, `dialog` o `role="dialog"`.
7. Idioma de UI y docs: español.

## Entorno y Git
Se trabaja en **GitHub Codespaces**; los commits/push se hacen allí. Nomenclatura `<tipo>(F<n>[-ámbito]): resumen` y tags por fase: ver `docs/GIT.md` y skill `git-workflow`. Cada prompt se entrega como ① prompt ② verificación ③ commit+push.

## Cierre de sesión / reanudación
Antes de terminar una sesión o fase: actualizar el estado en `CONTEXT.md`, registrar el prompt+commit en `docs/PROMPTS.md`, commit+push. Para retomar otro día: usar el prompt de `docs/RETOMAR.md`.

## Documentación obligatoria (cada fase)
- Cada prompt y su salida → `docs/PROMPTS.md` (skill `phase-logger`).
- Cada error y su solución → `docs/ERRORES.md` (skill `error-tracker`).
- Al cerrar fase, actualizar `docs/EVALUACION.md` con evidencia.

## Agentes (`.claude/agents/`)
`spec-writer` → `stitch-prompter` → `frontend-builder` → `ux-reviewer` → `qa-evaluator`

## Skills propias (`.claude/skills/`)
`spec-driven`, `tailwind-vanilla`, `ui-patterns`, `phase-logger`, `error-tracker`, `git-workflow`.
Skills instaladas a reutilizar: `frontend-design`, `ui-ux-pro-max`, `impeccable`, `design:accessibility-review`, `design:design-critique`, `webapp-testing`, `ponytail`.
