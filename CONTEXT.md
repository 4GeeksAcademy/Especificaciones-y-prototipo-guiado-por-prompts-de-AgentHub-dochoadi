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
| F3 | `index.html` | en curso (commiteados: P3.0 `345304a`; P3.1 layout `b91ee39`; P3.1b erratas `e803934`; P3.2 Dashboard `c4aea9f`; P3.2b docs `be7e815`. Sin commitear: P3.3 Usuarios. P3.1, P3.2 y P3.3 pendientes de verificación manual en navegador, ERR-004) |
| F4 | QA + `docs/EVALUACION.md` | pendiente |
| F5 | Entrega GitHub público | pendiente |

**Último commit/tag**: `be7e815` `docs(F3): registra hash de P3.2 y sincroniza estado` · último tag `F2-stitch` · **Hecho sin commit**: P3.3 (Usuarios: tabla de 7 usuarios, dropdown ⋮, modal de detalle y eliminar; utilidades reutilizables `badge()`, `dropdown()`, `campo()`; 42/42 comprobaciones en DOM simulado). **Decisiones fijadas**: SPECS manda sobre Stitch; sprite con `absolute h-0 w-0 overflow-hidden` (definitivo); título visible solo en el topbar (`<h2 class="sr-only">` por sección); el `index.html` anterior a SPECS (`0dfaf38`, borrado en `a9b4fe4`) es de la plantilla. **Próximo paso**: commit de P3.3 (`feat(F3-usuarios): tabla, dropdown y modal`) y luego P3.4 — Agentes (`feat(F3-agentes): skills colapsables y configurar`). Pendiente: verificación visual manual en navegador por la pestaña Ports, puerto 8000 (1440/768 px, claro/oscuro), ver ERR-004.
Reanudar sesión: `docs/RETOMAR.md`.

Detalle en `docs/FASES.md`. Evaluación en `docs/EVALUACION.md`. Errores en `docs/ERRORES.md`.
