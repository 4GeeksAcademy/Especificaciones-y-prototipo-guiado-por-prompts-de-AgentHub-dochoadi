---
name: tailwind-vanilla
description: Reglas de estilo para AgentHub: solo utilidades Tailwind vía CDN, sin style inline ni CSS propio, modo oscuro con dark:, paleta y patrones de badge/tarjeta/tabla. Usar al escribir o revisar index.html.
---

# Tailwind vanilla

## Setup (único permitido)
```html
<script src="https://cdn.tailwindcss.com"></script>
<script>tailwind.config = { darkMode: 'class' }</script>
```
Prohibido: `<style>`, `style="..."`, `<link rel=stylesheet>`, `@apply`, Google Fonts por CSS propio.

## Dark mode
Toda clase de color lleva su par: `bg-white dark:bg-slate-900`, `text-slate-900 dark:text-slate-100`, `border-slate-200 dark:border-slate-700`. Fondo app: `bg-slate-50 dark:bg-slate-950`.

## Paleta semántica
| Uso | Clases |
|---|---|
| Activo / éxito / resuelto | `bg-emerald-100 text-emerald-800 dark:bg-emerald-900/40 dark:text-emerald-300` |
| Inactivo / neutro | `bg-slate-100 text-slate-700 dark:bg-slate-800 dark:text-slate-300` |
| Fallando / crítico | `bg-red-100 text-red-800 dark:bg-red-900/40 dark:text-red-300` |
| Aviso / warning | `bg-amber-100 text-amber-800 dark:bg-amber-900/40 dark:text-amber-300` |
| Info / acento | `bg-indigo-100 text-indigo-800 dark:bg-indigo-900/40 dark:text-indigo-300` |

Badge base: `inline-flex items-center rounded-full px-2.5 py-0.5 text-xs font-medium`.
Tarjeta: `rounded-xl border bg-white p-5 shadow-sm dark:bg-slate-900`.
Tabla: contenedor `overflow-x-auto`, `thead` con `text-xs uppercase text-slate-500`, filas `hover:bg-slate-50 dark:hover:bg-slate-800/50`. Si la fila contiene dropdowns ⋮, no uses overflow-x-auto (recortaría el menú): dimensiona las columnas para 768 px (ver SPECS.md).

## Responsive
Sidebar: `w-64 hidden lg:flex` + versión compacta iconos `md:w-16` para tablet. Grids: `grid-cols-1 sm:grid-cols-2 xl:grid-cols-4`.

## Visibilidad por JS
Alternar con `hidden` / `invisible opacity-0` / `max-h-0`, nunca con `element.style`.
