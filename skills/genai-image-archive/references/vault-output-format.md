# Formato de Salida al Vault

## Estructura de Archivo de Transcripción

### Frontmatter estándar

```yaml
---
type: transcription
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [genealogy, document, transcription]
source: "fuentes/certificados/NOMBRE_ARCHIVO.pdf"
source_file: "fuentes/certificados/NOMBRE_ARCHIVO.pdf"
document_type: "bautismo"          # bautismo | matrimonio | defunción | otro
ocr_method: "opencode-vision"      # opencode-vision | manual
ocr_confidence: 0.95               # 0.0 - 1.0
ocr_quality: "good"                # good | fair | poor
person: "001_Nombre_Apellido"      # Nombre del archivo en personas/
sosa: 1                            # Número Sosa (si aplica)
place: "Madrid"
date: "1920-03-15"
---
```

### Cuerpo del archivo

```markdown
# Transcripción - [Tipo de Documento]

## Datos del Documento

| Campo | Valor |
|-------|-------|
| **Tipo** | Bautismo |
| **Fecha** | 15 de marzo de 1920 |
| **Lugar** | Parroquia de San Pedro, Madrid |
| **Referencia** | Registro Civil de Madrid, tomo 5, folio 123 |

## Transcripción Original

> [TEXTO COMPLETO OCR]
>
> "En la ciudad de Madrid, a los quince días del mes de marzo de mil novecientos veinte, yo, Antonio Ruiz, Presbítero, bauticé a José, hijo legítimo de José García López y María López Martínez, nieto de Antonio García Sánchez y Carmen Ruiz Martínez por parte paterna, y de Juan López Pérez y Ana Martínez Ruiz por parte materna. Padrino: Juan Martínez Sánchez. Doy fe."

## Entidades Extraídas

### Persona Principal
- **Nombre**: José García López
- **Fecha de nacimiento**: 15 de marzo de 1920 (inferred from bautismo)
- **Lugar de nacimiento**: Madrid
- **Tipo de registro**: Bautismo

### Padres
- **Padre**: José García López
- **Madre**: María López Martínez

### Abuelos
- **Abuelo paterno**: Antonio García Sánchez
- **Abuela paterna**: Carmen Ruiz Martínez
- **Abuelo materno**: Juan López Pérez
- **Abuela materna**: Ana Martínez Ruiz

### Otros
- **Padrino**: Juan Martínez Sánchez
- **Oficiante**: Antonio Ruiz, Presbítero

## Notas

- OCR con confianza 0.95
- Documento en buen estado de conservación
- Fecha confirmada por patrón de escritura
```

## Formato de Archivo de Persona (actualización)

Cuando se procesa un documento, se actualiza el archivo de persona correspondiente:

```yaml
---
type: person
cr_type: person
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [genealogy, person]
name: "José García López"
sosa: 001
born: "1920-03-15"
birth_place: "Madrid"
life_status: "deceased"
family: "García"
evidence_tier: "strong"
profile_status: "complete"
sources:
  - "fuentes/transcripciones/bautismo_1920.md"
  - "fuentes/certificados/bautismo_1920.pdf"
---
```

## Convenciones de Nombre de Archivo

### Transcripciones

```
fuentes/transcripciones/{tipo}_{año}_{secuencia}.md
```

| Tipo | Ejemplo |
|------|---------|
| bautismo | `bautismo_1920.md` |
| matrimonio | `matrimonio_1945.md` |
| defunción | `defuncion_1980.md` |
| múltiples del mismo año | `bautismo_1920_01.md`, `bautismo_1920_02.md` |

### Originales (certificados)

```
fuentes/certificados/{tipo}_{año}.{ext}
```

| Tipo | Ejemplo |
|------|---------|
| bautismo | `bautismo_1920.pdf` |
| matrimonio | `matrimonio_1945.jpg` |
| defunción | `defuncion_1980.tiff` |

## Reglas de Vinculación

### Persona ↔ Fuente

Cada transcripción en `fuentes/transcripciones/` DEBE incluir:

```yaml
person: "001_Nombre_Apellido"    # Nombre exacto del archivo en personas/
sosa: 1                          # Número Sosa
source_file: "fuentes/certificados/bautismo_1920.pdf"
```

### Wikilinks en transcripciones

```markdown
Ver también: [[004_Abuelo_Paterno]], [[006_Abuela_Materna]]
 Padres: [[002_Padre]] y [[003_Madre]]
```

## Calidad de OCR

### Campos de calidad

| Campo | Valores | Significado |
|-------|---------|-------------|
| `ocr_confidence` | 0.0 - 1.0 | Confianza promedio del OCR |
| `ocr_quality` | `good` | confidence ≥ 0.85 |
| `ocr_quality` | `fair` | confidence 0.60 - 0.84 |
| `ocr_quality` | `poor` | confidence < 0.60 |

### Acciones por calidad

| Calidad | Acción |
|---------|--------|
| `good` | Transcripción directa, revisión mínima |
| `fair` | Requiere revisión manual de entidades |
| `poor` | Requiere re-escaneo o transcripción manual completa |

## Flujo de Salida

```
1. Ejecutar OCR en imagen (OpenCode vision)
2. Guardar imagen original en fuentes/certificados/
3. Crear transcripción en fuentes/transcripciones/
4. Actualizar archivo de persona en personas/
5. Añadir referencia a Research_Log.md
```

### Research_Log.md

Añadir entrada al registro de investigación:

```markdown
## 2024-03-15 - Procesamiento OCR

- **Documento**: bautismo_1920.pdf
- **Método**: OpenCode vision
- **Confianza**: 0.95
- **Resultado**: Transcripción completa, entidades extraídas
- **Archivos**:
  - Original: `fuentes/certificados/bautismo_1920.pdf`
  - Transcripción: `fuentes/transcripciones/bautismo_1920.md`
  - Persona: `personas/001_José_García_López.md`
```
