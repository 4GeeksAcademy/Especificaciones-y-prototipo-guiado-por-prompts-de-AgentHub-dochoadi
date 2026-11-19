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
| F3 | `index.html` | en curso (commiteados: P3.0 `345304a`; P3.1 layout `b91ee39`; P3.1b erratas `e803934`; P3.2 Dashboard `c4aea9f`; P3.2b docs `be7e815`; P3.3 Usuarios `9b370ba`; P3.3b docs `ace1f65`. Sin commitear: P3.4 Agentes. P3.1–P3.4 pendientes de verificación manual en navegador, ERR-004) |
| F4 | QA + `docs/EVALUACION.md` | pendiente |
| F5 | Entrega GitHub público | pendiente |

**Último commit/tag**: `ace1f65` `docs(F3): registra hash de P3.3 y sincroniza evaluación` · último tag `F2-stitch` · **Commiteado en P3.3**: Usuarios (tabla de 7 usuarios, dropdown ⋮, modal de detalle y eliminar; utilidades reutilizables `esc()`, `icon()`, `badge()`, `dropdown()`, `campo()`, `FOOTER_CERRAR`; 42/42 comprobaciones en DOM simulado). **Hecho sin commit (P3.4)**: Agentes (tabla de 6 agentes con avatar, badge con punto, skills colapsables con `grid-rows`, modal «Configurar agente» con `<textarea>` editable y guardado en memoria, eliminar con `Set` de ocultos; `openModal()` usa `FOOTER_CERRAR` por defecto; icono `cog-6-tooth` redibujado; nuevas piezas `CHIP`, `chipsSkills()`, `nombreSkill()`, `BTN_PRIMARIO`; 133/133 en DOM simulado). `docs/EVALUACION.md` aún sin reflejar P3.4. **Decisiones fijadas**: SPECS manda sobre Stitch; sprite con `absolute h-0 w-0 overflow-hidden` (definitivo); título visible solo en el topbar (`<h2 class="sr-only">` por sección); el `index.html` anterior a SPECS (`0dfaf38`, borrado en `a9b4fe4`) es de la plantilla; **los borrados en Agentes y Skills usan un `Set` de IDs ocultos de la vista, nunca `splice` sobre `AGENTES`/`SKILLS`** (el dataset maestro alimenta Dashboard, Usuarios, Skills, Contrataciones y Log; S6.6/S7.6). **Próximo paso**: commit de P3.4 (`feat(F3-agentes): skills colapsables y configurar`); luego P3.4b (hash + `EVALUACION.md`) y P3.5 — Skills (`feat(F3-skills): catálogo y explicación`). Pendiente: verificación visual manual en navegador por la pestaña Ports, puerto 8000 (1440/768 px, claro/oscuro), ver ERR-004.
Reanudar sesión: `docs/RETOMAR.md`.

Detalle en `docs/FASES.md`. Evaluación en `docs/EVALUACION.md`. Errores en `docs/ERRORES.md`.
