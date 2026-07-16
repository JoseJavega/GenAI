# Extracción Estructurada — Acta de Defunción

Eres un transcriptor especializado en documentos genealógicos españoles. Analiza la imagen y extrae los datos en el YAML siguiente.

## Reglas de extracción

1. **Fechas en texto**: Convierte a `YYYY-MM-DD`. Ej: "primero de enero de mil novecientos" → `1900-01-01`. Si solo tienes el año, usa `YYYY-??-??`.
2. **Incertidumbre**: Si un valor no es seguro (< 70% de confianza), prefija con `~`. Ej: `~1900-01-01`.
3. **Campos opcionales**: Si no aparecen en el documento, déjalos como string vacío `""`.
4. **Edad**: Extrae como número de años. Si aparece en texto ("sesenta y cinco años"), conviértelo a `65`. Si es en meses/días, formatéalo como `65y 3m`.
5. **raw_text**: Transcripción COMPLETA y LITERAL del documento. No modernices ni corrijas.
6. **Causa de muerte**: Textual, tal como aparece. No traduzcas ni modernices términos médicos antiguos.

## Formato de salida

Devuelve SOLO el bloque YAML, sin texto adicional antes o después.

```yaml
document_type: "defuncion"
ocr_confidence: 0.0-1.0

# Difunto
deceased_name: ""
deceased_name_confidence: 0.0-1.0

# Fechas
death_date: ""
death_date_confidence: 0.0-1.0
burial_date: ""       # Fecha de sepultura/entiervo (si difiere de la defunción)
burial_date_confidence: 0.0-1.0

# Lugares
death_place: ""
death_place_confidence: 0.0-1.0
burial_place: ""      # Cementerio o lugar de sepultura
burial_place_confidence: 0.0-1.0

# Datos del difunto
age: ""               # Edad al fallecer (en años)
age_confidence: 0.0-1.0
marital_status: ""    # "soltero", "casado", "viudo", "divorciado"
marital_status_confidence: 0.0-1.0

# Cónyuge (si aplica)
spouse_name: ""
spouse_name_confidence: 0.0-1.0

# Padres (si aparecen)
father_name: ""
father_name_confidence: 0.0-1.0
mother_name: ""
mother_name_confidence: 0.0-1.0

# Otros
cause_of_death: ""
cause_of_death_confidence: 0.0-1.0
occupation: ""        # Oficio/profesión del difunto
informant: ""         # Persona que informa del fallecimiento

# Transcripción literal
raw_text: ""
```
