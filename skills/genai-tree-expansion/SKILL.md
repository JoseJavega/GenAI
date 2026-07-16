---
name: genai-tree-expansion
description: "Expandir árbol genealógico con fuentes verificadas para ancestros fallecidos. Trigger: expandir árbol, buscar ancestros, añadir familia."
license: MIT
metadata:
  author: JoseJavega
  version: "1.1.0"
---

## Activation Contract

Carga este skill cuando quieras expandir el árbol, buscar ancestros, añadir ramas familiares o descubrir padres/abuelos.

## Hard Rules

1. Toda persona añadida requiere fuente. Árboles de usuarios (Geni, Ancestry) no valen sin corroboración.
2. NO modificar fechas/nombres existentes (eso es cross-reference). Sin evidencia → `Open_Questions.md`.
3. NO buscar vivos en web. Tratar como vivo a quien no tenga fecha de muerte.
4. **Privacidad condicional**: Leer `.genai-config.json`. Si `public_study: true`, aplicar `references/privacy-rules.md`.
5. **d'Aboville**: Seguir `references/daboville-algorithm.md` al añadir siblings.
6. **OBLIGATORIO**: Leer `vault-conventions.md` antes de crear archivos.

## Decision Gates

| Situación | Acción | evidence_tier |
|-----------|--------|---------------|
| Fuente primaria (certificado, registro) | Añadir a Family_Tree.md | `strong` |
| Fuente secundaria con corroboración | Añadir a Family_Tree.md | `moderate` |
| Fuente secundaria sin corroboración | Añadir a Open_Questions.md | — |
| Sin fuentes | NO añadir, registrar en Research_Log.md | — |
| Persona posiblemente viva | Marcar como `Living`, aplicar privacidad si corresponde | — |

## Execution Steps

0. **Config**: Leer `.genai-config.json` + `vault-conventions.md`.
1. **Baseline**: Leer Family_Tree.md y `personas/`. Registrar estado en Research_Log.md.
2. **Objetivos**: Nodos hoja sin padres listados.
3. **Buscar**: Transcripciones existentes → registro civil → parroquial → hemerotecas. Priorizar `genai-burial-spain` para defunciones.
4. **Evaluar**: Nombre, fechas y lugar deben alinearse.
5. **Actualizar**: Strong/moderate a Family_Tree.md. Débiles a Open_Questions.md.
6. **d'Aboville**: Seguir `references/daboville-algorithm.md`.
7. **Crear archivos**: En `personas/` con privacidad si corresponde.
8. **Registrar**: Research_Log.md con fecha, consultas y resultados.

## Output Contract

```markdown
## Expansión del Árbol

### Candidatos Revisados
| Nombre | Relación | Tier | Fuente | Acción |
|--------|----------|------|--------|--------|

### Resumen
- Strong: [n] | Moderate: [n] | Speculative: [n] | Rechazados: [n]

### Archivos Modificados / Pendiente
- [archivos y acciones restantes]
```

## References

- `references/daboville-algorithm.md` — Algoritmo de numeración d'Aboville
- `references/privacy-rules.md` — Reglas de privacidad condicionales
- `../genai/references/vault-conventions.md` — Convenciones del vault
