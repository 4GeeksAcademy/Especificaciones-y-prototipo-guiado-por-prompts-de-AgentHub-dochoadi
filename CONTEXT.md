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
| F3 | `index.html` | cerrada (`b91ee39`…`ea5fd3d`; cierre `32504d5`, tag `F3-build`; verificación visual del usuario 1440/768 px, claro/oscuro) |
| F4 | QA + `docs/EVALUACION.md` | completa: 8 correcciones de accesibilidad ERR-005…ERR-012 (`d830e18`…`bad921d`) y evaluación E1–E12 ✅ (`eddb4ad`); **tag `F4-qa` pendiente del usuario** |
| F5 | Entrega GitHub público | lista: README final (`docs(F5): README final y enlace de entrega`), repo **público** con `SPECS.md` e `index.html` en la raíz de `main`; **tag `F5-entrega` pendiente del usuario** |

**Último commit/tag**: `docs(F5): README final y enlace de entrega` (P5.1) sobre `eddb4ad` `docs(F4): evalúa criterios E1-E12` · último tag `F3-build` · **Repositorio (público)**: <https://github.com/4GeeksAcademy/Especificaciones-y-prototipo-guiado-por-prompts-de-AgentHub-dochoadi>. **Estado**: prototipo completo (6 secciones), E1–E12 ✅ con evidencia en `docs/EVALUACION.md` (pruebas jsdom 643/643, greps de restricciones, orden SPECS → HTML y confirmación visual del usuario); mejoras no aplicadas en `docs/EVALUACION.md` §D. **Decisiones fijadas**: SPECS manda sobre Stitch y no se ha modificado desde `b3b4d64`; las clases de contraste de ERR-005 sustituyen a las literales de S9.1/S9.4/S9.5 (desviación documentada); borrados con `Set` de ocultos; sprite `absolute h-0 w-0 overflow-hidden`; un único `<h1>` en el topbar. **Pendiente del usuario**: crear y subir los tags `F4-qa` (sobre `eddb4ad`) y `F5-entrega` (sobre el commit de F5); opcionalmente, un vistazo en navegador a la transición del colapsable tras ERR-009 y al anillo de foco de ERR-011. **Próximo paso**: ninguno de construcción; solo los tags de cierre.
Reanudar sesión: `docs/RETOMAR.md`.

Detalle en `docs/FASES.md`. Evaluación en `docs/EVALUACION.md`. Errores en `docs/ERRORES.md`.
