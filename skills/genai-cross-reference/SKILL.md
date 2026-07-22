---
name: genai-cross-reference
description: "Auditar discrepancias entre el árbol genealógico y documentos fuente. Trigger: auditar fuentes, verificar datos, cruzar referencias."
license: MIT
metadata:
  author: JoseJavega
  version: "1.1.0"
---

## Activation Contract

Carga este skill cuando:
- Quieras auditar coherencia del árbol (batch)
- Necesites verificar fechas entre documentos
- Proceses una transcripción nueva (reconciliación post-OCR)
- Encontrar y corregir discrepancias
- Validar datos contra fuentes primarias

## Hard Rules

1. NUNCA modificar datos sin al menos dos fuentes que respalden el cambio.
2. SIEMPRE registrar cada discrepancia encontrada.
3. SIEMPRE reportar la fuente más confiable para cada campo.
4. NO asumir qué fuente es correcta sin evidencia.
5. Mantener registro de todas las auditorías en Research_Log.md.
6. **Ubicaciones**: Fichas en `personas/`, fuentes en `fuentes/`, transcripciones en `fuentes/transcripciones/`.
7. Al leer una transcripción, crear personas nuevas solo si no existen en `personas/` Y hay al menos 2 fuentes consistentes que la respalden (transcripción + otra fuente o corroboración interna del documento).
8. Actualizar bidireccionalmente: si se crea/modifica hijo, buscar y actualizar ficha del padre/madre.
9. Usar Conflict_Log.md para discrepancias (NO Research_Log).
10. Seguir `genai-tree-expansion/references/daboville-algorithm.md` para numeración al crear personas nuevas.

## Decision Gates

| Situación | Acción |
|-----------|--------|
| Dos fuentes primarias discrepan | Investigar más, no cambiar aún |
| Fuente primaria vs secundaria | Privilegiar primaria |
| Certificado oficial vs árbol | Privilegiar certificado |
| Sin resolución posible | Marcar como `unresolved` en Family_Tree.md |
| Persona nueva, alta confianza (>= 0.90) | Crear ficha automáticamente |
| Persona nueva, confianza media (0.70-0.89) | Crear con nota "pendiente verificar" |
| Persona nueva, baja confianza (< 0.70) | NO crear, registrar en Conflict_Log |
| Persona existe, fechas coinciden | Añadir fuente, mejorar tier si aplica |
| Persona existe, fechas difieren | Registrar en Conflict_Log, NO sobrescribir |
| Persona existe, campo faltante | Añadir campo con fuente |
| Relación padre-hijo detectada | Actualizar bidireccionalmente ambas fichas |

## Execution Steps

1. **Leer fichas de personas**: `glob personas/*.md` para extraer todas las personas con datos.
2. **Leer transcripciones**: `glob fuentes/transcripciones/*.md` para obtener fuentes documentales.
3. **Cruzar por persona**: Usar campo `person` del frontmatter de cada transcripción para vincular con la ficha correspondiente.
3.5. **Reconciliar**:
     Para cada persona mencionada en la transcripción:

     a) Buscar en `personas/*.md` por nombre exacto o parcial

     b) Si NO existe:
        - Determinar Sosa/d'Aboville según algoritmo (reference: daboville-algorithm.md)
        - Crear ficha en `personas/` con:
          * frontmatter: type, name, sosa, daboville, born, birth_place, life_status, family, evidence_tier, sources
          * body: referencias a fuentes
        - evidence_tier basado en ocr_confidence:
          * >= 0.90 → strong
          * 0.70-0.89 → moderate (con nota "pendiente de verificar")
          * < 0.70 → NO crear, registrar en Conflict_Log como "open"

     c) Si EXISTE:
        - Comparar campo por campo (born, died, birth_place, death_place, family)
        - Coincide → añadir fuente a `sources`, mejorar `evidence_tier` si aplica
        - No coincide → registrar en Conflict_Log.md con formato:
          ```
          ### [FECHA] - [SEVERIDAD] - [PERSONA] - [CAMPO]
          **Fuente A:** [archivo actual] → [dato actual]
          **Fuente B:** [nueva transcripción] → [dato nuevo]
          **Estado:** open
          ```
        - Campo faltante en ficha → añadir con nota de fuente, mantener `evidence_tier` existente

     d) Actualizar relaciones bidireccionales:
        - Si se crea/modifica hijo → buscar padres en `personas/`, actualizar ficha del padre con referencia al hijo
        - Si se crea/modifica padre → buscar hijos existentes, actualizar sus fichas con referencia al padre
4. **Identificar discrepancias**: Fechas, nombres, lugares que no coinciden entre ficha y transcripción.
5. **Clasificar**: Por severidad (crítica, menor, cosmética).
6. **Proponer resoluciones**: Con fuente citada.
7. **Actualizar vault**: Aplicar cambios aprobados en `personas/`.
8. **Registrar**: En Conflict_Log.md (discrepancias) y Research_Log.md (auditorías generales).

## Output Contract

```markdown
## Reconciliación

### Personas Creadas
| Nombre | Archivo | Fuente | Tier |

### Personas Actualizadas
| Nombre | Campo | Antes | Después | Fuente |

### Conflictos Detectados
| Persona | Campo | Fuente A | Fuente B | Estado |

## Auditoría de Referencias Cruzadas

### Discrepancias Encontradas
| Persona | Campo | Fuente A | Fuente B | Resolución |
|---------|-------|----------|----------|------------|

### Resumen
- Totales: [n]
- Críticas: [n]
- Menores: [n]

### Archivos Modificados
- [archivos]
```
