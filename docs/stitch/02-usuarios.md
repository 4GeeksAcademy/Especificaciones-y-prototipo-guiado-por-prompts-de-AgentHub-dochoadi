# Stitch — 02 Usuarios (claro y oscuro)

Válido como referencia **claro**. La pantalla «Usuarios (Modo Oscuro)» reutiliza los tokens claros (no es oscura): para dark usar la tabla de equivalencias de `00-global.md`.

## Estructura
1. (Extra de Stitch) 4 KPI: Total registrados 7 · Cuentas activas 4 · Invitaciones pendientes 2 · Bloqueados/Pausados 1 (Umbrella). **Solo si lo pide SPECS.md**; los números cuadran con la tabla.
2. Barra: `h2` «Directorio de usuarios» + píldora «7 usuarios»; búsqueda, «Filtros», «Añadir usuario» (decorativos).
3. Tabla: Nombre (avatar con iniciales + nombre + empresa) · Email (mono) · Plan (badge) · Estado (badge) · Acciones (⋮ alineado a la derecha). Pie «Mostrando 1 a 7 de 7 usuarios».
4. Dropdown ⋮: **Ver detalle** · (Reenviar enlace, extra) · **Eliminar** (rojo, separado por línea).
5. Modal «Detalle de usuario» (centrado, `max-w-lg`, cierre con ✕, botón «Cerrar» y backdrop).

## Datos (coinciden con el dataset maestro)
| ID | Nombre | Empresa | Email | Plan | Estado |
|---|---|---|---|---|---|
| US-01 | Laura Méndez | Acme Corp | laura.mendez@acmecorp.com | Enterprise | Activo |
| US-02 | Daniel Ortega | Acme Corp | daniel.ortega@acmecorp.com | Enterprise | Activo |
| US-03 | Peter Gibbons | Initech | peter.gibbons@initech.com | Pro | Activo |
| US-04 | Marta Ruiz | Globex | marta.ruiz@globex.com | Pro | Activo |
| US-05 | Javier Lozano | Globex | javier.lozano@globex.com | Pro | Pendiente |
| US-06 | Carlos Vidal | Umbrella | carlos.vidal@umbrella.com | Starter | Suspendido |
| US-07 | Sofía Navarro | Stark Labs | sofia.navarro@starklabs.io | Starter | Pendiente |
(Los IDs US-01…07 los infiere el orden; Stitch solo muestra US-05.)

## Modal de ejemplo (Javier Lozano · Globex)
ID US-05 · Rol Operador · Plan Pro · Estado Pendiente · Último acceso «Sin accesos» · Agentes asignados «2 · Nova, Vega» · aviso «Invitación enviada el 18 Nov 2026; caduca en 48 horas» · botones «Cerrar» y «Reenviar invitación».
En el prototipo el modal debe poblarse **para cada usuario** (no solo Javier): definir los campos extra (rol, fecha de alta, último acceso, nº de agentes) en `SPECS.md` §10.

## Qué mantener / cambiar
- Mantener: avatares con iniciales, empresa bajo el nombre, email en mono, badges de plan y estado, dropdown con acción destructiva separada.
- Cambiar: dropdown y modal cerrados al cargar; `onclick="…classList…"` inline → delegación de eventos con `data-*`; fila resaltada con dropdown abierto no debe quedar fija.
- Glitch a evitar: la fila de Carlos Vidal mostraba «Carlos Vidal / Umbrella» como subtítulo; debe ser solo «Umbrella».
