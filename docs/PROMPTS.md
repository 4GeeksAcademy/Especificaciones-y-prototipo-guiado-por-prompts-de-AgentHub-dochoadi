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
- **Salida**: `ui-patterns/SKILL.md` y `tailwind-vanilla/SKILL.md` reescritos según SPECS: backdrop `bg-slate-900/60 backdrop-blur-sm`, dropdown `w-48` con apertura hacia arriba en las 2 últimas filas, Esc con foco al ⋮, modal con `hidden`↔`flex`, `overflow-hidden` en `body` y foco, colapsable `grid-rows` + `opacity` con `aria-controls`, tema en `<html>` con `try/catch` y `aria-pressed/label`, navegación por hash, paleta de badges §11 (8 variantes), tablas sin `overflow-x-auto`, sidebar `w-16 lg:w-64`. Nueva sección «Iconos: sprite SVG inline» con los 28 iconos exigidos. `frontend-builder.md` con sprite, orden de trabajo y parada de verificación tras cada sección. Registrados los hashes de F2.
- **Revisión** (contra `docs/EVALUACION.md`): prepara E4 (sin `style`/CSS externo), E5–E8 (patrones dropdown/modal/colapsable alineados con CA-15…27), E9 (tema) y E12 (tablet). Sin HTML en el repo: E1 intacto. Huecos: el sprite con `<svg class="hidden">` (display:none) debe validarse visualmente en P3.1; la skill propone `absolute h-0 w-0 overflow-hidden` como respaldo.
- **Decisión**: aceptada.
- **Commit**: `docs(F3): alinea skills con SPECS.md` · _hash: pendiente_
- **Tag**: — (F3 sigue abierta; `F3-build` al cerrar)
- **Errores**: ninguno.

_(pendiente)_

## F4 — QA
_(pendiente)_

## F5 — Entrega
_(pendiente)_
