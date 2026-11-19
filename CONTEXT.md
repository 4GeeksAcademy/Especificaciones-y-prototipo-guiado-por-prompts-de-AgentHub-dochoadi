# CONTEXT — AgentHub

## Producto
AgentHub es un SaaS donde empresas **alquilan agentes de IA** preconfigurados, equipables con **skills** (navegar la web, leer documentos, gestionar calendarios…) y desplegables para tareas de negocio. Usuario del panel: **administrador interno** de AgentHub (equipo fundador/soporte).

## Entregable
Prototipo HTML del panel de administración, para que el equipo lo valide y lo entregue a backend. Rol: desarrollador frontend freelance. Enfoque **Vision to spec**: la spec es el contrato.

## Stack y restricciones
HTML + Tailwind CDN + JS vanilla · sin frameworks/build/backend · sin CSS propio ni `style` inline · datos hardcodeados · desktop y tablet · `dark:` de Tailwind.

## Secciones (sidebar persistente)
1 Dashboard · 2 Usuarios · 3 Agentes · 4 Skills · 5 Contrataciones · 6 Log de errores

## Componentes reutilizables
Sidebar, tarjeta de métrica, dropdown de acciones (⋮), modal, badge, lista de skills colapsable, toggle modo oscuro.

## Dataset maestro (coherencia entre secciones)
Fijar en la spec y no variar:
- **Skills (≥4)**: Navegación web, Lectura de documentos, Gestión de calendario, Envío de emails, (opc.) Consulta SQL.
- **Agentes (≥4)**: nombres únicos, p. ej. `Atlas`, `Nova`, `Orion`, `Lyra`; estados activo/inactivo/fallando; cada uno con propietario (cliente) y skills.
- **Usuarios (≥5)**, **Contratos (≥4)**, **Errores (≥6)** referencian SOLO esos agentes/clientes/skills.
(La lista final vive en `SPECS.md` §Datos; este fichero la referencia.)

## Fases y estado
| Fase | Entregable | Estado |
|---|---|---|
| F0 | Estructura Claude + docs | hecho al commitear |
| F1 | `SPECS.md` (commit propio) | cerrada (`b3b4d64`, tag `F1-spec`) |
| F2 | Propuesta Google Stitch (`docs/stitch/`) | cerrada (`a5be32d`, tag `F2-stitch`) |
| F3 | `index.html` | en curso (P3.0 `345304a`; P3.1 layout `b91ee39`; P3.2 Dashboard hecho sin commitear; P3.1 y P3.2 pendientes de verificación en navegador) |
| F4 | QA + `docs/EVALUACION.md` | pendiente |
| F5 | Entrega GitHub público | pendiente |

**Último commit/tag**: `b91ee39` `feat(F3-layout): sidebar, topbar, modo oscuro y navegación` · último tag `F2-stitch` · **Hecho sin commit**: P3.2 (Dashboard: 4 tarjetas calculadas desde `SKILLS`/`AGENTES`/`CONTRATOS` y placeholder de gráfico; `#icon-test` eliminado → 6 secciones). **Próximo paso**: commit de P3.2 (`feat(F3-dashboard): tarjetas de métricas y placeholder de gráfico`) y luego P3.3 — Usuarios. Pendiente: verificación visual en navegador (1440/768 px, claro/oscuro) y decidir el sprite (`hidden` vs `absolute h-0 w-0`); errata «(28)» → «(27)» en `.claude/skills/ui-patterns/SKILL.md`.
Reanudar sesión: `docs/RETOMAR.md`.

Detalle en `docs/FASES.md`. Evaluación en `docs/EVALUACION.md`. Errores en `docs/ERRORES.md`.
