# Evaluación — matriz de cumplimiento

Estado: ⬜ pendiente · ⚠️ parcial (hecho en parte o sin verificar en navegador) · ✅ cumple (con evidencia) · ❌ falla (ver ERRORES.md)
Un ✅ exige evidencia: comando + resultado, captura o commit.

## A. Criterios de evaluación del reto
| # | Criterio | Cómo se verifica | Estado | Evidencia |
|---|---|---|---|---|
| E1 | `SPECS.md` commiteado antes de cualquier HTML | `git log --reverse --name-status` | ⚠️ | `SPECS.md` se añade en `b3b4d64` (2026-11-18); el primer `index.html` del prototipo es `b91ee39` (posterior). **Aclaración**: `git log` muestra un `A index.html` anterior en `0dfaf38` («add initial», 2026-06-16), que es **de la plantilla** del repositorio y se borró en `a9b4fe4` («delete index») antes de empezar el proyecto. Falta: reflejar la aclaración en el README final (F5). |
| E2 | Spec: ≥3 specs/sección, inventario de componentes, criterios numerados | Revisión de `SPECS.md` | ✅ | `SPECS.md` §14 (autocomprobación): 5–8 specs por sección (§3 8, §4 5, §5 5, §6 6, §7 6, §8 5, §9 5); inventario de 7 componentes (§11.1–§11.7); criterios CA-01…CA-45 con ≥1 para dropdown (CA-15–19), modal (CA-20–24), colapsable (CA-25–27) y dark (CA-40–42). |
| E3 | Seis secciones presentes y accesibles desde el sidebar | Navegar cada enlace | ⚠️ | Hecho: `grep -c '<section' index.html` → 6; sidebar con 6 enlaces `href="#…"` y navegación por hash (`b91ee39`); Dashboard construido (`c4aea9f`); Usuarios construido (`9b370ba`; «Sección en construcción» → 4). Falta: contenido de Agentes, Skills, Contrataciones y Log y verificación en navegador de CA-06…CA-09 (ERR-004). |
| E4 | Tailwind consistente, sin estilos inline ni CSS externo | `grep -n 'style=\|<style\|\.css' index.html` → 0 | ✅ | 2026-11-19 sobre `c4aea9f` y revalidado sobre `9b370ba`: `grep -cE 'style=\|<style\|\.css\|rel="stylesheet"\|\.style\.' index.html` → 0 (CA-02) · `grep -c overflow-x-auto index.html` → 0 · único `<script src>` = CDN de Tailwind, sin `fetch`/XHR (CA-03). Revalidar al cerrar cada sección. |
| E5 | Todas las filas tienen dropdown ⋮ que abre, cierra al clic y al clic fuera | Prueba en Usuarios, Agentes, Skills, Contrataciones, Errores | ⚠️ | Hecho en **1 de 5** secciones: Usuarios (`9b370ba`), ⋮ en las 7 filas con `aria-expanded`, cierre al elegir opción, clic fuera y Esc, un solo menú abierto, 2 últimas filas hacia arriba (CA-15…CA-19) → DOM simulado 42/42 OK (P3.3). Falta: Agentes, Skills, Contrataciones, Log y verificación en navegador (ERR-004). |
| E6 | "Ver detalle" abre modal en ≥4 secciones | Usuarios, Skills, Contrataciones, Errores (+ Configurar en Agentes) | ⚠️ | Hecho en **1 de 4** secciones: modal «Detalle de usuario» con los 10 campos de S5.4 (CA-20, CA-24; DOM simulado, `9b370ba`). Falta: Skills, Contrataciones, Log, «Configurar» en Agentes y verificación en navegador. |
| E7 | Modales cierran con botón y con backdrop | Probar en cada modal | ⚠️ | Modal de usuario: cierre con «Cerrar», ✕, backdrop (clic dentro no cierra) y Esc, `overflow-hidden` retirado y foco devuelto al ⋮ (CA-21…CA-23; DOM simulado, `9b370ba`). Falta: resto de modales y verificación en navegador. |
| E8 | Skills de agentes colapsadas por defecto, expanden/colapsan con transición | Probar en Agentes | ⬜ | |
| E9 | Toggle dark/claro cambia todo y persiste entre secciones | Alternar y navegar las 6 vistas | ⚠️ | Hecho: `darkMode: 'class'`, toggle en topbar con `localStorage.theme`, `aria-pressed`/`aria-label`, script en `<head>` antes del pintado (`b91ee39`); todas las clases de layout, Dashboard y Usuarios (tabla, badges, menú ⋮, modal) con par `dark:` (`grep -c 'dark:' index.html` → 86 sobre `9b370ba`). Falta: verificación en navegador de CA-40…CA-42 (ERR-004) y `dark:` en las 4 secciones pendientes. |
| E10 | Datos coherentes (mismo agente en Agentes, Contrataciones, Errores) | Cruce de nombres | ⚠️ | Hecho: constantes `SKILLS`, `AGENTES` y `CONTRATOS` idénticas a SPECS §10.1, §10.2 y §10.4; tarjetas del Dashboard calculadas desde ellas (245,00 € · 3 · 2) (`c4aea9f`); `USUARIOS` con los 7 registros de §10.3 y «Agentes de la empresa» calculado desde `AGENTES` (p. ej. US-05 «2 · Nova, Vega», US-07 «0 · Ninguno») (`9b370ba`). Decisión: los borrados de Agentes/Skills ocultarán IDs con un `Set`, sin mutar el dataset maestro. Falta: `ERRORES`, campos ampliados (descripciones, permisos, `promptSistema`, trazas) y contador del sidebar calculado desde `ERRORES` (hoy constante `6`). |
| E11 | HTML semántico (`section`, `table`, `nav`, `header`, `main`) | `grep` + revisión | ⚠️ | Hecho: `<aside>`, `<nav>`, `<header>`, `<main>`, 6 `<section>`, `<article>` en tarjetas, modal con `role="dialog"` y `aria-modal`; **1 de 5** tablas: Usuarios con `<table>`/`<thead>`/`<tbody>` y `<th scope="col">`, acciones como `<button type="button">` (`grep -cE '<table\|<thead\|<tbody'` → 3, `9b370ba`). Falta: las 4 `<table>` de Agentes, Skills, Contrataciones y Log (CA-04). |
| E12 | Layout usable en escritorio y tablet | Capturas 1440 y 768 px | ⚠️ | Hecho: sidebar `w-16 lg:w-64`, sin `overflow-x-auto`; a 768 px quedan 656 px útiles (768 − 64 − 48) → Dashboard en 2 columnas (cálculo). Falta: capturas/verificación manual a 1440 y 768 px (ERR-004) y comprobar tablas y dropdowns (CA-10, CA-19). |

## B. Requisitos de la tarea (checklist del enunciado)
**Spec**: ☐ `SPECS.md` en raíz · ☐ descripción del producto y admin · ☐ stack y restricciones · ☐ ≥3 specs por sección · ☐ inventario · ☐ criterios numerados (≥1 por dropdown, modal, colapsable, dark) · ☐ commit previo al HTML · ☐ propuesta Google Stitch usada como guía.

**Dashboard**: ☐ 4 tarjetas (icono, etiqueta, valor) · ☐ área ancho completo para gráfico semanal.
**Usuarios**: ☐ ≥5 filas (nombre, email, plan, badge estado) · ☐ dropdown Ver detalle/Eliminar · ☐ modal registro completo (botón + backdrop).
**Agentes**: ☐ ≥4 agentes (nombre, propietario, badge, skills colapsada) · ☐ expandir/colapsar suave · ☐ dropdown Configurar/Eliminar; Configurar → modal con `<textarea>` editable con el prompt de sistema.
**Skills**: ☐ ≥4 skills (nombre, descripción, nº agentes) · ☐ explicación de "skill" en el panel · ☐ dropdown Ver detalle/Eliminar.
**Contrataciones**: ☐ ≥4 contratos (cliente, agente, skills, inicio/fin, importe) · ☐ dropdown; Ver detalle → desglose con precio por skill.
**Log de errores**: ☐ ≥6 entradas (timestamp, agente, badge por tipo/gravedad, descripción) · ☐ dropdown Ver detalle (traza) / Marcar como resuelto.
**Global**: ☐ toggle dark en topbar con `dark:` y persistente · ☐ dropdowns cierran al clic fuera · ☐ modales cierran en backdrop · ☐ solo JS vanilla, Tailwind solo CDN · ☐ sidebar persistente con sección activa · ☐ único `index.html` (o uno por sección).

**Entrega**: ☐ repo público en GitHub · ☐ `SPECS.md` e `index.html` visibles.

## C. Registro de verificaciones
| Fecha | Fase | Qué se probó | Resultado | Evidencia |
|---|---|---|---|---|
| 2026-11-19 | F3 (P3.2b) | Restricciones y estructura sobre `c4aea9f` (grep) | OK | `style=`/`<style`/`.css`/`rel="stylesheet"`/`.style.` → 0 · `overflow-x-auto` → 0 · `<script src>` → 1 (CDN) · `<section` → 6 · `<symbol` → 27 |
| 2026-11-19 | F3 (P3.2b) | Orden SPECS → HTML (`git log --reverse --name-status`) | OK con aclaración | `b3b4d64` (SPECS) < `b91ee39` (index.html del prototipo); `0dfaf38`/`a9b4fe4` = plantilla |
| 2026-11-19 | F3 | Verificación visual en navegador | Pendiente | ERR-004: la hace el usuario por la pestaña Ports (8000) |
| 2026-11-19 | F3 (P3.3 / P3.3b) | Usuarios: DOM simulado (jsdom, scratchpad) + restricciones (grep) sobre `9b370ba` | OK (sin navegador) | jsdom 42/42 (CA-15…CA-24, CA-39) · `style=`/CSS → 0 · `overflow-x-auto` → 0 · `<script src>` → 1 · `fetch(`/XHR → 0 · `<section` → 6 · `<symbol` → 27 · `<table\|<thead\|<tbody` → 3 · `dark:` → 86. Verificación manual en navegador de P3.1–P3.3: **pendiente** (sin resultado aportado). |
