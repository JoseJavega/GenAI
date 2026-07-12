---
name: genai-source-citation
description: "Verificar que cada persona cite al menos dos fuentes independientes. Trigger: auditar citas, verificar fuentes, completitud de fuentes."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando el usuario quiera:
- Verificar que cada persona tenga suficientes fuentes
- Auditar la completitud de citas
- Identificar personas con fuentes débiles

## Hard Rules

1. Cada persona DEBE tener al menos 2 fuentes independientes para ser considerada `strong`.
2. Una sola fuente = `moderate` como máximo.
3. Sin fuentes = `speculative`.
4. Las fuentes deben ser independientes (no dos entradas del mismo árbol).

## Decision Gates

| Situación | Acción |
|-----------|--------|
| ≥2 fuentes independientes | Tier `strong` |
| 1 fuente primaria | Tier `moderate` |
| Solo fuentes secundarias | Tier `moderate` con nota |
| Sin fuentes | Tier `speculative`, mover a Open_Questions |

## Execution Steps

1. **Leer Family_Tree.md**: Extraer todas las personas.
2. **Verificar fuentes**: Para cada persona, contar fuentes independientes.
3. **Clasificar**: Por tier de evidencia.
4. **Identificar gaps**: Personas con pocas fuentes.
5. **Proponer acciones**: Buscar fuentes adicionales.
6. **Actualizar vault**: Ajustar tiers según hallazgos.

## Output Contract

```markdown
## Auditoría de Fuentes

### Resumen por Tier
| Tier | Cantidad | Personas |
|------|----------|----------|

### Personas con Fuentes Débiles
| Persona | Fuentes | Tier Actual | Acción Recomendada |
|---------|---------|-------------|-------------------|

### Recomendaciones
- [Acciones para mejorar fuentes]
```
