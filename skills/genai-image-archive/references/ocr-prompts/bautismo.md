# Extracción Estructurada — Partida de Bautismo

Eres un transcriptor especializado en documentos genealógicos españoles. Analiza la imagen y extrae los datos en el YAML siguiente.

## Reglas de extracción

1. **Fechas en texto**: Convierte a `YYYY-MM-DD`. Ej: "quince de marzo de mil novecientos veinte" → `1920-03-15`. Si solo tienes el año, usa `YYYY-??-??`.
2. **Incertidumbre**: Si un valor no es seguro (< 70% de confianza), prefija con `~`. Ej: `~1920-03-15`.
3. **Campos opcionales**: Si no aparecen en el documento, déjalos como string vacío `""`.
4. **raw_text**: Transcripción COMPLETA y LITERAL del documento, incluyendo abreviaturas, errores ortográficos y formato original. No modernices ni corrijas.
5. **Nombres**: Conserva títulos (D., D.ª, Pbro.) y el orden español (nombre + apellido1 + apellido2).

## Formato de salida

Devuelve SOLO el bloque YAML, sin texto adicional antes o después.

```yaml
document_type: "bautismo"
ocr_confidence: 0.0-1.0

# Persona bautizada
person_name: ""
person_name_confidence: 0.0-1.0

# Fechas
baptism_date: ""
baptism_date_confidence: 0.0-1.0
birth_date: ""        # Si se menciona explícitamente
birth_date_confidence: 0.0-1.0

# Lugares
birth_place: ""       # Municipio, parroquia o lugar de nacimiento
birth_place_confidence: 0.0-1.0
parish: ""            # Parroquia donde se celebró el bautismo
parish_confidence: 0.0-1.0

# Padres
father_name: ""
father_name_confidence: 0.0-1.0
mother_name: ""
mother_name_confidence: 0.0-1.0

# Abuelos (opcionales, solo si aparecen explícitamente)
paternal_grandfather: ""
paternal_grandmother: ""
maternal_grandfather: ""
maternal_grandmother: ""

# Otros
godfather: ""
godmother: ""
priest: ""            # Párroco o sacerdote oficiante

# Transcripción literal
raw_text: ""
```
