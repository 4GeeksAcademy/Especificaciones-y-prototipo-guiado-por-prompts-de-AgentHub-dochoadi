# Trabajar en GitHub Codespaces

## Flujo
1. Los prompts se **generan en la sesión de planificación** y se **copian al Claude Code del Codespace**.
2. En el Codespace se ejecuta el prompt, se verifica y se hace commit + push con la nomenclatura de `docs/GIT.md`.
3. La salida/hash se pega de vuelta en la sesión de planificación para registrarla en `docs/PROMPTS.md` y revisarla contra `docs/EVALUACION.md`.

## Arranque (una vez)
Crear el Codespace desde el repo (`Code → Codespaces → Create`). En su terminal pegar `bootstrap-f0.sh` y después:
```bash
git status
git add -A && git commit -m "chore(F0): estructura Claude y limpieza de plantilla" && git push origin main
git tag -a F0-estructura -m "Fase 0: estructura" && git push origin F0-estructura
```

## Previsualizar el prototipo
```bash
python3 -m http.server 8000      # abrir el puerto 8000 reenviado
```
o extensión Live Server. No hay build ni dependencias.

## Formato de cada prompt entregado
① Prompt para Claude Code · ② verificación (bash) · ③ commit + push (+ tag al cerrar fase).
