# Stitch — Referencia global (shell y sistema de diseño)

Fuente: proyecto «AgentHub SaaS Admin Dashboard» en Stitch (prompts 0–6, 2026-11-18). Es **guía visual**, no código a copiar. Ante cualquier conflicto manda `SPECS.md`.
Este fichero resume el HTML exportado (13 pantallas) sin reproducirlo: el original usa `<style>`, `style=""`, hojas externas y tokens Material que NO se pueden usar (ver «Problemas generales»).

## 1. Shell (se repite en las 6 pantallas)
- **Sidebar** fijo a la izquierda. Mejor variante (Dashboard/Usuarios claro): `w-16 lg:w-64` → rail de iconos en tablet, **sin menú hamburguesa**. El resto de pantallas usa `w-64` fijo → unificar en `w-16 lg:w-64`.
- **Marca**: cuadrado índigo con «AH», «AgentHub», subtítulo «Panel de administración». (La pantalla Log de errores usó icono `hub` + «Consola Admin»: descartar.)
- **Nav** (orden fijo): Dashboard · Usuarios · Agentes · Skills · Contrataciones · Log de errores. Log de errores lleva píldora roja «6» (en rail de tablet: punto rojo sobre el icono).
- **Activo**: fondo índigo tenue + texto índigo + barra izquierda de 4 px (`border-l-4`).
- **Pie**: avatar «AD», «Admin interno», «soporte@agenthub.io».
- **Topbar** `sticky top-0 h-16` con `backdrop-blur`: título + subtítulo a la izquierda; a la derecha buscar, **toggle tema** (icono luna/sol), notificaciones con punto rojo, avatar.
- Subtítulos de topbar: Dashboard «Resumen de noviembre 2026» · Usuarios «Usuarios de las empresas cliente» · Agentes «Agentes desplegados y sus skills» · Skills «Catálogo de capacidades contratables» · Contrataciones «Contratos de alquiler de agentes» · Log «Incidencias técnicas de los agentes».

## 2. Tipografía
Inter (UI) y JetBrains Mono (IDs, timestamps, trazas). Escala usada: título de página 24/600 · título de tarjeta 16/600 · cuerpo 14 · secundario 12 · etiqueta de cabecera de tabla 11/600 en mayúsculas con `tracking-wider` · cifra KPI 30/700.
Decisión pendiente en `adaptacion.md`: Google Fonts exige `<link rel="stylesheet">` externo (choca con E4) → propuesta: `font-sans` del sistema y `font-mono` de Tailwind.

## 3. Equivalencias Stitch → Tailwind estándar (usar la paleta por defecto de Tailwind, sin `tailwind.config` de colores)
| Rol | Claro | Oscuro (`dark:`) |
|---|---|---|
| Fondo de página | `bg-slate-50` | `dark:bg-slate-950` |
| Tarjeta / tabla / panel | `bg-white border border-slate-200 shadow-sm rounded-xl` | `dark:bg-slate-900 dark:border-slate-800` |
| Texto principal | `text-slate-900` | `dark:text-slate-100` |
| Texto secundario | `text-slate-500` | `dark:text-slate-400` |
| Cabecera de tabla | `bg-slate-50 text-slate-500 text-[11px] font-semibold uppercase tracking-wider` | `dark:bg-slate-950/60 dark:text-slate-400` |
| Filas | `divide-y divide-slate-100 hover:bg-slate-50` | `dark:divide-slate-800/80 dark:hover:bg-slate-800/40` |
| Input | `bg-white border-slate-200 focus:ring-indigo-500` | `dark:bg-slate-950 dark:border-slate-800` |
| Botón primario | `bg-indigo-600 hover:bg-indigo-700 text-white rounded-lg` | `dark:hover:bg-indigo-500` |
| Botón secundario | `bg-white border border-slate-200 text-slate-700 hover:bg-slate-50` | `dark:bg-slate-900 dark:border-slate-800 dark:text-slate-300 dark:hover:bg-slate-800` |
| Dropdown ⋮ | `bg-white border border-slate-200 rounded-xl shadow-lg py-1 w-44` | `dark:bg-slate-800 dark:border-slate-700` |
| Modal | backdrop `bg-slate-900/40 backdrop-blur-sm`; panel `bg-white rounded-xl shadow-2xl max-w-lg` | backdrop `dark:bg-black/60`; panel `dark:bg-slate-900 dark:border dark:border-slate-800` |
| Acción destructiva | `text-red-600 hover:bg-red-50` | `dark:text-red-400 dark:hover:bg-red-950/40` |

## 4. Badges (punto de 6 px + texto; base `inline-flex items-center gap-1.5 rounded-full px-2.5 py-0.5 text-xs font-medium border`)
| Estado | Claro | Oscuro | Punto |
|---|---|---|---|
| Activo / Resuelto | `bg-emerald-50 text-emerald-700 border-emerald-200` | `dark:bg-emerald-950/60 dark:text-emerald-400 dark:border-emerald-800/60` | `bg-emerald-500` |
| Pendiente / Aviso | `bg-amber-50 text-amber-700 border-amber-200` | `dark:bg-amber-950/60 dark:text-amber-400 dark:border-amber-900/60` | `bg-amber-500` |
| Suspendido / Fallando / Crítico | `bg-red-50 text-red-700 border-red-200` | `dark:bg-red-950/60 dark:text-red-400 dark:border-red-800/60` | `bg-red-500` (Fallando con `animate-ping`) |
| Inactivo / Abierto | `bg-slate-100 text-slate-700 border-slate-200` | `dark:bg-slate-800 dark:text-slate-400 dark:border-slate-700` | `bg-slate-400` |
| Error | `bg-orange-50 text-orange-700 border-orange-200` | `dark:bg-orange-950/60 dark:text-orange-400 dark:border-orange-900/60` | `bg-orange-500` |
| Info | `bg-sky-50 text-sky-700 border-sky-200` | `dark:bg-sky-950/60 dark:text-sky-400 dark:border-sky-900/60` | `bg-sky-500` |
| Plan Enterprise (inferido) | `bg-purple-50 text-purple-700 border-purple-200` | `dark:bg-purple-950/60 dark:text-purple-300 dark:border-purple-800/60` | — |
| Plan Pro | `bg-indigo-50 text-indigo-700 border-indigo-200` | `dark:bg-indigo-950/60 dark:text-indigo-300 dark:border-indigo-800/60` | — |
| Plan Starter | `bg-slate-100 text-slate-700 border-slate-200` | `dark:bg-slate-800 dark:text-slate-300 dark:border-slate-700` | — |

## 5. Dropdown ⋮, modal y colapsable (lo que Stitch muestra vs lo que hay que construir)
- Stitch dibuja el dropdown **ya abierto** en una fila y el modal **ya visible**: en el prototipo ambos arrancan cerrados (`hidden`), con apertura/cierre en JS vanilla (ver skill `ui-patterns`).
- El colapsable de skills aparece expandido en una sola fila (Orion) y **sin transición**: en el prototipo todas arrancan colapsadas y usan `grid-rows-[0fr]` → `grid-rows-[1fr]` con `transition-all`.
- Las dos variantes (claro/oscuro) son **pantallas distintas** en Stitch, con colores fijos (`bg-slate-900`, `bg-[#020617]`…). En el prototipo es UNA pantalla con pares `claro dark:oscuro`.

## 6. Problemas generales de las exportaciones (no copiar)
1. `<style>` y `style=""` inline (p. ej. `font-variation-settings`, `width: 42.4%`, `onclick="…style.display='none'"`). Prohibido.
2. Hojas externas: Google Fonts (Inter, JetBrains Mono) y Material Symbols. Choca con «sin archivos CSS».
3. `tailwind.config` con ~50 colores tipo Material (`surface-container-lowest`, `on-surface`…): no es Tailwind estándar y complica `dark:`. Usar la paleta por defecto.
4. Ninguna pantalla usa variantes `dark:`; el modo oscuro son pantallas aparte. «Usuarios (Modo Oscuro)» ni siquiera es oscura (reutiliza los mismos tokens claros): **no usar como referencia de dark**.
5. Contenido fuera de spec: KPI strips extra, filtros, paginación, «Exportar», «Desplegar nuevo agente», «Carga de ejecución por skill», diagnósticos. Solo se mantiene lo que pida `SPECS.md`.
6. Las dos pantallas del **Prompt 0** (`00-global.png` claro y `00-global-dark.png` oscuro) son una muestra de componentes (badges, dropdown, modal) con datos inventados (1.284 / 142 agentes, «Nexus-Audit-09», «Echo-NLP-Support»…): usar **solo como referencia de forma** (sidebar, tabla, dropdown, modal), nunca sus datos. El Dashboard válido es el de 245,00 € (ver `01-dashboard.md`).
7. Stitch también entrega `DESIGN-stitch.md` (sistema de diseño «AgentHub Modern Operations» con tokens Material): referencia de intención visual, no se copian los tokens.
8. Tablas con `overflow-x-auto` que recortarían el menú ⋮: no usar (decisión de SPECS).
