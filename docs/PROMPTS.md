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
- **Commit**: `docs(F1): añade SPECS.md` · _hash: pendiente_
- **Tag**: `F1-spec`
- **Errores**: ninguno.

## F2 — Google Stitch
_(pendiente)_

## F3 — Construcción
_(pendiente)_

## F4 — QA
_(pendiente)_

## F5 — Entrega
_(pendiente)_
