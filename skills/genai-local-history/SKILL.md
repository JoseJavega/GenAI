---
name: genai-local-history
description: "Extraer datos de libros de historia local y archivos regionales. Trigger: historia local, libros antiguos, archivos regionales."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando el usuario quiera:
- Buscar información en libros de historia local
- Extraer datos de archivos regionales
- Encontrar menciones a familiares en publicaciones históricas

## Hard Rules

1. Verificar que las menciones correspondan a la familia correcta.
2. Contextualizar la información histórica.
3. Citar la fuente completa.
4. Distinguir entre hechos históricos y datos genealógicos.

## Decision Gates

| Situación | Acción |
|-----------|--------|
| Mención clara a familiar | Añadir a Family_Tree.md con fuente |
| Mención posible | Añadir a Open_Questions.md |
| Sin mención | Registrar en Research_Log.md |

## Execution Steps

1. **Identificar regiones**: Lugares donde vivió la familia.
2. **Buscar publicaciones**: Libros de historia local, monografías.
3. **Escanear/buscar**: Menciones a apellidos o personas.
4. **Verificar contexto**: Asegurar que la mención es relevante.
5. **Extraer datos**: Fechas, nombres, eventos.
6. **Actualizar vault**.

## Output Contract

```markdown
## Investigación de Historia Local

### Fuentes Consultadas
| Fuente | Tipo | Enlace |
|--------|------|--------|

### Menciones Encontradas
| Fuente | Mención | Relevancia | Acción |
|--------|---------|------------|--------|
```
