---
name: genai-tree-expansion
description: "Expandir árbol genealógico con fuentes verificadas para ancestros fallecidos. Trigger: expandir árbol, buscar ancestros, añadir familia."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando el usuario quiera:
- Expandir el árbol genealógico
- Buscar ancestros de una persona
- Añadir nuevas ramas familiares
- Descubrir padres, abuelos o bisabuelos

## Hard Rules

1. Cada persona añadida DEBE citar al menos una fuente.
2. NUNCA confiar en árboles de usuarios (Geni, Ancestry hints) sin corroboración.
3. NO añadir relaciones confirmadas solo de árboles de usuarios → tratarlas como leads.
4. NO modificar fechas o nombres existentes → eso es trabajo de cross-reference audit.
5. Marcar adiciones no verificadas como `(speculative)` o `evidence_tier: speculative`.
6. Preferir añadir a `Open_Questions.md` sobre añadir un ancestro débil a `Family_Tree.md`.
7. NO buscar personas vivas en web. Tratar al punto de partida, hermanos, padres y cualquier persona sin fecha de muerte como vivos.
8. NO publicar fechas exactas de nacimiento, direcciones ni datos de contacto de personas vivas.

## Decision Gates

| Situación | Acción |
|-----------|--------|
| Fuente primaria (certificado, registro) | Añadir a Family_Tree.md con tier `strong` |
| Fuente secundaria con corroboración | Añadir a Family_Tree.md con tier `moderate` |
| Fuente secundaria sin corroboración | Añadir a Open_Questions.md |
| Sin fuentes | NO añadir, registrar en Research_Log.md |
| Persona posiblemente viva | Marcar como `Living`, sin fechas exactas |

## Execution Steps

1. **Baseline**: Leer Family_Tree.md completamente. Registrar posture inicial en Research_Log.md.
2. **Identificar objetivos**: Para cada nodo hoja (ancestro sin padres listados), anotar nombre, fechas y lugar.
3. **Estrategia de búsqueda**: Para cada objetivo, buscar en orden:
   - "Find a Grave" "[NOMBRE]" [AÑO_MUERTE] [LUGAR_SEPULTURA]
   - "[NOMBRE]" padres nacidos [RANGO_AÑOS] [LUGAR]
   - site:geni.com "[NOMBRE]" [AÑO_NACIMIENTO]
   - site:wikitree.com "[NOMBRE]"
   - "[NOMBRE]" genealogía [LUGAR] [APELLIDO]
4. **Evaluar resultados**: Verificar nombre, fechas Y lugar. Todos tres deben alinearse.
5. **Actualizar vault**: Añadir relaciones `strong` o `moderate` a Family_Tree.md. Añadir relaciones débiles a Open_Questions.md.
6. **Registrar búsqueda**: En Research_Log.md, fecha, consultas, resultados (positivos y negativos).
7. **Reportar**: Conteo de candidatos fuertes, moderados, especulativos, rechazados.

## Output Contract

```markdown
## Expansión del Árbol

### Candidatos Revisados
| Nombre | Relación | Tier | Fuente | Acción |
|--------|----------|------|--------|--------|

### Resumen
- Strong: [n]
- Moderate: [n]
- Speculative: [n]
- Rechazados: [n]

### Archivos Modificados
- [archivos]

### Pendiente
- [acciones]
```
