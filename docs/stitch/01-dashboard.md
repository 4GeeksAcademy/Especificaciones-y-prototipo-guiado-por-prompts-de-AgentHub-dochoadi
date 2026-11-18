# Stitch — 01 Dashboard (claro y oscuro)

Referencia válida: `01-dashboard.png` (claro) y `01-dashboard-dark.png` (oscuro) con **245,00 €**. Las otras dos pantallas Dashboard (1.284 agentes, tabla «Nexus-Audit-09», 142 agentes, «Detalle del Agente» con VRAM/Claude 3.5) son la salida del Prompt 0, están guardadas como `00-global.png` / `00-global-dark.png` y solo sirven como muestra de componentes (sus datos son inventados).

## Estructura
1. Cabecera de contenido: `h1` «Vista General» + «Monitorización en tiempo real de operaciones, rendimiento financiero y estado de agentes autónomos.» En oscuro añade badge «En vivo» (punto verde con pulso).
2. Acciones (decorativas): «Filtrar período» (secundario) y «Exportar reporte» (primario índigo). Solo si `SPECS.md` las incluye.
3. Cuadrícula de **4 tarjetas** `grid grid-cols-1 md:grid-cols-2 gap-5`; cada tarjeta: etiqueta, valor grande 30/700, texto de apoyo y **icono en recuadro redondeado** a la derecha.
4. Placeholder de ancho completo `h-72 w-full rounded-xl`, icono de barras, título «Gráfico de actividad semanal», texto «Ejecuciones de agentes · 12–18 nov 2026 (pendiente de integrar)». La versión **oscura** lleva `border-2 border-dashed border-slate-800` (correcto); la clara no tiene borde discontinuo → **añadir** `border-2 border-dashed border-slate-300` (lo exige la spec).

## Tarjetas (datos y acentos)
| Tarjeta | Valor | Apoyo | Acento icono (claro / oscuro) |
|---|---|---|---|
| Ingresos totales del mes | 245,00 € | Noviembre 2026 · 4 contratos activos | emerald (`bg-emerald-50 text-emerald-600` / `dark:bg-emerald-950/40 dark:text-emerald-400`) |
| Pérdida por descuentos y cupones | 31,50 € | 12,9 % sobre ingresos del mes | amber |
| Agentes activos | 3 | de 6 agentes desplegados | indigo |
| Agentes fallando | 2 (en rojo) | Nova, Vega · enlace «ver log de errores» → sección Log | red |

Comprobaciones: 31,50 / 245 = 12,86 % ✓ · 4 contratos activos (CT-01, 02, 03, 06) ✓ · 3 activos + 1 inactivo + 2 fallando = 6 ✓ · 25+30 (Atlas) + 90 (Orion) + 35 (Sirius) + 65 (Vega) = 245 ✓.

## Qué mantener / cambiar
- Mantener: rejilla 2×2, acentos por tipo, `shadow-sm`, enlace «ver log de errores».
- Cambiar: `style="font-variation-settings…"` de los iconos (prohibido); borde discontinuo también en claro; iconos → SVG inline o el criterio decidido en `adaptacion.md`.
- Tarjeta «Agentes fallando» es enlazable: en el prototipo el enlace cambia a la sección Log (hash).
