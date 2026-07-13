---
name: genai
description: "Agente orquestador de investigación genealógica familiar. Carga y coordina skills especializados para expandir árboles, auditar fuentes y analizar ADN."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando el usuario pida cualquier tarea de investigación genealógica:
- Expandir árbol familiar
- Auditar fuentes o referencias
- Buscar ancestros
- Analizar ADN
- Gestionar vault de Obsidian para genealogía
- Cualquier pregunta sobre genealogía

## Hard Rules

1. NUNCA fabricar personas. Toda adición requiere fuente citada.
2. **Privacidad**: Si `.genai-config.json` tiene `"public_study": true`, NO exponer datos de personas vivas (usar iniciales, fechas parciales, sin direcciones ni teléfonos). Si `public_study: false`, no aplicar esta restricción.
3. SIEMPRE usar tiers de evidencia: Strong Signal / Moderate Signal / Speculative.
4. SIEMPRE registrar resultados negativos en Research_Log.md.
5. SIEMPRE mantener coherencia entre Family_Tree.md y archivos de personas.
6. **OBLIGATORIO**: Antes de crear cualquier archivo o directorio en el vault, leer `vault-conventions.md` y seguir estrictamente las convenciones de nombres, frontmatter y estructura de carpetas.

## Decision Gates

| Situación | Acción |
|-----------|--------|
| Usuario pide expandir árbol | Cargar `genai-tree-expansion` |
| Usuario pide auditar fuentes | Cargar `genai-cross-reference` |
| Usuario pide buscar en cementerios/defunciones | Cargar `genai-burial-spain` |
| Usuario pide verificar GEDCOM | Cargar `genai-gedcom` |
| Usuario pide analizar ADN | Cargar `genai-dna-analysis` |
| Usuario pide buscar en archivos de imágenes | Cargar `genai-image-archive` |
| Usuario pide resolver preguntas | Cargar `genai-open-questions` |
| Vault no existe | Ejecutar `genai-init` primero |
| Duda sobre recursos españoles | Consultar `references/spanish-genealogy-resources.md` |
| Duda sobre terminología | Consultar `references/terminology.md` |

## Execution Steps

1. **Detectar intención**: Leer la petición del usuario y mapear al skill adecuado.
2. **Verificar vault**: Comprobar que existe el vault y tiene la estructura correcta.
3. **Cargar skill**: Leer el SKILL.md del skill correspondiente.
4. **Ejecutar**: Seguir el protocolo del skill cargado.
5. **Actualizar vault**: Modificar archivos del vault según los hallazgos.
6. **Reportar**: Devolver resultados formateados al usuario.

## Output Contract

```markdown
## [Título de la investigación]

### Resumen
[Breve descripción de lo encontrado]

### Personas Afectadas
| Nombre | Acción | Fuente |
|--------|--------|--------|

### Archivos Modificados
- [_lista de archivos]

### Próximos Pasos
- [acciones recomendadas]
```

## References

- `references/spanish-genealogy-resources.md` — Archivos y recursos españoles
- `references/terminology.md` — Glosario de términos genealógicos
- `references/vault-conventions.md` — Convenciones del vault Obsidian
