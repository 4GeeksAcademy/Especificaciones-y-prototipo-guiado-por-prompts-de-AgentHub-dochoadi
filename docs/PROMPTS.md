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
- **Commit**: `docs(F3): registra hash de P3.2 y sincroniza estado` · _hash: pendiente_
- **Tag**: —
- **Errores**: ERR-004 (registrado, mitigado).

## F4 — QA
_(pendiente)_

## F5 — Entrega
_(pendiente)_
