# Stitch — 03 Agentes (claro y oscuro)

## Estructura
1. Cabecera: píldora «6 agentes» + «3 activos» + «2 fallando»; (extras decorativos: búsqueda, «Filtros», «Desplegar nuevo agente»).
2. Tabla: Agente (cuadrado con inicial + nombre + `AG-0n` mono) · Propietario · Estado (badge) · Skills (control «Skills (n) ▾») · ⋮.
3. **Skills colapsadas por defecto**; al pulsar el control se despliegan chips (Stitch solo muestra Orion desplegado y **sin transición**). En el prototipo: colapsado en todas, transición suave `grid-rows-[0fr]`↔`[1fr]`, `aria-expanded`, flecha que rota.
4. Dropdown ⋮: **Configurar** · (Historial de logs, extra) · **Eliminar** (rojo).
5. Modal «Configurar agente — Atlas · Acme Corp»: badge de estado, chips de skills, label «Prompt de sistema», `<textarea>` editable (mono, `rows=5`, `resize-none`), contador, botones «Cancelar» y «Guardar cambios».

## Datos (dataset maestro) y correcciones
| ID | Agente | Propietario | Estado | Skills (según SPECS) | Stitch mostraba |
|---|---|---|---|---|---|
| AG-01 | Atlas | Acme Corp | Activo | Navegación web, Lectura de documentos (2) | **«Skills (4)»** y «Análisis financiero» → ❌ corregir a 2 |
| AG-02 | Orion | Initech | Activo | Navegación web, Lectura de documentos, Consulta SQL (3) | ✓ |
| AG-03 | Sirius | Acme Corp | Activo | Gestión de calendario, Envío de emails (2) | ✓ (2) |
| AG-04 | Lyra | Umbrella | Inactivo | Envío de emails (1) | ✓ |
| AG-05 | Nova | Globex | Fallando | Navegación web, Gestión de calendario, Envío de emails (3) | ✓ (3) |
| AG-06 | Vega | Globex | Fallando | Lectura de documentos, Consulta SQL (2) | ✓ (2) |

- El modal de Atlas debe listar solo sus 2 skills reales. Cada agente necesita **su propio prompt de sistema** (SPECS.md §10); Stitch solo da el de Atlas: «Eres Atlas, agente de investigación de mercado de Acme Corp. Tu función principal es recopilar, analizar y sintetizar datos competitivos a partir de fuentes web públicas y documentos corporativos indexados…»
- Badges: Activo (verde), Inactivo (gris), Fallando (rojo con `animate-ping`).

## Extras de Stitch a descartar (no están en la spec y arrastran datos incoherentes)
- Tira de 4 métricas (disponibilidad 94,8 %, «15 skills en 6 agentes» ❌ real = 13, peticiones/hora, alertas).
- Panel «Carga de ejecución por skill» con barras (`style="width: 42.4%"` inline) y «Análisis financiero & Conciliación» (skill inexistente).
- Tarjeta «Diagnóstico Reciente» con «OpenAI GPT-4o … eu-central-1b» (datos inventados que contradicen el Log).

## Qué mantener / cambiar
- Mantener: iniciales en cuadrado, ID en mono, control «Skills (n)», badges, chips de skill.
- Cambiar: skills colapsadas con transición real; modal y dropdown cerrados; `style.display='none'` inline → clase `hidden`; cierre con Esc y backdrop (Stitch claro lo hace con script propio solo para Esc).
