---
name: ui-patterns
description: Patrones JS vanilla reutilizables para el panel AgentHub, alineados con SPECS.md: dropdown de acciones (⋮) con cierre al clic fuera, modal con backdrop/Esc, colapsable con transición grid-rows, toggle dark persistente, navegación por hash y sprite SVG inline de iconos. Sin element.style.
---

# Patrones UI en JS vanilla (delegación de eventos, sin `element.style`)

**SPECS.md manda** (§3, §11, S6.3, S9.5). Si algo de aquí lo contradice, vale SPECS. Referencia de HTML completo: `docs/stitch/adaptacion.md` → «Recetas Tailwind». Prohibido `style="…"`, `<style>` y asignar `element.style.*`: la visibilidad se alterna solo con clases.

## Dropdown ⋮ (SPECS §11.3)
```html
<div class="relative inline-block text-left" data-dropdown>
  <button type="button" data-dropdown-toggle aria-haspopup="menu" aria-expanded="false" aria-label="Acciones para Laura Méndez"
          class="rounded-lg p-1.5 text-slate-500 hover:bg-slate-100 hover:text-slate-700 dark:text-slate-400 dark:hover:bg-slate-800 dark:hover:text-slate-200">…</button>
  <div role="menu" class="hidden absolute right-0 z-40 mt-1 w-48 rounded-lg border border-slate-200 bg-white py-1 shadow-lg dark:border-slate-700 dark:bg-slate-900">
    <button type="button" role="menuitem" data-action="ver-usuario" data-id="US-01"
            class="flex w-full items-center gap-2 px-3 py-2 text-left text-sm text-slate-700 hover:bg-slate-100 dark:text-slate-200 dark:hover:bg-slate-800">Ver detalle</button>
    <!-- destructiva: text-red-600 hover:bg-red-50 dark:text-red-400 dark:hover:bg-red-950/40 · deshabilitada: disabled opacity-50 cursor-not-allowed -->
  </div>
</div>
```
- Ancho `w-48`, abre hacia abajo (`mt-1`). En las **dos últimas filas** de cada tabla abre hacia arriba: `bottom-full mb-1` en lugar de `mt-1` (recalcular al re-renderizar tras eliminar).
- Tablas **sin** `overflow-x-auto` ni `overflow-hidden` en el contenedor: recortarían el menú.
```js
const closeMenus = (except) => document.querySelectorAll('[data-dropdown]').forEach(d => {
  if (d === except) return;
  d.querySelector('[role=menu]').classList.add('hidden');
  d.querySelector('[data-dropdown-toggle]').setAttribute('aria-expanded', 'false');
});
document.addEventListener('click', e => {
  const t = e.target.closest('[data-dropdown-toggle]');
  const dd = t?.closest('[data-dropdown]');
  closeMenus(dd);                                  // clic fuera / en otro ⋮ → cierra los demás
  if (t) {
    const open = dd.querySelector('[role=menu]').classList.toggle('hidden') === false;
    t.setAttribute('aria-expanded', String(open)); // segundo clic en el mismo ⋮ lo cierra
  } else if (e.target.closest('[role=menuitem]')) closeMenus(); // elegir opción cierra (la acción se despacha aparte)
});
document.addEventListener('keydown', e => {
  if (e.key !== 'Escape') return;
  const abierto = document.querySelector('[data-dropdown] [role=menu]:not(.hidden)');
  if (!abierto) return;
  const dd = abierto.closest('[data-dropdown]');
  closeMenus(); dd.querySelector('[data-dropdown-toggle]').focus();  // Esc devuelve el foco al ⋮
});
```
Solo un menú abierto a la vez. Cambiar de sección también cierra (`closeMenus()` en `hashchange`).

## Modal único (SPECS §11.4)
```html
<div id="modal" data-backdrop class="fixed inset-0 z-50 hidden items-center justify-center bg-slate-900/60 p-4 backdrop-blur-sm">
  <div role="dialog" aria-modal="true" aria-labelledby="modal-title"
       class="max-h-[90vh] w-full max-w-lg overflow-y-auto rounded-xl bg-white shadow-xl dark:bg-slate-900 dark:ring-1 dark:ring-slate-800">
    <div class="flex items-start justify-between border-b border-slate-200 p-5 dark:border-slate-800">
      <div><h3 id="modal-title" class="text-lg font-semibold"></h3><p id="modal-subtitle" class="text-sm text-slate-500 dark:text-slate-400"></p></div>
      <button type="button" data-close-modal aria-label="Cerrar">✕ (icono x-mark)</button>
    </div>
    <div id="modal-body" class="p-5"></div>
    <div id="modal-footer" class="flex justify-end gap-3 border-t border-slate-200 p-4 dark:border-slate-800"></div>
  </div>
</div>
```
- Backdrop `bg-slate-900/60 backdrop-blur-sm`; panel `max-w-lg` (Usuarios, Skills) o `max-w-2xl` (Agentes, Contrataciones, Errores), cambiando la clase por JS.
- Abrir: quitar `hidden`, añadir `flex`, `document.body.classList.add('overflow-hidden')`, foco en el ✕ y recordar el ⋮ de origen. Cerrar: inverso, quitar `overflow-hidden` y devolver el foco al ⋮.
- Cierre con ✕, botón «Cerrar»/«Cancelar» (`data-close-modal`), clic en el backdrop (solo si `e.target` es el propio backdrop; clics dentro del panel no cierran) y `Escape`.
```js
const modal = document.getElementById('modal');
let origen = null;
const openModal = (btn, { titulo, subtitulo, ancho = 'max-w-lg', body, footer }) => {
  origen = btn;
  /* rellenar #modal-title/#modal-subtitle/#modal-body/#modal-footer; panel: reemplazar max-w-* por `ancho` */
  modal.classList.remove('hidden'); modal.classList.add('flex');
  document.body.classList.add('overflow-hidden');
  modal.querySelector('[data-close-modal]').focus();
};
const closeModal = () => {
  modal.classList.add('hidden'); modal.classList.remove('flex');
  document.body.classList.remove('overflow-hidden');
  origen?.focus();
};
modal.addEventListener('click', e => { if (e.target === modal || e.target.closest('[data-close-modal]')) closeModal(); });
document.addEventListener('keydown', e => { if (e.key === 'Escape' && !modal.classList.contains('hidden')) closeModal(); });
```
Como cada fila se renderiza desde JS, guarda `origen` antes de cerrar el menú (que ya devolvió/perdió el foco) para devolver el foco al ⋮.

## Colapsable con transición (SPECS S6.3, §11.6)
```html
<div>
  <button type="button" data-collapse-toggle aria-expanded="false" aria-controls="skills-AG-02"
          class="inline-flex items-center gap-1 text-sm font-medium text-indigo-600 hover:text-indigo-800 dark:text-indigo-400 dark:hover:text-indigo-300">
    Skills (3) <svg class="h-4 w-4 transition-transform duration-300">…chevron-down…</svg>
  </button>
  <div id="skills-AG-02" class="grid grid-rows-[0fr] opacity-0 transition-all duration-300 ease-in-out">
    <div class="overflow-hidden"><ul class="mt-2 flex flex-wrap gap-1.5">…chips…</ul></div>
  </div>
</div>
```
```js
document.addEventListener('click', e => {
  const b = e.target.closest('[data-collapse-toggle]'); if (!b) return;
  const panel = document.getElementById(b.getAttribute('aria-controls'));
  const abrir = b.getAttribute('aria-expanded') !== 'true';
  panel.classList.toggle('grid-rows-[1fr]', abrir);  panel.classList.toggle('opacity-100', abrir);
  panel.classList.toggle('grid-rows-[0fr]', !abrir); panel.classList.toggle('opacity-0', !abrir);
  b.querySelector('svg').classList.toggle('rotate-180', abrir);
  b.setAttribute('aria-expanded', String(abrir));
});
```
Todo arranca colapsado y cada agente es independiente. No uses `hidden` ni `element.style` (romperían la transición / R3).

## Toggle de tema persistente (SPECS S3.6)
En `<head>`, antes de pintar (evita parpadeo); sin valor guardado el tema es **claro**:
```html
<script>try { if (localStorage.getItem('theme') === 'dark') document.documentElement.classList.add('dark'); } catch (_) {}</script>
```
Al final del `<body>`:
```js
const root = document.documentElement, toggle = document.getElementById('theme-toggle');
const syncToggle = () => {
  const oscuro = root.classList.contains('dark');
  toggle.setAttribute('aria-pressed', String(oscuro));
  toggle.setAttribute('aria-label', oscuro ? 'Activar modo claro' : 'Activar modo oscuro');
};
toggle.addEventListener('click', () => {
  const oscuro = root.classList.toggle('dark');
  try { localStorage.setItem('theme', oscuro ? 'dark' : 'light'); } catch (_) {}
  syncToggle();
});
syncToggle();
```
Luna con `dark:hidden`, sol con `hidden dark:block`. La clase `dark` va en `<html>` (nunca en `body`).

## Navegación por hash (SPECS S3.4)
`<section id="view-dashboard|usuarios|agentes|skills|contrataciones|errores" class="hidden">` (todas con `hidden` salvo la activa). Hashes: `#dashboard #usuarios #agentes #skills #contrataciones #errores`.
```js
const VISTAS = ['dashboard','usuarios','agentes','skills','contrataciones','errores'];
const mostrar = () => {
  let v = location.hash.slice(1);
  if (!VISTAS.includes(v)) { v = 'dashboard'; history.replaceState(null, '', '#dashboard'); }
  VISTAS.forEach(n => document.getElementById('view-' + n).classList.toggle('hidden', n !== v));
  /* enlace activo: aria-current="page" + bg-indigo-50 text-indigo-700 dark:bg-indigo-500/10 dark:text-indigo-300 + span de barra; el resto, sin ellos */
  /* #page-title, #page-subtitle y document.title = 'AgentHub · <Título>' */
  closeMenus(); if (!modal.classList.contains('hidden')) closeModal(); window.scrollTo(0, 0);
};
addEventListener('hashchange', mostrar); mostrar();
```
Un único enlace activo. El sidebar no se re-renderiza.

## Iconos: sprite SVG inline
Sin fuentes de iconos ni imágenes externas (R6). Un sprite de `<symbol>` al principio de `<body>` y un `<use>` en cada sitio. Sin `style` ni ficheros externos.
```html
<body …>
<svg class="hidden" aria-hidden="true">
  <symbol id="i-users" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="…trazo Heroicons outline…"/></symbol>
  <!-- un <symbol id="i-…"> por icono -->
</svg>
```
Uso (los atributos de trazo van en el `<svg>` de uso; el `<symbol>` hereda `currentColor`):
```html
<svg class="h-5 w-5" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true"><use href="#i-users"/></svg>
```
Tamaños: `h-5 w-5` (nav, botones), `h-6 w-6` (tarjetas), `h-4 w-4` (dropdowns, chevron), `h-3.5 w-3.5` (badges), `h-10 w-10` (marcador del gráfico). En JS, un helper que devuelva ese `<svg>`: `const icon = (n, c = 'h-5 w-5') => \`<svg class="${c}" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true"><use href="#i-${n}"/></svg>\``.
Si algún navegador no pintara `<use>` con el sprite en `display:none`, cambia `hidden` por `absolute h-0 w-0 overflow-hidden` (misma función, sin `style`).

**Iconos que exige SPECS** (28): `squares-2x2`, `users`, `cpu-chip`, `puzzle-piece`, `document-text`, `exclamation-triangle` (nav, S3.2) · `banknotes`, `receipt-percent`, `chart-bar` (Dashboard, S4.1/S4.5) · `information-circle` (S7.1) · `globe-alt`, `document-magnifying-glass`, `calendar-days`, `envelope`, `circle-stack` (skills, S7.2) · `ellipsis-vertical`, `eye`, `trash`, `cog-6-tooth`, `arrow-down-tray`, `check-circle` (dropdowns, §11.3) · `x-circle`, `check` (badges, S9.2) · `chevron-down` (colapsable) · `x-mark` (✕ del modal) · `moon`, `sun` (toggle).

## Badge
Una función `badge(variante, texto, punto?)` genera el HTML con la paleta de SPECS §11 (ver `tailwind-vanilla`); el estado cambia en vivo (p. ej. Abierto → Resuelto).
