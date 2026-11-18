# Convención de Git (Codespaces)

Todo el trabajo se commitea y pushea **desde el Codespace**, rama `main`, un push tras cada commit.

## Formato de commit
```
<tipo>(F<n>[-ámbito]): <resumen imperativo en español, ≤ 72 car.>

[cuerpo opcional: Prompt: P<fase>.<n> · Refs: ERR-00X · Criterios: E1, E3]
```
Tipos: `chore` (estructura/config) · `docs` (specs, docs, bitácora) · `feat` (funcionalidad del prototipo) · `fix` (corrección; cita `ERR-00X`) · `style` (solo clases/estética) · `test` (verificaciones) · `refactor`.

## Mensajes canónicos por fase
| Fase | Commit(s) | Tag al cerrar |
|---|---|---|
| F0 | `chore(F0): estructura Claude y limpieza de plantilla` | `F0-estructura` |
| F1 | `docs(F1): añade SPECS.md` (**solo ese fichero**, antes de cualquier `.html`) · `docs(F1): registra prompts de la spec` | `F1-spec` |
| F2 | `docs(F2): prompt y propuesta visual de Stitch` · `docs(F2): adaptación de Stitch a la spec` | `F2-stitch` |
| F3 | `feat(F3-layout): sidebar, topbar y modo oscuro` · `feat(F3-dashboard): tarjetas y placeholder de gráfico` · `feat(F3-usuarios): tabla, dropdown y modal` · `feat(F3-agentes): skills colapsables y configurar` · `feat(F3-skills): catálogo y explicación` · `feat(F3-contrataciones): tabla y desglose` · `feat(F3-errores): log, badges y resolver` | `F3-build` |
| F4 | `fix(F4): ERR-00X <resumen>` · `docs(F4): evalúa criterios E1-E12` | `F4-qa` |
| F5 | `docs(F5): README final y enlace de entrega` | `F5-entrega` |

## Comandos
```bash
git status && git diff --stat                      # revisar antes
git add <ficheros concretos>                       # nunca `git add .` sin mirar status
git commit -m "docs(F1): añade SPECS.md" -m "Prompt: P1.1"
git push origin main
# cierre de fase
git tag -a F1-spec -m "Fase 1: especificación"; git push origin F1-spec
```

## Verificación E1 (spec antes que HTML)
```bash
git log --reverse --name-status --format='%h %s' | grep -E 'SPECS.md|\.html|^[0-9a-f]{7} '
```
`SPECS.md` debe aparecer en un commit anterior al primer `.html`.

## Registro
Tras cada commit anotar mensaje + hash corto (`git log -1 --format=%h`) en la entrada de `docs/PROMPTS.md`; los `fix` también en `docs/ERRORES.md`.
