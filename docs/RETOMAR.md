# Retomar el proyecto en una nueva sesión

Pega este prompt al iniciar una sesión nueva (Claude Code en Codespace, o la sesión de planificación) para que Claude recupere todo el contexto **sin modificar nada**.

## Prompt de reanudación

```text
Vamos a retomar el proyecto AgentHub (prototipo del panel de administración, enfoque spec-first, trabajo en GitHub Codespaces). NO modifiques ningún fichero hasta que yo lo confirme.

1. Lee, en este orden: CLAUDE.md, CONTEXT.md, docs/FASES.md, docs/GIT.md, docs/CODESPACE.md.
2. Lee las ÚLTIMAS 3 entradas de docs/PROMPTS.md, las filas con estado "abierto" de docs/ERRORES.md y el estado actual de docs/EVALUACION.md (criterios E1–E12 y checklist).
3. Si existen, lee SPECS.md, docs/stitch/* e index.html (solo para conocer su estado).
4. Ejecuta: git status, git log --oneline -15, git tag, y git log --reverse --name-status --format='%h %s' (para comprobar que SPECS.md precede a cualquier .html).
5. Revisa las memorias del proyecto si tienes acceso a ellas.

Después respóndeme SOLO con:
- Fase actual y última fase cerrada (último commit y tag).
- Criterios E1–E12 pendientes o fallidos y errores abiertos.
- Cambios sin commitear, si los hay.
- El SIGUIENTE prompt a ejecutar, en formato ① prompt para Claude Code, ② verificación bash, ③ commit+push con la nomenclatura de docs/GIT.md.
Espera mi confirmación antes de hacer nada.
```

## Antes de cerrar cada sesión o fase (para que este prompt siga funcionando)
1. Actualizar la tabla de estado de `CONTEXT.md` (fase, último commit/tag, próximo paso).
2. Registrar el prompt ejecutado y su commit en `docs/PROMPTS.md` (skill `phase-logger`).
3. Actualizar `docs/EVALUACION.md` y `docs/ERRORES.md` si hubo cambios.
4. Commit + push de esos cambios (`docs(F<n>): actualiza estado y bitácora`).
