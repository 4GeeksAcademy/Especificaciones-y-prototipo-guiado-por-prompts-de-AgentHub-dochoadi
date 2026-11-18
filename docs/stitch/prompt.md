# Prompts para Google Stitch — AgentHub Admin Panel (F2 · P2.1)

> Fuente: `SPECS.md` (commit `b3b4d64`). Destino: <https://stitch.withgoogle.com/>.
> La salida de Stitch es **guía visual, no entregable**: el `index.html` se construye a partir de `SPECS.md` (F3).

## Dirección visual (resumen)

| Aspecto | Decisión | Origen en la spec |
|---|---|---|
| Tipo de producto | Panel de administración SaaS B2B, sobrio, orientado a datos | §1 |
| Neutros | Escala `slate` (fondo `slate-50` / `slate-950`, superficies `white` / `slate-900`, bordes `slate-200` / `slate-800`) | S3.1, §11 |
| Acento | `indigo-600` (marca "AH", ítem activo, botones primarios, enlaces) | S3.2, S3.3, §11.4 |
| Semáforo | emerald (activo/resuelto), amber (pendiente/aviso), red (fallando/crítico/suspendido); extras orange (Error), sky (Info), violet (Enterprise) | §11.5 |
| Tipografía | Sans moderna tipo Inter (en F3 se usará la pila `font-sans` por defecto de Tailwind: sin fuentes externas) | R3, R6 |
| Forma | Tarjetas `rounded-xl`, borde 1 px, sombra sutil; badges en píldora | §11.2, §11.5 |
| Densidad | Media: tablas `text-sm`, cabeceras en mayúsculas grises | S3.8 |
| Layout | Sidebar fijo 256 px (rail de 64 px en tablet) + topbar sticky con título, subtítulo y toggle claro/oscuro | S3.2–S3.7 |

Convenciones de los prompts: estilo redactado en inglés, etiquetas de UI en español, cada prompt autocontenido (repite estilo, sidebar y topbar) y con versión **clara y oscura**. Recuento de caracteres incluido bajo cada bloque (incluye saltos de línea; límite 1.500).

---

## Prompt 0 — Sistema de diseño y layout global

```text
Design a professional SaaS admin dashboard shell for "AgentHub" (B2B platform renting AI agents). Generate a LIGHT and a DARK version.
Style: clean, calm, data-dense (medium density), Inter-like sans, slate neutrals, indigo-600 accent, status traffic light emerald/amber/red. Cards rounded-xl, 1px slate border, subtle shadow. Light bg slate-50, surfaces white; dark bg slate-950, surfaces slate-900, borders slate-800.
Left sidebar, fixed, 256px: square indigo logo "AH" + "AgentHub" / "Panel de administración". Nav items with outline icons, in order: Dashboard, Usuarios, Agentes, Skills, Contrataciones, Log de errores (red pill counter "6"). Active item "Dashboard": indigo-50 bg, indigo text, 4px indigo left bar. Footer: avatar "AD", "Admin interno", "soporte@agenthub.io".
Topbar 64px, sticky, translucent white with blur: left title "Dashboard" + small grey subtitle "Resumen de noviembre 2026"; right a moon/sun icon button for dark mode toggle.
Tablet variant (768px): sidebar becomes a 64px icon rail (icons only, red dot on Log de errores), no hamburger.
Show samples: pill badges (Activo green, Pendiente amber, Suspendido red, Inactivo grey), a ⋮ row dropdown, and a centered modal over a dark blurred backdrop with ✕ and footer buttons "Cerrar" + primary indigo.
Spanish UI labels. No illustrations, no gradients.
```

Recuento: **1.329 caracteres**.

---

## Prompt 1 — Dashboard

```text
AgentHub admin panel, screen "Dashboard". Pro SaaS look: slate neutrals, indigo-600 accent, emerald/amber/red status, Inter sans, rounded-xl bordered cards, subtle shadow, medium density. LIGHT (slate-50 bg, white cards) and DARK (slate-950 bg, slate-900 cards) versions.
Layout: fixed left sidebar (256px), logo "AH" AgentHub, items Dashboard, Usuarios, Agentes, Skills, Contrataciones, Log de errores (red counter 6); active "Dashboard" indigo-50 bg + indigo left bar. Sticky topbar: "Dashboard" title, grey subtitle "Resumen de noviembre 2026", moon toggle right. Spanish labels.
Content: 2x2 grid of metric cards (label, big value, small grey note, colored icon square at right): "Ingresos totales del mes" 245,00 € (note "Noviembre 2026 · 4 contratos activos", emerald banknotes icon); "Pérdida por descuentos y cupones" 31,50 € ("12,9 % sobre ingresos del mes", amber); "Agentes activos" 3 ("de 6 agentes desplegados", indigo cpu icon); "Agentes fallando" 2 in red ("Nova, Vega · ver log de errores", red warning icon, clickable).
Below, a full-width 288px-tall dashed-border placeholder, centered bar-chart icon, "Gráfico de actividad semanal" and small "Ejecuciones de agentes · 12–18 nov 2026 (pendiente de integrar)". No real chart.
```

Recuento: **1.242 caracteres**.

---

## Prompt 2 — Usuarios

```text
AgentHub admin panel, screen "Usuarios". Pro SaaS look: slate neutrals, indigo-600 accent, emerald/amber/red status, Inter sans, rounded-xl bordered cards, subtle shadow, medium density. LIGHT (slate-50 bg, white cards) and DARK (slate-950 bg, slate-900 cards) versions.
Layout: fixed left sidebar (256px), logo "AH" AgentHub, items Dashboard, Usuarios, Agentes, Skills, Contrataciones, Log de errores (red counter 6); active "Usuarios" indigo-50 bg + indigo left bar. Sticky topbar: "Usuarios" title, grey subtitle "Usuarios de las empresas cliente", moon toggle right. Spanish labels.
Content: "7 usuarios" count, table with uppercase headers NOMBRE, EMAIL, PLAN, ESTADO, ⋮ column; name bold, company below; emails like laura.mendez@acmecorp.com (domains initech.com, globex.com, umbrella.com, starklabs.io). Rows:
Laura Méndez, Acme Corp, Enterprise, Activo; Daniel Ortega, Acme Corp, Enterprise, Activo; Peter Gibbons, Initech, Pro, Activo; Marta Ruiz, Globex, Pro, Activo; Javier Lozano, Globex, Pro, Pendiente; Carlos Vidal, Umbrella, Starter, Suspendido; Sofía Navarro, Stark Labs, Starter, Pendiente.
Badges: Enterprise violet, Pro indigo, Starter grey; Activo green, Pendiente amber, Suspendido red. Open ⋮ menu on one row: "Ver detalle", "Eliminar" (red trash).
Modal "Detalle de usuario" (Javier Lozano, ✕), 2-col: ID US-05, Rol Operador, Plan Pro, Estado Pendiente, Último acceso "Sin accesos", Agentes de la empresa "2 · Nova, Vega"; button "Cerrar".
```

Recuento: **1.463 caracteres**.

---

## Prompt 3 — Agentes

```text
AgentHub admin panel, screen "Agentes". Pro SaaS look: slate neutrals, indigo-600 accent, emerald/amber/red status, Inter sans, rounded-xl bordered cards, subtle shadow, medium density. LIGHT (slate-50 bg, white cards) and DARK (slate-950 bg, slate-900 cards) versions.
Layout: fixed left sidebar (256px), logo "AH" AgentHub, items Dashboard, Usuarios, Agentes, Skills, Contrataciones, Log de errores (red counter 6); active "Agentes" indigo-50 bg + indigo left bar. Sticky topbar: "Agentes" title, grey subtitle "Agentes desplegados y sus skills", moon toggle right. Spanish labels.
Content: "6 agentes" count, table with uppercase headers AGENTE, PROPIETARIO, ESTADO, SKILLS, ⋮ column. Agent cell: indigo square with initial, bold name, grey ID. Rows:
Atlas AG-01, Acme Corp, Activo; Orion AG-02, Initech, Activo; Sirius AG-03, Acme Corp, Activo; Lyra AG-04, Umbrella, Inactivo; Nova AG-05, Globex, Fallando; Vega AG-06, Globex, Fallando.
Status badges: Activo green, Inactivo grey, Fallando red (pulsing dot). Skills cell: indigo link "Skills (n)" with chevron, collapsed by default; show Orion expanded with grey chips "Navegación web", "Lectura de documentos", "Consulta SQL".
Open ⋮ menu on Nova: "Configurar", "Eliminar" (red). Modal "Configurar agente" / "Atlas · Acme Corp" with ✕: Activo badge + chips, label "Prompt de sistema", monospace textarea "Eres Atlas, agente de investigación de mercado de Acme Corp…", buttons "Cancelar" and indigo "Guardar cambios".
```

Recuento: **1.472 caracteres**.

---

## Prompt 4 — Skills

```text
AgentHub admin panel, screen "Skills". Pro SaaS look: slate neutrals, indigo-600 accent, emerald/amber/red status, Inter sans, rounded-xl bordered cards, subtle shadow, medium density. LIGHT (slate-50 bg, white cards) and DARK (slate-950 bg, slate-900 cards) versions.
Layout: fixed left sidebar (256px), logo "AH" AgentHub, items Dashboard, Usuarios, Agentes, Skills, Contrataciones, Log de errores (red counter 6); active "Skills" indigo-50 bg + indigo left bar. Sticky topbar: "Skills" title, grey subtitle "Catálogo de capacidades contratables", moon toggle right. Spanish labels.
Top: indigo info callout with left border, info icon, title "¿Qué es una skill?" and text "Una skill es una capacidad que se habilita en un agente para que pueda actuar fuera de la conversación…"
Then "5 skills" count and table, uppercase headers SKILL, DESCRIPCIÓN, PRECIO/MES, AGENTES (indigo pill "3 agentes"), ⋮. Skill cell: grey icon square, bold name, grey ID. Descriptions: one short Spanish line. Rows:
Navegación web SK-01, 25,00 €, 3 agentes; Lectura de documentos SK-02, 30,00 €, 3 agentes; Gestión de calendario SK-03, 20,00 €, 2 agentes; Envío de emails SK-04, 15,00 €, 3 agentes; Consulta SQL SK-05, 35,00 €, 2 agentes.
Open ⋮ on Consulta SQL: "Ver detalle", "Eliminar" (red). Modal "Detalle de skill" / "Consulta SQL": ID SK-05, Permisos "Credenciales de BD de solo lectura", "Agentes con la skill habilitada": chips Orion (Activo), Vega (Fallando); button "Cerrar".
```

Recuento: **1.466 caracteres**.

---

## Prompt 5 — Contrataciones

```text
AgentHub admin, screen "Contrataciones". Pro SaaS: slate, indigo-600 accent, emerald/amber/red status, Inter, rounded-xl bordered cards. LIGHT (slate-50/white) and DARK (slate-950/slate-900) versions.
Left sidebar: "AH" AgentHub logo; Dashboard, Usuarios, Agentes, Skills, Contrataciones, Log de errores; active "Contrataciones" indigo-50 + indigo left bar. Topbar: "Contrataciones", subtitle "Contratos de alquiler de agentes", moon toggle right. Spanish labels.
"6 contratos" count; table headers CLIENTE, AGENTE, SKILLS CONTRATADAS, INICIO, FIN, IMPORTE, ⋮. Client + grey ID CT-0n; agent + status badge (Activo green, Finalizado grey, Cancelado red); skill chips by name (SK-01 Navegación web, SK-02 Lectura de documentos, SK-03 Gestión de calendario, SK-04 Envío de emails, SK-05 Consulta SQL). Rows:
Acme Corp, Atlas, Activo, SK-01+SK-02, 01/01/2026–31/12/2026, 660,00 €; Initech, Orion, Activo, SK-01+SK-02+SK-05, 01/03/2026–31/12/2026, 900,00 €; Acme Corp, Sirius, Activo, SK-03+SK-04, 01/06/2026–30/11/2026, 210,00 €; Globex, Nova, Finalizado, SK-01+SK-03+SK-04, 01/05/2026–31/10/2026, 360,00 €; Umbrella, Lyra, Cancelado, SK-04, 01/02/2026–30/04/2026, 45,00 €; Globex, Vega, Activo, SK-02+SK-05, 01/07/2026–31/12/2026, 390,00 €.
⋮ menu: "Ver detalle", "Descargar factura". Modal "Detalle de contrato": table Skill, Precio/mes, Meses, Subtotal; Navegación web 25,00 € 12 300,00 €; Lectura de documentos 30,00 € 12 360,00 €; Total del contrato 660,00 €.
```

Recuento: **1.463 caracteres**.

---

## Prompt 6 — Log de errores

```text
AgentHub admin, screen "Log de errores". Pro SaaS: slate, indigo-600 accent, emerald/amber/red status, Inter, rounded-xl bordered cards. LIGHT (slate-50/white) and DARK (slate-950/slate-900) versions.
Left sidebar: "AH" AgentHub logo; Dashboard, Usuarios, Agentes, Skills, Contrataciones, Log de errores (red badge 6); active "Log de errores" indigo-50 + indigo left bar. Topbar: "Log de errores", subtitle "Incidencias técnicas de los agentes", moon toggle right. Spanish labels.
"8 entradas · 6 abiertas" count; table headers TIMESTAMP, AGENTE (name + grey owner), GRAVEDAD, DESCRIPCIÓN (grey LOG-00n prefix), ESTADO, ⋮. Owners: Orion Initech, Nova/Vega Globex, Atlas Acme Corp, Lyra Umbrella. Severity badges: Crítico red, Error orange, Aviso amber, Info sky blue. Estado: Abierto grey, Resuelto green. Rows, newest first:
18/11/2026 09:02:10 Orion Info Abierto; 18/11/2026 08:31:44 Vega Error Abierto; 17/11/2026 07:58:12 Vega Crítico Abierto: Credenciales de base de datos rechazadas; 15/11/2026 22:05:58 Orion Error Abierto; 12/11/2026 09:20:31 Atlas Aviso Resuelto; 30/10/2026 16:45:09 Nova Crítico Abierto; 29/10/2026 03:12:47 Nova Error Abierto; 28/04/2026 10:14:03 Lyra Info Resuelto.
⋮ on LOG-006 open: "Ver detalle", "Marcar como resuelto" (green). Modal "Detalle del error" with fields and dark mono "Traza completa": "AuthError: password authentication failed for user 'vega_ro'…"; buttons green "Marcar como resuelto", "Cerrar".
```

Recuento: **1.444 caracteres**.

---

## Cómo usarlos

1. **Orden de ejecución**: Prompt 0 primero (fija el sistema visual en el proyecto de Stitch) y después Prompts 1 → 6 en el mismo proyecto, para que Stitch reutilice estilos.
2. Pega cada bloque tal cual (sin la línea de recuento). Si Stitch genera solo una variante, pide a continuación: *"Now generate the same screen in dark mode"* (o *"light mode"*).
3. Si el resultado se desvía (p. ej. inventa menús, gráficos reales o nombres ajenos al dataset), corrige con un mensaje corto de seguimiento en vez de reescribir el prompt.
4. **Exporta capturas** PNG de cada pantalla en ambos temas (ancho de escritorio, ~1440 px) y guárdalas en `docs/stitch/`:

   | Prompt | Claro | Oscuro |
   |---|---|---|
   | 0 Global | `00-global.png` | `00-global-dark.png` |
   | 1 Dashboard | `01-dashboard.png` | `01-dashboard-dark.png` |
   | 2 Usuarios | `02-usuarios.png` | `02-usuarios-dark.png` |
   | 3 Agentes | `03-agentes.png` | `03-agentes-dark.png` |
   | 4 Skills | `04-skills.png` | `04-skills-dark.png` |
   | 5 Contrataciones | `05-contrataciones.png` | `05-contrataciones-dark.png` |
   | 6 Log de errores | `06-errores.png` | `06-errores-dark.png` |

   Opcional: `00-global-tablet.png` con el rail de iconos de 768 px.
5. Con las capturas guardadas, el siguiente paso (P2.2) es `docs/stitch/adaptacion.md`: tabla *propuesta Stitch → decisión*.

## Qué NO copiar de Stitch

- **Su código tal cual**: el HTML/CSS que exporte Stitch no entra en el repo; `index.html` se escribe desde `SPECS.md`.
- **CSS propio**: nada de `<style>`, atributos `style="…"`, `<link rel="stylesheet">`, `@apply`, ficheros `.css` ni `element.style` (R3). Solo utilidades Tailwind con su par `dark:`.
- **Frameworks y librerías**: ni React/Vue/Alpine/jQuery, ni librerías de componentes o de gráficos, ni build (R4). Solo Tailwind por CDN + JS vanilla.
- **Recursos externos**: Google Fonts, fuentes de iconos, imágenes o avatares remotos (R5, R6). Iconos = SVG inline Heroicons outline.
- **Contenido inventado**: secciones, menús, filtros, buscadores, paginación, gráficos reales, KPIs o nombres que no estén en `SPECS.md` §10. Cualquier dato que Stitch altere se corrige con el dataset maestro.
- **Interacciones**: el comportamiento (dropdowns, modales, colapsable, toggle, navegación por hash) lo define `SPECS.md` §11, no Stitch.
- **Layout no previsto**: menú hamburguesa en tablet, `overflow-x-auto` en tablas, sidebar oculto (S3.7).
