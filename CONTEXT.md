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
| F3 | `index.html` | **construida** (6 secciones completas; tag `F3-build` pendiente de la verificación manual en navegador, ERR-004). Commits: P3.0 `345304a`; P3.1 layout `b91ee39`; P3.1b `e803934`; P3.2 Dashboard `c4aea9f`; P3.2b `be7e815`; P3.3 Usuarios `9b370ba`; P3.3b `ace1f65`; P3.4 Agentes `e1d95c5`; P3.5 Skills `b213a33`; P3.6 Contrataciones `3e4116b`; P3.7 Log `ea5fd3d`; P3.7b cierre de docs |
| F4 | QA + `docs/EVALUACION.md` | pendiente |
| F5 | Entrega GitHub público | pendiente |

**Último commit/tag**: `docs(F3): cierra construcción y sincroniza evaluación` (P3.7b; el hash se anota en el siguiente commit de docs) sobre `ea5fd3d` `feat(F3-log): errores, trazas y resolver` · último tag `F2-stitch` · **Estado del prototipo**: `index.html` con las 6 secciones construidas desde el dataset maestro (`SKILLS`, `AGENTES`, `USUARIOS`, `CONTRATOS`, `ERRORES`), sin placeholders; contador de errores del sidebar calculado desde `ERRORES`; 569 comprobaciones en DOM simulado (jsdom en el scratchpad, fuera del repo) sin fallos; restricciones por grep OK (0 `style`/CSS, 0 `overflow-x-auto`, 1 `<script src>`, 6 `section`, 27 `symbol`). Piezas reutilizables: `esc()`, `icon()`, `badge(variante, texto, prefijo, mini)`, `dropdown()` (opciones `peligro`/`exito`/`deshabilitado`), `campo()`, `FOOTER_CERRAR` (pie por defecto de `openModal()`), `CHIP`, `BTN_PRIMARIO`, `BTN_EXITO`. **Decisiones fijadas**: SPECS manda sobre Stitch; sprite con `absolute h-0 w-0 overflow-hidden` (definitivo); título visible solo en el topbar (`<h2 class="sr-only">` por sección); el `index.html` anterior a SPECS (`0dfaf38`, borrado en `a9b4fe4`) es de la plantilla; **los borrados en Agentes y Skills usan un `Set` de IDs ocultos de la vista, nunca `splice` sobre `AGENTES`/`SKILLS`**; en tablet, Contrataciones muestra Fin bajo Inicio (S3.7); «Descargar factura» cierra el menú y no hace nada más (CA-34); el estado del contrato lleva prefijo `sr-only` «Contrato: ». **Pendiente del usuario**: verificación visual manual en navegador de P3.1–P3.7 por la pestaña Ports, puerto 8000 (1440/768 px, claro/oscuro), ERR-004 — no hay ningún resultado anotado todavía. **Próximo paso**: tras esa verificación (y corregir lo que falle con `fix(F3)`/`ERR-00X`), tag `F3-build`; después F4 — `ux-reviewer` y `qa-evaluator` (`docs(F4): evalúa criterios E1-E12`, tag `F4-qa`).
Reanudar sesión: `docs/RETOMAR.md`.

Detalle en `docs/FASES.md`. Evaluación en `docs/EVALUACION.md`. Errores en `docs/ERRORES.md`.
