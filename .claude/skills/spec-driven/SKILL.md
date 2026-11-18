---
name: spec-driven
description: Método Vision-to-spec para AgentHub: plantilla de SPECS.md, nivel de detalle exigido y checklist de calidad. Usar al redactar o revisar la especificación.
---

# Spec-driven (Vision → Spec → Código)

**Test de calidad**: un agente de código IA debe poder construir el panel desde `SPECS.md` sin hacer ninguna pregunta.

## Cómo escribir una spec buena
Cada spec = `componente + contenido + comportamiento + estilo`. Ejemplo:
> (1) Cuatro tarjetas de métricas en cuadrícula responsive 2×2 (`grid grid-cols-1 sm:grid-cols-2`), cada una con icono, etiqueta y valor hardcodeado (`$48.250`); (2) acentos distintos por tipo (verde ingresos, ámbar descuentos, azul activos, rojo fallando) y `shadow-sm`; (3) debajo, div ancho completo con `border-2 border-dashed` y etiqueta centrada "Gráfico de actividad semanal".

Evita: "tabla bonita", "algunos usuarios", "colores adecuados".

## Plantilla
```
# SPECS — AgentHub Admin Panel
## 1. Producto   ## 2. Stack y restricciones   ## 3. Layout global
## 4. Dashboard  ## 5. Usuarios  ## 6. Agentes  ## 7. Skills
## 8. Contrataciones  ## 9. Log de errores
## 10. Datos hardcodeados   ## 11. Inventario de componentes
## 12. Criterios de aceptación (numerados)   ## 13. Trazabilidad
```

## Checklist
- [ ] ≥3 specs por cada una de las 6 secciones
- [ ] Mínimos de datos: 5 usuarios · 4 agentes · 4 skills · 4 contratos · 6 errores
- [ ] Dataset coherente (mismos agentes/clientes/skills en todas las secciones)
- [ ] Inventario con los 7 componentes
- [ ] Criterios numerados con ≥1 por: dropdown, modal (botón y backdrop), colapsable, dark mode
- [ ] Restricciones: sin frameworks, sin `style`, sin CSS propio, Tailwind CDN
- [ ] Commit separado antes de cualquier HTML
