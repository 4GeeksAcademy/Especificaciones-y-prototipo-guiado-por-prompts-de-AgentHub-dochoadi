# Stitch — 06 Log de errores (claro y oscuro)

## Estructura
1. Cabecera: título «Registro de auditoría / Incidencias» + píldora «8 entradas · 6 abiertas»; contadores por gravedad: Crítico 2 · Errores 3 · Aviso 1 · Info 2 (suman 8 ✓).
2. Pestañas de filtro (Todos / Crítico / Error / Aviso / Info) y búsqueda: **decorativas** salvo que SPECS.md pida lo contrario (el filtrado real no se evalúa).
3. Tabla cronológica descendente: Timestamp (mono) · Agente (nombre + «(cliente)») · Gravedad (badge) · Descripción («LOG-00n:» en mono + texto) · Estado (Abierto/Resuelto) · ⋮.
4. Dropdown ⋮: **Ver detalle** · **Marcar como resuelto** (verde) · (Copiar identificador, extra).
5. Modal «Detalle del error»: LOG-00n · agente; cuadrícula Gravedad / Timestamp / Estado / Agente; descripción completa; **traza completa** en bloque oscuro `font-mono` con botón «Copiar traza»; botones «Cerrar» y «Marcar como resuelto».

## Datos (8 entradas; solo agentes Nova, Vega, Orion, Atlas, Lyra)
| ID | Timestamp | Agente | Gravedad | Descripción | Estado |
|---|---|---|---|---|---|
| LOG-008 | 18/11/2026 09:02:10 | Orion (Initech) | Info | Healthcheck periódico completado con latencia nominal (142ms) | Abierto |
| LOG-007 | 18/11/2026 08:31:44 | Vega (Globex) | Error | Timeout en consulta externa al servicio de indexación tras 3 reintentos | Abierto |
| LOG-006 | 17/11/2026 07:58:12 | Vega (Globex) | Crítico | Credenciales de base de datos rechazadas | Abierto |
| LOG-005 | 15/11/2026 22:05:58 | Orion (Initech) | Error | Fallo de deserialización JSON en endpoint de webhook corporativo | Abierto |
| LOG-004 | 12/11/2026 09:20:31 | Atlas (Acme Corp) | Aviso | Consumo de tokens cercano al umbral del 85% para la ventana horaria | Resuelto |
| LOG-003 | 30/10/2026 16:45:09 | Nova (Globex) | Crítico | Excepción no controlada en pipeline de análisis sintáctico de PDF | Abierto |
| LOG-002 | 29/10/2026 03:12:47 | Nova (Globex) | Error | Límite de tasa (rate-limit 429) alcanzado en API de terceros | Abierto |
| LOG-001 | 28/04/2026 10:14:03 | Lyra (Umbrella) | Info | Reinicio programado de contenedor Docker completado con éxito | Resuelto |

Abiertas = 6 ✓ (coincide con el «6» del sidebar). Con Info: LOG-008 y LOG-001; Error: 007, 005, 002; Crítico: 006, 003; Aviso: 004.

## Modal de ejemplo (LOG-006 · Vega · Globex)
Gravedad Crítico · 17/11/2026 07:58:12 · Abierto · descripción «Credenciales de base de datos rechazadas. Conexión denegada al intentar ejecutar la skill SK-05 (Consulta SQL).» · traza:
```
AuthError: password authentication failed for user 'vega_ro'
    at Connection.parseE (/app/node_modules/pg/lib/connection.js:614:13)
    at Connection.parseMessage (/app/node_modules/pg/lib/connection.js:413:19)
    at Socket.<anonymous> (/app/node_modules/pg/lib/connection.js:129:22)
    at Socket.emit (events.js:314:20)
    at addChunk (_stream_readable.js:298:12)
    at readableAddChunk (_stream_readable.js:273:9)
    severity: FATAL, code: 28P01, database: analytics_db, host: db.globex.internal:5432
```
**Falta:** Stitch solo trae la traza de LOG-006. El prototipo necesita una traza coherente para las 8 entradas (la de Nova/PDF y la de Vega/timeout deben ser plausibles con su skill: SK-02 Lectura de documentos, SK-05 Consulta SQL…).

## Comportamiento «Marcar como resuelto»
El modal de Stitch cambia el botón a «Resuelto» y se cierra a los 700 ms con `setTimeout`. En el prototipo: pasa la fila a badge «Resuelto» (verde), baja el contador «6 no resueltos» y el «6» del sidebar, y (opcional) actualiza el contador de gravedad.

## Qué mantener / cambiar
- Mantener: ID `LOG-00n` en mono, agente con cliente entre paréntesis, badges por gravedad (4 colores distintos), traza en bloque oscuro, resaltado de fila crítica abierta (`bg-red-50`/`dark:bg-red-950/20`).
- Cambiar: sin `onclick` inline ni `navigator.clipboard` sin comprobar; la sidebar de esta pantalla usaba otra marca (ver `00-global.md`).
