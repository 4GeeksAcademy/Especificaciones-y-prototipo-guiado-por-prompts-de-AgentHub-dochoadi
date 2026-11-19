# Bitácora de prompts y salidas

Formato de entrada: ver skill `.claude/skills/phase-logger/SKILL.md`. Orden cronológico, sin borrar.

## F0 — Estructura

### P0.1 — Planificación de estructura Claude (2026-11-18)
- **Agente/skill**: sesión principal (modo plan)
- **Prompt**:
  > Planificar plataforma de alquiler de agentes IA (panel admin). Actuar como experto Backend/Frontend/UX/TS/CSS. Crear estructura claude (CONTEXT, README, Agents, Skills) con las skills instaladas. Limpiar el repo de plantilla (4Geeks), documentar cada prompt por fases, documento de evaluación según criterios (imagen 3) y fichero de errores.
- **Salida**: `CLAUDE.md`, `CONTEXT.md`, `README.md`, 5 agentes, 5 skills, `docs/` (FASES, PROMPTS, EVALUACION, ERRORES); eliminados `.vscode/`, `README.cn.md`, `README.es.md`, `learn.json`, `server.py`.
- **Revisión**: estructura lista; EVALUACION.md contiene los 12 criterios (todos pendientes).
- **Decisión**: aceptada.
- **Commit**: `chore(F0): estructura Claude y limpieza de plantilla` · _hash: completar desde Codespace_
- **Tag**: `F0-estructura`
- **Errores**: ninguno.

### P0.2 — Adaptación a Codespaces y nomenclatura Git (2026-11-18)
- **Agente/skill**: sesión principal
- **Prompt**:
  > El proyecto se correrá en Codespaces: generar aquí los prompts, copiarlos al Codespace y hacer commit/push desde allí con nomenclatura por cambio y fase para documentar todo.
- **Salida**: `docs/GIT.md`, `docs/CODESPACE.md`, `.devcontainer/devcontainer.json`, skill `git-workflow`, `bootstrap-f0.sh` (recrea toda la estructura en el Codespace); `CLAUDE.md`, `phase-logger` y `error-tracker` actualizados.
- **Revisión**: soporta E1 (orden de commits verificable) y trazabilidad prompt→commit.
- **Decisión**: aceptada.
- **Commit**: incluido en `chore(F0)…` · _hash: completar_
- **Errores**: ninguno.

## F1 — Especificación

### P1.1 — Redacción de SPECS.md (2026-11-18)
- **Agente/skill**: `spec-writer` (subagente) · skills `spec-driven`, `git-workflow`
- **Prompt** (resumen fiel; literal en la sesión):
  > Fase F1 — Especificación. Usa el agente `spec-writer` y las skills `spec-driven` y `git-workflow`. Lee antes CLAUDE.md y CONTEXT.md. OBJETIVO: crear SPECS.md en la raíz. NO escribas HTML ni hagas commits. Debe poder implementarlo un agente de código sin hacer preguntas.
  > CONTEXTO: SaaS de alquiler de agentes IA equipables con skills; usuario = administrador interno; único index.html con Tailwind CDN + JS vanilla, sin frameworks/jQuery/build/backend, sin CSS propio ni `style`, modo oscuro con `dark:` (darkMode 'class'); español; escritorio y tablet.
  > ESTRUCTURA OBLIGATORIA: 1 Producto y admin · 2 Stack y restricciones · 3 Layout global (sidebar 6 enlaces + sección activa, topbar con toggle persistente en localStorage, tablet) · 4–9 Dashboard, Usuarios, Agentes, Skills, Contrataciones, Log de errores, cada una con ≥3 specs numeradas (S4.1…) con componente, contenido, comportamiento y clases Tailwind orientativas · 10 Datos hardcodeados · 11 Inventario (sidebar, tarjeta de métrica, dropdown ⋮, modal, badge, lista colapsable, toggle dark) · 12 Criterios CA-01… (dropdown, modal botón/backdrop/Esc, colapsable, dark persistente, sin style/CSS, HTML semántico, 6 secciones, "Ver detalle" en ≥4 secciones, datos coherentes, responsive) · 13 Trazabilidad enunciado→spec y E1–E12→CA.
  > REQUISITOS POR SECCIÓN: Dashboard 4 tarjetas + placeholder de gráfico semanal; Usuarios tabla + ⋮ Ver detalle/Eliminar; Agentes con skills colapsadas + Configurar (textarea con prompt de sistema)/Eliminar; Skills catálogo + bloque explicativo + Ver detalle/Eliminar; Contrataciones con desglose por skill + Descargar factura; Log de errores con badge de gravedad + Ver detalle (traza)/Marcar como resuelto.
  > DATASET MAESTRO: SK-01 Navegación web 25 € · SK-02 Lectura de documentos 30 € · SK-03 Gestión de calendario 20 € · SK-04 Envío de emails 15 € · SK-05 Consulta SQL 35 €. Agentes Atlas/Orion/Sirius/Lyra/Nova/Vega (propietario, estado, skills fijados); contadores activos=3, fallando=2, agentes por skill 3/3/2/3/2. ≥6 usuarios (+ Stark Labs), ≥5 contratos (importe = Σ precio skills × meses), ≥7 errores (solo Nova, Vega, Orion, Atlas, Lyra; ≥1 resuelto). AL TERMINAR: autocomprobación, entrada P1.1 en PROMPTS.md, actualizar tabla de estado de CONTEXT.md; no tocar otros ficheros ni crear HTML.
- **Salida**: `SPECS.md` (569 líneas, §1–§14). Specs: Layout S3.1–S3.8, Dashboard S4.1–S4.5, Usuarios S5.1–S5.5, Agentes S6.1–S6.6, Skills S7.1–S7.6, Contrataciones S8.1–S8.5, Errores S9.1–S9.5. Inventario §11.1–§11.7. Criterios CA-01…CA-45. Trazabilidad §13.1 y §13.2; autocomprobación §14.
  Dataset: 5 skills, 6 agentes, 7 usuarios, 6 contratos (CT-01 660 €, CT-02 900 €, CT-03 210 €, CT-04 360 €, CT-05 45 €, CT-06 390 €), 8 errores LOG-001…008 (2 resueltos). Dashboard: ingresos del mes 245 € (cuotas de los contratos activos), descuentos 31,50 €, activos 3, fallando 2.
- **Revisión** (contra `docs/EVALUACION.md`):
  - E1: preparado; `SPECS.md` aún sin commit y sin ningún `.html` en el repo. Se cumple al hacer el commit propio antes de F3.
  - E2: cubierto (≥3 specs/sección, inventario de 7 componentes, CA numerados con al menos uno por dropdown CA-15–19, modal CA-20–24, colapsable CA-25–27, dark CA-40–42).
  - Verificado a mano: aritmética de contratos e ingresos, contadores por estado/skill/empresa, errores solo de agentes permitidos.
  - Huecos/supuestos a vigilar: se añadió un 6.º contrato CT-06 Globex–Vega (activo), no pedido explícitamente; los descuentos no restan del importe de los contratos; "Eliminar" usa `confirm()` nativo; las tablas no usan `overflow-x-auto` (se diseñan para caber en 768 px), lo que se desvía de la skill `tailwind-vanilla`.
- **Decisión**: aceptada (pendiente de revisión del usuario de los supuestos anteriores).
- **Commit**: `docs(F1): añade SPECS.md` · `b3b4d64`
- **Tag**: `F1-spec`
- **Errores**: ninguno.

## F2 — Google Stitch

### P2.1 — Corrección ERR-001 y prompts de Google Stitch (2026-11-18)
- **Agente/skill**: sesión principal (tarea A) · `stitch-prompter` como subagente (tarea B) · skills `tailwind-vanilla`, `error-tracker`, `phase-logger`
- **Prompt** (resumen fiel; literal en la sesión):
  > Fase F2, parte 1 (P2.1). Lee CLAUDE.md, CONTEXT.md, docs/GIT.md y SPECS.md. No crees HTML. No hagas commits.
  > TAREA A — Corregir ERR-001: las skills frontend-design, ui-ux-pro-max, impeccable, design:accessibility-review, design:design-critique, webapp-testing y ponytail no están instaladas en el Codespace. En stitch-prompter, frontend-builder, ux-reviewer y qa-evaluator, cada referencia debe decir "usa la skill X si está instalada; si no, aplica directamente las reglas de este agente, de la skill tailwind-vanilla y de SPECS.md"; el agente no debe detenerse por una skill ausente. qa-evaluator: alternativa grep + revisión manual guiada, Playwright solo si está disponible. En tailwind-vanilla (línea Tabla): sin overflow-x-auto si hay dropdowns ⋮; dimensionar para 768 px. Añadir ERR-001 (F2) y ERR-002 (F1, agentes no visibles como subagentes) a docs/ERRORES.md.
  > TAREA B — Con stitch-prompter, crear docs/stitch/prompt.md: Prompt 0 (sistema de diseño y layout global: sidebar con 6 ítems y estado activo, topbar con toggle claro/oscuro, slate + indigo + semáforo emerald/amber/red, versión clara y oscura) y Prompts 1–6 (uno por pantalla, con datos reales de SPECS.md, ⋮ abierto en una fila, badges, skills expandidas en un agente y el modal/tarjeta de detalle principal). Cada prompt ≤ 1.500 caracteres, autocontenido, estilo en inglés y etiquetas en español. Secciones finales "Cómo usarlos" y "Qué NO copiar de Stitch".
  > CIERRE: entrada P2.1; hash b3b4d64 y tag F1-spec en P1.1; CONTEXT.md con F1 cerrada, F2 en curso y próximo paso P2.2. No tocar otros ficheros ni crear tag.
- **Salida**:
  - Tarea A: los 4 agentes con referencias condicionales y apartado "Skills opcionales" (`.claude/agents/*.md`); `qa-evaluator` y `ux-reviewer` con alternativa grep + revisión manual guiada; regla de tablas en `.claude/skills/tailwind-vanilla/SKILL.md`; ERR-001 y ERR-002 en `docs/ERRORES.md`. A petición posterior del usuario, `CLAUDE.md` deja de decir que esas skills están "instaladas" y las marca como opcionales.
  - Tarea B: `docs/stitch/prompt.md` (153 líneas): dirección visual, Prompts 0–6, "Cómo usarlos" (orden 0→6; capturas `00-global.png` … `06-errores.png`, oscuras con sufijo `-dark`) y "Qué NO copiar de Stitch". Longitud medida por script: 1.329 · 1.242 · 1.463 · 1.471 · 1.466 · 1.460 · 1.443 caracteres (todos ≤ 1.500).
  - Datos recortados para caber en el límite: emails de usuarios como regla con ejemplo, descripciones de skills a inventar por Stitch, skills de contratos por ID, solo LOG-006 con descripción y extracto de traza. Se corrige en F3 desde SPECS.md (la captura es guía visual, no fuente de datos).
- **Revisión** (contra `docs/EVALUACION.md`):
  - Requisito B "propuesta Google Stitch usada como guía": en curso (faltan ejecución, capturas y `adaptacion.md` en P2.2).
  - E1 se mantiene: no hay ningún `.html` en el repo.
  - E10: los importes y nombres de los prompts coinciden con SPECS.md §10 (comprobado CT-01…CT-06, 245,00 €, 31,50 €, subtotales del desglose de CT-01).
  - E12: el layout de tablet (rail de iconos de 64 px, sin `overflow-x-auto`) se pide en el Prompt 0, coherente con S3.7.
  - Huecos: P2.2 debe revisar que Stitch no meta datos inventados ni un menú hamburguesa en tablet.
- **Decisión**: aceptada.
- **Commit**: `fix(F2): ERR-001 agentes no dependen de skills no instaladas` · `211eabf` + `docs(F2): prompts de Google Stitch derivados de SPECS.md` · `3ebe8f3`
- **Tag**: — (F2 sigue abierta; `F2-stitch` al cerrar P2.2)
- **Errores**: ERR-001, ERR-002.

### P2.2 — Adaptación de Stitch a la spec (2026-11-18)
- **Agente/skill**: sesión principal · skills `phase-logger`, `error-tracker`, `ui-patterns`, `tailwind-vanilla`
- **Prompt** (resumen fiel; literal en la sesión):
  > Fase F2, parte 2 (P2.2). Lee CLAUDE.md, CONTEXT.md, SPECS.md y todos los .md de docs/stitch/ (los .png son opcionales). No crees HTML. No hagas commits. SPECS.md manda sobre Stitch.
  > TAREA A — Tabla «Discrepancias» en docs/stitch/adaptacion.md: md de Stitch vs SPECS.md (datos, textos de UI, acciones de dropdown, campos de modales), ✅/❌ y qué prevalece; incluir fallos ya anotados (Atlas «Skills (4)» → 2; «15 skills» → 13; Usuarios oscuro que no es oscuro).
  > TAREA B — «Propuesta Stitch → decisión» (Elemento | Stitch | Decisión | Motivo): tokens Material, `<style>`/`style=""`/`onclick`, Google Fonts y Material Symbols, dark como pantallas separadas, dropdowns y modales abiertos, skills de Orion desplegadas, sidebar `w-64`, extras fuera de spec, `overflow-x-auto`, `animate-in`, marca «Consola Admin».
  > TAREA C — «Recetas Tailwind (claro + dark:)»: sidebar, topbar con toggle, tarjeta de métrica, 9 badges, fila con dropdown ⋮ cerrado y abierto, modal, skills colapsable, desglose de contrato.
  > TAREA D — «Datos que faltan para F3»: modal de 7 usuarios, prompt de cada agente, permisos de cada skill, desglose de 6 contratos, traza de 8 entradas; indicar § de SPECS o si hay que añadirlo.
  > CIERRE: entrada P2.2 en PROMPTS.md, ERR-003 en ERRORES.md, lista de ficheros de stitch, CONTEXT.md → F2 completada y próximo paso F3. No tocar otros ficheros ni crear HTML.
- **Salida**: `docs/stitch/adaptacion.md` con 4 secciones: Discrepancias (datos, textos/shell, dropdowns, modales, fallos consolidados, diferencias skill `ui-patterns` vs SPECS), Propuesta → decisión (≈ 45 filas), Recetas Tailwind (9 bloques HTML sin `style`) y Datos que faltan. Decisiones clave: paleta Tailwind estándar con pares `dark:` (SPECS §11 manda sobre `00-global.md` en badges); fuente del sistema (`font-sans`/`font-mono`); iconos con sprite SVG inline `<symbol>/<use>` (alternativa: diccionario JS de paths); todos los extras de Stitch descartados; sidebar `w-16 lg:w-64`. No se ha creado HTML ni se han tocado otros ficheros salvo los de cierre.
  Hallazgo: **SPECS.md ya define todos los datos** que Stitch no trae (§10.1–§10.5, S5.4, S8.4); no hay que ampliar la spec. Los `.png` no existen en el repo; el análisis usa solo los md. Se eliminó `docs/stitch/README.md` (duplicaba el README raíz); su índice pasa a `adaptacion.md`.
- **Revisión** (contra `docs/EVALUACION.md`):
  - Requisito B «propuesta Google Stitch usada como guía»: cubierto con `prompt.md`, md de Stitch y `adaptacion.md`.
  - E4: decisiones que evitan `<style>`, `style=`, hojas externas y `element.style` (verificación futura: `grep -nE 'style=|<style|rel="stylesheet"|\.style\.' index.html` → 0).
  - E5–E8: dropdown/modal/colapsable arrancan cerrados; recetas alineadas con S6.3, §11.3, §11.4.
  - E10: datos de Stitch contrastados con SPECS §10; incoherencias detectadas (Atlas 4 skills, 15 skills, descripciones del Log, LOG-003 con skill que Nova no tiene).
  - E12: rail `w-16 lg:w-64`, sin `overflow-x-auto`, columna Precio/mes oculta en tablet.
  - Huecos: sin capturas de Stitch; el sprite SVG debe validarse visualmente en F3; la skill `ui-patterns` difiere de SPECS en detalles (ver sección 6 de `adaptacion.md`).
- **Decisión**: aceptada (F2 completada).
- **Commit**: `docs(F2): adaptación de Stitch a la spec` · `a5be32d`
- **Tag**: `F2-stitch`
- **Errores**: ERR-003.

## F3 — Construcción

### P3.0 — Alinear skills y agente con SPECS.md (2026-11-18)
- **Agente/skill**: sesión principal · skills `ui-patterns`, `tailwind-vanilla`, `phase-logger`
- **Prompt** (resumen fiel; literal en la sesión):
  > Fase F3, parte 0 (P3.0): preparación. Lee CLAUDE.md, CONTEXT.md, docs/GIT.md, SPECS.md, docs/stitch/adaptacion.md (sección 6) y las skills ui-patterns y tailwind-vanilla. No crees HTML. No hagas commits. SPECS.md manda.
  > TAREA A — (1) Editar ui-patterns y tailwind-vanilla para que no contradigan SPECS (backdrop, dropdown, colapsable, cierres, tema, navegación, paleta de badges §11, tablas sin overflow-x-auto). (2) Añadir a ui-patterns «Iconos: sprite SVG inline» (`<symbol id="i-…">` en `<svg class="hidden">`, `<use href>` en uso) con la lista de iconos de SPECS. (3) Revisar frontend-builder: sprite, orden P3.1 layout → Dashboard → Usuarios → Agentes → Skills → Contrataciones → Log, verificación en navegador tras cada sección. (4) grep de `style=` y `<style` en skills y agentes: solo como prohibición.
  > TAREA B — Hashes pendientes: P2.1 → 211eabf y 3ebe8f3; P2.2 → a5be32d, tag F2-stitch; ERR-001/002 → 211eabf; ERR-003 → a5be32d; CONTEXT: F2 cerrada, F3 en curso, próximo paso P3.1.
  > CIERRE — Entrada P3.0; commit previsto «docs(F3): alinea skills con SPECS.md», sin tag. No tocar otros ficheros ni crear HTML.
- **Salida**: `ui-patterns/SKILL.md` y `tailwind-vanilla/SKILL.md` reescritos según SPECS: backdrop `bg-slate-900/60 backdrop-blur-sm`, dropdown `w-48` con apertura hacia arriba en las 2 últimas filas, Esc con foco al ⋮, modal con `hidden`↔`flex`, `overflow-hidden` en `body` y foco, colapsable `grid-rows` + `opacity` con `aria-controls`, tema en `<html>` con `try/catch` y `aria-pressed/label`, navegación por hash, paleta de badges §11 (8 variantes), tablas sin `overflow-x-auto`, sidebar `w-16 lg:w-64`. Nueva sección «Iconos: sprite SVG inline» con los 27 iconos exigidos. `frontend-builder.md` con sprite, orden de trabajo y parada de verificación tras cada sección. Registrados los hashes de F2.
- **Revisión** (contra `docs/EVALUACION.md`): prepara E4 (sin `style`/CSS externo), E5–E8 (patrones dropdown/modal/colapsable alineados con CA-15…27), E9 (tema) y E12 (tablet). Sin HTML en el repo: E1 intacto. Huecos: el sprite con `<svg class="hidden">` (display:none) debe validarse visualmente en P3.1; la skill propone `absolute h-0 w-0 overflow-hidden` como respaldo.
- **Decisión**: aceptada.
- **Commit**: `docs(F3): alinea skills con SPECS.md` · `345304a`
- **Tag**: — (F3 sigue abierta; `F3-build` al cerrar)
- **Errores**: ninguno.

### P3.1 — Esqueleto de index.html: layout, sprite, tema y navegación (2026-11-18)
- **Agente/skill**: sesión principal (lectura de `.claude/agents/frontend-builder.md` y `ui-patterns`/`tailwind-vanilla`; no se invocó como subagente) · skills `ui-patterns`, `tailwind-vanilla`, `phase-logger`
- **Prompt** (resumen fiel; literal en la sesión):
  > Fase F3, parte 1 (P3.1): esqueleto de index.html. SOLO index.html con el esqueleto: head (charset, viewport, título, CDN Tailwind + darkMode class, script de tema antes de pintar, sin style/CSS externo, fuente del sistema); sprite SVG de los 28 iconos de SPECS; sidebar `w-16 lg:w-64` con 6 enlaces, indicador activo, píldora «6» y pie; topbar sticky con título/subtítulo, toggle de tema, notificaciones y avatar; `<main>` con 6 secciones vacías («Sección en construcción») y navegación por hash; núcleo JS vanilla con delegación (dropdown ⋮, modal genérico, colapsable); sección temporal #icon-test; accesibilidad; autocomprobación con grep, `<symbol>`=28, JS con node --check; servidor en 8000. No hacer commits. SPECS manda.
- **Salida**: `index.html` (≈ 1 fichero, único; sin HTML adicional). Resultados de la autocomprobación:
  - `grep -c 'style='` → 0 · `grep '<style|rel="stylesheet"|<link'` → sin resultados.
  - `<symbol` → **27** (no 28: la lista de SPECS/skill contiene 27 nombres únicos; ver discrepancia abajo).
  - `<aside>`, `<nav>`, `<header>`, `<main>` → 1 cada uno; `<section>` → 7 (6 vistas + `#icon-test` temporal).
  - JS: `node --check` sobre el script principal → sintaxis OK.
  - Servidor `python3 -m http.server 8000` en marcha (http://localhost:8000/index.html → HTTP 200). **No se ha comprobado visualmente** en navegador.
- **Revisión** (contra `docs/EVALUACION.md`):
  - E3 parcial: 6 enlaces del sidebar con `href="#…"` y navegación por hash implementada; falta verificar en navegador.
  - E4 parcial: 0 `style=`, 0 `<style>`, 0 hojas externas. Única carga externa: CDN de Tailwind.
  - E9 parcial: toggle con `localStorage` y clase `dark` en `<html>`; script de `<head>` para evitar parpadeo.
  - E11: `aside`, `nav`, `header`, `main`, `section` presentes.
  - E12 parcial: rail `w-16` en tablet y `lg:w-64`; sin `overflow-x-auto`. Falta captura a 768 px.
  - Desviaciones registradas:
    1. **Sprite**: se usó `class="absolute h-0 w-0 overflow-hidden"` en lugar de `class="hidden"` (lo pedido en P3.0). La decisión se tomó **sin probarlo** en navegador, por precaución; se revertirá a `hidden` si la prueba visual lo permite.
    2. **Títulos**: SPECS S3.1 pide `<h2 class="sr-only">` por sección y `<h1>` solo en el topbar; se siguió SPECS, no el «`<h1>` en cada sección» del prompt.
    3. **Iconos**: 27 únicos, no 28. Los trazos son **simplificados, escritos a mano**, no los paths oficiales de Heroicons: hay que revisarlos visualmente en #icon-test.
    4. **Sección extra**: `#icon-test` es la séptima `<section>`; CA-04 exige 6. Se elimina en P3.2.
- **Decisión**: iterar — pendiente verificación visual en navegador (1440 y 768 px, claro y oscuro) y revertir sprite a `hidden` si pinta bien. Próxima entrada: P3.2 (Dashboard).
  - **Actualización (P3.2b, 2026-11-19)**: la decisión del sprite es **definitiva**: `class="absolute h-0 w-0 overflow-hidden"`. **No se revierte a `hidden`** (evita el riesgo conocido de los sprites con `display:none`, que pueden romper referencias internas como gradientes o máscaras, sin coste visual ni de accesibilidad).
- **Commit**: `feat(F3-layout): sidebar, topbar, modo oscuro y navegación` · `b91ee39`
- **Tag**: — (F3 sigue abierta; `F3-build` al cerrar)
- **Errores**: ninguno registrado (ERR-004 no creado: el fallo del sprite no se ha observado).

### P3.2 — Sección Dashboard: tarjetas de métricas y placeholder de gráfico (2026-11-19)
- **Agente/skill**: sesión principal siguiendo `.claude/agents/frontend-builder.md` (no invocado como subagente, como en P3.1) · skills `tailwind-vanilla`, `ui-patterns`, `phase-logger`
- **Prompt** (resumen fiel; literal en la sesión):
  > Fase F3, parte 2 (P3.2): sección Dashboard. Lee CLAUDE.md, CONTEXT.md, SPECS.md (§4, §10, §11 y CA aplicables), docs/stitch/01-dashboard.md y adaptacion.md (receta «tarjeta de métrica»). Usa frontend-builder. Edita SOLO index.html (y docs de cierre). No hagas commits. SPECS manda sobre Stitch.
  > 1. #dashboard (conservar h2 sr-only): cabecera «Vista General» con texto de apoyo de SPECS (sin «Filtrar período»/«Exportar reporte» si SPECS no los pide); 4 tarjetas en `grid grid-cols-1 md:grid-cols-2 gap-5` (245,00 € · 31,50 € · 3 · 2 en rojo con enlace a #errores); iconos del sprite con acentos emerald/amber/indigo/red claro+dark; placeholder `h-72 w-full rounded-xl border-2 border-dashed` con chart-bar, «Gráfico de actividad semanal» y «Ejecuciones de agentes · pendiente de integrar».
  > 2. Eliminar #icon-test: 6 `<section>` en `<main>`, 27 `<symbol>`. 3. Valores coherentes con el dataset. 4. Solo Tailwind, dark: en todo, semántico, sin overflow-x-auto, comprobar a 768 px. 5. grep + node --check, servidor en 8000. 6. Cierre: PROMPTS P3.2 y CONTEXT.
- **Salida**: `index.html`:
  - `#view-dashboard`: 3 `<article>` + 1 `<a href="#errores">` (tarjeta 4, `group`, anillo rojo en hover, foco `ring-indigo-500`) con la estructura de §11.2 y acentos de S4.3; marcador `role="img"` con `aria-label` (S4.5).
  - Valores calculados en JS una sola vez al cargar (S4.2) desde nuevas constantes `SKILLS`, `AGENTES`, `CONTRATOS` (campos mínimos de §10.1, §10.2 y §10.4; se ampliarán en P3.3–P3.6). En el HTML solo hay «—» de reserva. `DESCUENTOS_MES = 31.5` hardcodeado (§10.6).
  - `#view-icon-test` eliminado y sus referencias JS (`TEST`, título temporal, excepción en la navegación).
- **Desviaciones del prompt (manda SPECS)**:
  1. **Sin cabecera visible «Vista General»**: S3.1 dice que el título visible está en el topbar y el `<h2>` es `sr-only`; SPECS §4 no define cabecera ni texto de apoyo. Tampoco acciones «Filtrar período»/«Exportar reporte».
  2. **Rejilla** `grid grid-cols-1 gap-6 sm:grid-cols-2` (S4.1), no `md:grid-cols-2 gap-5` (Stitch). A 768 px ambas dan 2 columnas.
  3. **Texto del placeholder** «Ejecuciones de agentes · 12–18 nov 2026 (pendiente de integrar)» (S4.5).
  4. **Tarjeta 4**: la tarjeta entera es el enlace (S4.4); «ver log de errores» es texto dentro de él, no un enlace anidado.
- **Verificación**:
  - `grep -cE 'style=|<style|\.css|rel="stylesheet"|\.style\.' index.html` → 0 (CA-02) · `overflow-x-auto` → 0 · único `<script src>` = CDN Tailwind, sin `fetch`/XHR (CA-03).
  - `<section>` en `<main>` → **6** (CA-04) · `<symbol` → **27** · `icon-test` → 0.
  - `node --check` del script → OK. Ejecución con DOM simulado: `245,00 €` · «Noviembre 2026 · 4 contratos activos» · `31,50 €` · «12,9 % sobre ingresos del mes» · `3` · «de 6 agentes desplegados» · `2` · «Nova, Vega» (CA-12).
  - `http://localhost:8000/index.html` → 200.
  - **No verificado en navegador**: no hay navegador en el Codespace y la instalación de Playwright/Chromium fue denegada por permisos. A 768 px, por cálculo: 768 − 64 (rail) − 48 (padding) = 656 px → 2 columnas de 316 px, sin scroll horizontal. Pendiente comprobar visualmente: 1440/768 px, claro/oscuro, CA-13 y CA-14 (clic → #errores), y el sprite (`absolute h-0 w-0` vs `hidden`, arrastrado de P3.1).
- **Revisión** (contra `docs/EVALUACION.md`): E3 deja de fallar por la séptima sección (CA-04 → 6). E4 ✅ por grep. E9: todas las clases nuevas con par `dark:`. E10: Dashboard calculado desde el mismo dataset que usarán Agentes y Contrataciones. E11: `article`, `a`, `section`. E12 pendiente de capturas.
- **Decisión**: aceptada a falta de la verificación visual.
- **Commit**: `feat(F3-dashboard): tarjetas de métricas y placeholder de gráfico` · `c4aea9f`
- **Tag**: — (F3 sigue abierta)
- **Errores**: ninguno registrado (la falta de navegador se registra como ERR-004 en P3.2b).

### P3.2b — Sincronizar documentación (2026-11-19)
- **Agente/skill**: sesión principal · skills `phase-logger`, `error-tracker`
- **Prompt** (resumen fiel; literal en la sesión):
  > Fase F3, parte 2b (P3.2b): sincronizar documentación. Solo toca CONTEXT.md, docs/PROMPTS.md, docs/ERRORES.md y docs/EVALUACION.md. No toques index.html. No hagas commits.
  > 1. PROMPTS: P3.2 → hash c4aea9f; en P3.1, decisión del sprite definitiva (`absolute h-0 w-0 overflow-hidden`), sin revertir a `hidden`; añadir entrada P3.2b. 2. ERRORES: ERR-004 (F3, mitigado), Codespace sin navegador y Playwright denegado → verificación manual del usuario por la pestaña Ports (8000). 3. EVALUACION: E2 ✅, E4 ✅, E1 ⚠️ con la aclaración de la plantilla; E3, E9, E10, E11, E12 ⚠️ con lo hecho y lo que falta; no marcar ✅ nada sin verificar. 4. CONTEXT: último commit c4aea9f; P3.0, P3.1, P3.1b y P3.2 commiteados; quitar la errata «(28)» y la duda del sprite; próximo paso P3.3 Usuarios.
- **Salida**:
  - `docs/PROMPTS.md`: hash de P3.2 (`c4aea9f`); nota de sprite definitivo en P3.1; esta entrada. P3.1b (`e803934`, erratas «28 → 27 iconos» en `ui-patterns` y en esta bitácora) no tenía entrada propia; queda referenciada aquí.
  - `docs/ERRORES.md`: ERR-004 (mitigado).
  - `docs/EVALUACION.md`: leyenda con ⚠️ (parcial); E2 y E4 ✅ con evidencia; E1, E3, E9, E10, E11, E12 ⚠️; E5–E8 siguen ⬜; registro de verificación del 2026-11-19 (sección C).
  - `CONTEXT.md`: estado de F3 y último commit actualizados; eliminadas la errata «(28)» (resuelta en `e803934`) y la duda del sprite.
- **Verificación**: `git status --short` → solo los 4 ficheros de docs; `index.html` sin cambios. Comprobaciones que respaldan la matriz (2026-11-19): `grep -cE 'style=|<style|\.css|rel="stylesheet"|\.style\.' index.html` → 0 · `grep -c overflow-x-auto index.html` → 0 · `grep -c '<script src' index.html` → 1 (CDN Tailwind) · `<section` → 6 · `<symbol` → 27 · `grep -rn "(28)" .claude` → sin resultados.
- **Revisión** (contra `docs/EVALUACION.md`): solo documentación; ningún criterio cambia por código. E2 y E4 pasan a ✅ por evidencia ya existente.
- **Decisión**: aceptada.
- **Commit**: `docs(F3): registra hash de P3.2 y sincroniza estado` · `be7e815`
- **Tag**: —
- **Errores**: ERR-004 (registrado, mitigado).

### P3.3 — Sección Usuarios: tabla, dropdown y modal (2026-11-19)
- **Agente/skill**: sesión principal siguiendo `.claude/agents/frontend-builder.md` (no invocado como subagente) · skills `ui-patterns`, `tailwind-vanilla`, `phase-logger`
- **Prompt** (resumen fiel; literal en la sesión):
  > Fase F3, parte 3 (P3.3): sección Usuarios. Lee CLAUDE.md, CONTEXT.md, SPECS.md (§5, §10.3, §11.3, §11.4, §11.5, CA-15…CA-24, CA-39), docs/stitch/02-usuarios.md y adaptacion.md. Sigue frontend-builder y las skills ui-patterns y tailwind-vanilla. SPECS manda sobre Stitch. Edita SOLO index.html y, al cierre, PROMPTS.md y CONTEXT.md. No hagas commits.
  > 1. Constante USUARIOS (7 registros §10.3); «Agentes de la empresa» calculado desde AGENTES. 2. badge(variante, texto) reutilizable (§11.5): plan Starter slate / Pro indigo / Enterprise violet; estado activo emerald / suspendido red / pendiente amber. 3. Recuento «7 usuarios» y tabla table/thead/tbody (S3.8, sin overflow-x-auto) renderizada por JS. 4. Dropdown ⋮ por fila con el núcleo de delegación: aria-label «Acciones para <nombre>», Ver detalle (eye) y Eliminar (trash, rojo); 2 últimas filas hacia arriba; recalcular tras re-render. 5. Modal «Detalle de usuario» (S5.4) max-w-lg, dl de 10 campos, «Sin accesos», «0 · Ninguno»; cierre con Cerrar, ✕, backdrop y Esc; foco al ⋮. 6. Eliminar (S5.5) con confirm literal; aceptar → «6 usuarios»; cancelar no cambia nada. 7. dark: en todo; sin style/CSS; 27 símbolos; 6 section; comentario P3.5 → P3.7. 8. Autocomprobación (greps, node --check, servidor 8000) y qué queda sin verificar en navegador.
- **Salida**: `index.html`:
  - Marcado: `#view-usuarios` con recuento `#usuarios-count` (`aria-live="polite"`) y contenedor S3.8 > `<table>` con `<thead>` (5 `<th scope="col">`, el último `sr-only` «Acciones») y `<tbody id="usuarios-tbody">` vacío; las filas se generan por JS.
  - Datos: constante `USUARIOS` con los 7 registros de §10.3 (`ultimoAcceso: null` en US-05 y US-07). `agentesDe(empresa)` filtra `AGENTES` por propietario.
  - Utilidades **reutilizables para P3.4–P3.7**: `esc()`, `plural()`, `icon()`, `badge(variante, texto, prefijo)` con las 8 variantes de §11, `dropdown(nombre, opciones, arriba)` (§11.3, con `data-action`/`data-id`), `toggleDe()`, constantes de celda/fila (`CELDA`, `FILA`, `VACIA`), `FOOTER_CERRAR` y `campo(dt, dd, ancho)` para los `<dl>` de los modales.
  - Lógica: `renderUsuarios()` (recuento + filas; las 2 últimas con `bottom-full mb-1`), `verUsuario()` (modal único, `max-w-lg`, 10 campos), `eliminarUsuario()` (confirm literal, `splice` + re-render). Delegación en `#view-usuarios`: cierra el menú antes de ejecutar la acción y pasa el ⋮ como origen del foco.
  - Comentario del contador del sidebar corregido a «dato real en P3.7».
- **Desviaciones (manda SPECS)**:
  1. **Sin extras de Stitch**: ni KPI superiores, ni búsqueda/filtros/«Añadir usuario», ni «Reenviar enlace», ni pie «Mostrando 1 a 7». Tampoco avatar con iniciales ni email en `font-mono`: S5.1 no los pide (nombre + empresa, email `text-slate-600`).
  2. **Celdas `px-3 lg:px-4`** también en `<th>` (S3.8 dice `px-4`), aplicando S3.7 para que la tabla quepa a 768 px.
  3. **Modal**: Email y «Agentes de la empresa» ocupan 2 columnas (`sm:col-span-2`) por su longitud; S5.4 no fija el ancho de cada campo.
  4. **Añadidos menores no definidos en SPECS**: hover `bg-red-50` en la opción «Eliminar»; fila de estado vacío «No hay usuarios.» si se borran todos; al cancelar el `confirm` el foco vuelve al ⋮ (al aceptar, la fila desaparece y el foco queda en el documento).
- **Verificación**:
  - `grep -cE 'style=|<style|\.css|rel="stylesheet"|\.style\.' index.html` → 0 (CA-02) · `overflow-x-auto` → 0 · `<script src` → 1 (CDN) · `fetch`/XHR → 0 (CA-03).
  - `<section` → 6 (CA-04) · `<symbol` → 27 · `id: 'US-0` → 7 · `<table|<thead|<tbody` → 3 · «Sección en construcción» → 4 · `P3.5` → 0.
  - `node --check` del script principal → OK.
  - **DOM simulado (jsdom 24, instalado solo en el scratchpad, fuera del repo)**: script de 42 comprobaciones → **42 OK · 0 fallos**. Cubre: 7 filas y recuento; colores de badges por plan/estado; subtítulo de Carlos Vidal = solo «Umbrella» (glitch de Stitch evitado); aria-label de cada ⋮; opciones e iconos; CA-15 (abre, `aria-expanded`), CA-16, CA-17 (clic en topbar y en cabecera de tabla), CA-18 (uno a la vez, Esc + foco), CA-19 (filas 6–7 hacia arriba; tras borrar, 5–6); CA-20/21 (Cerrar y ✕), CA-22 (clic dentro no cierra, backdrop sí), CA-23 (Esc, `overflow-hidden` retirado, foco al ⋮), CA-24 (10 campos; US-05 «Sin accesos» y «2 · Nova, Vega»; US-07 «0 · Ninguno»; US-01 «2 · Atlas, Sirius»; US-06 «1 · Lyra»; US-03 «1 · Orion»); CA-39 (texto literal del confirm, cancelar = 7, aceptar = «6 usuarios»); Dashboard intacto; sin errores JS (salvo `window.scrollTo`, no implementado en jsdom).
  - `python3 -m http.server 8000` → `http://localhost:8000/index.html` HTTP 200.
  - **Sin verificar en navegador (ERR-004)**: aspecto visual a 1440 y 768 px (que la tabla quepa sin scroll horizontal y que los menús no se recorten, CA-10/CA-19), claro/oscuro de badges, menús y modal (CA-40), transición/hover, `backdrop-blur`, el `confirm()` nativo real y que Tailwind CDN genere las clases inyectadas por JS. Cálculo a 768 px: 656 px útiles; contenido estimado ≈ 610 px (nombre ≈ 100, email ≈ 190, plan ≈ 80, estado ≈ 85, ⋮ 32, padding 120).
  - **Verificación manual en navegador (actualización P3.3b, 2026-11-19)**: **pendiente**. El prompt P3.3b no incluía el resultado (el marcador «[PEGA AQUÍ…]» llegó sin rellenar); no se da por verificado. Anotar aquí el resultado cuando el usuario lo aporte (Ports → 8000: 1440/768 px, claro/oscuro). En P3.4 se volvió a pedir la anotación y el marcador llegó otra vez sin rellenar: sigue **pendiente**.
- **Revisión** (contra `docs/EVALUACION.md`): E5 ⚠️ (dropdown completo en Usuarios; faltan 4 secciones). E6 ⚠️ (modal «Ver detalle» en 1 de 4 secciones). E7 ⚠️ (cierre con botón, ✕, backdrop y Esc probado en DOM simulado; falta navegador). E10 ⚠️ (`USUARIOS` coherente con §10.3; agentes por empresa calculados desde `AGENTES`). E11 ⚠️ (primera `<table>` con `<thead>`/`<tbody>` y `th scope`; faltan 4). E4 sigue ✅.
- **Decisión**: aceptada a falta de la verificación visual manual.
- **Commit**: `feat(F3-usuarios): tabla, dropdown y modal` · `9b370ba`
- **Tag**: — (F3 sigue abierta)
- **Errores**: ninguno nuevo (ERR-004 sigue mitigado).

### P3.3b — Sincronizar documentación tras Usuarios (2026-11-19)
- **Agente/skill**: sesión principal · skill `phase-logger`
- **Prompt** (resumen fiel; literal en la sesión):
  > Fase F3, P3.3b: sincronizar documentación. Solo toca CONTEXT.md, docs/PROMPTS.md y docs/EVALUACION.md. No toques index.html. No hagas commits.
  > 1. PROMPTS: en P3.3, hash 9b370ba; añadir entrada P3.3b; anotar en P3.3 el resultado de la verificación manual en navegador. 2. CONTEXT: último commit 9b370ba; P3.3 commiteado; próximo paso P3.4 Agentes; decisión nueva: los borrados (Agentes y Skills) usan un Set de IDs ocultos en vez de splice sobre el dataset maestro. 3. EVALUACION: E5, E6, E7 → ⚠️ con la evidencia de P3.3; E11 ⚠️ (1 de 5 tablas); E3 y E10 con lo hecho en Usuarios; E4 ✅ revalidado sobre 9b370ba; fila nueva en la sección C; nada ✅ sin navegador.
- **Contexto**: la revisión de inicio de sesión detectó que P3.3 ya estaba commiteado y pusheado (`9b370ba`, árbol limpio), pero `CONTEXT.md` lo daba como «sin commitear», `PROMPTS.md` tenía «hash: pendiente» y `EVALUACION.md` no se había actualizado en P3.3.
- **Salida**:
  - `docs/PROMPTS.md`: hash `9b370ba` en P3.3; verificación manual anotada como **pendiente** (el resultado no venía en el prompt); esta entrada.
  - `CONTEXT.md`: estado de F3, último commit, próximo paso P3.4 y nueva decisión fijada (borrados con `Set` de IDs ocultos).
  - `docs/EVALUACION.md`: E5, E6, E7 ⬜ → ⚠️; E3, E9, E10, E11 actualizados con Usuarios; E4 ✅ revalidado sobre `9b370ba`; fila nueva en C.
- **Decisión técnica registrada**: en Agentes y Skills, «Eliminar» oculta la fila mediante un `Set` de IDs ocultos de la vista y **no** hace `splice` sobre `AGENTES`/`SKILLS`. Motivo: `precioSkill()` hace `SKILLS.find(...).precio` (un `splice` rompería Contrataciones con TypeError) y S6.6/S7.6 exigen que el borrado no afecte a otras secciones (Usuarios calcula «Agentes de la empresa» y Skills cuenta agentes desde `AGENTES`). En Usuarios se mantiene el `splice` de `USUARIOS` porque ningún otro dato depende de él.
- **Verificación** (2026-11-19, sobre `9b370ba`): `grep -cE 'style=|<style|\.css|rel="stylesheet"|\.style\.' index.html` → 0 · `overflow-x-auto` → 0 · `<script src` → 1 (CDN) · `fetch(`/XHR → 0 · `<section` → 6 · `<symbol` → 27 · `<table|<thead|<tbody` → 3 · `dark:` → 86 · «Sección en construcción» → 4. `index.html` sin cambios.
- **Revisión** (contra `docs/EVALUACION.md`): solo documentación; los cambios de estado reflejan evidencia ya existente de P3.3 (DOM simulado), sin ✅ nuevos.
- **Decisión**: aceptada.
- **Commit**: `docs(F3): registra hash de P3.3 y sincroniza evaluación` · `ace1f65`
- **Tag**: —
- **Errores**: ninguno nuevo (ERR-004 sigue mitigado; verificación manual de P3.1–P3.3 pendiente).

### P3.4 — Sección Agentes: skills colapsables y configurar (2026-11-19)
- **Agente/skill**: sesión principal siguiendo `.claude/agents/frontend-builder.md` (no invocado como subagente) · skills `ui-patterns`, `tailwind-vanilla`, `phase-logger`
- **Prompt** (resumen fiel; literal en la sesión):
  > Antes de empezar, anota en P3.3 el resultado de mi verificación manual de P3.1–P3.3: [marcador sin rellenar].
  > Fase F3, P3.4: sección Agentes. Lee CLAUDE.md, CONTEXT.md, SPECS.md (§6, §10.2, §11.3–§11.6, CA-15…CA-23, CA-25…CA-29, CA-39, CA-40), docs/stitch/03-agentes.md y adaptacion.md. Sigue frontend-builder, ui-patterns y tailwind-vanilla. SPECS manda sobre Stitch. Edita SOLO index.html y, al cierre, PROMPTS.md y CONTEXT.md. No hagas commits.
  > 1. promptSistema literal en los 6 AGENTES sin tocar campos existentes. 2. #view-agentes: recuento «6 agentes» (aria-live), tabla S3.8 (th scope, último sr-only), sin overflow-x-auto; filas por JS align-top: avatar + nombre + ID, propietario, badge con punto (fallando animate-pulse), colapsable §11.6 (skills-AG-0n, chips), dropdown Configurar (cog-6-tooth) / Eliminar; 2 últimas filas hacia arriba. 3. Reutilizar data-collapse-toggle. 4. Modal «Configurar agente» max-w-2xl, metadatos, textarea#prompt-sistema rows=10 con clases de S6.5, recuento en input, Cancelar / Guardar cambios (en memoria, sin toast); ✕, backdrop y Esc sin guardar; al abrir en otras secciones el pie vuelve a FOOTER_CERRAR. 5. Eliminar con confirm literal y Set de IDs ocultos (sin splice). 6. Delegación en #view-agentes como Usuarios. 7. dark: en todo, sin style/CSS, 27 symbols, 6 sections; revisar cog-6-tooth. 8. Autocomprobación y cierre.
- **Salida**: `index.html`:
  - Datos: `promptSistema` literal de §10.2 añadido a los 6 objetos de `AGENTES` (campos existentes intactos).
  - Marcado: `#view-agentes` con `#agentes-count` (`aria-live="polite"`) y contenedor S3.8 > `<table>` con `<thead>` (5 `<th scope="col">`, el último `sr-only` «Acciones») y `<tbody id="agentes-tbody">`.
  - Lógica: `renderAgentes()` (avatar con inicial, nombre + ID, propietario, `badgeEstadoAgente()` con punto vía el prefijo de `badge()`, colapsable §11.6 con `skills-AG-0n`, `dropdown()` Configurar/Eliminar, 2 últimas filas `bottom-full`); `configurarAgente()` (modal `max-w-2xl`, metadatos, `<label for>` + `<textarea id="prompt-sistema" rows="10">` con las clases literales de S6.5, recuento «n caracteres» en `input`, pie Cancelar + Guardar cambios); guardado por `data-guardar-agente` en el objeto en memoria; `eliminarAgente()` con `confirm` literal y `Set` `agentesOcultos` (sin mutar `AGENTES`); delegación en `#view-agentes`. Nuevas piezas reutilizables: `CHIP`, `nombreSkill()`, `chipsSkills()`, `BTN_PRIMARIO`.
  - `openModal()`: el pie por defecto es ahora `FOOTER_CERRAR` y se reescribe siempre, así el pie «Cancelar/Guardar» de Configurar no se arrastra a otros modales.
  - Icono `cog-6-tooth`: los dos círculos de P3.1 (no reconocibles) se sustituyen por un engranaje de 8 dientes (polígono generado + círculo central). Siguen 27 `<symbol>`.
  - El colapsable usa el manejador `data-collapse-toggle` existente sin cambios.
- **Desviaciones / añadidos (SPECS manda)**:
  1. **Sin extras de Stitch**: ni píldoras «3 activos / 2 fallando», ni búsqueda/filtros/«Desplegar nuevo agente», ni «Historial de logs» en el menú, ni tira de métricas, panel de carga o «Diagnóstico reciente».
  2. **Textarea**: `rows="10"` y `max-w-2xl` (S6.5), no `rows=5 resize-none` de Stitch. Se añade `aria-describedby="prompt-count"` (accesibilidad, no definido en SPECS).
  3. **Separación label/textarea** con un contenedor `mt-5 flex flex-col gap-2` para no alterar las clases literales del `<textarea>`.
  4. **Añadidos menores no definidos en SPECS**: `aria-label` «Skills de <nombre>» en las listas de chips; al borrar se conserva qué listas estaban expandidas (el re-render no las colapsa); estado vacío «No hay agentes.»; al cancelar el `confirm`, el foco vuelve al ⋮.
- **Verificación**:
  - `grep -cE 'style=|<style|\.css|rel="stylesheet"|\.style\.' index.html` → 0 (CA-02) · `overflow-x-auto` → 0 · `<script src` → 1 (CDN) · `fetch(`/XHR → 0 (CA-03).
  - `<section` → 6 (CA-04) · `<symbol` → 27 · «Sección en construcción» → 3 · `promptSistema` → 10 (6 datos + 4 usos) · `<table|<thead|<tbody` → 6 · `AGENTES.splice` → 0 · `dark:` → 100.
  - `node --check` del script principal → OK.
  - **DOM simulado (jsdom 24 en el scratchpad, fuera del repo)**: 133 comprobaciones → **133 OK · 0 fallos**. Cubre: 6 filas y recuento; avatar, nombre, ID, propietario, badge, `align-top`, «Skills (n)» y chips de cada agente; CA-29 (emerald / slate con punto gris / rojo con `animate-pulse` solo en Nova y Vega); CA-25 (6 colapsadas, `grid-rows-[0fr] opacity-0`, ids `skills-AG-0n`, transición 300 ms, hijo `overflow-hidden`, sin `hidden`); CA-26 (expande con `grid-rows-[1fr] opacity-100` y `rotate-180`, re-clic colapsa); CA-27 (independiente); CA-15…CA-19 (abre con `aria-expanded`, uno a la vez, clic en topbar y en otra celda cierra, Esc + foco, 2.º clic cierra, filas 5–6 hacia arriba y tras borrar 4–5); CA-16; CA-20…CA-23 y CA-28 (título/subtítulo, `max-w-2xl`, prompt literal, `rows=10`, editable, recuento en vivo y singular «1 carácter»; clic en textarea no cierra; Cancelar, backdrop, Esc y ✕ cierran **sin guardar**; Guardar guarda y al reabrir se ve el texto; foco al ✕ al abrir y al ⋮ al cerrar; `overflow-hidden` retirado); pie de Usuarios vuelve a «Cerrar» y `max-w-lg` tras abrir Configurar; CA-39 (confirm literal, cancelar = 6, aceptar = «5 agentes»); **tras eliminar Atlas: Dashboard 3 activos · 2 fallando (Nova, Vega) · 245,00 €, Usuarios con 7 filas y Laura Méndez sigue con «2 · Atlas, Sirius»**; estado vacío «0 agentes»; sin errores JS.
  - `python3 -m http.server 8000` → `http://localhost:8000/index.html` HTTP 200.
  - **Sin verificar en navegador (ERR-004)**: transición visible de ~300 ms y rotación del chevron (CA-26), punto pulsante (CA-29), engranaje reconocible a 16 px, aspecto a 1440 y 768 px (tabla sin scroll horizontal y menús sin recortar, CA-10/CA-19), claro/oscuro de tabla, chips, badges, menú y modal con textarea (CA-40), anillo de foco del textarea, `confirm()` nativo y que Tailwind CDN genere las clases inyectadas por JS (`grid-rows-[…]`, `max-w-2xl`, `animate-pulse`). Cálculo a 768 px: 656 px útiles; contenido estimado ≈ 590 px (agente ≈ 110, propietario ≈ 80, estado ≈ 95, skills ≈ 150 con la chip más larga, ⋮ 32, padding 120).
- **Revisión** (contra `docs/EVALUACION.md`): E5 ⚠️ (2 de 5 secciones). E6 ⚠️ (sin cambio en «Ver detalle»; «Configurar» en Agentes ya cumple su parte de CA-20). E7 ⚠️ (2 modales). E8 ⚠️ (colapsable implementado y probado en DOM simulado; falta ver la transición). E10 ⚠️ (skills por nombre desde `SKILLS`; borrado sin efectos en otras secciones). E11 ⚠️ (2 de 5 tablas). E4 sigue ✅. `docs/EVALUACION.md` no se toca en este prompt (se sincroniza en el siguiente docs).
- **Decisión**: aceptada a falta de la verificación visual manual.
- **Verificación manual en navegador (actualización en P3.5, 2026-11-19)**: **pendiente**. El prompt de P3.5–P3.7 pedía anotarla, pero el marcador «[escribe aquí…]» llegó sin rellenar; no se da por verificado.
- **Commit**: `feat(F3-agentes): skills colapsables y configurar` · `e1d95c5`
- **Tag**: — (F3 sigue abierta)
- **Errores**: ninguno nuevo (ERR-004 sigue mitigado).

### P3.5 — Sección Skills: catálogo, modal y eliminar (2026-11-19)
- **Agente/skill**: sesión principal siguiendo `.claude/agents/frontend-builder.md` · skills `ui-patterns`, `tailwind-vanilla`, `phase-logger`
- **Prompt** (resumen fiel; el mismo prompt cubre P3.5, P3.6 y P3.7, con un commit por parte y push al final):
  > Fase F3, partes 5–7. Lee CLAUDE.md, CONTEXT.md, SPECS.md (§7, §8, §9, §10.1, §10.4, §10.5, §11 y CA), docs/stitch/04–06 y adaptacion.md. SPECS manda sobre Stitch. Reutiliza badge, dropdown, campo, FOOTER_CERRAR, esc, icon. PASO 0: árbol limpio y commit feat(F3-agentes); anotar hash y verificación manual en P3.4. COMMITS: uno por parte con mensajes exactos, push solo al final, nunca tag ni force.
  > P3.5: ampliar SKILLS (desc, descCompleta, permisos literales de §10.1); bloque «¿Qué es una skill?»; catálogo con precio y nº de agentes calculado desde AGENTES; dropdown Ver detalle/Eliminar; modal con descCompleta, permisos y badge mini (text-[10px]); «Sin agentes» si 0; eliminar con confirm literal y Set de ocultos (nunca splice). Verificación con tabla «comando | esperado | obtenido | ✅/❌».
- **Salida**: `index.html`:
  - Datos: `SKILLS` ampliada con `desc`, `descCompleta` y `permisos` **extraídos por script directamente de la tabla §10.1** de `SPECS.md` (texto literal; `id`, `nombre` y `precio` sin cambios).
  - Marcado: `<aside>` S7.1 (borde índigo, icono information-circle, `<h3>` y texto literal) encima de `#skills-count` (`aria-live`) y tabla S3.8 con 5 `<th scope="col">` (Precio/mes `hidden lg:table-cell`, último `sr-only` «Acciones»).
  - Lógica: `renderSkills()` (icono propio en cuadrado, nombre + ID, descripción breve `max-w-md`, precio en €, badge indigo «n agentes» calculado con `agentesConSkill()` sobre el `AGENTES` completo, `dropdown()` Ver detalle/Eliminar, 2 últimas filas hacia arriba); `verSkill()` (modal `max-w-lg`, `<dl>` de 6 campos de S7.5, chips de agentes con `badgeEstadoAgente(a, true)` mini, «Sin agentes» si 0); `eliminarSkill()` (confirm literal, `Set` `skillsOcultas`); delegación en `#view-skills`.
  - `badge()` acepta un 4.º parámetro `mini` (`BADGE_MINI`: `px-1.5 py-px text-[10px] leading-4`), y `badgeEstadoAgente(a, mini)` lo propaga.
- **Desviaciones / decisiones (manda SPECS)**:
  1. **Sin extras de Stitch**: ni KPI («Total habilidades», «Gasto combinado»), ni búsqueda ni «Nueva skill». Texto del bloque explicativo = S7.1, no el de Stitch.
  2. **Recuento de agentes sobre el dataset maestro**: ocultar un agente en la vista Agentes no cambia «n agentes» ni el modal de Skills (S6.6: el borrado no afecta a otras secciones). Probado.
  3. **Precio/mes** alineado a la derecha con `tabular-nums` (Stitch; SPECS no fija la alineación).
  4. «Sin agentes» no se da con el dataset actual (mínimo 2 por skill); está implementado para el caso vacío.
  5. Añadidos menores: estado vacío «No hay skills.»; al cancelar el `confirm`, el foco vuelve al ⋮.
- **Verificación** (2026-11-19, script `verify.sh` en el scratchpad):

  | Comando | Esperado | Obtenido | |
  |---|---|---|---|
  | `git status --short` | solo la parte | ` M index.html` (+ este fichero) | ✅ |
  | `grep -cE 'style=\|<style\|\.css\|rel="stylesheet"\|\.style\.'` | 0 | 0 | ✅ |
  | `grep -c overflow-x-auto` | 0 | 0 | ✅ |
  | `grep -c '<section'` | 6 | 6 | ✅ |
  | `grep -c '<symbol'` | 27 | 27 | ✅ |
  | `grep -c 'Sección en construcción'` | 2 | 2 | ✅ |
  | `grep -c '<script src'` | 1 | 1 | ✅ |
  | `grep -n 'SKILLS.splice\|AGENTES.splice\|CONTRATOS.splice'` | sin resultados | sin resultados | ✅ |
  | `node --check` script principal | OK | OK | ✅ |
  | jsdom `test-skills.js` | 0 fallos | 103 OK · 0 fallos | ✅ |
  | jsdom `test-agentes.js` (regresión P3.4) | 0 fallos | 133 OK · 0 fallos | ✅ |
  | `curl :8000/index.html` | 200 | 200 | ✅ |

  La prueba jsdom cubre: CA-30 (bloque y texto literal, encima del catálogo); CA-31 (3/3/2/3/2 agentes, nombres en el modal: Atlas·Orion·Nova / Atlas·Orion·Vega / Sirius·Nova / Sirius·Lyra·Nova / Orion·Vega, badge mini con estado: SK-05 «Orion Activo», «Vega Fallando»); descripción breve, precio, icono e ID de las 5; dropdown (CA-15…CA-19: abre, otro ⋮, clic en sidebar y en cabecera, Esc + foco, 2 últimas hacia arriba, opción cierra); modal (CA-20…CA-23: clic dentro no cierra, backdrop, Esc, ✕, Cerrar, foco al ⋮, `overflow-hidden`); recuentos intactos tras ocultar Atlas en Agentes; CA-39 (confirm literal, cancelar = 5, aceptar = «4 skills», 2 últimas recalculadas); **tras eliminar «Navegación web», Agentes sigue mostrándola en Orion, «Configurar» funciona y el Dashboard sigue con 245,00 €**; estado vacío; Usuarios con 7 filas; sin errores JS.
  Primera ejecución: 1 fallo **de la propia prueba** (buscaba la cadena literal `hidden lg:table-cell` y las clases están en otro orden); se corrigió la prueba para comprobar ambas clases en `<th>` y `<td>`.
  - **Sin verificar en navegador (ERR-004)**: aspecto del bloque S7.1 y de la tabla a 1440/768 px (a 768 px la columna Precio/mes se oculta), legibilidad del badge mini a 10 px, iconos propios de cada skill, claro/oscuro (CA-40).
- **Revisión**: E5 ⚠️ (3 de 5 secciones). E6 ⚠️ (2 de 4 «Ver detalle»). E7 ⚠️ (3 modales). E10 ⚠️ (descripciones y permisos literales; recuentos calculados). E11 ⚠️ (3 de 5 tablas). E4 sigue ✅.
- **Decisión**: aceptada a falta de la verificación visual manual.
- **Actualización en P3.6**: la prueba `test-skills.js` se amplió para comprobar Contrataciones tras eliminar «Navegación web» (CT-01 sigue mostrando el chip, 660,00 € en la tabla y en el `tfoot` del modal) → **105 OK · 0 fallos**.
- **Commit**: `feat(F3-skills): catálogo, modal y eliminar` · `b213a33`
- **Tag**: —
- **Errores**: ninguno nuevo.

### P3.6 — Sección Contrataciones: tabla y desglose (2026-11-19)
- **Agente/skill**: sesión principal siguiendo `.claude/agents/frontend-builder.md` · skills `ui-patterns`, `tailwind-vanilla`, `phase-logger`
- **Prompt** (resumen fiel; mismo prompt que P3.5):
  > P3.6: CONTRATOS con los 6 contratos de §10.4 (ampliar sin romper el Dashboard); importe = Σ precio × meses calculado (660/900/210/360/45/390 €). Tabla S3.8 con cliente (+ID), agente, chips, inicio, fin, importe y estado del contrato rotulado «Contrato» (Activo emerald, Finalizado slate, Cancelado red); dropdown Ver detalle / Descargar factura (sin lógica). Modal «Detalle de contrato» con desglose Skill | Precio/mes | Meses | Subtotal, tfoot con total y línea de cálculo (S8.5), botones Cerrar y Descargar factura. Sin extras de Stitch.
- **Salida**: `index.html`:
  - Datos: `CONTRATOS` ya tenía todos los campos de §10.4 desde P3.2 (id, cliente, agente, skills, inicio, fin, meses, estado): **no se modifica**. El importe se calcula con `importe(c) = precioMensual(c) × c.meses` (S8.5); el Dashboard sigue usando `precioMensual()` sin cambios.
  - Marcado: `#contratos-count` (`aria-live`) + tabla S3.8 con 7 `<th scope="col">`.
  - Lógica: `renderContratos()` (cliente + ID; agente con el badge del estado del contrato debajo, con prefijo `sr-only` «Contrato: » y `title="Estado del contrato"`; chips `CHIP` siempre visibles; Inicio/Fin `whitespace-nowrap`; importe a la derecha `font-semibold tabular-nums`; `dropdown()` Ver detalle / Descargar factura; 2 últimas filas hacia arriba); `verContrato()` (modal `max-w-2xl`, subtítulo «CT-0n · Cliente — Agente», `<dl>` `grid-cols-2 lg:grid-cols-4` Estado/Inicio/Fin/Duración, tabla de desglose con `<caption class="sr-only">`, `<tfoot>` «Total mensual» y «Total del contrato» `text-base font-semibold`, línea de cálculo `font-mono text-xs`; pie «Descargar factura» (secundario, sin lógica) + «Cerrar»); delegación en `#view-contrataciones`.
- **Desviaciones / decisiones (manda SPECS)**:
  1. **Tablet (768 px)**: por cálculo, 7 columnas ocupan ≈ 706 px y solo hay 656 útiles. Se aplica S3.7 («columnas secundarias ocultas con `hidden lg:table-cell`»): la columna **Fin** solo se muestra desde `lg`; en tablet la fecha de fin aparece bajo Inicio (`lg:hidden`, con `sr-only` «Fin: ») y la cabecera dice «Inicio / Fin». Con eso ≈ 607 px. No se pierde información.
  2. **«Contrato»**: el prompt pedía rotular el badge; se sigue la decisión ya tomada en `docs/stitch/adaptacion.md` (prefijo `sr-only` «Contrato: » + `title`) para no cambiar el texto visible del badge de S8.2 («Activo», «Finalizado», «Cancelado»).
  3. **«Descargar factura»**: el prompt decía «no cierra nada»; SPECS S8.3 y CA-34 dicen que **cierra el menú** y no hace nada más. Se sigue SPECS: en el menú cierra el menú y devuelve el foco al ⋮; en el pie del modal no hace nada (el modal sigue abierto).
  4. **Línea de cálculo** con un solo skill sin paréntesis: `15,00 € × 3 meses = 45,00 €` (§10.4 «15 × 3»).
  5. Sin KPI, búsqueda, filtros ni «Nueva contratación» de Stitch. Sin opción Eliminar (S8.3).
- **Verificación** (2026-11-19, `verify.sh`):

  | Comando | Esperado | Obtenido | |
  |---|---|---|---|
  | `git status --short` | solo la parte | ` M index.html` (+ este fichero) | ✅ |
  | `grep -cE 'style=\|<style\|\.css\|rel="stylesheet"\|\.style\.'` | 0 | 0 | ✅ |
  | `grep -c overflow-x-auto` | 0 | 0 | ✅ |
  | `grep -c '<section'` | 6 | 6 | ✅ |
  | `grep -c '<symbol'` | 27 | 27 | ✅ |
  | `grep -c 'Sección en construcción'` | 1 | 1 | ✅ |
  | `grep -c '<script src'` | 1 | 1 | ✅ |
  | `grep -n 'SKILLS.splice\|AGENTES.splice\|CONTRATOS.splice'` | sin resultados | sin resultados | ✅ |
  | `node --check` script principal | OK | OK | ✅ |
  | jsdom `test-contratos.js` | 0 fallos | 135 OK · 0 fallos | ✅ |
  | jsdom `test-skills.js` (ampliada) | 0 fallos | 105 OK · 0 fallos | ✅ |
  | jsdom `test-agentes.js` | 0 fallos | 133 OK · 0 fallos | ✅ |
  | `curl :8000/index.html` | 200 | 200 | ✅ |

  La prueba cubre: 6 filas y recuento; cliente + ID, agente + badge de contrato (emerald/slate/red), chips, Inicio (+ Fin en tablet), Fin (lg); **CA-32 importes 660/900/210/360/45/390 €**; **suma de activos 2.160 €** y Dashboard intacto con 245,00 €; **CA-44** (skills de cada contrato = skills del agente en la vista Agentes); dropdown y modal (CA-15…CA-23); para los 6 contratos: título y subtítulo, `max-w-2xl`, `<dl>` Estado/Inicio/Fin/Duración, una fila por skill con subtotal = precio × meses, `tfoot` total mensual y total del contrato = importe de la tabla, línea de cálculo exacta (**CA-33**), pie «Descargar factura» + «Cerrar», y «Descargar factura» del modal no lo cierra; **CA-34** (menú cerrado, sin modal, sin `confirm`, foco al ⋮, sin errores en consola).
  - **Sin verificar en navegador (ERR-004)**: que la tabla quepa a 768 px con Fin bajo Inicio (CA-10), menús de las 2 últimas filas sin recortar (CA-19), aspecto del modal con tabla de desglose, claro/oscuro (CA-40).
- **Revisión**: E5 ⚠️ (4 de 5 secciones). E6 ⚠️ (3 de 4 «Ver detalle»). E7 ⚠️ (4 modales). E10 ⚠️ (importes calculados y coherentes con Agentes, Skills y Dashboard). E11 ⚠️ (4 de 5 tablas + tabla de desglose con `caption`, `thead`, `tbody`, `tfoot` y `th scope="row"`). E4 sigue ✅.
- **Decisión**: aceptada a falta de la verificación visual manual.
- **Commit**: `feat(F3-contrataciones): tabla y desglose` · `3e4116b`
- **Tag**: —
- **Errores**: ninguno nuevo.

### P3.7 — Sección Log de errores: errores, trazas y resolver (2026-11-19)
- **Agente/skill**: sesión principal siguiendo `.claude/agents/frontend-builder.md` · skills `ui-patterns`, `tailwind-vanilla`, `phase-logger`
- **Prompt** (resumen fiel; mismo prompt que P3.5):
  > P3.7: ERRORES con LOG-001…LOG-008 y sus 8 trazas LITERALES de §10.5; orden descendente interpretando dd/mm/aaaa hh:mm:ss. Tabla S3.8: timestamp mono, agente (+cliente), badge de gravedad (Crítico red, Error orange, Aviso amber, Info sky), descripción con «LOG-00n:» en mono, estado (Abierto slate / Resuelto emerald), dropdown Ver detalle / Marcar como resuelto; fila crítica abierta resaltada (bg-red-50 / dark:bg-red-950/20). Modal «Detalle del error» con Skill implicada «Nombre (SK-0n)», traza en <pre> oscuro (sin botón copiar), Cerrar y Marcar como resuelto. Resolver desde menú o modal: fila a «Resuelto», recuento y píldora/punto del sidebar CALCULADOS desde ERRORES; ya resuelto → opción desactivada. Dashboard sin cambios.
- **Salida**: `index.html`:
  - Datos: constante `ERRORES` con las 8 entradas de §10.5 (id, ts, agente, skill, gravedad, estado, descripcion, traza). **Generada por script desde `SPECS.md`** (tabla + 8 bloques de traza): texto literal, trazas en template literals sin escapes (se comprobó que no contienen `` ` ``, `\` ni `${`).
  - Marcado: `#errores-resumen` (`aria-live`) + tabla S3.8 con 6 `<th scope="col">`.
  - `dropdown()` admite opciones `exito` (`text-emerald-700 dark:text-emerald-400`, S9.3) y `deshabilitado` (`disabled aria-disabled="true"` + `opacity-50 cursor-not-allowed`, §11.3).
  - Lógica: `renderErrores()` (ordena una copia por `claveTs()` = aaaammddhhmmss descendente; timestamp `font-mono text-xs whitespace-nowrap`; agente + propietario desde `AGENTES`; `badgeGravedad()` con x-circle mini en Crítico; descripción con «LOG-00n:» en mono; `badgeEstadoError()` Abierto slate / Resuelto emerald con check; resaltado `bg-red-50 dark:bg-red-950/20` en crítico abierto; `opacity-60` en resueltos; menú «Marcar como resuelto» o «Ya resuelto» deshabilitado); `verError()` (modal `max-w-2xl`, `<dl>` de 7 campos de S9.4, «Traza completa» + `<pre>` con las clases literales de S9.4, pie «Marcar como resuelto» emerald solo si está abierto + «Cerrar»); `resolverError()` (S9.5: cambia el estado en memoria, re-renderiza, recalcula recuento y contador del sidebar y devuelve el foco al ⋮ nuevo de esa fila; no se puede reabrir); desde el modal: cierra y resuelve.
  - **Contador del sidebar calculado**: `actualizarContadorErrores()` sustituye a la constante `6` de P3.1. Píldora (`lg:inline`) y punto de tablet se ocultan si llega a 0.
- **Desviaciones / decisiones (manda SPECS)**:
  1. **Recuento**: «8 entradas · 6 abiertas» (S9.1), no «n no resueltos» (Stitch / prompt).
  2. **Modal**: campos de S9.4 en su orden (Timestamp, Agente, Propietario, Skill implicada, Gravedad, Estado, Descripción), no la cuadrícula de 4 de Stitch; «Skill implicada» como «Nombre (SK-0n)» según el prompt.
  3. **Resalte de fila crítica abierta** (`bg-red-50 dark:bg-red-950/20`): no está en SPECS; se añade por el prompt y `docs/stitch/06-errores.md` («mantener»). Al resolver la fila pierde el resalte y recibe `opacity-60` (S9.5).
  4. **«Ya resuelto»** deshabilitado (S9.3), no se oculta.
  5. Sin pestañas de filtro, búsqueda, «Copiar identificador» ni «Copiar traza» de Stitch.
  6. La descripción lleva «LOG-00n:» con dos puntos (prompt/Stitch); SPECS solo pide el ID delante.
- **Verificación** (2026-11-19, `verify.sh`):

  | Comando | Esperado | Obtenido | |
  |---|---|---|---|
  | `git status --short` | solo la parte | ` M index.html` (+ este fichero) | ✅ |
  | `grep -cE 'style=\|<style\|\.css\|rel="stylesheet"\|\.style\.'` | 0 | 0 | ✅ |
  | `grep -c overflow-x-auto` | 0 | 0 | ✅ |
  | `grep -c '<section'` | 6 | 6 | ✅ |
  | `grep -c '<symbol'` | 27 | 27 | ✅ |
  | `grep -c 'Sección en construcción'` | 0 | 0 | ✅ |
  | `grep -c '<script src'` | 1 | 1 | ✅ |
  | `grep -n 'SKILLS.splice\|AGENTES.splice\|CONTRATOS.splice'` | sin resultados | sin resultados | ✅ |
  | `node --check` script principal | OK | OK | ✅ |
  | jsdom `test-errores.js` | 0 fallos | 151 OK · 0 fallos | ✅ |
  | jsdom `test-contratos.js` | 0 fallos | 135 OK · 0 fallos | ✅ |
  | jsdom `test-skills.js` | 0 fallos | 105 OK · 0 fallos | ✅ |
  | jsdom `test-agentes.js` | 0 fallos | 133 OK · 0 fallos | ✅ |
  | jsdom `test-usuarios.js` (regresión + navegación, nueva) | 0 fallos | 45 OK · 0 fallos | ✅ |
  | `curl :8000/index.html` | 200 | 200 | ✅ |

  La prueba del Log cubre: **CA-35** (orden LOG-008 → LOG-001; timestamp, agente + propietario, colores de gravedad red/orange/amber/sky, descripción con ID, estado); **CA-38** (LOG-001 y LOG-004 «Resuelto» al cargar, «Ya resuelto» deshabilitado, `opacity-60`); resalte solo en LOG-006 y LOG-003; CA-44 (solo Nova, Vega, Orion, Atlas, Lyra); dropdown y modal (CA-15…CA-23); para los 8: `<dl>` de 7 campos con valores correctos, Skill «Nombre (SK-0n)», pie según estado, sin botón copiar y **CA-36 traza idéntica carácter a carácter a la de `SPECS.md`** (leída del propio fichero), incluido `<a8f3c1@…>` escapado; **CA-37** (resolver LOG-006 desde el menú: badge «Resuelto» verde, gravedad intacta, «Ya resuelto», «5 abiertas», **sidebar 6 → 5**, foco al ⋮; resolver LOG-003 desde el modal: cierra, «4 abiertas», sidebar 4, foco al ⋮, al reabrir sin botón); Dashboard «Agentes fallando» sigue en 2; con 0 abiertas, píldora y punto ocultos; el estado se mantiene al navegar. La prueba de Usuarios añade: CA-24, CA-39, CA-07…CA-09, CA-04 (5 tablas con `thead`/`tbody`, 6 `section`, todos los `<button type="button">`) y CA-45.
  - **Sin verificar en navegador (ERR-004)**: legibilidad de los 4 colores de gravedad y del resaltado crítico en claro/oscuro (CA-35, CA-40), `<pre>` con scroll interno, `disabled` real del menú, desaparición de la píldora (1440 px) y del punto (768 px), y ancho a 768 px: por cálculo ≈ 638 de 656 px (la columna Descripción queda estrecha y con varias líneas).
- **Revisión**: E5 ⚠️ (5 de 5 secciones en DOM simulado). E6 ⚠️ (4 de 4 «Ver detalle» + Configurar). E7 ⚠️ (5 modales). E10 ⚠️ (`ERRORES` completo; contador del sidebar calculado). E11 ⚠️ (5 de 5 tablas). E4 sigue ✅. Pendiente solo la verificación visual.
- **Decisión**: aceptada a falta de la verificación visual manual.
- **Commit**: `feat(F3-log): errores, trazas y resolver` · _hash: se anota en el commit de cierre_
- **Tag**: — (`F3-build` solo tras la verificación del usuario en navegador)
- **Errores**: ninguno nuevo.

## F4 — QA
_(pendiente)_

## F5 — Entrega
_(pendiente)_
