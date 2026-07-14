---
name: genai-cross-reference
description: "Auditar discrepancias entre el árbol genealógico y documentos fuente. Trigger: auditar fuentes, verificar datos, cruzar referencias."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando el usuario quiera:
- Auditar la coherencia del árbol
- Verificar fechas entre documentos
- Encontrar y corregir discrepancias
- Validar datos contra fuentes primarias

## Hard Rules

1. NUNCA modificar datos sin al menos dos fuentes que respalden el cambio.
2. SIEMPRE registrar cada discrepancia encontrada.
3. SIEMPRE reportar la fuente más confiable para cada campo.
4. NO asumir qué fuente es correcta sin evidencia.
5. Mantener registro de todas las auditorías en Research_Log.md.
6. **Ubicaciones**: Fichas en `personas/`, fuentes en `fuentes/`, transcripciones en `fuentes/transcripciones/`.

## Decision Gates

| Situación | Acción |
|-----------|--------|
| Dos fuentes primarias discrepan | Investigar más, no cambiar aún |
| Fuente primaria vs secundaria | Privilegiar primaria |
| Certificado oficial vs árbol | Privilegiar certificado |
| Sin resolución posible | Marcar como `unresolved` en Family_Tree.md |

## Execution Steps

1. **Leer fichas de personas**: `glob personas/*.md` para extraer todas las personas con datos.
2. **Leer transcripciones**: `glob fuentes/transcripciones/*.md` para obtener fuentes documentales.
3. **Cruzar por persona**: Usar campo `person` del frontmatter de cada transcripción para vincular con la ficha correspondiente.
4. **Identificar discrepancias**: Fechas, nombres, lugares que no coinciden entre ficha y transcripción.
5. **Clasificar**: Por severidad (crítica, menor, cosmética).
6. **Proponer resoluciones**: Con fuente citada.
7. **Actualizar vault**: Aplicar cambios aprobados en `personas/`.
8. **Registrar**: En Research_Log.md y Family_Tree.md (sección Data Discrepancies).

## Output Contract

```markdown
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
