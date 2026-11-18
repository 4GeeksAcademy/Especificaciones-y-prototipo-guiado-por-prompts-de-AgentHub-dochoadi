---
name: error-tracker
description: Registra errores del proyecto en docs/ERRORES.md con causa, solución y verificación. Usar cuando falle un criterio, un test, un comportamiento del navegador o una restricción (style inline, CSS propio, orden de commits).
---

# Error tracker

Añade una fila a la tabla de `docs/ERRORES.md` (ID incremental `ERR-001`…):

| Campo | Contenido |
|---|---|
| ID | ERR-00X |
| Fecha | YYYY-MM-DD |
| Fase | F1…F5 |
| Síntoma | qué se vio (mensaje, captura, criterio E# afectado) |
| Causa raíz | por qué ocurrió (no el síntoma) |
| Solución | cambio aplicado (fichero + hash del commit `fix(F<n>): ERR-00X …`) |
| Verificación | cómo se comprobó que está resuelto |
| Estado | abierto · resuelto · descartado |

Reglas: registrar al detectarlo, no al final; si reaparece, reabrir la misma fila; enlazar con la entrada `P<fase>.<n>` de `docs/PROMPTS.md`; añadir una línea de "Lección" si evita repetirlo.
