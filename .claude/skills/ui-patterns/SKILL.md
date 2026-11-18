---
name: ui-patterns
description: Patrones JS vanilla reutilizables para el panel AgentHub: dropdown de acciones (⋮) con cierre al clic fuera, modal con backdrop/Esc, colapsable con transición, toggle dark persistente y navegación de secciones sin element.style.
---

# Patrones UI en JS vanilla (delegación de eventos, sin `element.style`)

## Dropdown ⋮
```html
<div class="relative" data-dropdown>
  <button data-dropdown-toggle aria-haspopup="menu" aria-expanded="false" class="rounded p-1.5 hover:bg-slate-100 dark:hover:bg-slate-800">⋮</button>
  <div role="menu" class="hidden absolute right-0 z-20 mt-1 w-40 rounded-lg border bg-white py-1 shadow-lg dark:bg-slate-900">
    <button role="menuitem" data-open-modal="user-1" class="block w-full px-3 py-2 text-left text-sm">Ver detalle</button>
  </div>
</div>
```
```js
const closeMenus = (except) => document.querySelectorAll('[data-dropdown]').forEach(d => {
  if (d === except) return;
  d.querySelector('[role=menu]').classList.add('hidden');
  d.querySelector('[data-dropdown-toggle]').setAttribute('aria-expanded', 'false');
});
document.addEventListener('click', e => {
  const t = e.target.closest('[data-dropdown-toggle]');
  const dd = t?.closest('[data-dropdown]');
  closeMenus(dd);                       // clic fuera o en otro dropdown → cierra
  if (!t) return;
  const menu = dd.querySelector('[role=menu]');
  const open = menu.classList.toggle('hidden') === false;
  t.setAttribute('aria-expanded', open);
});
```
Al elegir una opción del menú, cerrar con `closeMenus()`.

## Modal (plantilla única + backdrop)
```html
<div id="modal" class="hidden fixed inset-0 z-50 flex items-center justify-center bg-black/50 p-4" data-backdrop role="dialog" aria-modal="true">
  <div class="w-full max-w-lg rounded-xl bg-white p-6 shadow-xl dark:bg-slate-900">
    <button data-close-modal aria-label="Cerrar">✕</button>
    <div id="modal-body"></div>
  </div>
</div>
```
```js
const modal = document.getElementById('modal');
const openModal = html => { document.getElementById('modal-body').innerHTML = html; modal.classList.remove('hidden'); };
const closeModal = () => modal.classList.add('hidden');
modal.addEventListener('click', e => { if (e.target === modal || e.target.closest('[data-close-modal]')) closeModal(); });
document.addEventListener('keydown', e => e.key === 'Escape' && closeModal());
```
El backdrop es el propio contenedor: clic dentro del panel no cierra (`e.target === modal`).

## Colapsable con transición (skills del agente)
```html
<button data-collapse-toggle aria-expanded="false">Skills (3) ▾</button>
<div class="grid grid-rows-[0fr] transition-all duration-300"><ul class="overflow-hidden">…</ul></div>
```
```js
document.addEventListener('click', e => {
  const b = e.target.closest('[data-collapse-toggle]'); if (!b) return;
  const panel = b.nextElementSibling;
  const open = panel.classList.toggle('grid-rows-[1fr]');
  panel.classList.toggle('grid-rows-[0fr]', !open);
  b.setAttribute('aria-expanded', open);
});
```

## Dark mode persistente
```js
const root = document.documentElement;
if (localStorage.getItem('theme') === 'dark') root.classList.add('dark');
document.getElementById('theme-toggle').addEventListener('click', () => {
  localStorage.setItem('theme', root.classList.toggle('dark') ? 'dark' : 'light');
});
```
Poner el script de lectura en `<head>` para evitar parpadeo.

## Navegación de secciones
`<section id="view-users" data-view class="hidden">`; `location.hash` → mostrar una vista, ocultar el resto, marcar enlace activo con `aria-current="page"` + clases de activo.
