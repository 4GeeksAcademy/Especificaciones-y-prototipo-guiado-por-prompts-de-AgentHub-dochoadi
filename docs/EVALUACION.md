# Evaluación — matriz de cumplimiento

Estado: ⬜ pendiente · ✅ cumple (con evidencia) · ❌ falla (ver ERRORES.md)
Un ✅ exige evidencia: comando + resultado, captura o commit.

## A. Criterios de evaluación del reto
| # | Criterio | Cómo se verifica | Estado | Evidencia |
|---|---|---|---|---|
| E1 | `SPECS.md` commiteado antes de cualquier HTML | `git log --reverse --name-status` | ⬜ | |
| E2 | Spec: ≥3 specs/sección, inventario de componentes, criterios numerados | Revisión de `SPECS.md` | ⬜ | |
| E3 | Seis secciones presentes y accesibles desde el sidebar | Navegar cada enlace | ⬜ | |
| E4 | Tailwind consistente, sin estilos inline ni CSS externo | `grep -n 'style=\|<style\|\.css' index.html` → 0 | ⬜ | |
| E5 | Todas las filas tienen dropdown ⋮ que abre, cierra al clic y al clic fuera | Prueba en Usuarios, Agentes, Skills, Contrataciones, Errores | ⬜ | |
| E6 | "Ver detalle" abre modal en ≥4 secciones | Usuarios, Skills, Contrataciones, Errores (+ Configurar en Agentes) | ⬜ | |
| E7 | Modales cierran con botón y con backdrop | Probar en cada modal | ⬜ | |
| E8 | Skills de agentes colapsadas por defecto, expanden/colapsan con transición | Probar en Agentes | ⬜ | |
| E9 | Toggle dark/claro cambia todo y persiste entre secciones | Alternar y navegar las 6 vistas | ⬜ | |
| E10 | Datos coherentes (mismo agente en Agentes, Contrataciones, Errores) | Cruce de nombres | ⬜ | |
| E11 | HTML semántico (`section`, `table`, `nav`, `header`, `main`) | `grep` + revisión | ⬜ | |
| E12 | Layout usable en escritorio y tablet | Capturas 1440 y 768 px | ⬜ | |

## B. Requisitos de la tarea (checklist del enunciado)
**Spec**: ☐ `SPECS.md` en raíz · ☐ descripción del producto y admin · ☐ stack y restricciones · ☐ ≥3 specs por sección · ☐ inventario · ☐ criterios numerados (≥1 por dropdown, modal, colapsable, dark) · ☐ commit previo al HTML · ☐ propuesta Google Stitch usada como guía.

**Dashboard**: ☐ 4 tarjetas (icono, etiqueta, valor) · ☐ área ancho completo para gráfico semanal.
**Usuarios**: ☐ ≥5 filas (nombre, email, plan, badge estado) · ☐ dropdown Ver detalle/Eliminar · ☐ modal registro completo (botón + backdrop).
**Agentes**: ☐ ≥4 agentes (nombre, propietario, badge, skills colapsada) · ☐ expandir/colapsar suave · ☐ dropdown Configurar/Eliminar; Configurar → modal con `<textarea>` editable con el prompt de sistema.
**Skills**: ☐ ≥4 skills (nombre, descripción, nº agentes) · ☐ explicación de "skill" en el panel · ☐ dropdown Ver detalle/Eliminar.
**Contrataciones**: ☐ ≥4 contratos (cliente, agente, skills, inicio/fin, importe) · ☐ dropdown; Ver detalle → desglose con precio por skill.
**Log de errores**: ☐ ≥6 entradas (timestamp, agente, badge por tipo/gravedad, descripción) · ☐ dropdown Ver detalle (traza) / Marcar como resuelto.
**Global**: ☐ toggle dark en topbar con `dark:` y persistente · ☐ dropdowns cierran al clic fuera · ☐ modales cierran en backdrop · ☐ solo JS vanilla, Tailwind solo CDN · ☐ sidebar persistente con sección activa · ☐ único `index.html` (o uno por sección).

**Entrega**: ☐ repo público en GitHub · ☐ `SPECS.md` e `index.html` visibles.

## C. Registro de verificaciones
| Fecha | Fase | Qué se probó | Resultado | Evidencia |
|---|---|---|---|---|
| | | | | |
