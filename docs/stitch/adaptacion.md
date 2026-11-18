# Adaptación de Stitch a la spec (F2 · P2.2)

Fuentes: `SPECS.md` (commit `b3b4d64`, **manda**), `docs/stitch/00-global.md`, `01…06-*.md`, `DESIGN-stitch.md`, `prompt.md`. Los `.png` de Stitch **no están en el repo**: este análisis se basa solo en los md.
Regla: ante cualquier conflicto prevalece `SPECS.md`. Stitch se usa como guía de forma (jerarquía, espaciado, densidad), nunca como código ni como fuente de datos.

Ficheros de `docs/stitch/` (índice; esta carpeta no lleva README propio, el único es el de la raíz):
- `prompt.md` — prompts 0–6 derivados de `SPECS.md` (P2.1)
- `00-global.md` … `06-errores.md` — resumen del HTML exportado por Stitch, global y por pantalla
- `DESIGN-stitch.md` — sistema de diseño «AgentHub Modern Operations» (solo intención visual)
- `adaptacion.md` — este documento (P2.2)
- Capturas `*.png`: **no existen** en el repo (opcionales); si se añaden, nombrarlas `00-global.png` … `06-errores.png`, las oscuras con sufijo `-dark`.

Leyenda: ✅ coincide · ❌ difiere (se indica qué prevalece).

---

## Discrepancias

### 1. Datos

| Área | Elemento | Stitch | SPECS.md | Estado | Prevalece |
|---|---|---|---|---|---|
| Agentes | Skills de Atlas | «Skills (4)» + «Análisis financiero» | 2: SK-01, SK-02 (§10.2) | ❌ | SPECS |
| Agentes | Skills de Orion, Sirius, Lyra, Nova, Vega | 3 · 2 · 1 · 3 · 2 | 3 · 2 · 1 · 3 · 2 | ✅ | — |
| Agentes | Nombres, IDs AG-01…06, propietarios, estados | Atlas/Acme/Activo … Vega/Globex/Fallando | idénticos (§10.2) | ✅ | — |
| Agentes | Total de skills habilitadas | «15 skills en 6 agentes» | 13 (2+3+2+1+3+2) | ❌ | SPECS (y se descarta la tira) |
| Agentes | Prompt de sistema de Atlas | «…recopilar, analizar y sintetizar datos competitivos…» | texto literal distinto (§10.2) | ❌ | SPECS |
| Agentes | Prompts de los otros 5 agentes | no existen | definidos (§10.2) | ❌ falta en Stitch | SPECS |
| Agentes | Diagnóstico «OpenAI GPT-4o … eu-central-1b» | presente | no existe y contradice el Log | ❌ | SPECS (descartar) |
| Agentes | «Análisis financiero & Conciliación» (skill) | presente | no existe en el catálogo | ❌ | SPECS (descartar) |
| Skills | Catálogo SK-01…05 y precios 25/30/20/15/35 € | iguales | iguales | ✅ | — |
| Skills | Nº de agentes por skill y nombres | 3·3·2·3·2 (Atlas, Orion, Nova / Atlas, Orion, Vega / Sirius, Nova / Sirius, Lyra, Nova / Orion, Vega) | iguales (§10.1, S7.3) | ✅ | — |
| Skills | Descripciones breves (5) | frases distintas («Permite al agente explorar páginas web públicas…») | texto de §10.1 | ❌ | SPECS |
| Skills | Permisos de SK-05 | «Credenciales de BD de solo lectura» + detalle TLS/RLS/timeout | «Credenciales de BD de solo lectura» | ✅ (el detalle extra se descarta) | SPECS |
| Skills | Texto del bloque «¿Qué es una skill?» | versión corta («…de forma segura y gobernada») | texto literal largo de S7.1 | ❌ | SPECS |
| Skills | KPI «Gasto combinado/mes 125,00 €» | suma de precios del catálogo (25+30+20+15+35 = 125 ✓) | no existe | ❌ extra | SPECS (descartar) |
| Usuarios | 7 usuarios, empresas, emails, planes, estados | iguales | iguales (§10.3) | ✅ | — |
| Usuarios | IDs US-01…07 | inferidos por orden | definidos | ✅ | — |
| Usuarios | Campos del modal | ID, Rol, Plan, Estado, Último acceso, Agentes asignados + aviso de invitación; solo Javier Lozano | 10 campos: ID, Nombre, Email, Empresa, Rol, Plan, Estado, Fecha de alta, Último acceso, Agentes de la empresa (S5.4) | ❌ | SPECS |
| Usuarios | Agentes de Javier | «2 · Nova, Vega» | «2 · Nova, Vega» | ✅ | — |
| Usuarios | Plan Enterprise | `purple` | `violet` (S5.2, §11) | ❌ | SPECS |
| Usuarios | Fila de Carlos Vidal | subtítulo «Carlos Vidal / Umbrella» (glitch) | solo «Umbrella» | ❌ | SPECS |
| Usuarios | KPI (7 · 4 · 2 · 1) | cuadran con la tabla | no existen | ❌ extra | SPECS (descartar) |
| Contratos | 6 contratos: cliente, agente, skills, fechas, meses, importes | 660 · 900 · 210 · 360 · 45 · 390 € | iguales (§10.4) | ✅ | — |
| Contratos | KPI «Facturación activa 2.160,00 €» y «Duración media 7,2 meses» | cuentan bien (660+900+210+390 · 43/6) | no existen; 2.160 € podría confundirse con los 245,00 € del Dashboard | ❌ extra | SPECS (descartar) |
| Contratos | Chips de skills | «SK-01 Navegación web» | solo el nombre de la skill (S8.1 → S6.3) | ❌ | SPECS |
| Contratos | Columna Agente | icono + nombre + badge | nombre + badge de estado del contrato debajo (S8.1) | ❌ | SPECS |
| Contratos | Desglose del modal | Skill / Precio/Mes / Meses / Subtotal + «Total del contrato» | + fila «Total mensual», línea de cálculo, `dl` con Estado/Inicio/Fin/Duración (S8.4) | ❌ incompleto | SPECS |
| Errores | 8 entradas: ID, timestamp, agente (cliente), gravedad, estado | iguales (LOG-001…008, 6 abiertas, 2 resueltas) | iguales (§10.5) | ✅ | — |
| Errores | Descripciones de LOG-001, 002, 003, 004, 005, 007, 008 | distintas (p. ej. LOG-003 «Excepción… análisis sintáctico de PDF», LOG-007 «…servicio de indexación…», LOG-008 «Healthcheck…142 ms») | las de §10.5 (p. ej. LOG-003 «Token OAuth de calendario revocado») | ❌ | SPECS |
| Errores | LOG-006 | «Credenciales de base de datos rechazadas» | igual | ✅ | — |
| Errores | Skill de LOG-003 (Nova) | PDF → SK-02, que **Nova no tiene** | SK-03 Gestión de calendario | ❌ incoherente en Stitch | SPECS |
| Errores | Traza de LOG-006 | stack `pg/lib/connection.js`, `analytics_db` | traza literal de §10.5 (`PgDriver.connect`, `globex_contabilidad`) | ❌ | SPECS |
| Errores | Trazas de las otras 7 | no existen | definidas (§10.5) | ❌ falta en Stitch | SPECS |
| Errores | Contadores por gravedad (2·3·1·2) | cuadran con las 8 entradas | no existen | ❌ extra | SPECS (descartar) |
| Dashboard | 4 tarjetas: 245,00 € · 31,50 € · 3 · 2 y textos de apoyo | iguales | iguales (S4.1) | ✅ | — |
| Dashboard | Gráfico semanal | borde discontinuo solo en oscuro | `border-2 border-dashed` en ambos temas (S4.5) | ❌ | SPECS |
| Dashboard | Texto del marcador | «Gráfico de actividad semanal» / «Ejecuciones de agentes · 12–18 nov 2026 (pendiente de integrar)» | igual | ✅ | — |
| Dashboard | Datos de la muestra del Prompt 0 | 1.284 / 142 agentes, «Nexus-Audit-09», «Echo-NLP-Support» | no existen | ❌ inventados | SPECS (ignorar) |
| Contadores | Sidebar «6» en Log de errores; «8 entradas · 6 abiertas» | 6 / 6 | 6 / 6 | ✅ | — |
| Contadores | Recuentos sobre las tablas | píldoras «7 usuarios», «6 agentes + 3 activos + 2 fallando», «Mostrando 1 a 7 de 7» | `<p>` de texto: «7 usuarios», «6 agentes», «5 skills», «6 contratos», «8 entradas · 6 abiertas» (S3.8) | ❌ | SPECS |

### 2. Textos de UI y shell

| Elemento | Stitch | SPECS.md | Estado | Prevalece |
|---|---|---|---|---|
| Marca | «AH · AgentHub · Panel de administración» (5 pantallas); Log de errores: icono `hub` + «Consola Admin» | «AH» / «AgentHub» / «Panel de administración» (S3.2) | ❌ en Log | SPECS |
| Orden y texto de la navegación | Dashboard · Usuarios · Agentes · Skills · Contrataciones · Log de errores | idéntico | ✅ | — |
| Ancho del sidebar | `w-16 lg:w-64` en Dashboard/Usuarios; `w-64` fijo en el resto | `w-16 lg:w-64` (S3.2, S3.7) | ❌ | SPECS |
| Menú hamburguesa en móvil/tablet | `DESIGN-stitch.md` lo prevé para <768 px | no hay hamburguesa; móvil no es objetivo (R10, S3.7) | ❌ | SPECS |
| Estado activo del nav | `border-l-4` | `<span>` absoluto `w-1` + `bg-indigo-50 text-indigo-700 dark:bg-indigo-500/10 dark:text-indigo-300` (S3.3) | ❌ | SPECS |
| Pie del sidebar | «AD · Admin interno · soporte@agenthub.io» | igual (solo `lg:flex`) | ✅ | — |
| Subtítulos del topbar (6) | iguales | iguales (S3.5) | ✅ | — |
| Topbar a la derecha | buscar, toggle, notificaciones con punto rojo, avatar | solo toggle (S3.5) | ❌ | SPECS |
| Título de página | `h1` «Vista General» / «Registro de auditoría / Incidencias» dentro del contenido | `h1#page-title` en el topbar; `h2` `sr-only` por sección (S3.1) | ❌ | SPECS |
| Tipografía | Inter + JetBrains Mono (hojas externas) | sin CSS externo (R3) | ❌ | SPECS |
| Iconos | Material Symbols (hoja externa + `font-variation-settings` inline) | SVG inline Heroicons (R6) | ❌ | SPECS |
| Cabecera de tabla | `bg-slate-50`, `text-[11px] tracking-wider` | sin fondo, `text-xs tracking-wide` (S3.8) | ❌ | SPECS |
| Filas de tabla | `divide-y` | `border-b border-slate-100 last:border-0` (S3.8) | ❌ | SPECS |
| Badges | `-50/-700` con borde; dark `-950/60`; Fallando con `animate-ping` | `-100/-800` sin borde; dark `-900/40` (§11); Fallando `animate-pulse` (S6.2) | ❌ | SPECS |
| Acentos de tarjeta | `bg-emerald-50 …/ dark:bg-emerald-950/40` | `bg-emerald-100 text-emerald-700 dark:bg-emerald-900/40 dark:text-emerald-300` (S4.3) | ❌ | SPECS |
| Rejilla de tarjetas | `md:grid-cols-2` | `sm:grid-cols-2` (S4.1) | ❌ (irrelevante a 768 px) | SPECS |
| Textos de botones | «Cerrar», «Cancelar», «Guardar cambios», «Reenviar invitación», «Descargar factura PDF», «Copiar traza» | «Cerrar», «Cancelar», «Guardar cambios», «Descargar factura»; sin Reenviar ni Copiar | ❌ en 3 | SPECS |
| Título del modal de agente | «Configurar agente — Atlas · Acme Corp» en una línea | título «Configurar agente» + subtítulo «Atlas · Acme Corp» (S6.5) | ❌ | SPECS |
| Etiqueta del precio en el modal de skill | «Tarifa mensual» | «Precio/mes» (S7.5) | ❌ | SPECS |
| Skills (tabla) | columna Precio/Mes siempre visible | `hidden lg:table-cell` (S7.2) | ❌ | SPECS |
| Aviso en modal de usuario | «Invitación enviada el 18 Nov 2026; caduca en 48 horas» | no existe | ❌ extra | SPECS |
| Modal de errores: campos | Gravedad, Timestamp, Estado, Agente | + Propietario, Skill implicada, Descripción (S9.4) | ❌ incompleto | SPECS |
| «Marcar como resuelto» en el modal | botón pasa a «Resuelto» y cierra a los 700 ms con `setTimeout` | aplica S9.5 y cierra el modal al instante | ❌ | SPECS |
| Fila de error crítico abierto | `bg-red-50` | no hay resalte; la fila resuelta lleva `opacity-60` (S9.5) | ❌ | SPECS |
| «Nueva contratación» | icono `+` y texto «+» duplicados | no existe el botón | ❌ | SPECS (descartar) |

### 3. Acciones de dropdown ⋮

| Sección | Stitch | SPECS.md | Estado | Prevalece |
|---|---|---|---|---|
| Usuarios | Ver detalle · Reenviar enlace · Eliminar | Ver detalle · Eliminar (S5.3) | ❌ (+1) | SPECS |
| Agentes | Configurar · Historial de logs · Eliminar | Configurar · Eliminar (S6.4) | ❌ (+1) | SPECS |
| Skills | Ver detalle · Eliminar | igual (S7.4) | ✅ | — |
| Contrataciones | Ver detalle · Descargar factura (sin lógica) | igual; sin Eliminar (S8.3) | ✅ | — |
| Errores | Ver detalle · Marcar como resuelto · Copiar identificador | Ver detalle · Marcar como resuelto (→ «Ya resuelto» deshabilitado) (S9.3) | ❌ (+1; falta estado «Ya resuelto») | SPECS |
| Estado inicial | menú dibujado **abierto** en una fila | cerrado al cargar (`hidden`) | ❌ | SPECS |
| Anchura / color | `w-44`, `dark:bg-slate-800` | `w-48`, `dark:bg-slate-900 dark:border-slate-700` (§11.3) | ❌ | SPECS |
| Apertura hacia arriba en las 2 últimas filas | no contemplada | obligatoria (§11.3, CA-19) | ❌ falta en Stitch | SPECS |

### 4. Campos de modales

| Modal | Stitch | SPECS.md | Estado | Prevalece |
|---|---|---|---|---|
| Usuario | `max-w-lg`, ✕, «Cerrar» + «Reenviar invitación» | `max-w-lg`, ✕, backdrop, Esc, pie solo «Cerrar» | ❌ | SPECS |
| Agente | textarea `rows=5 resize-none` + contador, Cancelar/Guardar | textarea `rows=10` editable, contador, Cancelar/Guardar, `max-w-2xl` | ❌ rows/ancho | SPECS |
| Skill | ID, tarifa, permisos, chips de agentes con estado | ID, Nombre, Precio/mes, Permisos, Descripción completa, Agentes con badge mini (S7.5) | ❌ incompleto | SPECS |
| Contrato | desglose + total | `dl` 4 cols + desglose + tfoot (Total mensual / Total) + línea de cálculo; pie «Descargar factura» + «Cerrar» | ❌ incompleto | SPECS |
| Error | traza con «Copiar traza» | `<pre>` con traza literal; sin copiar; «Marcar como resuelto» (oculto si ya resuelto) | ❌ | SPECS |
| Backdrop | `bg-slate-900/40` (claro) / `black/60` (oscuro) | `bg-slate-900/60 backdrop-blur-sm` en ambos (§11.4) | ❌ | SPECS |
| Cierre | Esc con script propio solo en una pantalla | ✕, botón, backdrop y Esc en todos (CA-21…23) | ❌ | SPECS |

### 5. Fallos ya anotados en los md de Stitch (consolidado)

| Fallo | Dónde se anotó | Resolución |
|---|---|---|
| Atlas «Skills (4)» | `03-agentes.md` | 2 skills |
| «15 skills en 6 agentes» | `03-agentes.md` | 13 (y tira descartada) |
| «Usuarios (Modo Oscuro)» no es oscura | `00-global.md` §6.4, `02-usuarios.md` | no usarla; dark = pares `dark:` de la receta |
| Dashboard claro sin borde discontinuo | `01-dashboard.md` | añadir `border-dashed` en claro |
| Prompt 0 con datos inventados (1.284 / 142 agentes…) | `00-global.md` §6.6 | solo referencia de forma |
| Marca «Consola Admin» | `00-global.md` §1 | «AgentHub / Panel de administración» |
| Sidebar `w-64` fijo | `00-global.md` §1 | `w-16 lg:w-64` |
| Glitch «Carlos Vidal / Umbrella» | `02-usuarios.md` | solo «Umbrella» |
| Signo `+` duplicado en «Nueva contratación» | `05-contrataciones.md` | botón descartado |
| `animate-in fade-in zoom-in-95` (plugin inexistente) | `04-skills.md` | `transition` + `opacity`/`scale` estándar o nada |
| Skills de Orion desplegadas sin transición | `03-agentes.md`, `00-global.md` §5 | todas colapsadas, `grid-rows` |
| Vega «Activo» en Contrataciones y «Fallando» en Agentes | `05-contrataciones.md` | son dos estados distintos (contrato vs agente); no es error |
| Traza solo de LOG-006 | `06-errores.md` | las 8 vienen de §10.5 |

### 6. Skill `ui-patterns` vs SPECS (para F3)

La skill propia difiere de SPECS en: backdrop `bg-black/50` (SPECS `bg-slate-900/60 backdrop-blur-sm`), dropdown `w-40` (SPECS `w-48`), colapsable sin `opacity-0/100` y con `overflow-hidden` en la `ul` (SPECS: `div` hijo con `overflow-hidden` + `ul`), modal sin `max-h-[90vh]`/`overflow-y-auto` ni foco/`overflow-hidden` en `body`. Las recetas de abajo siguen **SPECS**; el patrón JS (delegación de eventos, `closeMenus`, `e.target === modal`) de la skill sí es válido.

---

## Propuesta Stitch → decisión

| Elemento | Stitch | Decisión | Motivo |
|---|---|---|---|
| Tokens Material (`surface-container-lowest`, `on-surface`, `primary`… ~50 colores en `tailwind.config`) | Paleta Material inline en cada pantalla | **Cambiar** → paleta Tailwind estándar con pares claro + `dark:`; `tailwind.config = { darkMode: 'class' }` y nada más (R2) | R2/R9; los tokens Material no existen en Tailwind estándar y complican `dark:`. Equivalencias en `00-global.md` §3 (p. ej. `surface` → `bg-slate-50 dark:bg-slate-950`, tarjeta → `bg-white dark:bg-slate-900`, `on-surface` → `text-slate-900 dark:text-slate-100`, `primary` → `indigo-600`). Donde `00-global.md` y SPECS §11 difieren (badges `-50/-700` vs `-100/-800`), manda SPECS |
| `<style>` en el `<head>` | Presente en las exportaciones | **Descartar** | R3, CA-02 |
| `style=""` (`font-variation-settings`, `width: 42.4%`…) | Presente | **Descartar** → clases Tailwind (`w-…`, `h-…`); iconos SVG, sin variation settings | R3, CA-02 |
| `onclick="…classList…"` / `style.display='none'` inline | Presente | **Descartar** → delegación de eventos en `document` con `data-dropdown-toggle`, `data-open-modal`, `data-close-modal`, `data-collapse-toggle`; visibilidad solo con clases (`hidden`, `grid-rows-[0fr]`…) | R3/R4; `ui-patterns` |
| `<script>` propios de Stitch (`classList.add('dark')`, `setTimeout` 700 ms, `navigator.clipboard`) | Presente | **Descartar** | Un único `<script>` al final + el de `<head>` para el tema (R4) |
| Google Fonts (Inter, JetBrains Mono) | `<link rel="stylesheet">` externo | **Descartar.** Recomendado: fuente del sistema. `font-sans` (pila `ui-sans-serif, system-ui`) para UI y `font-mono` (`ui-monospace`) para trazas, timestamps y prompt; sin ninguna hoja externa | R3 prohíbe `<link rel="stylesheet">`; CA-02. Se pierde la métrica exacta de Inter, es aceptable en un prototipo |
| Material Symbols (hoja externa) | `<link rel="stylesheet">` externo | **Descartar.** Recomendado: **sprite SVG inline** de Heroicons outline: un `<svg class="absolute h-0 w-0 overflow-hidden" aria-hidden="true">` al inicio de `<body>` con un `<symbol id="i-…">` por icono (≈ 25: squares-2x2, users, cpu-chip, puzzle-piece, document-text, exclamation-triangle, banknotes, receipt-percent, chart-bar, information-circle, globe-alt, document-magnifying-glass, calendar-days, envelope, circle-stack, ellipsis-vertical, eye, trash, cog-6-tooth, arrow-down-tray, check-circle, check, x-circle, x-mark, chevron-down, moon, sun) y cada uso `<svg class="h-5 w-5" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true"><use href="#i-eye"/></svg>`; en JS, un helper `icon(nombre, clases)`. Alternativa: diccionario JS de paths y SVG completo en cada uso | Cumple R6 (SVG inline, sin red) y evita repetir cada path en 5 tablas × N filas. El sprite no usa `style` ni `.css`. Validar a ojo en F3 que `stroke="currentColor"` hereda bien a través de `<use>` (es el comportamiento estándar) |
| Modo oscuro como pantallas separadas (colores fijos `bg-[#020617]`, etc.) | 2 pantallas por sección | **Cambiar** → una sola pantalla con `dark:` en cada clase de color + `#theme-toggle` en el topbar con `localStorage.theme` y script en `<head>` | R9, S3.6, CA-40…42 |
| «Usuarios (Modo Oscuro)» | Reutiliza tokens claros | **Descartar** como referencia de dark | No es oscura (`00-global.md` §6.4) |
| Dropdowns dibujados abiertos | ⋮ abierto en una fila | **Cambiar** → cerrados al cargar (`hidden`, `aria-expanded="false"`); un único menú abierto; cierre por clic fuera/Esc/opción; hacia arriba en las 2 últimas filas | §11.3, CA-15…19 |
| Modales dibujados visibles | Visibles | **Cambiar** → un único `#modal` con `hidden`, contenido por JS | §11.4, CA-20…23 |
| Skills de Orion desplegadas, sin transición | Chips visibles, sin animar | **Cambiar** → las 6 colapsadas: `grid grid-rows-[0fr] opacity-0 transition-all duration-300 ease-in-out` ↔ `grid-rows-[1fr] opacity-100`, chevron `rotate-180`, `aria-expanded` | S6.3, §11.6, CA-25…27 |
| Sidebar fijo `w-64` en la mayoría | 4 de 6 pantallas | **Cambiar** → `w-16 lg:w-64` en todas (rail de iconos en tablet, sin hamburguesa, `title`/`aria-label` en cada enlace) | S3.2, S3.7, CA-10/11 |
| Estado activo `border-l-4` | Borde de 4 px | **Cambiar** → `<span>` absoluto `w-1` + fondo/texto indigo y `aria-current="page"` | S3.3 (la barra no desplaza el icono) |
| Marca «Consola Admin» + icono `hub` (Log de errores) | Marca distinta | **Cambiar** → «AgentHub / Panel de administración» con logotipo «AH» | S3.2, coherencia entre pantallas |
| Topbar con buscador, campana y avatar | Extras | **Descartar** | S3.5: solo título, subtítulo y toggle |
| KPI strips (Usuarios 4, Agentes 4, Skills 3, Contrataciones 4) | Tiras de métricas por sección | **Descartar** | SPECS no los pide; añaden cifras (15 skills, 2.160 €, 125 €) que confunden con el Dashboard y obligarían a mantener más cálculos coherentes |
| Contadores por gravedad y pestañas Todos/Crítico/Error/Aviso/Info | Log de errores | **Descartar** | SPECS no pide filtrar; solo «8 entradas · 6 abiertas». El badge de gravedad ya da el código de color |
| Filtros / «Filtrar período» / «Filtros» / «Filtrar» | Botones | **Descartar** | Sin lógica = botón muerto que un revisor pulsaría; SPECS no los incluye |
| Búsqueda (topbar y por tabla) | Inputs | **Descartar** | Ídem; el filtrado no se evalúa |
| Paginación «Mostrando 1 a 7 de 7» | Pie de tabla | **Descartar** | Solo hay `<p>` de recuento (S3.8) |
| «Exportar reporte» / Exportar | Botón primario en Dashboard | **Descartar** | No está en S4 |
| «Desplegar nuevo agente», «Añadir usuario», «Nueva skill», «Nueva contratación» | Botones de alta | **Descartar** | SPECS no define altas; sin formulario ni lógica |
| Panel «Carga de ejecución por skill» (barras con `style="width:…"`) | Agentes | **Descartar** | No está en SPECS y usa `style` inline |
| Tarjeta «Diagnóstico Reciente» (OpenAI GPT-4o…) | Agentes | **Descartar** | Datos inventados que contradicen el Log |
| Badge «En vivo» (Dashboard oscuro) | Solo en oscuro | **Descartar** | Inconsistente entre temas y no pedido |
| Delta de métrica «+12,4 % vs período anterior» (`DESIGN-stitch.md`) | Pill de variación | **Descartar** | SPECS fija el texto secundario de cada tarjeta |
| Acciones extra: Reenviar enlace/invitación, Historial de logs, Copiar identificador, Copiar traza | Dropdowns y modales | **Descartar** | SPECS fija 2 opciones por menú; `navigator.clipboard` sin comprobar |
| Avatar con iniciales en la tabla de usuarios | Nombre con avatar | **Descartar** | S5.1 no lo pide (el avatar con inicial es solo de agentes, S6.1) |
| Email en `font-mono` | Usuarios | **Cambiar** → `text-slate-600 dark:text-slate-300` | S5.1 |
| Banner «¿Qué es una skill?» | Presente | **Mantener** (con el texto de S7.1) | Lo exige S7.1/CA-30 |
| Rejilla 2×2 de tarjetas, acentos por tipo, enlace de la tarjeta «Agentes fallando» | Presente | **Mantener** | S4.1, S4.3, S4.4 |
| Placeholder del gráfico semanal | Presente | **Mantener** y añadir borde discontinuo en claro | S4.5 |
| Avatar cuadrado con inicial de agente, ID mono bajo el nombre | Presente | **Mantener** (ID en `text-xs text-slate-500`) | S6.1 |
| Badges con punto, 3 estados de agente | Presente | **Mantener** con paleta SPECS y `animate-pulse` | S6.2, §11 |
| Traza en bloque oscuro `font-mono` | Presente | **Mantener** como `<pre>` | S9.4 |
| Tablas con `overflow-x-auto` | Algunas exportaciones | **Descartar** → sin `overflow-x-auto`; dimensionar para 688 px útiles a 768 px (`text-sm`, `px-3 lg:px-4`, `hidden lg:table-cell` en Precio/mes) | S3.7: recortaría el menú ⋮ (CA-10, CA-19) |
| `animate-in fade-in zoom-in-95` (plugin `tailwindcss-animate`) | Modales/menús | **Descartar** → sin animación de entrada, o `transition-opacity`/`transition-transform` estándar | El CDN estándar no incluye el plugin; el modal de SPECS no pide animación |
| `animate-ping` en el punto de «Fallando» | Presente | **Cambiar** → `animate-pulse` | S6.2 |
| Etiqueta «Contrato» para el badge de contratos | Propuesta en `05-contrataciones.md` | **Cambiar** (mínimo) → badge debajo del nombre del agente como pide S8.1, con `<span class="sr-only">Contrato: </span>` | Evita confundir contrato con estado del agente sin añadir texto visible no especificado |
| Marca en Log de errores | «Consola Admin» | **Cambiar** → «AgentHub / Panel de administración» | S3.2 |

---

## Recetas Tailwind (claro + dark:)

Referencia mínima: solo clases (sin `style`, sin CSS). Siguen **SPECS.md** (§3, §11); el comportamiento JS está en `ui-patterns` (con las diferencias de la sección 6). Los iconos se escriben `<svg …><use href="#i-nombre"/></svg>` (sprite inline); abreviado `ICONO` donde no aporta.

### Sidebar (rail en tablet, estado activo)
```html
<aside class="fixed inset-y-0 left-0 z-30 flex w-16 flex-col border-r border-slate-200 bg-white lg:w-64 dark:border-slate-800 dark:bg-slate-900">
  <div class="flex h-16 items-center justify-center gap-3 border-b border-slate-200 lg:justify-start lg:px-5 dark:border-slate-800">
    <span class="grid h-9 w-9 place-items-center rounded-lg bg-indigo-600 font-bold text-white">AH</span>
    <div class="hidden lg:block">
      <p class="font-semibold text-slate-900 dark:text-slate-100">AgentHub</p>
      <p class="text-xs text-slate-500 dark:text-slate-400">Panel de administración</p>
    </div>
  </div>
  <nav aria-label="Navegación principal" class="flex-1 p-2 lg:p-3">
    <ul class="space-y-1">
      <!-- ACTIVO -->
      <li>
        <a href="#dashboard" title="Dashboard" aria-label="Dashboard" aria-current="page"
           class="relative flex items-center justify-center gap-3 rounded-lg bg-indigo-50 px-3 py-2 text-sm font-medium text-indigo-700 lg:justify-start dark:bg-indigo-500/10 dark:text-indigo-300">
          <span class="absolute inset-y-1 left-0 w-1 rounded-r bg-indigo-600 dark:bg-indigo-400"></span>
          <svg class="h-5 w-5" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true"><use href="#i-squares-2x2"/></svg>
          <span class="hidden lg:inline">Dashboard</span>
        </a>
      </li>
      <!-- INACTIVO (sin aria-current ni span de barra; con contador) -->
      <li>
        <a href="#errores" title="Log de errores" aria-label="Log de errores"
           class="relative flex items-center justify-center gap-3 rounded-lg px-3 py-2 text-sm font-medium text-slate-600 hover:bg-slate-100 hover:text-slate-900 lg:justify-start dark:text-slate-400 dark:hover:bg-slate-800 dark:hover:text-slate-100">
          <svg class="h-5 w-5" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true"><use href="#i-exclamation-triangle"/></svg>
          <span class="hidden lg:inline">Log de errores</span>
          <span id="errores-count" class="ml-auto hidden rounded-full bg-red-100 px-2 text-xs font-semibold text-red-800 lg:inline dark:bg-red-900/40 dark:text-red-300">6</span>
          <span id="errores-dot" class="absolute right-1 top-1 h-2 w-2 rounded-full bg-red-500 lg:hidden"></span>
        </a>
      </li>
    </ul>
  </nav>
  <div class="mt-auto hidden items-center gap-3 border-t border-slate-200 p-3 lg:flex dark:border-slate-800">
    <span class="grid h-8 w-8 place-items-center rounded-full bg-slate-200 text-xs font-semibold dark:bg-slate-700">AD</span>
    <div><p class="text-xs font-medium">Admin interno</p><p class="text-xs text-slate-500 dark:text-slate-400">soporte@agenthub.io</p></div>
  </div>
</aside>
<!-- contenedor de contenido: <div class="pl-16 lg:pl-64"> header + main -->
```
Si el contador llega a 0, JS añade `hidden` al pill y al punto (el punto lleva `lg:hidden`: ocultarlo con `hidden` basta porque `hidden` va después en la cascada de display).

### Topbar con toggle
```html
<header class="sticky top-0 z-20 flex h-16 items-center justify-between border-b border-slate-200 bg-white/80 px-6 backdrop-blur dark:border-slate-800 dark:bg-slate-900/80">
  <div>
    <h1 id="page-title" class="text-lg font-semibold">Dashboard</h1>
    <p id="page-subtitle" class="text-xs text-slate-500 dark:text-slate-400">Resumen de noviembre 2026</p>
  </div>
  <button id="theme-toggle" type="button" aria-label="Activar modo oscuro" aria-pressed="false"
          class="rounded-lg p-2 text-slate-600 hover:bg-slate-100 dark:text-slate-300 dark:hover:bg-slate-800">
    <svg class="h-5 w-5 dark:hidden" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true"><use href="#i-moon"/></svg>
    <svg class="hidden h-5 w-5 dark:block" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true"><use href="#i-sun"/></svg>
  </button>
</header>
```
En `<head>`, antes de pintar: `if (localStorage.getItem('theme') === 'dark') document.documentElement.classList.add('dark')` (en `try/catch`).

### Tarjeta de métrica
```html
<article class="flex items-start justify-between gap-4 rounded-xl border border-slate-200 bg-white p-5 shadow-sm dark:border-slate-800 dark:bg-slate-900">
  <div>
    <p class="text-sm font-medium text-slate-500 dark:text-slate-400">Ingresos totales del mes</p>
    <p class="mt-2 text-3xl font-bold tabular-nums text-slate-900 dark:text-white">245,00 €</p>
    <p class="mt-1 text-xs text-slate-500 dark:text-slate-400">Noviembre 2026 · 4 contratos activos</p>
  </div>
  <span class="grid h-11 w-11 place-items-center rounded-lg bg-emerald-100 text-emerald-700 dark:bg-emerald-900/40 dark:text-emerald-300">
    <svg class="h-6 w-6" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true"><use href="#i-banknotes"/></svg>
  </span>
</article>
<!-- Tarjeta 4: <a href="#errores"> en lugar de <article>, + "transition hover:ring-2 hover:ring-red-200 focus-visible:ring-2 focus-visible:ring-indigo-500 dark:hover:ring-red-900";
     el valor lleva "text-red-600 dark:text-red-400" y el icono bg-red-100 text-red-700 dark:bg-red-900/40 dark:text-red-300 -->
<!-- Rejilla: <div class="grid grid-cols-1 gap-6 sm:grid-cols-2"> -->
```

### Badges (los 9 de `00-global.md`, con la paleta de SPECS §11)
```html
<!-- base: inline-flex items-center gap-1.5 rounded-full px-2.5 py-0.5 text-xs font-medium whitespace-nowrap -->
<!-- 1 Activo / Resuelto (en Resuelto, icono check en lugar de punto) -->
<span class="inline-flex items-center gap-1.5 whitespace-nowrap rounded-full bg-emerald-100 px-2.5 py-0.5 text-xs font-medium text-emerald-800 dark:bg-emerald-900/40 dark:text-emerald-300"><span class="h-1.5 w-1.5 rounded-full bg-emerald-500"></span>Activo</span>
<!-- 2 Pendiente / Aviso -->
<span class="inline-flex items-center gap-1.5 whitespace-nowrap rounded-full bg-amber-100 px-2.5 py-0.5 text-xs font-medium text-amber-800 dark:bg-amber-900/40 dark:text-amber-300"><span class="h-1.5 w-1.5 rounded-full bg-amber-500"></span>Pendiente</span>
<!-- 3 Suspendido / Fallando / Crítico (Fallando: punto con animate-pulse; Crítico: icono x-circle w-3.5 h-3.5) -->
<span class="inline-flex items-center gap-1.5 whitespace-nowrap rounded-full bg-red-100 px-2.5 py-0.5 text-xs font-medium text-red-800 dark:bg-red-900/40 dark:text-red-300"><span class="h-1.5 w-1.5 animate-pulse rounded-full bg-red-500"></span>Fallando</span>
<!-- 4 Inactivo / Abierto / Finalizado / Starter -->
<span class="inline-flex items-center gap-1.5 whitespace-nowrap rounded-full bg-slate-100 px-2.5 py-0.5 text-xs font-medium text-slate-700 dark:bg-slate-800 dark:text-slate-300"><span class="h-1.5 w-1.5 rounded-full bg-slate-400"></span>Inactivo</span>
<!-- 5 Error (gravedad) -->
<span class="inline-flex items-center gap-1.5 whitespace-nowrap rounded-full bg-orange-100 px-2.5 py-0.5 text-xs font-medium text-orange-800 dark:bg-orange-900/40 dark:text-orange-300"><span class="h-1.5 w-1.5 rounded-full bg-orange-500"></span>Error</span>
<!-- 6 Info (gravedad) -->
<span class="inline-flex items-center gap-1.5 whitespace-nowrap rounded-full bg-sky-100 px-2.5 py-0.5 text-xs font-medium text-sky-800 dark:bg-sky-900/40 dark:text-sky-300"><span class="h-1.5 w-1.5 rounded-full bg-sky-500"></span>Info</span>
<!-- 7 Plan Enterprise (Stitch: purple → SPECS: violet) -->
<span class="inline-flex items-center whitespace-nowrap rounded-full bg-violet-100 px-2.5 py-0.5 text-xs font-medium text-violet-800 dark:bg-violet-900/40 dark:text-violet-300">Enterprise</span>
<!-- 8 Plan Pro / nº de agentes por skill -->
<span class="inline-flex items-center whitespace-nowrap rounded-full bg-indigo-100 px-2.5 py-0.5 text-xs font-medium text-indigo-800 dark:bg-indigo-900/40 dark:text-indigo-300">Pro</span>
<!-- 9 Plan Starter (= clases de 4, sin punto) -->
<span class="inline-flex items-center whitespace-nowrap rounded-full bg-slate-100 px-2.5 py-0.5 text-xs font-medium text-slate-700 dark:bg-slate-800 dark:text-slate-300">Starter</span>
```
Mapa de uso: Activo/Resuelto/Contrato activo → 1 · Pendiente/Aviso → 2 · Suspendido/Fallando/Crítico/Cancelado → 3 · Inactivo/Abierto/Finalizado → 4 · Error → 5 · Info → 6. En JS, una sola función `badge(variante, texto)` (§11.5).

### Tabla y fila con dropdown ⋮ (cerrado)
```html
<p class="mb-3 text-sm text-slate-500 dark:text-slate-400">7 usuarios</p>
<div class="rounded-xl border border-slate-200 bg-white shadow-sm dark:border-slate-800 dark:bg-slate-900">
  <table class="w-full text-left text-sm">
    <thead>
      <tr class="border-b border-slate-200 dark:border-slate-800">
        <th scope="col" class="px-3 py-3 text-xs font-semibold uppercase tracking-wide text-slate-500 lg:px-4 dark:text-slate-400">Nombre</th>
        <!-- … Email, Plan, Estado … -->
        <th scope="col" class="px-3 py-3 text-right lg:px-4"><span class="sr-only">Acciones</span></th>
      </tr>
    </thead>
    <tbody>
      <tr class="border-b border-slate-100 last:border-0 hover:bg-slate-50 dark:border-slate-800 dark:hover:bg-slate-800/50">
        <td class="px-3 py-3 lg:px-4"><p class="font-medium">Laura Méndez</p><p class="text-xs text-slate-500 dark:text-slate-400">Acme Corp</p></td>
        <td class="px-3 py-3 text-slate-600 lg:px-4 dark:text-slate-300">laura.mendez@acmecorp.com</td>
        <!-- badges de plan y estado -->
        <td class="px-3 py-3 text-right lg:px-4">
          <div class="relative inline-block text-left" data-dropdown>
            <button type="button" data-dropdown-toggle aria-haspopup="menu" aria-expanded="false" aria-label="Acciones para Laura Méndez"
                    class="rounded-lg p-1.5 text-slate-500 hover:bg-slate-100 hover:text-slate-700 dark:text-slate-400 dark:hover:bg-slate-800 dark:hover:text-slate-200">
              <svg class="h-5 w-5" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true"><use href="#i-ellipsis-vertical"/></svg>
            </button>
            <div role="menu" class="absolute right-0 z-40 mt-1 hidden w-48 rounded-lg border border-slate-200 bg-white py-1 shadow-lg dark:border-slate-700 dark:bg-slate-900">
              <button type="button" role="menuitem" data-action="ver-usuario" data-id="US-01" class="flex w-full items-center gap-2 px-3 py-2 text-left text-sm text-slate-700 hover:bg-slate-100 dark:text-slate-200 dark:hover:bg-slate-800">
                <svg class="h-4 w-4" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true"><use href="#i-eye"/></svg>Ver detalle
              </button>
              <button type="button" role="menuitem" data-action="eliminar-usuario" data-id="US-01" class="flex w-full items-center gap-2 px-3 py-2 text-left text-sm text-red-600 hover:bg-red-50 dark:text-red-400 dark:hover:bg-red-950/40">
                <svg class="h-4 w-4" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true"><use href="#i-trash"/></svg>Eliminar
              </button>
            </div>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
</div>
```
Sin `overflow-hidden` ni `overflow-x-auto` en el contenedor. Hijo de `thead` sin fondo (S3.8).

### Dropdown abierto
Mismo markup; JS quita `hidden` del `role="menu"` y pone `aria-expanded="true"` en el botón. En las **dos últimas filas** de cada tabla, el menú cambia `mt-1` por `bottom-full mb-1`:
```html
<button type="button" data-dropdown-toggle aria-haspopup="menu" aria-expanded="true" aria-label="Acciones para Javier Lozano" class="rounded-lg p-1.5 text-slate-500 hover:bg-slate-100 hover:text-slate-700 dark:text-slate-400 dark:hover:bg-slate-800 dark:hover:text-slate-200">…</button>
<div role="menu" class="absolute bottom-full right-0 z-40 mb-1 w-48 rounded-lg border border-slate-200 bg-white py-1 shadow-lg dark:border-slate-700 dark:bg-slate-900">…</div>
```
Opción deshabilitada (Errores): `<button type="button" role="menuitem" disabled class="… cursor-not-allowed opacity-50">Ya resuelto</button>`. Opción verde: `text-emerald-700 dark:text-emerald-400`.

### Modal (único, reutilizable)
```html
<div id="modal" data-backdrop class="fixed inset-0 z-50 hidden items-center justify-center bg-slate-900/60 p-4 backdrop-blur-sm">
  <div role="dialog" aria-modal="true" aria-labelledby="modal-title"
       class="max-h-[90vh] w-full max-w-lg overflow-y-auto rounded-xl bg-white shadow-xl dark:bg-slate-900 dark:ring-1 dark:ring-slate-800">
    <div class="flex items-start justify-between border-b border-slate-200 p-5 dark:border-slate-800">
      <div>
        <h3 id="modal-title" class="text-lg font-semibold">Detalle de usuario</h3>
        <p id="modal-subtitle" class="text-sm text-slate-500 dark:text-slate-400">Laura Méndez</p>
      </div>
      <button type="button" data-close-modal aria-label="Cerrar" class="rounded-lg p-1.5 text-slate-500 hover:bg-slate-100 dark:text-slate-400 dark:hover:bg-slate-800">
        <svg class="h-5 w-5" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true"><use href="#i-x-mark"/></svg>
      </button>
    </div>
    <div id="modal-body" class="p-5">
      <dl class="grid grid-cols-1 gap-x-6 gap-y-4 sm:grid-cols-2">
        <div><dt class="text-xs uppercase text-slate-500 dark:text-slate-400">ID</dt><dd class="text-sm font-medium">US-01</dd></div>
        <!-- … 10 campos … -->
      </dl>
    </div>
    <div id="modal-footer" class="flex justify-end gap-3 border-t border-slate-200 p-4 dark:border-slate-800">
      <button type="button" data-close-modal class="rounded-lg border border-slate-300 px-4 py-2 text-sm font-medium hover:bg-slate-50 dark:border-slate-700 dark:hover:bg-slate-800">Cerrar</button>
      <!-- primario: rounded-lg bg-indigo-600 px-4 py-2 text-sm font-medium text-white hover:bg-indigo-700 -->
    </div>
  </div>
</div>
```
Abrir = quitar `hidden` y añadir `flex`; cerrar = lo inverso (así no depende del orden de la cascada). El ancho se cambia a `max-w-2xl` por clase en Agentes, Contratos y Errores. `body` recibe `overflow-hidden` mientras está abierto. El cierre por backdrop solo si `e.target === modal`. Textarea de Agentes: `rows="10"` con las clases de S6.5; traza: `<pre class="max-h-72 overflow-auto rounded-lg bg-slate-900 p-4 font-mono text-xs leading-relaxed text-slate-100 dark:bg-black/60">`.

### Lista de skills colapsable (colapsada por defecto)
```html
<div>
  <button type="button" data-collapse-toggle aria-expanded="false" aria-controls="skills-AG-02"
          class="inline-flex items-center gap-1 text-sm font-medium text-indigo-600 hover:text-indigo-800 dark:text-indigo-400 dark:hover:text-indigo-300">
    Skills (3)
    <svg class="h-4 w-4 transition-transform duration-300" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true"><use href="#i-chevron-down"/></svg>
  </button>
  <div id="skills-AG-02" class="grid grid-rows-[0fr] opacity-0 transition-all duration-300 ease-in-out">
    <div class="overflow-hidden">
      <ul class="mt-2 flex flex-wrap gap-1.5">
        <li class="rounded-md bg-slate-100 px-2 py-0.5 text-xs text-slate-700 dark:bg-slate-800 dark:text-slate-300">Navegación web</li>
        <li class="rounded-md bg-slate-100 px-2 py-0.5 text-xs text-slate-700 dark:bg-slate-800 dark:text-slate-300">Lectura de documentos</li>
        <li class="rounded-md bg-slate-100 px-2 py-0.5 text-xs text-slate-700 dark:bg-slate-800 dark:text-slate-300">Consulta SQL</li>
      </ul>
    </div>
  </div>
</div>
<!-- expandido: el panel pasa a "grid-rows-[1fr] opacity-100" y el svg añade "rotate-180"; aria-expanded="true".
     JS alterna SOLO clases (nada de element.style ni hidden). -->
```

### Desglose de contrato (cuerpo del modal `max-w-2xl`, ejemplo CT-01)
```html
<dl class="grid grid-cols-2 gap-x-6 gap-y-4 lg:grid-cols-4">
  <div><dt class="text-xs uppercase text-slate-500 dark:text-slate-400">Estado</dt><dd class="mt-1"><!-- badge Activo --></dd></div>
  <div><dt class="text-xs uppercase text-slate-500 dark:text-slate-400">Inicio</dt><dd class="text-sm font-medium">01/01/2026</dd></div>
  <div><dt class="text-xs uppercase text-slate-500 dark:text-slate-400">Fin</dt><dd class="text-sm font-medium">31/12/2026</dd></div>
  <div><dt class="text-xs uppercase text-slate-500 dark:text-slate-400">Duración</dt><dd class="text-sm font-medium">12 meses</dd></div>
</dl>
<table class="mt-5 w-full text-left text-sm">
  <thead>
    <tr class="border-b border-slate-200 dark:border-slate-800">
      <th scope="col" class="py-2 text-xs font-semibold uppercase tracking-wide text-slate-500 dark:text-slate-400">Skill</th>
      <th scope="col" class="py-2 text-right text-xs font-semibold uppercase tracking-wide text-slate-500 dark:text-slate-400">Precio/mes</th>
      <th scope="col" class="py-2 text-right text-xs font-semibold uppercase tracking-wide text-slate-500 dark:text-slate-400">Meses</th>
      <th scope="col" class="py-2 text-right text-xs font-semibold uppercase tracking-wide text-slate-500 dark:text-slate-400">Subtotal</th>
    </tr>
  </thead>
  <tbody>
    <tr class="border-b border-slate-100 dark:border-slate-800"><td class="py-2">Navegación web</td><td class="py-2 text-right tabular-nums">25,00 €</td><td class="py-2 text-right tabular-nums">12</td><td class="py-2 text-right tabular-nums">300,00 €</td></tr>
    <tr class="border-b border-slate-100 dark:border-slate-800"><td class="py-2">Lectura de documentos</td><td class="py-2 text-right tabular-nums">30,00 €</td><td class="py-2 text-right tabular-nums">12</td><td class="py-2 text-right tabular-nums">360,00 €</td></tr>
  </tbody>
  <tfoot>
    <tr class="border-t border-slate-200 dark:border-slate-800"><th scope="row" colspan="3" class="py-2 text-right font-medium text-slate-600 dark:text-slate-300">Total mensual</th><td class="py-2 text-right tabular-nums">55,00 €</td></tr>
    <tr><th scope="row" colspan="3" class="py-2 text-right text-base font-semibold">Total del contrato</th><td class="py-2 text-right text-base font-semibold tabular-nums">660,00 €</td></tr>
  </tfoot>
</table>
<p class="mt-3 font-mono text-xs text-slate-500 dark:text-slate-400">(25,00 € + 30,00 €) × 12 meses = 660,00 €</p>
```
(`colspan` es atributo HTML, no estilo.) Pie del modal: «Descargar factura» (secundario, sin lógica) y «Cerrar».

---

## Datos que faltan para F3

Stitch solo trae un ejemplo de cada modal; **SPECS.md ya define todos los datos** necesarios, así que no hay que añadir nada a la spec para empezar F3. Lo único que hay que generar son los módulos JS a partir de esas tablas.

| Dato pendiente | Stitch trae | ¿Lo define SPECS.md? | Acción en F3 |
|---|---|---|---|
| Modal de usuario × 7 | solo Javier Lozano (US-05) | **Sí**: S5.4 (10 campos) + §10.3 (rol, alta, último acceso, empresa, plan, estado de los 7); «Agentes de la empresa» se calcula filtrando `AGENTES` por `propietario` (Stark Labs → «0 · Ninguno») | Renderizar desde `USUARIOS` |
| Prompt de sistema × 6 agentes | solo Atlas (y con texto distinto) | **Sí**: §10.2, texto literal de Atlas, Orion, Sirius, Lyra, Nova y Vega | Campo `promptSistema` en `AGENTES`; usar el de SPECS, no el de Stitch |
| Permisos × 5 skills | solo SK-05 | **Sí**: §10.1 columna «Permisos requeridos» + descripción completa | Campos `permisos` y `descripcionCompleta` en `SKILLS` |
| Desglose × 6 contratos | solo CT-01 | **Sí**: §10.4 (skills, meses, importes) + S8.4/S8.5 (se calcula en JS) | Calcular subtotales, total mensual y línea de cálculo; comprobar 660 · 900 · 210 · 360 · 45 · 390 |
| Traza × 8 entradas | solo LOG-006 (y distinta) | **Sí**: §10.5, trazas literales LOG-001…008 | Campo `traza` en `ERRORES`; «Skill implicada» sale del ID SK-0n |
| Agentes con skill, en el modal de skill | chips solo en SK-05 | **Sí**: S7.5 + S7.3 (se calcula) con badge mini de estado del agente | Derivar de `AGENTES` |

Detalles que SPECS no cierra del todo y que se resuelven al construir (no bloquean, no requieren cambiar la spec):
- **Paths de los iconos**: SPECS nombra los Heroicons (R6, S3.2, S7.2…) pero no incluye los trazos. Hay que escribir el sprite (≈ 27 iconos, lista en la tabla de decisiones). Es el mayor trabajo manual de F3.
- **Sprite vs SVG en cada uso**: R6 dice «SVG inline». El sprite `<symbol>/<use>` lo cumple; si el equipo prefiere interpretación estricta, usar el diccionario JS de paths (misma salida visual).
- **Foco accesible del colapsable**: colapsado con `opacity-0`, los enlaces/botones internos seguirían en el orden de tabulación; aquí no hay controles dentro (solo chips), así que no afecta.
- **Dropdown hacia arriba tras eliminar filas**: «las dos últimas filas» debe recalcularse al re-renderizar la tabla tras `Eliminar` o resolver.
- **Nombre del modal de agente**: Stitch lo mostraba en una línea; en SPECS son título + subtítulo (S6.5).
- **Etiqueta «Contrato»** del badge de estado de contrato: ver tabla de decisiones (`sr-only`).
