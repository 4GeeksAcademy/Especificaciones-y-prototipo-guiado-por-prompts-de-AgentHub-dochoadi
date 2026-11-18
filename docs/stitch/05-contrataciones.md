# Stitch — 05 Contrataciones (claro y oscuro)

## Estructura
1. (Extra) 4 KPI: Facturación activa 2.160,00 € (= CT-01+02+03+06 ✓) · Contratos en vigor 4 · Finalizados/Cancelados 2 · Duración media 7,2 meses (12+10+6+6+3+6 = 43 / 6 ✓).
2. Barra «Contratos registrados · 6 contratos»; búsqueda, «Filtrar», «Nueva contratación» (decorativos; Stitch duplicó el signo: icono `+` y texto «+ Nueva contratación» → dejar uno).
3. Tabla: Cliente (nombre + `CT-0n`) · Agente (icono + nombre + badge) · Skills contratadas (chips «SK-01 Navegación web») · Inicio · Fin · Importe (derecha, negrita) · ⋮.
4. Dropdown ⋮: **Ver detalle** · **Descargar factura** (sin lógica).
5. Modal «Detalle de contrato» con **desglose** (tabla Skill | Precio/Mes | Meses | Subtotal + total).

## Datos (importe = Σ precio × meses; coinciden con SPECS)
| ID | Cliente | Agente | Estado contrato | Skills | Inicio | Fin | Meses | Cálculo | Importe |
|---|---|---|---|---|---|---|---|---|---|
| CT-01 | Acme Corp | Atlas | Activo | SK-01, SK-02 | 01/01/2026 | 31/12/2026 | 12 | (25+30)×12 | 660,00 € |
| CT-02 | Initech | Orion | Activo | SK-01, SK-02, SK-05 | 01/03/2026 | 31/12/2026 | 10 | (25+30+35)×10 | 900,00 € |
| CT-03 | Acme Corp | Sirius | Activo | SK-03, SK-04 | 01/06/2026 | 30/11/2026 | 6 | (20+15)×6 | 210,00 € |
| CT-04 | Globex | Nova | Finalizado | SK-01, SK-03, SK-04 | 01/05/2026 | 31/10/2026 | 6 | (25+20+15)×6 | 360,00 € |
| CT-05 | Umbrella | Lyra | Cancelado | SK-04 | 01/02/2026 | 30/04/2026 | 3 | 15×3 | 45,00 € |
| CT-06 | Globex | Vega | Activo | SK-02, SK-05 | 01/07/2026 | 31/12/2026 | 6 | (30+35)×6 | 390,00 € |

## Modal de ejemplo (CT-01 · Acme Corp · Atlas)
Estado Activo · «01/01/2026 – 31/12/2026 (12 meses)» · desglose: SK-01 Navegación web 25,00 € × 12 = 300,00 € · SK-02 Lectura de documentos 30,00 € × 12 = 360,00 € · **Total del contrato 660,00 €** · botones «Cerrar» y «Descargar factura PDF».
Generar el desglose para los 6 contratos con los meses de la tabla.

## Atención
- El badge junto al agente es el **estado del contrato** (Activo/Finalizado/Cancelado), no el estado del agente: Vega aparece «Activo» aquí pero «Fallando» en Agentes. Para evitar confusión, rotular la columna o el badge como «Contrato».
- Paleta de estados de contrato: Activo → emerald, Finalizado → slate, Cancelado → red (ver `00-global.md`).
- La pantalla oscura de Stitch usa `bg-inverse-surface` y `classList.add('dark')` en un script propio: no copiar; usar pares `dark:`.
