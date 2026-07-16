---
name: genai-burial-spain
description: "Localizar defunciones, entierros y registros de cementerio en España. Trigger: defunción, entierro, cementerio, sepultura, esquela, registro civil."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando quieras buscar una defunción o entierro en España: Registro Civil, registros parroquiales, cementerios, esquelas o Find a Grave como secundario.

## Hard Rules

1. **Pipeline estricto**: Ejecuta los pasos en orden. Si el paso N da resultado, no continúes al N+1 salvo que se pida exhaustividad.
2. **Fuente completa**: Cita nombre del archivo, fondo, signatura y URL.
3. **Negativos también se registran**: "Busqué X y no encontré nada" va a Research_Log.md.
4. **Find a Grave es secundario**: No confiar sin corroboración con otra fuente.
5. **OBLIGATORIO**: Leer `../genai/references/vault-conventions.md` antes de crear archivos en el vault.

## Pipeline de Búsqueda

| Paso | Fuente | Período |
|------|--------|---------|
| 1 | Registro Civil | 1870+ |
| 2 | Registros Parroquiales | 1563-1985 |
| 3 | Cementerios Municipales | 1850+ |
| 4 | Hemerotecas y Esquelas | 1800+ |
| 5 | Find a Grave (secundario) | Global |

Para cada paso, consulta `references/search-sources.md` para URLs y estrategias de búsqueda.

## Decision Gates

| Situación | evidence_tier |
|-----------|---------------|
| Acta de defunción (Registro Civil) | `strong` |
| Libro de defunciones (parroquia) | `strong` |
| Solo cementerio municipal (con ubicación) | `moderate` |
| Solo esquela/hemeroteca | `moderate` |
| Solo Find a Grave sin otra fuente | `speculative` |
| Sin ningún resultado | Research_Log.md como negativa |

## Output Contract

```markdown
## Búsqueda de Defunción/Entierro

### Resultado
| Persona | Fecha | Lugar | Fuente | Tier | Tumba |
|---------|-------|-------|--------|------|-------|

### Fuentes Consultadas
| Fuente | Resultado | Notas |
|--------|-----------|-------|

### Búsquedas Negativas
- [registro no encontrado en ...]
```

## References

- `references/search-sources.md` — URLs y estrategias detalladas por paso
- `../genai/references/vault-conventions.md` — Convenciones del vault
- `../genai/references/spanish-genealogy-resources.md` — Archivos y recursos españoles
