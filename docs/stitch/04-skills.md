# Stitch — 04 Skills (claro y oscuro)

## Estructura
1. **Banner explicativo** (obligatorio por el briefing) arriba: icono info, título «¿Qué es una skill?», texto «Una skill es una capacidad que se habilita en un agente para que pueda actuar fuera de la conversación, interactuando con APIs externas, bases de datos o servicios en la nube de forma segura y gobernada.» (claro: fondo índigo tenue; oscuro: `bg-indigo-950/40 border-indigo-800/60 border-l-4 border-l-indigo-500`).
2. (Extra) 3 KPI: Total habilidades 5 · Vínculos con agentes 13 ✓ · Gasto combinado/mes 125,00 € (suma de precios del catálogo; renombrar «Precio combinado del catálogo» para no confundir con facturación).
3. Cabecera «5 skills · Capacidades registradas en el sistema»; búsqueda y «Nueva skill» decorativos.
4. Tabla: Skill (icono + nombre + `SK-0n` mono) · Descripción · Precio/Mes (derecha, `tabular-nums`) · Agentes (píldora «N agentes») · ⋮.
5. Dropdown ⋮: **Ver detalle** · **Eliminar**. Modal «Detalle de skill».

## Datos (todos coinciden con el dataset)
| ID | Skill | Precio/mes | Agentes habilitada | Descripción |
|---|---|---|---|---|
| SK-01 | Navegación web | 25,00 € | 3 (Atlas, Orion, Nova) | Permite al agente explorar páginas web públicas y extraer contenido relevante en tiempo real. |
| SK-02 | Lectura de documentos | 30,00 € | 3 (Atlas, Orion, Vega) | Indexa y procesa archivos PDF, CSV y DOCX para análisis textual contextual. |
| SK-03 | Gestión de calendario | 20,00 € | 2 (Sirius, Nova) | Consulta disponibilidad, reserva reuniones y gestiona invitaciones en Google Calendar y Outlook. |
| SK-04 | Envío de emails | 15,00 € | 3 (Sirius, Lyra, Nova) | Redacción asistida y despacho transaccional de correos corporativos autenticados. |
| SK-05 | Consulta SQL | 35,00 € | 2 (Orion, Vega) | Ejecución de consultas estructuradas en almacenes de datos analíticos. |

## Modal de ejemplo (Consulta SQL · SK-05)
Identificador SK-05 · Tarifa mensual 35,00 € / mes · Permisos configurados «Credenciales de BD de solo lectura» (+ detalle de TLS/RLS/timeout) · «Agentes con la skill habilitada»: chips **Orion · Activo** y **Vega · Fallando**.
En el prototipo hay que generar el modal para **cada** skill (permisos propios y agentes de la tabla de arriba, con su estado).

## Qué mantener / cambiar
- Mantener: banner explicativo, precio alineado a la derecha, píldora de nº de agentes, chips con estado en el modal.
- Cambiar: modal y dropdown cerrados; sin `animate-in` de plugins que no existen en Tailwind CDN estándar (`animate-in fade-in zoom-in-95` pertenece a un plugin: usar `transition` + `opacity`/`scale` propios o nada).
