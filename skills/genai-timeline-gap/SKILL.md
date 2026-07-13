---
name: genai-timeline-gap
description: "Encontrar huecos en la línea temporal donde deberían existir registros. Trigger: análisis temporal, huecos, buscar registros faltantes."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando el usuario quiera:
- Encontrar períodos sin registros
- Identificar eventos faltantes (nacimiento, matrimonio, defunción)
- Planificar búsquedas en archivos

## Hard Rules

1. Cada persona debería tener al menos: nacimiento, matrimonio (si aplica), defunción.
2. Los huecos de más de 10 años son críticos.
3. Considerar la disponibilidad de registros por época y lugar.

## Decision Gates

| Situación | Acción |
|-----------|--------|
| Sin registro de nacimiento | Buscar en parroquia o archivo municipal |
| Sin registro de matrimonio | Buscar en parroquia del lugar de la esposa |
| Sin registro de defunción | Cargar `genai-burial-spain` (Registro Civil → parroquial → cementerio → hemeroteca) |

## Execution Steps

1. **Leer Family_Tree.md**: Construir línea temporal de cada persona.
2. **Identificar gaps**: Períodos sin eventos documentados.
3. **Priorizar**: Gaps críticos primero (generaciones recientes).
4. **Proponer búsquedas**: Archivos específicos para cada gap.
5. **Registrar**: En Research_Log.md.

## Output Contract

```markdown
## Análisis de Línea Temporal

### Huecos Encontrados
| Persona | Período | Gap | Prioridad | Búsqueda Sugerida |
|---------|---------|-----|-----------|-------------------|

### Resumen
- Personas analizadas: [n]
- Huecos totales: [n]
- Críticos: [n]
```
