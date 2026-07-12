---
name: genai-dna-analysis
description: "Analizar datos de ADN por cromosoma para mapear segmentos genéticos. Trigger: ADN, cromosomas, segmentos, genética."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando el usuario quiera:
- Analizar resultados de ADN
- Mapear segmentos por cromosoma
- Identificar coincidencias genéticas
- Interpretar estimaciones étnicas

## Hard Rules

1. Interpretar CM (centimorgans) correctamente.
2. Usar tablas deRelationship para estimar parentesco.
3. Considerar que el ADN se hereda de forma aleatoria.
4. Nunca afirmar parentesco sin suficientes CM.

## Decision Gates

| CM Compartidos | Parentesco Estimado |
|----------------|---------------------|
| 680-820 | Tío/a |
| 340-410 | Primo hermano |
| 170-205 | Primo segundo |
| 68-85 | Primo tercero |

## Execution Steps

1. **Leer Genetic_Profile.md**: Datos actuales.
2. **Analizar coincidencias**: CM compartidos con cada match.
3. **Mapear cromosomas**: Segmentos por cromosoma.
4. **Identificar líneas**: A qué rama familiar corresponde cada match.
5. **Actualizar vault**.

## Output Contract

```markdown
## Análisis de ADN

### Coincidencias Significativas
| Match | CM | Parentesco Estimado | Línea |
|-------|----|--------------------|-------|

### Segmentos por Cromosoma
| Cromosoma | Segmento | CM | Ancestro |
|-----------|----------|----|----------|
```
