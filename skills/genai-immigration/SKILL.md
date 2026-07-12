---
name: genai-immigration
description: "Localizar manifiestos de pasajeros y registros de naturalización. Trigger: inmigración, manifiestos, naturalización, emigración."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando el usuario quiera:
- Buscar manifiestos de pasajeros
- Encontrar registros de naturalización
- Rastrear rutas de inmigración

## Hard Rules

1. Verificar que el pasajero coincida en nombre, edad y fecha.
2. Considerar variaciones de nombres al transliterar.
3. Usar bases de datos especializadas en inmigración.
4. Registrar el puerto de origen y destino.

## Decision Gates

| Situación | Acción |
|-----------|--------|
| Manifiesto verificado | Añadir datos de inmigración |
| Coincidencia parcial | Marcar como `moderate` |
| Sin manifiesto | Buscar en fuentes alternativas |

## Execution Steps

1. **Identificar inmigrantes**: Personas con origen extranjero.
2. **Buscar manifiestos**: Ellis Island, Castle Garden, puertos españoles.
3. **Verificar**: Nombre, edad, fecha, lugar de origen.
4. **Actualizar vault**: Datos de inmigración.

## Output Contract

```markdown
## Investigación de Inmigración

### Manifiestos Encontrados
| Persona | Buque | Fecha | Origen | Destino |
|---------|-------|-------|--------|---------|

### Naturalizaciones
| Persona | Fecha | Lugar | Referencia |
|---------|-------|-------|------------|
```
