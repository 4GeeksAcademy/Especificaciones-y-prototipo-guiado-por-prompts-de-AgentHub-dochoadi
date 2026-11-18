# SPECS — AgentHub Admin Panel

> Contrato de implementación (fase F1). Un agente de código debe poder construir `index.html` a partir de este documento **sin hacer preguntas**. Todo lo que no está aquí no se implementa; todo lo que está aquí es obligatorio. Fecha de referencia del prototipo: **18/11/2026** ("hoy" para estados y métricas).

---

## 1. Producto y usuario administrador

**AgentHub** es un SaaS B2B en el que empresas cliente **alquilan agentes de IA** preconfigurados. Cada agente se **equipa con skills** (capacidades contratables por separado: navegar la web, leer documentos, gestionar calendarios, enviar emails, consultar bases de datos SQL) y se despliega para tareas de negocio. El cliente paga una cuota mensual por cada skill activada en cada agente durante la vigencia de su contrato.

**Usuario del panel**: el **administrador interno** de AgentHub (equipo fundador / soporte). No es un cliente. Necesita:
- Ver de un vistazo la salud del negocio (ingresos, descuentos) y de la plataforma (agentes activos y fallando).
- Consultar y gestionar usuarios de las empresas cliente, agentes desplegados, catálogo de skills y contrataciones.
- Diagnosticar incidencias técnicas en el log de errores y marcarlas como resueltas.

**Objetivo del prototipo**: validar la experiencia con el equipo y servir de referencia visual y funcional al equipo de backend. No hay persistencia de datos (salvo la preferencia de tema).

---

## 2. Stack y restricciones

| # | Restricción | Detalle obligatorio |
|---|---|---|
| R1 | Fichero único | Todo el prototipo vive en **un único `index.html`** en la raíz, con las 6 secciones. |
| R2 | Tailwind por CDN | Exactamente: `<script src="https://cdn.tailwindcss.com"></script>` seguido de `<script>tailwind.config = { darkMode: 'class' }</script>` en `<head>`. |
| R3 | Sin CSS propio | Prohibidos `<style>`, atributos `style="…"`, `<link rel="stylesheet">`, `@apply`, ficheros `.css`. Prohibido también asignar `element.style.*` desde JS. La visibilidad se alterna **solo** con clases (`hidden`, `opacity-0`, `grid-rows-[0fr]`…). |
| R4 | JS vanilla | Un `<script>` al final de `<body>` con JavaScript ES2020 sin librerías (sin jQuery, sin React/Vue/Alpine, sin módulos externos, sin build). Un segundo `<script>` mínimo en `<head>` solo para aplicar el tema guardado (ver S3.6). |
| R5 | Sin backend | Ninguna petición de red salvo el CDN de Tailwind. Datos en constantes JS (§10). Acciones (eliminar, resolver, guardar prompt) modifican solo el estado en memoria y el DOM; se pierden al recargar. |
| R6 | Iconos | SVG inline (trazos de Heroicons outline 24×24, `fill="none" stroke="currentColor" stroke-width="1.5"`), con clases `w-5 h-5` (o `w-6 h-6` en tarjetas). Sin fuentes de iconos ni imágenes externas. |
| R7 | Idioma y formato | UI en **español**. Importes con `Intl.NumberFormat('es-ES', { style: 'currency', currency: 'EUR' })` → `245,00 €`. Fechas `dd/mm/aaaa`; timestamps `dd/mm/aaaa hh:mm:ss`. |
| R8 | HTML semántico | `<aside>` + `<nav>` (sidebar), `<header>` (topbar), `<main>`, una `<section>` por vista con `<h2>`, `<table>/<thead>/<tbody>/<th scope="col">` para listados, `<button type="button">` para toda acción, `role="dialog" aria-modal="true"` en el modal, `role="menu"/"menuitem"` en dropdowns. |
| R9 | Modo oscuro | Clase `dark` en `<html>`; **toda** clase de color lleva su par `dark:` (paleta en §11). |
| R10 | Responsive | Diseñado para **escritorio (≥1024 px)** y **tablet (768–1023 px)**. Móvil < 768 px no es objetivo (se usa el layout de tablet). |

---

## 3. Layout global

**S3.1 — Estructura de página.** `<body class="bg-slate-50 text-slate-900 dark:bg-slate-950 dark:text-slate-100 antialiased">`. Contiene: `<aside>` fijo a la izquierda (`fixed inset-y-0 left-0 z-30`), y un contenedor de contenido con margen izquierdo igual al ancho del sidebar (`pl-16 lg:pl-64`) que incluye el `<header>` (topbar) y el `<main class="p-6 lg:p-8 max-w-7xl">`. Dentro de `<main>` hay **6 `<section>`**, con ids `view-dashboard`, `view-usuarios`, `view-agentes`, `view-skills`, `view-contrataciones`, `view-errores`, cada una con `aria-labelledby` apuntando a su `<h2>` (que es `sr-only` porque el título visible está en el topbar). Solo una sección es visible; las demás llevan `hidden`.

**S3.2 — Sidebar persistente.** `<aside class="w-16 lg:w-64 flex flex-col border-r border-slate-200 bg-white dark:border-slate-800 dark:bg-slate-900">`.
- Cabecera (h-16, `border-b`): logotipo cuadrado `h-9 w-9 rounded-lg bg-indigo-600 text-white font-bold grid place-items-center` con el texto "AH"; a su derecha (solo `lg:block`, oculto en tablet) "AgentHub" (`font-semibold`) y debajo "Panel de administración" (`text-xs text-slate-500 dark:text-slate-400`).
- `<nav aria-label="Navegación principal">` con `<ul>` de **6 enlaces** `<a href="#…">`, en este orden, cada uno con icono + texto:

| Orden | Texto | href | Icono Heroicons |
|---|---|---|---|
| 1 | Dashboard | `#dashboard` | squares-2x2 |
| 2 | Usuarios | `#usuarios` | users |
| 3 | Agentes | `#agentes` | cpu-chip |
| 4 | Skills | `#skills` | puzzle-piece |
| 5 | Contrataciones | `#contrataciones` | document-text |
| 6 | Log de errores | `#errores` | exclamation-triangle |

- Enlace base: `flex items-center gap-3 rounded-lg px-3 py-2 text-sm font-medium text-slate-600 hover:bg-slate-100 hover:text-slate-900 dark:text-slate-400 dark:hover:bg-slate-800 dark:hover:text-slate-100`; en tablet `justify-center lg:justify-start`.
- El texto del enlace va en `<span class="hidden lg:inline">`; en tablet el enlace lleva además `title` y `aria-label` con el texto, para que siga siendo accesible.
- El enlace "Log de errores" muestra a la derecha un contador de errores **abiertos** (`ml-auto rounded-full bg-red-100 px-2 text-xs font-semibold text-red-800 dark:bg-red-900/40 dark:text-red-300`); valor inicial **6**; en tablet el contador se muestra como punto rojo `absolute top-1 right-1 h-2 w-2 rounded-full bg-red-500` (enlace `relative`). Se decrementa al marcar un error como resuelto (S9.5). Si llega a 0, se oculta.
- Pie del sidebar (`mt-auto border-t p-3`, solo `lg:flex`): avatar circular "AD" (`h-8 w-8 rounded-full bg-slate-200 dark:bg-slate-700 text-xs font-semibold`) + "Admin interno" / "soporte@agenthub.io" (`text-xs`).

**S3.3 — Indicador de sección activa.** El enlace de la vista visible recibe `aria-current="page"` y las clases `bg-indigo-50 text-indigo-700 dark:bg-indigo-500/10 dark:text-indigo-300` más una barra izquierda (`relative` + `<span>` absoluto `left-0 inset-y-1 w-1 rounded-r bg-indigo-600 dark:bg-indigo-400`, visible solo en el activo). Los demás enlaces pierden esas clases y el atributo. Solo hay **un** enlace activo a la vez.

**S3.4 — Navegación por hash.** La vista visible se determina con `location.hash` (`#dashboard`, `#usuarios`, `#agentes`, `#skills`, `#contrataciones`, `#errores`). Al cargar sin hash o con un hash desconocido se muestra Dashboard (y se reemplaza el hash por `#dashboard` con `history.replaceState`). En el evento `hashchange`: se muestra la sección correspondiente (quitar `hidden`), se ocultan las demás, se actualiza el indicador activo (S3.3), el título del topbar (S3.5), `document.title = "AgentHub · <Título>"`, se cierran dropdowns y modal abiertos y se hace `window.scrollTo(0, 0)`. El sidebar no se re-renderiza: es persistente.

**S3.5 — Topbar.** `<header class="sticky top-0 z-20 flex h-16 items-center justify-between border-b border-slate-200 bg-white/80 px-6 backdrop-blur dark:border-slate-800 dark:bg-slate-900/80">`.
- Izquierda: `<h1 id="page-title" class="text-lg font-semibold">` con el título de la sección activa (mismos textos que el sidebar: "Dashboard", "Usuarios", "Agentes", "Skills", "Contrataciones", "Log de errores") y debajo `<p class="text-xs text-slate-500 dark:text-slate-400">` con un subtítulo fijo por sección:
  - Dashboard: "Resumen de noviembre 2026"
  - Usuarios: "Usuarios de las empresas cliente"
  - Agentes: "Agentes desplegados y sus skills"
  - Skills: "Catálogo de capacidades contratables"
  - Contrataciones: "Contratos de alquiler de agentes"
  - Log de errores: "Incidencias técnicas de los agentes"
- Derecha: toggle de modo oscuro (S3.6).

**S3.6 — Toggle claro/oscuro persistente.** Botón `id="theme-toggle"` `type="button"` en el topbar: `rounded-lg p-2 text-slate-600 hover:bg-slate-100 dark:text-slate-300 dark:hover:bg-slate-800`, con dos SVG: luna (`dark:hidden`) y sol (`hidden dark:block`). `aria-label` = "Activar modo oscuro" en claro / "Activar modo claro" en oscuro; `aria-pressed` = `true` en oscuro.
- Al hacer clic: `document.documentElement.classList.toggle('dark')` y `localStorage.setItem('theme', 'dark' | 'light')`.
- Script en `<head>` (antes de pintar): si `localStorage.getItem('theme') === 'dark'` añade `dark` a `<html>`. Si no hay valor guardado, el tema por defecto es **claro**.
- Como es una sola página, el tema se mantiene al navegar entre las 6 secciones y tras recargar.

**S3.7 — Tablet (768–1023 px).** El sidebar pasa a **rail de iconos** `w-16` (solo iconos centrados, sin textos, sin pie), sigue fijo y visible: no hay menú hamburguesa. El contenido usa `pl-16`. Las cuadrículas pasan a 2 columnas o 1 según cada sección. Las tablas **no** llevan `overflow-x-auto` (recortaría los dropdowns): se diseñan para caber en 688 px útiles (texto `text-sm`, celdas `px-3 py-3 lg:px-4`, columnas secundarias ocultas con `hidden lg:table-cell` donde se indique). Ningún contenido provoca scroll horizontal de la página a 768 px.

**S3.8 — Contenedor de tabla común.** Todas las tablas van dentro de `<div class="rounded-xl border border-slate-200 bg-white shadow-sm dark:border-slate-800 dark:bg-slate-900">` (sin `overflow-hidden`). `<table class="w-full text-left text-sm">`; `<thead>` sin fondo, con `border-b border-slate-200 dark:border-slate-800` y `<th>` `px-4 py-3 text-xs font-semibold uppercase tracking-wide text-slate-500 dark:text-slate-400`; filas `border-b border-slate-100 last:border-0 hover:bg-slate-50 dark:border-slate-800 dark:hover:bg-slate-800/50`. Última columna sin título visible (`<th><span class="sr-only">Acciones</span></th>`), alineada a la derecha, contiene el dropdown ⋮ (§11.3). Encima de cada tabla, una barra con el recuento: `<p class="mb-3 text-sm text-slate-500 dark:text-slate-400">` ("7 usuarios", "6 agentes", etc., actualizado al eliminar).

---

## 4. Dashboard (`#dashboard`)

**S4.1 — Cuadrícula de tarjetas de métricas.** Cuatro tarjetas de métrica (§11.2) en `grid grid-cols-1 gap-6 sm:grid-cols-2` (2×2 en escritorio y tablet), en este orden:

| # | Etiqueta | Valor | Texto secundario | Icono | Acento |
|---|---|---|---|---|---|
| 1 | Ingresos totales del mes | **245,00 €** | "Noviembre 2026 · 4 contratos activos" | banknotes | emerald |
| 2 | Pérdida por descuentos y cupones | **31,50 €** | "12,9 % sobre ingresos del mes" | receipt-percent | amber |
| 3 | Agentes activos | **3** | "de 6 agentes desplegados" | cpu-chip | indigo |
| 4 | Agentes fallando | **2** | "Nova, Vega · ver log de errores" | exclamation-triangle | red |

**S4.2 — Origen de los valores.** Las tarjetas 1, 3 y 4 se **calculan en JS** al cargar a partir del dataset (§10): (1) suma de `precioMensual` (suma de precios de sus skills) de los contratos con `estado === 'activo'` → 55 + 90 + 35 + 65 = **245,00 €**; (3) nº de agentes con `estado === 'activo'` → 3; (4) nº de agentes con `estado === 'fallando'` → 2, y el texto secundario lista sus nombres. La tarjeta 2 es un valor **hardcodeado** `31,50 €` (desglose en §10.6). Eliminar filas en otras secciones **no** recalcula el Dashboard (los valores se calculan una sola vez al cargar).

**S4.3 — Estilo de acento por tarjeta.** Cada tarjeta lleva el icono dentro de un cuadrado `h-11 w-11 rounded-lg grid place-items-center` con el color de acento: emerald `bg-emerald-100 text-emerald-700 dark:bg-emerald-900/40 dark:text-emerald-300`; amber `bg-amber-100 text-amber-700 dark:bg-amber-900/40 dark:text-amber-300`; indigo `bg-indigo-100 text-indigo-700 dark:bg-indigo-900/40 dark:text-indigo-300`; red `bg-red-100 text-red-700 dark:bg-red-900/40 dark:text-red-300`. El valor de la tarjeta 4 se pinta en `text-red-600 dark:text-red-400`; los demás en `text-slate-900 dark:text-white`.

**S4.4 — Tarjeta 4 enlazada.** La tarjeta "Agentes fallando" es un `<a href="#errores">` con `hover:ring-2 hover:ring-red-200 dark:hover:ring-red-900 transition`; al hacer clic navega a Log de errores. Las otras tres tarjetas son `<article>` no interactivos.

**S4.5 — Marcador del gráfico semanal.** Debajo de las tarjetas (`mt-6`), un `<div>` de **ancho completo** `h-72 rounded-xl border-2 border-dashed border-slate-300 bg-white/50 dark:border-slate-700 dark:bg-slate-900/50 flex flex-col items-center justify-center gap-2 text-slate-400 dark:text-slate-500`, con icono chart-bar (`w-10 h-10`), la etiqueta centrada **"Gráfico de actividad semanal"** (`text-sm font-medium`) y debajo "Ejecuciones de agentes · 12–18 nov 2026 (pendiente de integrar)" (`text-xs`). Lleva `role="img"` y `aria-label="Marcador de posición: gráfico de actividad semanal"`. No contiene gráfico real.

---

## 5. Gestión de usuarios (`#usuarios`)

**S5.1 — Tabla de usuarios.** Tabla (S3.8) con los **7 usuarios** de §10.3, columnas: **Nombre** (nombre en `font-medium` y debajo, `text-xs text-slate-500`, la empresa), **Email** (`text-slate-600 dark:text-slate-300`), **Plan** (badge neutro/acento, S5.2), **Estado** (badge, S5.2), **Acciones** (⋮). Recuento encima: "7 usuarios".

**S5.2 — Badges de plan y estado.** Plan: Starter → neutro (slate), Pro → indigo, Enterprise → `bg-violet-100 text-violet-800 dark:bg-violet-900/40 dark:text-violet-300`. Estado: `activo` → emerald con texto "Activo"; `suspendido` → red "Suspendido"; `pendiente` → amber "Pendiente". Badge base en §11.5.

**S5.3 — Dropdown de acciones.** Cada fila tiene un dropdown ⋮ (§11.3) con `aria-label="Acciones para <nombre>"` y dos opciones: **"Ver detalle"** (icono eye) y **"Eliminar"** (icono trash, `text-red-600 dark:text-red-400`). Elegir cualquier opción cierra el menú.

**S5.4 — Modal "Ver detalle" de usuario.** Abre el modal (§11.4) tamaño `max-w-lg` con título "Detalle de usuario" y subtítulo el nombre. Cuerpo: `<dl class="grid grid-cols-1 sm:grid-cols-2 gap-x-6 gap-y-4">` con pares `<dt class="text-xs uppercase text-slate-500">`/`<dd class="text-sm font-medium">` de **todos** los campos: ID, Nombre, Email, Empresa, Rol, Plan (badge), Estado (badge), Fecha de alta, Último acceso ("Sin accesos" si es `null`), Agentes de la empresa (número y nombres, p. ej. "2 · Atlas, Sirius"; "0 · Ninguno"). Pie: botón "Cerrar". Se cierra con el botón "Cerrar", el botón ✕, clic en el backdrop o Esc.

**S5.5 — Eliminar usuario.** "Eliminar" muestra `confirm('¿Eliminar a <nombre>? La acción solo afecta a esta vista.')`. Si se acepta, se elimina el usuario del array en memoria, se quita su fila y se actualiza el recuento ("6 usuarios"). Si se cancela, no cambia nada.

---

## 6. Gestión de agentes (`#agentes`)

**S6.1 — Listado de agentes.** Tabla (S3.8) con los **6 agentes** de §10.2, columnas: **Agente** (avatar `h-9 w-9 rounded-lg bg-indigo-100 text-indigo-700 dark:bg-indigo-900/40 dark:text-indigo-300 font-semibold grid place-items-center` con la inicial + nombre `font-medium` + ID `text-xs text-slate-500`), **Propietario** (empresa cliente), **Estado** (badge), **Skills** (lista colapsable, S6.3), **Acciones** (⋮). Alineación vertical `align-top`. Recuento: "6 agentes".

**S6.2 — Badge de estado del agente.** `activo` → emerald "Activo" con punto `h-1.5 w-1.5 rounded-full bg-emerald-500`; `inactivo` → slate "Inactivo" con punto `bg-slate-400`; `fallando` → red "Fallando" con punto `bg-red-500 animate-pulse`.

**S6.3 — Lista de skills colapsable.** Componente §11.6 en la celda Skills. Botón `type="button"` con texto "Skills (n)" (n = nº de skills del agente) y un chevron-down `w-4 h-4 transition-transform duration-300` que rota (`rotate-180`) al expandir; `aria-expanded="false"` y `aria-controls` hacia el panel. Panel **colapsado por defecto** (`grid grid-rows-[0fr] opacity-0 transition-all duration-300 ease-in-out`, hijo `overflow-hidden`). Al expandir: `grid-rows-[1fr] opacity-100`. Contenido: `<ul class="mt-2 flex flex-wrap gap-1.5">` con un chip por skill (`rounded-md bg-slate-100 px-2 py-0.5 text-xs text-slate-700 dark:bg-slate-800 dark:text-slate-300`) con el nombre de la skill (p. ej. "Navegación web"). Un segundo clic colapsa con la misma transición. Cada agente se expande/colapsa de forma independiente.

**S6.4 — Dropdown de acciones.** Opciones: **"Configurar"** (icono cog-6-tooth) y **"Eliminar"** (rojo). Elegir una opción cierra el menú.

**S6.5 — Modal "Configurar".** Modal `max-w-2xl`, título "Configurar agente", subtítulo "<Nombre> · <Propietario>". Cuerpo: fila de metadatos (badge de estado + chips de skills) y un `<label for="prompt-sistema" class="text-sm font-medium">Prompt de sistema</label>` con un **`<textarea id="prompt-sistema" rows="10">` editable** (`w-full rounded-lg border border-slate-300 bg-white p-3 font-mono text-sm text-slate-800 focus:border-indigo-500 focus:ring-2 focus:ring-indigo-500/30 dark:border-slate-700 dark:bg-slate-950 dark:text-slate-200`) precargado con el `promptSistema` del agente (§10.2). Debajo, `text-xs text-slate-500` con el recuento de caracteres actualizado en `input`. Pie: "Cancelar" (secundario; descarta cambios y cierra) y "Guardar cambios" (primario `bg-indigo-600 text-white hover:bg-indigo-700`; guarda el texto en el objeto del agente en memoria y cierra). Al reabrir "Configurar" del mismo agente se ve el texto guardado. Cierre también con ✕, backdrop y Esc (sin guardar).

**S6.6 — Eliminar agente.** `confirm('¿Eliminar el agente <nombre>? La acción solo afecta a esta vista.')`; si se acepta se quita la fila y se actualiza el recuento. No modifica otras secciones.

---

## 7. Skills (`#skills`)

**S7.1 — Bloque explicativo "¿Qué es una skill?".** Encima del catálogo, `<aside class="mb-6 flex gap-4 rounded-xl border-l-4 border-indigo-500 bg-indigo-50 p-5 dark:bg-indigo-500/10">` con icono information-circle (`text-indigo-600 dark:text-indigo-300`), título `<h3 class="font-semibold">¿Qué es una skill?</h3>` y el texto literal:
> "Una skill es una capacidad que se habilita en un agente de AgentHub para que pueda actuar fuera de la conversación: navegar por la web, leer documentos, gestionar un calendario, enviar emails o consultar una base de datos. Cada skill se contrata por separado con una cuota mensual por agente, y el importe de un contrato es la suma de las skills del agente multiplicada por los meses de vigencia."

**S7.2 — Catálogo de skills.** Tabla (S3.8) con las **5 skills** de §10.1, columnas: **Skill** (icono propio en cuadrado `h-9 w-9 rounded-lg bg-slate-100 dark:bg-slate-800` + nombre `font-medium` + ID `text-xs text-slate-500`), **Descripción** (descripción breve, `text-slate-600 dark:text-slate-300`, `max-w-md`), **Precio/mes** (`hidden lg:table-cell`, formato €), **Agentes** (badge indigo "n agentes"), **Acciones** (⋮). Iconos: SK-01 globe-alt, SK-02 document-magnifying-glass, SK-03 calendar-days, SK-04 envelope, SK-05 circle-stack. Recuento: "5 skills".

**S7.3 — Nº de agentes calculado.** La columna Agentes se calcula en JS contando los agentes de §10.2 cuyo array `skills` incluye el ID: SK-01 = 3, SK-02 = 3, SK-03 = 2, SK-04 = 3, SK-05 = 2. Texto "1 agente" en singular, "n agentes" en plural.

**S7.4 — Dropdown de acciones.** Opciones **"Ver detalle"** y **"Eliminar"** (rojo). Elegir una cierra el menú.

**S7.5 — Modal "Ver detalle" de skill.** Modal `max-w-lg`, título "Detalle de skill", subtítulo el nombre. Cuerpo `<dl>` en 2 columnas: ID, Nombre, Precio/mes, Permisos requeridos, Descripción completa (`sm:col-span-2`) y "Agentes con la skill habilitada" (`sm:col-span-2`) con la lista de nombres de agentes en chips, cada uno con su badge de estado mini. Pie: "Cerrar". Cierre con botón, ✕, backdrop y Esc.

**S7.6 — Eliminar skill.** `confirm('¿Eliminar la skill <nombre> del catálogo? La acción solo afecta a esta vista.')`; si se acepta se quita la fila y se actualiza el recuento. No modifica las skills mostradas en Agentes ni Contrataciones.

---

## 8. Contrataciones (`#contrataciones`)

**S8.1 — Tabla de contratos.** Tabla (S3.8) con los **6 contratos** de §10.4, columnas: **Cliente** (empresa `font-medium` + ID de contrato `text-xs text-slate-500`), **Agente** (nombre + debajo badge de estado del contrato, S8.2), **Skills contratadas** (chips como S6.3, `flex flex-wrap gap-1`, siempre visibles), **Inicio**, **Fin** (`whitespace-nowrap`, `dd/mm/aaaa`), **Importe** (alineado a la derecha, `font-semibold tabular-nums`, total pagado), **Acciones** (⋮). Recuento: "6 contratos".

**S8.2 — Badge de estado del contrato.** `activo` → emerald "Activo"; `finalizado` → slate "Finalizado"; `cancelado` → red "Cancelado".

**S8.3 — Dropdown de acciones.** Opciones **"Ver detalle"** (icono eye) y **"Descargar factura"** (icono arrow-down-tray). "Descargar factura" **solo cierra el menú** (sin lógica, sin descarga, sin alerta). No hay opción Eliminar en contratos.

**S8.4 — Modal "Ver detalle" con desglose.** Modal `max-w-2xl`, título "Detalle de contrato", subtítulo "<ID> · <Cliente> — <Agente>". Cuerpo:
1. `<dl>` de 4 columnas en escritorio (`grid-cols-2 lg:grid-cols-4`): Estado (badge), Inicio, Fin, Duración ("12 meses").
2. Tabla de desglose `<table>` con columnas **Skill | Precio/mes | Meses | Subtotal**, una fila por skill contratada (Subtotal = precio × meses).
3. `<tfoot>` con dos filas: "Total mensual" (suma de precios/mes) y **"Total del contrato"** (`font-semibold text-base`), igual al importe de la tabla principal.
4. Línea de cálculo `text-xs text-slate-500 font-mono`, p. ej. `(25,00 € + 30,00 €) × 12 meses = 660,00 €`.
Pie: "Descargar factura" (secundario, sin lógica) y "Cerrar". Cierre con botón, ✕, backdrop y Esc.

**S8.5 — Cálculo en JS.** El importe se calcula como `suma(precio de cada skill del contrato) × meses` a partir de §10.1 y §10.4, y debe coincidir con los valores de la tabla §10.4 (660, 900, 210, 360, 45, 390 €).

---

## 9. Log de errores (`#errores`)

**S9.1 — Tabla de errores.** Tabla (S3.8) con las **8 entradas** de §10.5, **ordenadas por timestamp descendente** (la más reciente arriba), columnas: **Timestamp** (`whitespace-nowrap font-mono text-xs`, `dd/mm/aaaa hh:mm:ss`), **Agente** (nombre + propietario en `text-xs text-slate-500`), **Gravedad** (badge, S9.2), **Descripción** (breve, `text-slate-600 dark:text-slate-300`, con el ID `LOG-00n` delante en `font-mono text-xs text-slate-400`), **Estado** (badge Abierto/Resuelto), **Acciones** (⋮). Recuento: "8 entradas · 6 abiertas" (se actualiza al resolver).

**S9.2 — Badge de gravedad con código de color.** `Crítico` → `bg-red-100 text-red-800 dark:bg-red-900/40 dark:text-red-300` con icono x-circle mini; `Error` → `bg-orange-100 text-orange-800 dark:bg-orange-900/40 dark:text-orange-300`; `Aviso` → amber (paleta §11.5); `Info` → `bg-sky-100 text-sky-800 dark:bg-sky-900/40 dark:text-sky-300`. Estado: `abierto` → slate "Abierto"; `resuelto` → emerald "Resuelto" con icono check.

**S9.3 — Dropdown de acciones.** Opciones **"Ver detalle"** y **"Marcar como resuelto"** (icono check-circle, `text-emerald-700 dark:text-emerald-400`). Si el error ya está resuelto, la opción se muestra como "Ya resuelto", con `disabled` y `opacity-50 cursor-not-allowed`. Elegir una opción habilitada cierra el menú.

**S9.4 — Modal "Ver detalle" con traza.** Modal `max-w-2xl`, título "Detalle del error", subtítulo "<ID> · <Agente>". Cuerpo: `<dl>` (Timestamp, Agente, Propietario, Skill implicada, Gravedad (badge), Estado (badge), Descripción `sm:col-span-2`), y bajo el rótulo "Traza completa" un `<pre class="max-h-72 overflow-auto rounded-lg bg-slate-900 p-4 font-mono text-xs leading-relaxed text-slate-100 dark:bg-black/60">` con la traza literal de §10.5. Pie: botón "Marcar como resuelto" (primario emerald `bg-emerald-600 text-white hover:bg-emerald-700`; oculto si ya está resuelto) y "Cerrar". Pulsar "Marcar como resuelto" en el modal aplica S9.5 y cierra el modal. Cierre con botón, ✕, backdrop y Esc.

**S9.5 — Marcar como resuelto (solo cliente).** Cambia `estado` del error en memoria a `resuelto`; en su fila, el badge de Estado pasa de "Abierto" (slate) a **"Resuelto"** (emerald) sin recargar, la opción del menú pasa a "Ya resuelto" deshabilitada, la fila recibe `opacity-60`, el recuento "x abiertas" baja en 1 y el contador del sidebar (S3.2) baja en 1. El badge de gravedad se mantiene. No hay forma de reabrir.

---

## 10. Datos hardcodeados (dataset maestro)

Constantes JS en el `<script>` final: `SKILLS`, `AGENTES`, `USUARIOS`, `CONTRATOS`, `ERRORES`. Son la **única fuente** de datos: todas las tablas y modales se renderizan desde ellas (no se duplica contenido en HTML estático). Los nombres de agentes, empresas y skills son idénticos en todas las secciones. Empresas cliente: **Acme Corp, Initech, Globex, Umbrella, Stark Labs**.

### 10.1 Skills (`SKILLS`)

| ID | Nombre | Precio/mes | Descripción breve (tabla) | Descripción completa (modal) | Permisos requeridos | Agentes (calculado) |
|---|---|---|---|---|---|---|
| SK-01 | Navegación web | 25 € | Abre páginas, rellena formularios y extrae información de sitios web. | Permite al agente controlar un navegador sin interfaz para visitar URLs, seguir enlaces, rellenar formularios y extraer texto o tablas. Respeta robots.txt y un límite de 60 peticiones por minuto. | Acceso a Internet saliente | 3 (Atlas, Orion, Nova) |
| SK-02 | Lectura de documentos | 30 € | Lee y resume PDF, Word y hojas de cálculo. | Extrae texto, tablas y metadatos de PDF, DOCX y XLSX de hasta 300 páginas, con OCR para documentos escaneados, y genera resúmenes estructurados. | Lectura del almacenamiento del cliente | 3 (Atlas, Orion, Vega) |
| SK-03 | Gestión de calendario | 20 € | Crea, mueve y cancela reuniones en el calendario. | Se integra vía OAuth con Google Calendar y Microsoft 365 para consultar disponibilidad, proponer huecos, crear, mover y cancelar eventos e invitar asistentes. | OAuth de calendario | 2 (Sirius, Nova) |
| SK-04 | Envío de emails | 15 € | Redacta y envía emails transaccionales y de seguimiento. | Redacta y envía emails desde el dominio verificado del cliente vía SMTP, con plantillas, adjuntos y reintento automático ante errores temporales. | SMTP del dominio del cliente | 3 (Sirius, Lyra, Nova) |
| SK-05 | Consulta SQL | 35 € | Ejecuta consultas de solo lectura sobre bases de datos. | Ejecuta consultas SELECT de solo lectura sobre PostgreSQL o MySQL del cliente, con tiempo máximo de 60 s por consulta y devolución de resultados en tabla. | Credenciales de BD de solo lectura | 2 (Orion, Vega) |

### 10.2 Agentes (`AGENTES`)

| ID | Nombre | Propietario | Estado | Skills | Precio mensual (suma) |
|---|---|---|---|---|---|
| AG-01 | Atlas | Acme Corp | activo | SK-01, SK-02 | 25 + 30 = 55 € |
| AG-02 | Orion | Initech | activo | SK-01, SK-02, SK-05 | 25 + 30 + 35 = 90 € |
| AG-03 | Sirius | Acme Corp | activo | SK-03, SK-04 | 20 + 15 = 35 € |
| AG-04 | Lyra | Umbrella | inactivo | SK-04 | 15 € |
| AG-05 | Nova | Globex | fallando | SK-01, SK-03, SK-04 | 25 + 20 + 15 = 60 € |
| AG-06 | Vega | Globex | fallando | SK-02, SK-05 | 30 + 35 = 65 € |

Derivados: activos = 3 (Atlas, Orion, Sirius) · inactivos = 1 (Lyra) · fallando = 2 (Nova, Vega). Agentes por empresa: Acme Corp 2 · Initech 1 · Globex 2 · Umbrella 1 · Stark Labs 0.

**Prompt de sistema (`promptSistema`, texto literal para el `<textarea>`):**
- **Atlas**: "Eres Atlas, agente de investigación de mercado de Acme Corp. Usa la navegación web para localizar información pública de competidores y la lectura de documentos para resumir informes. Responde en español, cita siempre la URL o el documento de origen y no inventes cifras."
- **Orion**: "Eres Orion, analista de operaciones de Initech. Consulta la base de datos de pedidos solo con sentencias SELECT, contrasta los resultados con los informes PDF del equipo y busca en la web referencias de proveedores. Entrega conclusiones en tablas y señala cualquier dato inconsistente."
- **Sirius**: "Eres Sirius, asistente de agenda comercial de Acme Corp. Gestiona el calendario del equipo de ventas: propone huecos, agenda reuniones con clientes y envía el email de confirmación con la agenda. No agendes fuera del horario 9:00–18:00 (CET)."
- **Lyra**: "Eres Lyra, agente de seguimiento de Umbrella. Envía emails de seguimiento a clientes que no han respondido en 7 días, con tono cordial y breve. Nunca adjuntes documentos internos ni envíes más de un recordatorio por semana al mismo contacto."
- **Nova**: "Eres Nova, coordinadora de eventos de Globex. Busca en la web salas y proveedores, reserva fechas en el calendario corporativo y envía invitaciones por email a los asistentes. Confirma siempre con el responsable antes de cerrar una reserva."
- **Vega**: "Eres Vega, agente de reporting financiero de Globex. Extrae datos de la base de datos contable con consultas de solo lectura y cruza las cifras con los PDF de facturas. Genera un resumen mensual y marca cualquier diferencia superior al 1 %."

### 10.3 Usuarios (`USUARIOS`)

| ID | Nombre | Email | Empresa | Rol | Plan | Estado | Fecha de alta | Último acceso | Agentes de la empresa |
|---|---|---|---|---|---|---|---|---|---|
| US-01 | Laura Méndez | laura.mendez@acmecorp.com | Acme Corp | Administradora de cuenta | Enterprise | activo | 12/11/2025 | 18/11/2026 09:42 | 2 · Atlas, Sirius |
| US-02 | Daniel Ortega | daniel.ortega@acmecorp.com | Acme Corp | Operador | Enterprise | activo | 08/01/2026 | 17/11/2026 18:05 | 2 · Atlas, Sirius |
| US-03 | Peter Gibbons | peter.gibbons@initech.com | Initech | Administrador de cuenta | Pro | activo | 20/02/2026 | 16/11/2026 11:30 | 1 · Orion |
| US-04 | Marta Ruiz | marta.ruiz@globex.com | Globex | Administradora de cuenta | Pro | activo | 03/12/2025 | 18/11/2026 08:47 | 2 · Nova, Vega |
| US-05 | Javier Lozano | javier.lozano@globex.com | Globex | Operador | Pro | pendiente | 14/11/2026 | Sin accesos (`null`) | 2 · Nova, Vega |
| US-06 | Carlos Vidal | carlos.vidal@umbrella.com | Umbrella | Administrador de cuenta | Starter | suspendido | 25/01/2026 | 30/04/2026 17:12 | 1 · Lyra |
| US-07 | Sofía Navarro | sofia.navarro@starklabs.io | Stark Labs | Administradora de cuenta | Starter | pendiente | 15/11/2026 | Sin accesos (`null`) | 0 · Ninguno |

"Agentes de la empresa" se **calcula** filtrando `AGENTES` por `propietario === empresa`. El plan es por empresa (todos los usuarios de una empresa comparten plan).

### 10.4 Contrataciones (`CONTRATOS`)

Regla: **importe total pagado = (suma del precio/mes de las skills del agente) × meses de vigencia**. Las skills de cada contrato son las del agente. El pago es por adelantado; en un contrato cancelado, "Fin" es la fecha efectiva de baja y solo se pagan los meses consumidos.

| ID | Cliente | Agente | Skills contratadas | Inicio | Fin | Meses | Estado | Cálculo | Importe |
|---|---|---|---|---|---|---|---|---|---|
| CT-01 | Acme Corp | Atlas | SK-01, SK-02 | 01/01/2026 | 31/12/2026 | 12 | activo | (25 + 30) × 12 = 55 × 12 | **660,00 €** |
| CT-02 | Initech | Orion | SK-01, SK-02, SK-05 | 01/03/2026 | 31/12/2026 | 10 | activo | (25 + 30 + 35) × 10 = 90 × 10 | **900,00 €** |
| CT-03 | Acme Corp | Sirius | SK-03, SK-04 | 01/06/2026 | 30/11/2026 | 6 | activo | (20 + 15) × 6 = 35 × 6 | **210,00 €** |
| CT-04 | Globex | Nova | SK-01, SK-03, SK-04 | 01/05/2026 | 31/10/2026 | 6 | finalizado | (25 + 20 + 15) × 6 = 60 × 6 | **360,00 €** |
| CT-05 | Umbrella | Lyra | SK-04 | 01/02/2026 | 30/04/2026 | 3 | cancelado | 15 × 3 | **45,00 €** |
| CT-06 | Globex | Vega | SK-02, SK-05 | 01/07/2026 | 31/12/2026 | 6 | activo | (30 + 35) × 6 = 65 × 6 | **390,00 €** |

Total histórico contratado: 660 + 900 + 210 + 360 + 45 + 390 = **2.565,00 €** (no se muestra; solo verificación).

### 10.5 Log de errores (`ERRORES`)

Solo agentes Nova, Vega, Orion, Atlas y Lyra. Cada error ocurre dentro de la vigencia del contrato de su agente y sobre una skill que ese agente tiene. Orden cronológico ascendente por ID; la tabla se muestra en orden descendente (LOG-008 arriba). Abiertos iniciales: **6**; resueltos: **2** (LOG-001, LOG-004).

| ID | Timestamp | Agente | Skill | Gravedad | Descripción breve | Estado inicial |
|---|---|---|---|---|---|---|
| LOG-001 | 28/04/2026 10:14:03 | Lyra | SK-04 | Info | Reintento de envío de email completado al 2.º intento | resuelto |
| LOG-002 | 29/10/2026 03:12:47 | Nova | SK-01 | Error | Timeout de 30 s al cargar una página externa | abierto |
| LOG-003 | 30/10/2026 16:45:09 | Nova | SK-03 | Crítico | Token OAuth de calendario revocado: autenticación fallida | abierto |
| LOG-004 | 12/11/2026 09:20:31 | Atlas | SK-01 | Aviso | Rate limit alcanzado en navegación web (HTTP 429) | resuelto |
| LOG-005 | 15/11/2026 22:05:58 | Orion | SK-05 | Error | Consulta SQL cancelada por superar 60 s | abierto |
| LOG-006 | 17/11/2026 07:58:12 | Vega | SK-05 | Crítico | Credenciales de base de datos rechazadas | abierto |
| LOG-007 | 18/11/2026 08:31:44 | Vega | SK-02 | Error | Timeout procesando un PDF de 214 páginas | abierto |
| LOG-008 | 18/11/2026 09:02:10 | Orion | SK-01 | Info | Navegación completada tras reintento automático | abierto |

**Trazas completas (texto literal para el `<pre>` del modal):**

LOG-001
```
[2026-04-28T10:14:03Z] INFO agent=Lyra (AG-04) skill=SK-04 envio_emails task=tsk_4a1e7
  attempt=1 smtp.send -> 421 4.7.0 Try again later (mx.umbrella.com)
  retry.backoff=2000ms
  attempt=2 smtp.send -> 250 OK message_id=<a8f3c1@mail.agenthub.io>
  recipients=3 elapsed=2.41s result=RECOVERED
```

LOG-002
```
[2026-10-29T03:12:47Z] ERROR agent=Nova (AG-05) skill=SK-01 navegacion_web task=tsk_9f21c
TimeoutError: Navigation timeout of 30000 ms exceeded
    at Page.goto (browser/page.js:212:15)
    at WebSkill.fetch (skills/navegacion_web/index.js:88:22)
    at AgentRuntime.runStep (runtime/agent.js:341:9)
    at async Task.execute (runtime/task.js:57:5)
url=https://proveedores.globex.com/salas?ciudad=madrid retries=3/3 result=FAILED
```

LOG-003
```
[2026-10-30T16:45:09Z] CRITICAL agent=Nova (AG-05) skill=SK-03 gestion_calendario task=tsk_b07d2
AuthError: invalid_grant - Token has been expired or revoked
    at OAuthClient.refresh (integrations/oauth/client.js:131:11)
    at CalendarSkill.createEvent (skills/gestion_calendario/index.js:64:18)
    at AgentRuntime.runStep (runtime/agent.js:341:9)
    at async Task.execute (runtime/task.js:57:5)
provider=google_calendar account=eventos@globex.com action=AGENT_SET_FAILING
```

LOG-004
```
[2026-11-12T09:20:31Z] WARN agent=Atlas (AG-01) skill=SK-01 navegacion_web task=tsk_c3390
RateLimitWarning: HTTP 429 Too Many Requests
    at WebSkill.fetch (skills/navegacion_web/index.js:88:22)
    at RateLimiter.check (runtime/ratelimit.js:42:7)
limit=60 req/min observed=61 retry_after=45s
queued_requests=12 action=THROTTLE resolved_by=soporte@agenthub.io
```

LOG-005
```
[2026-11-15T22:05:58Z] ERROR agent=Orion (AG-02) skill=SK-05 consulta_sql task=tsk_d5e81
QueryTimeoutError: canceling statement due to statement timeout (60000 ms)
    at PgDriver.query (skills/consulta_sql/drivers/pg.js:77:13)
    at SqlSkill.run (skills/consulta_sql/index.js:52:20)
    at AgentRuntime.runStep (runtime/agent.js:341:9)
    at async Task.execute (runtime/task.js:57:5)
db=initech_pedidos_ro query="SELECT ... FROM pedidos JOIN lineas_pedido ..." rows_scanned=18432210
```

LOG-006
```
[2026-11-17T07:58:12Z] CRITICAL agent=Vega (AG-06) skill=SK-05 consulta_sql task=tsk_e6f04
AuthError: password authentication failed for user "vega_ro"
    at PgDriver.connect (skills/consulta_sql/drivers/pg.js:31:9)
    at SqlSkill.run (skills/consulta_sql/index.js:47:15)
    at AgentRuntime.runStep (runtime/agent.js:341:9)
    at async Task.execute (runtime/task.js:57:5)
db=globex_contabilidad host=db.globex.internal:5432 action=AGENT_SET_FAILING
```

LOG-007
```
[2026-11-18T08:31:44Z] ERROR agent=Vega (AG-06) skill=SK-02 lectura_documentos task=tsk_f1a29
TimeoutError: Document processing exceeded 120000 ms
    at OcrEngine.process (skills/lectura_documentos/ocr.js:118:17)
    at DocSkill.read (skills/lectura_documentos/index.js:73:21)
    at AgentRuntime.runStep (runtime/agent.js:341:9)
    at async Task.execute (runtime/task.js:57:5)
file=facturas_octubre_2026.pdf pages=214 processed=162 result=FAILED
```

LOG-008
```
[2026-11-18T09:02:10Z] INFO agent=Orion (AG-02) skill=SK-01 navegacion_web task=tsk_0b7c6
  attempt=1 GET https://catalogo.proveedor-initech.com -> ECONNRESET
  retry.backoff=1500ms
  attempt=2 GET https://catalogo.proveedor-initech.com -> 200 OK (1.8s)
  result=RECOVERED pages_extracted=4
```

### 10.6 Métricas del Dashboard

| Métrica | Valor | Cuenta |
|---|---|---|
| Ingresos totales del mes (noviembre 2026) | **245,00 €** | Cuota mensual de los contratos activos en noviembre: CT-01 55 + CT-02 90 + CT-03 35 + CT-06 65 = 245 €. |
| Pérdida por descuentos y cupones | **31,50 €** (hardcodeado) | Descuento por volumen de Acme Corp 15 % sobre sus cuotas (55 + 35 = 90 €) = 13,50 € + cupón `STARTUP20` de Initech 20 % sobre 90 € = 18,00 € → 31,50 €. 31,50 / 245 = 12,9 %. |
| Agentes activos | **3** | Atlas, Orion, Sirius. |
| Agentes fallando | **2** | Nova, Vega. |

Supuesto: los descuentos se registran como gasto comercial (abono aparte) y **no** alteran el importe de los contratos de §10.4, que se muestran a precio de lista.

---

## 11. Inventario de componentes reutilizables

Paleta de badges (base `inline-flex items-center gap-1 rounded-full px-2.5 py-0.5 text-xs font-medium whitespace-nowrap`):

| Variante | Clases | Usos |
|---|---|---|
| emerald (éxito) | `bg-emerald-100 text-emerald-800 dark:bg-emerald-900/40 dark:text-emerald-300` | Usuario activo, agente activo, contrato activo, error resuelto |
| slate (neutro) | `bg-slate-100 text-slate-700 dark:bg-slate-800 dark:text-slate-300` | Plan Starter, agente inactivo, contrato finalizado, error abierto |
| red (crítico) | `bg-red-100 text-red-800 dark:bg-red-900/40 dark:text-red-300` | Usuario suspendido, agente fallando, contrato cancelado, gravedad Crítico |
| orange | `bg-orange-100 text-orange-800 dark:bg-orange-900/40 dark:text-orange-300` | Gravedad Error |
| amber (aviso) | `bg-amber-100 text-amber-800 dark:bg-amber-900/40 dark:text-amber-300` | Usuario pendiente, gravedad Aviso |
| sky | `bg-sky-100 text-sky-800 dark:bg-sky-900/40 dark:text-sky-300` | Gravedad Info |
| indigo (acento) | `bg-indigo-100 text-indigo-800 dark:bg-indigo-900/40 dark:text-indigo-300` | Plan Pro, nº de agentes por skill |
| violet | `bg-violet-100 text-violet-800 dark:bg-violet-900/40 dark:text-violet-300` | Plan Enterprise |

### 11.1 Sidebar
- **Dónde**: todas las vistas (persistente, nunca se oculta).
- **Estructura**: `<aside>` > cabecera de marca + `<nav aria-label="Navegación principal">` > `<ul>` de 6 `<li><a>` (icono + `<span>` texto) + pie de admin (S3.2).
- **Estados**: enlace normal, hover, activo (`aria-current="page"`, fondo indigo y barra lateral, S3.3); ancho `w-64` (≥1024 px) o `w-16` rail de iconos (<1024 px). Contador de errores abiertos en "Log de errores".
- **Comportamiento**: clic → cambia `location.hash` → muestra la vista (S3.4). El enlace activo se recalcula en cada `hashchange`.

### 11.2 Tarjeta de métrica
- **Dónde**: Dashboard (×4).
- **Estructura**: `<article class="rounded-xl border border-slate-200 bg-white p-5 shadow-sm dark:border-slate-800 dark:bg-slate-900 flex items-start justify-between gap-4">` > bloque texto (`<p class="text-sm font-medium text-slate-500 dark:text-slate-400">` etiqueta, `<p class="mt-2 text-3xl font-bold tabular-nums">` valor, `<p class="mt-1 text-xs text-slate-500 dark:text-slate-400">` secundario) + cuadrado de icono con acento (S4.3).
- **Estados**: estático; la variante enlazada (`<a>`, tarjeta 4) tiene hover con anillo rojo y foco visible `focus-visible:ring-2 focus-visible:ring-indigo-500`.
- **Comportamiento**: valores renderizados por JS al cargar (S4.2).

### 11.3 Dropdown de acciones (⋮)
- **Dónde**: última columna de cada fila en Usuarios, Agentes, Skills, Contrataciones y Log de errores.
- **Estructura**: `<div class="relative inline-block text-left" data-dropdown>` > `<button type="button" data-dropdown-toggle aria-haspopup="menu" aria-expanded="false" aria-label="Acciones para <nombre>" class="rounded-lg p-1.5 text-slate-500 hover:bg-slate-100 hover:text-slate-700 dark:hover:bg-slate-800 dark:hover:text-slate-200">` con icono ellipsis-vertical + `<div role="menu" class="hidden absolute right-0 z-40 mt-1 w-48 rounded-lg border border-slate-200 bg-white py-1 shadow-lg dark:border-slate-700 dark:bg-slate-900">` con `<button type="button" role="menuitem" class="flex w-full items-center gap-2 px-3 py-2 text-left text-sm text-slate-700 hover:bg-slate-100 dark:text-slate-200 dark:hover:bg-slate-800">` (icono `w-4 h-4` + texto). Opción destructiva en rojo.
- **Posición**: abre hacia abajo (`mt-1`); en las **dos últimas filas** de cada tabla abre hacia arriba (`bottom-full mb-1` en lugar de `mt-1`) para no salirse de la pantalla.
- **Estados**: cerrado (`hidden`, `aria-expanded="false"`), abierto (sin `hidden`, `aria-expanded="true"`), opción deshabilitada (`disabled opacity-50 cursor-not-allowed`).
- **Comportamiento** (delegación de eventos en `document`): clic en ⋮ abre su menú y **cierra cualquier otro** abierto (solo uno a la vez); segundo clic en el mismo ⋮ lo cierra; clic en una opción ejecuta la acción y **cierra el menú**; clic en **cualquier punto fuera** del menú lo cierra; `Escape` lo cierra y devuelve el foco al botón ⋮; cambiar de sección lo cierra.

### 11.4 Modal
- **Dónde**: Ver detalle (Usuarios, Skills, Contrataciones, Log de errores) y Configurar (Agentes). Un **único** modal reutilizable cuyo contenido se rellena por JS.
- **Estructura**: `<div id="modal" class="hidden fixed inset-0 z-50 flex items-center justify-center bg-slate-900/60 p-4 backdrop-blur-sm" data-backdrop>` > panel `<div role="dialog" aria-modal="true" aria-labelledby="modal-title" class="w-full max-w-lg max-h-[90vh] overflow-y-auto rounded-xl bg-white shadow-xl dark:bg-slate-900 dark:ring-1 dark:ring-slate-800">` (el ancho `max-w-lg`/`max-w-2xl` se cambia por clase según el contenido) con: cabecera (`flex items-start justify-between border-b border-slate-200 p-5 dark:border-slate-800`: `<h3 id="modal-title" class="text-lg font-semibold">` + subtítulo `text-sm text-slate-500` + botón ✕ `data-close-modal aria-label="Cerrar"`), cuerpo `id="modal-body"` (`p-5`), pie `id="modal-footer"` (`flex justify-end gap-3 border-t p-4`). Botón secundario: `rounded-lg border border-slate-300 px-4 py-2 text-sm font-medium hover:bg-slate-50 dark:border-slate-700 dark:hover:bg-slate-800`; primario: `rounded-lg bg-indigo-600 px-4 py-2 text-sm font-medium text-white hover:bg-indigo-700`.
- **Estados**: cerrado (`hidden`), abierto (sin `hidden`; `<body>` recibe `overflow-hidden`).
- **Comportamiento**: al abrir, foco en el botón ✕; se cierra con (a) botón ✕, (b) botón "Cerrar"/"Cancelar" del pie, (c) clic en el backdrop (solo si `event.target` es el propio backdrop: los clics dentro del panel **no** cierran), (d) tecla `Escape`. Al cerrar se quita `overflow-hidden` del body y el foco vuelve al botón ⋮ que lo originó.

### 11.5 Badge
- **Dónde**: estado de usuario y plan (Usuarios), estado de agente (Agentes, modal de Skills), nº de agentes (Skills), estado de contrato (Contrataciones), gravedad y estado (Log de errores), y dentro de los modales.
- **Estructura**: `<span>` con la clase base y una variante de la paleta de arriba; opcionalmente punto de color o icono `w-3.5 h-3.5` a la izquierda.
- **Estados**: definidos por el valor del dato; el badge de estado de error cambia en vivo de "Abierto" (slate) a "Resuelto" (emerald) (S9.5).
- **Comportamiento**: ninguno propio (no es interactivo). Una función JS `badge(variante, texto)` genera el HTML para evitar duplicar clases.

### 11.6 Lista de skills colapsable
- **Dónde**: columna Skills de la tabla de Agentes (una por agente).
- **Estructura**: `<div>` > `<button type="button" data-collapse-toggle aria-expanded="false" aria-controls="skills-AG-0n" class="inline-flex items-center gap-1 text-sm font-medium text-indigo-600 hover:text-indigo-800 dark:text-indigo-400 dark:hover:text-indigo-300">Skills (n) <svg chevron class="w-4 h-4 transition-transform duration-300"></svg></button>` + `<div id="skills-AG-0n" class="grid grid-rows-[0fr] opacity-0 transition-all duration-300 ease-in-out">` > `<div class="overflow-hidden">` > `<ul>` de chips.
- **Estados**: colapsado (por defecto: `grid-rows-[0fr] opacity-0`, chevron sin rotar, `aria-expanded="false"`); expandido (`grid-rows-[1fr] opacity-100`, chevron `rotate-180`, `aria-expanded="true"`).
- **Comportamiento**: clic alterna entre estados con transición de 300 ms (altura y opacidad); re-clic colapsa. Independiente por agente. No se usa `hidden` ni `element.style` (rompería la transición / R3).

### 11.7 Toggle de modo oscuro
- **Dónde**: extremo derecho del topbar, visible en todas las vistas.
- **Estructura**: `<button id="theme-toggle" type="button" aria-pressed aria-label>` con SVG luna (`dark:hidden`) y sol (`hidden dark:block`) (S3.6).
- **Estados**: claro (por defecto, muestra luna) / oscuro (muestra sol, `aria-pressed="true"`).
- **Comportamiento**: alterna la clase `dark` en `<html>`, guarda `theme` en `localStorage` y actualiza `aria-label`/`aria-pressed`. El script de `<head>` aplica el valor guardado antes del primer pintado.

---

## 12. Criterios de aceptación

Cada criterio es verificable a mano en el navegador (1440 px y 768 px) o con `grep` sobre `index.html`.

**Estructura y restricciones**
- **CA-01** — Existe un único `index.html` en la raíz que incluye `<script src="https://cdn.tailwindcss.com">` y `tailwind.config = { darkMode: 'class' }`.
- **CA-02** — `grep -nE 'style=|<style|\.css|rel="stylesheet"|\.style\.' index.html` devuelve 0 coincidencias.
- **CA-03** — No se carga ninguna librería JS (ningún `<script src>` distinto del CDN de Tailwind); no hay `fetch`/`XMLHttpRequest`.
- **CA-04** — El HTML contiene `<aside>`, `<nav>`, `<header>`, `<main>`, exactamente 6 `<section>` (ids `view-*` de S3.1) y cada listado es un `<table>` con `<thead>` y `<tbody>`; las acciones son `<button type="button">`.
- **CA-05** — El commit que añade `SPECS.md` es anterior a cualquier commit que añada un `.html` (`git log --reverse --name-status`).

**Layout y navegación**
- **CA-06** — El sidebar muestra los 6 enlaces en el orden de S3.2 y permanece visible en las 6 secciones.
- **CA-07** — Al hacer clic en cada enlace se muestra solo su sección, el título del topbar cambia al de la sección y `location.hash` cambia al valor de S3.4.
- **CA-08** — Solo el enlace de la sección visible tiene `aria-current="page"` y el estilo activo (fondo indigo + barra lateral).
- **CA-09** — Cargar `index.html` sin hash o con `#inexistente` muestra el Dashboard; cargar `index.html#errores` abre directamente Log de errores.
- **CA-10** — A 768 px de ancho el sidebar es un rail de iconos de 64 px, todas las tablas se ven completas sin scroll horizontal de la página y los dropdowns no quedan recortados.
- **CA-11** — A 1440 px el sidebar mide 256 px con textos visibles y las tarjetas del Dashboard forman una cuadrícula 2×2.

**Dashboard**
- **CA-12** — Se ven 4 tarjetas con icono, etiqueta y valor: "Ingresos totales del mes" 245,00 €, "Pérdida por descuentos y cupones" 31,50 €, "Agentes activos" 3, "Agentes fallando" 2, cada una con su color de acento (emerald, amber, indigo, red).
- **CA-13** — Bajo las tarjetas hay un área de ancho completo con borde discontinuo y el texto centrado "Gráfico de actividad semanal".
- **CA-14** — Clic en la tarjeta "Agentes fallando" navega a Log de errores.

**Dropdown ⋮**
- **CA-15** — En Usuarios, Agentes, Skills, Contrataciones y Log de errores, **cada fila** tiene un botón ⋮; al pulsarlo se abre su menú con las opciones de S5.3, S6.4, S7.4, S8.3 y S9.3 respectivamente, y `aria-expanded` pasa a `true`.
- **CA-16** — Al pulsar cualquier opción del menú, el menú se cierra.
- **CA-17** — Con un menú abierto, un clic en cualquier zona fuera de él (otra parte de la tabla, sidebar, topbar, fondo) lo cierra.
- **CA-18** — Abrir el ⋮ de otra fila cierra el anterior (nunca hay dos menús abiertos); pulsar `Escape` cierra el menú abierto.
- **CA-19** — Los menús de las dos últimas filas se abren hacia arriba y ningún menú queda cortado por el contenedor de la tabla.

**Modal**
- **CA-20** — "Ver detalle" abre un modal en Usuarios, Skills, Contrataciones y Log de errores (4 secciones), y "Configurar" abre un modal en Agentes.
- **CA-21** — Cada modal se cierra con el botón "Cerrar"/"Cancelar" y con el botón ✕.
- **CA-22** — Cada modal se cierra al hacer clic en el backdrop oscuro; un clic dentro del panel (texto, `<textarea>`, tabla) **no** lo cierra.
- **CA-23** — Cada modal se cierra al pulsar `Escape`; tras cerrar, la página vuelve a hacer scroll (sin `overflow-hidden` en `<body>`).
- **CA-24** — El modal de usuario muestra los 10 campos de S5.4 con los datos de §10.3 (p. ej. US-05 muestra "Sin accesos" y "2 · Nova, Vega").

**Agentes y colapsable**
- **CA-25** — Al cargar Agentes, las listas de skills de los 6 agentes están colapsadas (chips no visibles, `aria-expanded="false"`).
- **CA-26** — Clic en "Skills (n)" expande la lista con transición visible de ~300 ms (altura y opacidad) y rota el chevron; otro clic la colapsa con la misma transición.
- **CA-27** — Expandir un agente no expande los demás.
- **CA-28** — "Configurar" abre un modal con un `<textarea>` editable que contiene el prompt de sistema del agente (§10.2); tras editar y pulsar "Guardar cambios", reabrir "Configurar" del mismo agente muestra el texto editado; "Cancelar" descarta los cambios.
- **CA-29** — Los badges de estado muestran Atlas, Orion y Sirius como "Activo" (verde), Lyra "Inactivo" (gris) y Nova y Vega "Fallando" (rojo con punto pulsante).

**Skills**
- **CA-30** — La sección Skills muestra el bloque "¿Qué es una skill?" con el texto de S7.1 encima del catálogo.
- **CA-31** — El catálogo lista 5 skills con nombre, descripción breve y nº de agentes: SK-01 3, SK-02 3, SK-03 2, SK-04 3, SK-05 2; el modal de detalle lista los nombres de esos agentes.

**Contrataciones**
- **CA-32** — La tabla muestra 6 contratos con cliente, agente, skills contratadas, inicio, fin e importe: 660,00 €, 900,00 €, 210,00 €, 360,00 €, 45,00 €, 390,00 €.
- **CA-33** — El modal de detalle de un contrato muestra una fila por skill con su precio individual, meses y subtotal, el total mensual, el total del contrato (igual al importe de la tabla) y la línea de cálculo (p. ej. CT-01: `(25,00 € + 30,00 €) × 12 meses = 660,00 €`).
- **CA-34** — "Descargar factura" cierra el menú y no produce ninguna otra acción ni error en consola.

**Log de errores**
- **CA-35** — La tabla muestra 8 entradas ordenadas de más reciente (LOG-008) a más antigua (LOG-001), con timestamp, agente, badge de gravedad coloreado (Crítico rojo, Error naranja, Aviso ámbar, Info azul cielo), descripción y estado.
- **CA-36** — "Ver detalle" muestra la traza completa de §10.5 en un bloque monoespaciado.
- **CA-37** — "Marcar como resuelto" en un error abierto cambia su badge de estado a "Resuelto" (verde) sin recargar, deshabilita la opción ("Ya resuelto"), y reduce en 1 el recuento de abiertos y el contador del sidebar (6 → 5).
- **CA-38** — LOG-001 y LOG-004 aparecen ya como "Resuelto" al cargar.

**Eliminar**
- **CA-39** — "Eliminar" en Usuarios, Agentes o Skills pide confirmación nativa; al aceptar desaparece la fila y el recuento baja en 1; al cancelar no cambia nada.

**Modo oscuro**
- **CA-40** — El toggle del topbar alterna claro/oscuro y cambia **todo** el panel: fondo, sidebar, topbar, tarjetas, tablas, badges, menús y modales (ningún elemento queda con fondo blanco en oscuro).
- **CA-41** — Tras activar el modo oscuro, navegar por las 6 secciones lo mantiene; recargar la página también lo mantiene (`localStorage.theme === 'dark'`) sin parpadeo en claro.
- **CA-42** — Volver a pulsar el toggle restaura el modo claro y guarda `localStorage.theme === 'light'`.

**Coherencia de datos**
- **CA-43** — Los nombres de agentes (Atlas, Orion, Sirius, Lyra, Nova, Vega), empresas (Acme Corp, Initech, Globex, Umbrella, Stark Labs) y skills (SK-01…SK-05) son idénticos en Usuarios, Agentes, Skills, Contrataciones y Log de errores; no aparece ningún nombre fuera de §10.
- **CA-44** — Las skills de cada contrato coinciden con las del agente en Agentes, y los errores solo referencian Nova, Vega, Orion, Atlas y Lyra con skills que ese agente tiene.
- **CA-45** — Mínimos de datos cumplidos: 7 usuarios (≥5), 6 agentes (≥4), 5 skills (≥4), 6 contratos (≥4), 8 errores (≥6).

---

## 13. Trazabilidad

### 13.1 Requisito del enunciado → sección de la spec

| Requisito | Spec | CA |
|---|---|---|
| Descripción del producto y del administrador | §1 | — |
| Stack: HTML + Tailwind CDN + JS vanilla, sin frameworks/backend | §2 (R1–R5) | CA-01, CA-03 |
| Sin CSS propio ni `style` | §2 (R3) | CA-02 |
| Sidebar persistente con 6 secciones e indicador activo | S3.2, S3.3, S3.4, §11.1 | CA-06–CA-09 |
| Topbar con título y toggle dark persistente | S3.5, S3.6, §11.7 | CA-40–CA-42 |
| Escritorio y tablet | S3.7, R10 | CA-10, CA-11 |
| Dashboard: 4 tarjetas (ingresos, descuentos, activos, fallando) | S4.1–S4.4, §10.6, §11.2 | CA-12, CA-14 |
| Dashboard: área de gráfico semanal | S4.5 | CA-13 |
| Usuarios: tabla nombre/email/plan/estado badge | S5.1, S5.2 | CA-24, CA-45 |
| Usuarios: dropdown Ver detalle / Eliminar + modal | S5.3–S5.5 | CA-15–CA-24, CA-39 |
| Agentes: listado con propietario, badge y skills colapsadas | S6.1–S6.3, §11.6 | CA-25–CA-27, CA-29 |
| Agentes: Configurar → modal con `<textarea>` prompt / Eliminar | S6.4–S6.6 | CA-28, CA-39 |
| Skills: catálogo nombre/descripción/nº agentes | S7.2, S7.3 | CA-31 |
| Skills: explicación de "skill" | S7.1 | CA-30 |
| Skills: dropdown Ver detalle / Eliminar | S7.4–S7.6 | CA-15, CA-20, CA-39 |
| Contrataciones: tabla cliente/agente/skills/fechas/importe | S8.1, S8.2, §10.4 | CA-32 |
| Contrataciones: Ver detalle con desglose + otra acción | S8.3–S8.5 | CA-33, CA-34 |
| Errores: tabla timestamp/agente/badge gravedad/descripción | S9.1, S9.2 | CA-35 |
| Errores: Ver detalle con traza / Marcar como resuelto | S9.3–S9.5 | CA-36–CA-38 |
| Todos los dropdowns cierran al clic fuera | §11.3 | CA-17 |
| Todos los modales cierran en backdrop | §11.4 | CA-22 |
| Datos hardcodeados y coherentes | §10 | CA-43–CA-45 |
| HTML semántico | R8, S3.1 | CA-04 |
| Inventario de componentes | §11 | — |
| SPECS.md commiteado antes del HTML | Cabecera, CLAUDE.md | CA-05 |

### 13.2 Criterios de evaluación (docs/EVALUACION.md) → CA

| Criterio | Descripción | CA |
|---|---|---|
| E1 | SPECS.md antes de cualquier HTML | CA-05 |
| E2 | ≥3 specs/sección, inventario, criterios numerados | §4–§9, §11, §12 (ver §14) |
| E3 | 6 secciones accesibles desde el sidebar | CA-06, CA-07, CA-08, CA-09 |
| E4 | Tailwind consistente, sin inline ni CSS externo | CA-01, CA-02, CA-03 |
| E5 | Dropdown ⋮ en todas las filas: abre, cierra al clic y al clic fuera | CA-15, CA-16, CA-17, CA-18, CA-19 |
| E6 | "Ver detalle" abre modal en ≥4 secciones | CA-20, CA-24, CA-28, CA-33, CA-36 |
| E7 | Modales cierran con botón y con backdrop | CA-21, CA-22, CA-23 |
| E8 | Skills colapsadas por defecto, transición | CA-25, CA-26, CA-27 |
| E9 | Toggle dark cambia todo y persiste | CA-40, CA-41, CA-42 |
| E10 | Datos coherentes | CA-29, CA-31, CA-32, CA-43, CA-44, CA-45 |
| E11 | HTML semántico | CA-04 |
| E12 | Escritorio y tablet | CA-10, CA-11 |

---

## 14. Autocomprobación de la spec

| Comprobación | Resultado |
|---|---|
| §3 Layout global ≥3 specs | 8 (S3.1–S3.8) ✔ |
| §4 Dashboard ≥3 specs | 5 (S4.1–S4.5) ✔ |
| §5 Usuarios ≥3 specs | 5 (S5.1–S5.5) ✔ |
| §6 Agentes ≥3 specs | 6 (S6.1–S6.6) ✔ |
| §7 Skills ≥3 specs | 6 (S7.1–S7.6) ✔ |
| §8 Contrataciones ≥3 specs | 5 (S8.1–S8.5) ✔ |
| §9 Log de errores ≥3 specs | 5 (S9.1–S9.5) ✔ |
| Inventario con 7 componentes | §11.1–§11.7 ✔ |
| Criterios numerados | CA-01…CA-45 ✔ (dropdown CA-15–19, modal CA-20–24, colapsable CA-25–27, dark CA-40–42) |
| Mínimos de datos | 7 usuarios · 6 agentes · 5 skills · 6 contratos · 8 errores ✔ |
| Agentes activos / fallando | 3 (Atlas, Orion, Sirius) / 2 (Nova, Vega) = tarjetas 3 y 4 ✔ |
| Agentes por skill | SK-01 3 · SK-02 3 · SK-03 2 · SK-04 3 · SK-05 2 (total 13 = 2+3+2+1+3+2) ✔ |
| Agentes por empresa = campo de usuarios | Acme 2 · Initech 1 · Globex 2 · Umbrella 1 · Stark Labs 0 ✔ |
| Importes de contratos | 660 + 900 + 210 + 360 + 45 + 390 = 2.565 € ✔ |
| Ingresos del mes | 55 + 90 + 35 + 65 = 245 € (contratos activos CT-01, 02, 03, 06) ✔ |
| Skills de contrato = skills del agente | 6/6 ✔ |
| Errores: solo Nova, Vega, Orion, Atlas, Lyra; skill del agente; dentro de vigencia | 8/8 ✔ |
| Errores abiertos iniciales = contador sidebar | 6 (8 − LOG-001 − LOG-004) ✔ |
