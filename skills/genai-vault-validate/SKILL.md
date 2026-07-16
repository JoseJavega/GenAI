---
name: genai-vault-validate
description: "Validar estructura del vault, frontmatter y privacidad. Trigger: validar vault, auditar estructura, comprobar privacidad, linter, verificar archivos."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando quieras validar la estructura del vault: archivos core, frontmatter de personas, privacidad y patrones obsoletos.

## Hard Rules

1. **Read-only**: Solo lees y reportas. No modificas nada.
2. **Reportar todo**: No te pares en el primer error. Enumera TODOS los problemas.
3. **Privacidad CONDICIONAL**: Depende de `public_study` en `.genai-config.json`. Las violaciones de privacidad solo son error si `public_study: true`.
4. **Campos obligatorios**: `life_status`, `evidence_tier`, `profile_status` son obligatorios en toda persona.
5. **Investigación autónoma**: La búsqueda de personas vivas en web NO está permitida, independientemente de `public_study`.

## Decision Gates

| Comprobación | Método | Gravedad |
|---|---|---|
| Archivos core existen | `glob <vault>/*.md` contra lista fija | error |
| Persona tiene frontmatter completo | Leer YAML, validar campos obligatorios | error |
| Persona viva expone fechas (si public_study=true) | born/died exactos (YYYY-MM-DD) en frontmatter o cuerpo | error grave |
| Persona viva expone fechas (si public_study=false) | born/died exactos (YYYY-MM-DD) en frontmatter o cuerpo | warning informativo |
| Patrones obsoletos | `confidence:` (usar `evidence_tier:`) o `tesseract` (usar `opencode-vision`) | warning |
| Duplicados por casing | Core file en minúsculas junto al original | warning |
| Config ausente | `.genai-config.json` no encontrado | warning |

## Execution Steps

0. **Cargar configuración**: Buscar `.genai-config.json` en la raíz del vault. Leer `public_study`. Si no existe, reportar warning y asumir `public_study: true` (máxima protección por defecto).
1. **Core files**: Verificar los 11 archivos raíz (`_Index`, `Family_Tree`, `Research_Log`, `Open_Questions`, `Data_Inventory`, `Timeline`, `Genetic_Profile`, `Chromosome_Painting`, `Witness_Network`, `Unresolved_Persons`, `Research_Strategy` — todos `.md`). Detectar duplicados por minúsculas.
2. **Personas**: `glob personas/*.md`, leer YAML, validar `life_status` + `evidence_tier` + `profile_status`. Si living/unknown, comprobar born/died. La gravedad depende de `public_study`.
3. **Patrones obsoletos**: Buscar `confidence:` y `tesseract` en todos los `.md`.
4. **Informe**: Compilar todo en el Output Contract.

## Output Contract

```markdown
## Validación de Vault

### Estado
✅ Válido | ❌ Problemas encontrados

### Archivos Core
| Archivo | Estado |
|---------|--------|
| _Index.md | ✅ / ❌ |

### Personas Auditadas: [N]
| Archivo | Estado | Privacidad |
|---------|--------|------------|

### Errores
- [descripción]

### Advertencias
- [patrones obsoletos, duplicados, etc.]

### Resumen
- Archivos core: N/N
- Personas con errores: N
- Violaciones de privacidad: N
- Patrones obsoletos: N
```

## References

- `../../agents/genai-analysis.md` — Sub-agente que carga este skill
- `../genai/references/vault-conventions.md` — Convenciones validadas aquí
