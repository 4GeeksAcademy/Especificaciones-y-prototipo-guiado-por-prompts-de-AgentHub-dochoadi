# AgentHub — Prototipo del panel de administración

Prototipo navegable del panel interno de **AgentHub**, una plataforma SaaS en la que empresas **alquilan agentes de IA** preconfigurados y los equipan con **skills** (navegar la web, leer documentos, gestionar calendarios, enviar emails, consultar bases de datos). El panel lo usa el **administrador interno** de AgentHub para supervisar usuarios, agentes, skills, contratos e incidencias.

El proyecto sigue un enfoque **spec-first** (*Vision to spec*): primero se escribió y commiteó la especificación [`SPECS.md`](SPECS.md) y después se construyó el HTML a partir de ella.

- **Repositorio**: <https://github.com/4GeeksAcademy/Especificaciones-y-prototipo-guiado-por-prompts-de-AgentHub-dochoadi>
- **Especificación (contrato)**: [`SPECS.md`](SPECS.md)
- **Prototipo**: [`index.html`](index.html)

## Cómo verlo

No necesita instalación, build ni backend.

- **Opción 1**: abrir `index.html` directamente en el navegador.
- **Opción 2** (recomendada, igual que en Codespaces): servir la carpeta y abrir el puerto 8000.
  ```bash
  python3 -m http.server 8000
  # http://localhost:8000/index.html  (en Codespaces: pestaña «Ports» → 8000 → «Open in Browser»)
  ```

Se puede entrar directamente a una sección con su hash, p. ej. `index.html#errores`. El modo oscuro se activa con el botón de la barra superior y se recuerda entre visitas.

## Las 6 secciones

| Sección | Qué muestra | Interacciones |
|---|---|---|
| **Dashboard** | 4 tarjetas de métricas (ingresos del mes, descuentos, agentes activos, agentes fallando) y área para el gráfico semanal | La tarjeta «Agentes fallando» lleva al Log |
| **Usuarios** | 7 usuarios con email, plan y estado | Menú ⋮: Ver detalle (modal de 10 campos) · Eliminar |
| **Agentes** | 6 agentes con propietario, estado y skills | Lista de skills colapsable con transición · ⋮: Configurar (modal con el prompt de sistema editable) · Eliminar |
| **Skills** | Explicación «¿Qué es una skill?» y catálogo de 5 skills con precio y nº de agentes | ⋮: Ver detalle (permisos y agentes) · Eliminar |
| **Contrataciones** | 6 contratos con cliente, agente, skills, fechas e importe | ⋮: Ver detalle (desglose por skill con subtotal y total) · Descargar factura (sin lógica) |
| **Log de errores** | 8 incidencias ordenadas por fecha, con gravedad por color | ⋮: Ver detalle (traza completa) · Marcar como resuelto (actualiza el contador del sidebar) |

Todos los datos están **hardcodeados** en constantes JS (`SKILLS`, `AGENTES`, `USUARIOS`, `CONTRATOS`, `ERRORES`) y son coherentes entre secciones: los mismos agentes, clientes y skills aparecen en todas, y los importes y contadores se calculan a partir de ellos (dataset en `SPECS.md` §10).

## Stack y restricciones

- **HTML5 semántico** (`header`, `nav`, `main`, `section`, `table`/`thead`/`tbody`, `role="dialog"`).
- **Tailwind CSS vía CDN** (`darkMode: 'class'`). Sin CSS propio, sin atributos `style` y sin hojas externas.
- **JavaScript vanilla** en un único `<script>`: sin frameworks, sin librerías, sin `fetch`.
- Iconos en un **sprite SVG inline**.
- Diseñado para **escritorio (1440 px) y tablet (768 px)**: en tablet el sidebar pasa a un rail de iconos y las tablas caben sin scroll horizontal.
- Accesibilidad revisada en F4: foco visible, trampa de foco en el modal, `aria-expanded`/`aria-controls`, recuentos con `aria-live` y contraste AA.

## Estructura del repositorio

| Ruta | Contenido |
|---|---|
| [`SPECS.md`](SPECS.md) | Especificación: producto, stack, layout, specs por sección, dataset, componentes, criterios de aceptación CA-01…CA-45 |
| [`index.html`](index.html) | Prototipo (único fichero) |
| [`CONTEXT.md`](CONTEXT.md) | Contexto del producto y estado del proyecto |
| [`CLAUDE.md`](CLAUDE.md) | Reglas del proyecto para el asistente |
| [`.claude/agents/`](.claude/agents/) | Agentes: spec-writer, stitch-prompter, frontend-builder, ux-reviewer, qa-evaluator |
| [`.claude/skills/`](.claude/skills/) | Skills: spec-driven, tailwind-vanilla, ui-patterns, phase-logger, error-tracker, git-workflow |
| [`docs/stitch/`](docs/stitch/) | Propuesta visual de Google Stitch por pantalla y su [adaptación a la spec](docs/stitch/adaptacion.md) |
| [`docs/EVALUACION.md`](docs/EVALUACION.md) | Matriz de criterios E1–E12 con evidencia y mejoras futuras |
| [`docs/PROMPTS.md`](docs/PROMPTS.md) | Bitácora de cada prompt, su salida, verificación y commit |
| [`docs/ERRORES.md`](docs/ERRORES.md) | Registro de errores con causa, solución y verificación |
| [`docs/FASES.md`](docs/FASES.md) | Plan por fases y checklists |
| [`docs/GIT.md`](docs/GIT.md) | Convención de commits y tags |
| [`docs/CODESPACE.md`](docs/CODESPACE.md) | Flujo de trabajo en GitHub Codespaces |
| [`docs/RETOMAR.md`](docs/RETOMAR.md) | Prompt para retomar el proyecto en otra sesión |

## Flujo spec-first

| Fase | Entregable | Commits clave | Tag |
|---|---|---|---|
| F0 Estructura | Agentes, skills, docs y limpieza de la plantilla | `ffc7cab` | `F0-estructura` |
| F1 Spec | `SPECS.md` en un commit propio, antes de cualquier HTML | `b3b4d64` | `F1-spec` |
| F2 Stitch | Propuesta visual y adaptación a la spec (`docs/stitch/`) | `3ebe8f3`, `43c4828`, `a5be32d` | `F2-stitch` |
| F3 Construcción | `index.html`, una sección por commit | `b91ee39` layout · `c4aea9f` Dashboard · `9b370ba` Usuarios · `e1d95c5` Agentes · `b213a33` Skills · `3e4116b` Contrataciones · `ea5fd3d` Log | `F3-build` |
| F4 QA | Revisión UX/accesibilidad (8 correcciones ERR-005…ERR-012) y evaluación E1–E12 | `d830e18` … `bad921d` · `eddb4ad` evaluación | `F4-qa` |
| F5 Entrega | Este README y repositorio público | `docs(F5): README final y enlace de entrega` | `F5-entrega` |

Cada prompt quedó registrado en [`docs/PROMPTS.md`](docs/PROMPTS.md) con su verificación (greps de restricciones, pruebas en DOM simulado y verificación visual en navegador) y cada incidencia en [`docs/ERRORES.md`](docs/ERRORES.md).

### Aclaración sobre el orden SPECS → HTML (criterio E1)

`git log --reverse --name-status` muestra un `A index.html` en el commit `0dfaf38` («add initial»), **anterior** a `SPECS.md`. Ese fichero pertenece a la **plantilla de 4Geeks Academy** de la que parte el repositorio y se **eliminó** en `a9b4fe4` («delete index»), antes de empezar el proyecto (F0). En este proyecto, `SPECS.md` se añade en `b3b4d64` y el primer `index.html` del prototipo en `b91ee39`, que es posterior. `SPECS.md` no se ha modificado desde entonces.

```bash
git log --reverse --name-status --format='%h %s' -- SPECS.md index.html
```

## Evaluación

El estado de cada criterio (E1–E12) con su evidencia está en [`docs/EVALUACION.md`](docs/EVALUACION.md).
