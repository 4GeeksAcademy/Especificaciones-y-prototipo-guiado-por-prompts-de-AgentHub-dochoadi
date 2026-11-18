---
name: tailwind-vanilla
description: Reglas de estilo para AgentHub alineadas con SPECS.md: solo utilidades Tailwind vía CDN, sin style inline ni CSS propio, modo oscuro con dark:, paleta de badges (§11), tarjetas y tablas sin overflow-x-auto. Usar al escribir o revisar index.html.
---

# Tailwind vanilla

**SPECS.md manda** (R2, R3, R9, §3.8, §11). Recetas HTML completas: `docs/stitch/adaptacion.md` → «Recetas Tailwind».

## Setup (único permitido)
```html
<script src="https://cdn.tailwindcss.com"></script>
<script>tailwind.config = { darkMode: 'class' }</script>
```
Sin `theme.extend` ni colores personalizados: paleta estándar de Tailwind.
**Prohibido**: `<style>`, `style="…"`, `<link rel="stylesheet">`, `@apply`, ficheros `.css`, Google Fonts, Material Symbols y `element.style.*` desde JS. Fuente: la del sistema (`font-sans` por defecto, `font-mono` para timestamps, trazas y prompt). Iconos: sprite SVG inline (ver `ui-patterns`). Sin plugins (`animate-in`, `zoom-in-95` no existen).

## Dark mode
Clase `dark` en `<html>`. **Toda** clase de color lleva su par `dark:`. Base: `<body class="bg-slate-50 text-slate-900 dark:bg-slate-950 dark:text-slate-100 antialiased">`; tarjeta/tabla `bg-white dark:bg-slate-900`; bordes `border-slate-200 dark:border-slate-800`; texto secundario `text-slate-500 dark:text-slate-400`; hover de fila `hover:bg-slate-50 dark:hover:bg-slate-800/50`.

## Paleta de badges (SPECS §11)
Base: `inline-flex items-center gap-1 rounded-full px-2.5 py-0.5 text-xs font-medium whitespace-nowrap` (sin borde).
| Variante | Clases | Usos |
|---|---|---|
| emerald | `bg-emerald-100 text-emerald-800 dark:bg-emerald-900/40 dark:text-emerald-300` | activo, resuelto |
| slate | `bg-slate-100 text-slate-700 dark:bg-slate-800 dark:text-slate-300` | Starter, inactivo, finalizado, abierto |
| red | `bg-red-100 text-red-800 dark:bg-red-900/40 dark:text-red-300` | suspendido, fallando (punto `bg-red-500 animate-pulse`), cancelado, Crítico |
| orange | `bg-orange-100 text-orange-800 dark:bg-orange-900/40 dark:text-orange-300` | gravedad Error |
| amber | `bg-amber-100 text-amber-800 dark:bg-amber-900/40 dark:text-amber-300` | pendiente, gravedad Aviso |
| sky | `bg-sky-100 text-sky-800 dark:bg-sky-900/40 dark:text-sky-300` | gravedad Info |
| indigo | `bg-indigo-100 text-indigo-800 dark:bg-indigo-900/40 dark:text-indigo-300` | plan Pro, nº de agentes por skill |
| violet | `bg-violet-100 text-violet-800 dark:bg-violet-900/40 dark:text-violet-300` | plan Enterprise |

Puntos de estado: `h-1.5 w-1.5 rounded-full` en `bg-emerald-500` / `bg-slate-400` / `bg-red-500 animate-pulse`.

## Componentes
- **Tarjeta de métrica**: `rounded-xl border border-slate-200 bg-white p-5 shadow-sm dark:border-slate-800 dark:bg-slate-900 flex items-start justify-between gap-4`; valor `mt-2 text-3xl font-bold tabular-nums`; icono en `grid h-11 w-11 place-items-center rounded-lg` con acento (SPECS S4.3).
- **Botón primario** `rounded-lg bg-indigo-600 px-4 py-2 text-sm font-medium text-white hover:bg-indigo-700`; **secundario** `rounded-lg border border-slate-300 px-4 py-2 text-sm font-medium hover:bg-slate-50 dark:border-slate-700 dark:hover:bg-slate-800`.
- **Modal**: backdrop `bg-slate-900/60 backdrop-blur-sm`, panel `max-w-lg` / `max-w-2xl` (SPECS §11.4).
- **Dropdown ⋮**: menú `w-48 rounded-lg border border-slate-200 bg-white py-1 shadow-lg dark:border-slate-700 dark:bg-slate-900` (SPECS §11.3).

## Tablas (SPECS S3.8, S3.7)
Contenedor `rounded-xl border border-slate-200 bg-white shadow-sm dark:border-slate-800 dark:bg-slate-900` **sin `overflow-hidden` ni `overflow-x-auto`** (recortarían el menú ⋮). `<table class="w-full text-left text-sm">`; `thead` sin fondo con `border-b border-slate-200 dark:border-slate-800`; `th` `px-3 py-3 lg:px-4 text-xs font-semibold uppercase tracking-wide text-slate-500 dark:text-slate-400`; filas `border-b border-slate-100 last:border-0 hover:bg-slate-50 dark:border-slate-800 dark:hover:bg-slate-800/50`. Se dimensionan para 688 px útiles a 768 px; columnas secundarias con `hidden lg:table-cell` (Precio/mes en Skills). Recuento encima: `<p class="mb-3 text-sm text-slate-500 dark:text-slate-400">`.

## Responsive (SPECS S3.1, S3.7, R10)
Objetivo: escritorio ≥1024 px y tablet 768–1023 px; móvil no es objetivo. Sidebar fijo siempre visible `w-16 lg:w-64` (rail de iconos en tablet, **sin menú hamburguesa**); contenido `pl-16 lg:pl-64`. Tarjetas del Dashboard `grid grid-cols-1 gap-6 sm:grid-cols-2`. Sin scroll horizontal de la página a 768 px.

## Visibilidad e interacción por JS
Alternar **solo con clases**: `hidden`, `opacity-0`/`opacity-100`, `grid-rows-[0fr]`/`grid-rows-[1fr]`, `rotate-180`, `overflow-hidden` en `body` con el modal abierto. Transiciones con utilidades (`transition-all duration-300 ease-in-out`). Nunca `element.style`.
Verificación: `grep -nE 'style=|<style|\.css|rel="stylesheet"|\.style\.' index.html` → 0 coincidencias (CA-02).
