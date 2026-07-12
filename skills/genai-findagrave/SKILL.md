---
name: genai-findagrave
description: "Localizar memoriales en Find a Grave y cementerios equivalentes. Trigger: buscar en cementerios, Find a Grave, memoriales."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando el usuario quiera:
- Buscar memoriales en Find a Grave
- Localizar cementerios
- Verificar fechas de defunción y sepultura
- Obtener información de epitafios

## Hard Rules

1. Verificar que el memorial corresponda a la persona correcta (nombre + fechas + lugar).
2. NO confiar en árboles vinculados sin corroboración.
3. Extraer solo datos respaldados por el memorial.
4. Registrar si no se encuentra memorial.

## Decision Gates

| Situación | Acción |
|-----------|--------|
| Memorial coincide完全amente | Añadir enlace a Family_Tree.md |
| Memorial parcialmente coincide | Marcar como `moderate` |
| Sin memorial | Registrar en Research_Log.md como resultado negativo |

## Execution Steps

1. **Identificar objetivos**: Personas fallecidas en Family_Tree.md sin enlace a cementerio.
2. **Buscar en Find a Grave**: `"Find a Grave" "[NOMBRE]" [AÑO_MUERTE] [LUGAR]`
3. **Validar memorial**: Verificar nombre, fechas, lugar de sepultura.
4. **Extraer datos**: Fecha de nacimiento, defunción, sepultura, epitafio.
5. **Actualizar vault**: Añadir enlace y datos verificados.
6. **Registrar**: Búsqueda y resultado.

## Output Contract

```markdown
## Búsqueda en Cementerios

### Memoriales Encontrados
| Persona | Memorial | Fuente | Datos Extraídos |
|---------|----------|--------|-----------------|

### Sin Memorial
| Persona | Búsqueda Realizada | Resultado |
|---------|-------------------|-----------|
```
