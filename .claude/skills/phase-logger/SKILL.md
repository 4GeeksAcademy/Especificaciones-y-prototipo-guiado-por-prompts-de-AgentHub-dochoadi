---
name: phase-logger
description: Registra cada prompt y su salida en docs/PROMPTS.md por fase, con decisión y revisión contra los criterios de evaluación. Usar tras cada prompt significativo.
---

# Phase logger

Tras **cada** prompt relevante añade una entrada al final de `docs/PROMPTS.md` bajo su fase:

```markdown
### P<fase>.<n> — <título corto>  (<YYYY-MM-DD>)
- **Agente/skill**: <spec-writer | …>
- **Prompt** (literal o resumen fiel):
  > …
- **Salida**: <ficheros creados/modificados, resumen de 2-5 líneas>
- **Revisión** (contra `docs/EVALUACION.md`): criterios cubiertos E1, E3… · huecos: …
- **Decisión**: aceptada | iterar (nuevo prompt P<fase>.<n+1>) | descartada — motivo
- **Commit**: `<tipo>(F<n>…): mensaje` · `<hash corto>` (ver `docs/GIT.md`)
- **Tag**: `F<n>-nombre` (solo si cierra fase)
- **Errores**: ver ERR-00X (si aplica)
```

Cierre de sesión/fase: actualizar el estado en `CONTEXT.md` (fase, último commit/tag, próximo paso) para que `docs/RETOMAR.md` funcione.

Reglas: orden cronológico; no borrar entradas (si se itera, nueva entrada); prompts largos van literales en bloque `>`; enlazar commit si existe. Al cerrar fase, actualizar la tabla de estado en `CONTEXT.md` y `docs/FASES.md`.
