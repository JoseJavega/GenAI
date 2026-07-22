---
type: index
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [genealogy, family-history]
---

# Genealogía

Índice maestro de investigación familiar, ascendencia genética y archivos digitalizados.

## Sistema de Investigación

| Capa | Archivo | Descripción |
|------|---------|-------------|
| Árbol | [[Family_Tree]] | Árbol completo fusionado (todas las ramas) |
| Registro | [[Research_Log]] | Registro cronológico de acciones y hallazgos |
| Preguntas | [[Open_Questions]] | Lagunas de investigación por prioridad |
| Inventario | [[Data_Inventory]] | Todas las fuentes etiquetadas por tipo y fiabilidad |
| Línea Temporal | [[Timeline]] | Cada evento datado, ordenado cronológicamente |

## Regiones (Investigación Geográfica)

| Región | Familias | Señal Genética |
|--------|----------|----------------|
| [[Regiones/NOMBRE_REGION]] | [APELLIDO-1], [APELLIDO-2] | [porcentaje]% [categoría] |

## Apellidos (Investigación de Origen)

| Apellido | Origen | Notas |
|----------|--------|-------|
| [[Apellidos/APELLIDO]] | [País] | [Nota breve sobre origen] |

## Colecciones de Fuentes

| Colección | Archivos | Tamaño | Fuente | Estado |
|-----------|----------|--------|--------|--------|
| [Nombre Colección] | [count] | [size] | `~/Archivos/Genealogía/[ruta]/` | [Estado OCR] |

## Ubicaciones de Archivos

- **Originales (escaneados)**: `~/Archivos/Genealogía/`
- **Vault (notas, transcripciones)**: esta carpeta
- **Genómica**: (ruta de datos de ADN si aplica)

## Líneas Familiares

### [Línea Materna/Paterna]

- [[Apellido/]] — Descripción breve de esta línea familiar

### [Otra Línea]

- [[Apellido/]] — Descripción breve

## Archivos de Personas

### Convención de Nombres (Sosa-d'Aboville)

Las fichas de personas se almacenan en `personas/`. Cada archivo sigue el formato: `SSS-HH_Nombre_Apellido1_Apellido2.md`

| Sosa | d'Aboville | Nombre | Apellido(s) | Relación |
|------|------------|--------|-------------|----------|
| 001 | - | Luis | Jávega García | Yo (de cujus) |
| 002 | - | Padre de Luis | - | Padre |
| 003 | - | Madre de Luis | - | Madre |
| 004 | - | Antonio | Jávega López | Abuelo paterno |
| 004 | 01 | Tío paterno | - | Hermano del abuelo 4 |
| 004 | 01.02 | Primo paterno | - | Hijo del tío 4-01 |
| 005 | - | María | García Ruiz | Abuela paterna |
| 006 | - | Abuelo materno | - | Abuelo materno |
| 006 | 01 | Tío materno | - | Hermano del abuelo 6 |

### Ejemplo de Estructura

```
personas/
├── 001_Luis_Jávega_García.md        # De cujus (con ambos apellidos)
├── 002_Padre_de_Luis.md             # Padre (Sosa 2, descriptor)
├── 003_Madre_de_Luis.md             # Madre (Sosa 3, descriptor)
├── 004_Antonio_Jávega_López.md      # Abuelo paterno (Sosa 4, nombre real)
├── 004-01_Tío_Paterno.md            # Tío paterno (descriptor)
├── 004-01.02_Primo_Paterno.md       # Primo paterno (descriptor)
├── 005_María_García_Ruiz.md         # Abuela paterna (Sosa 5, nombre real)
├── 006_Abuelo_Materno.md            # Abuelo materno (Sosa 6, descriptor)
├── 006-01_Tío_Materno.md            # Tío materno (descriptor)
└── 006-01.01_Primo_Materno.md       # Primo materno (descriptor)
```

## Fuentes Documentales

### Estructura de `fuentes/`

```
fuentes/
├── certificados/          # PDFs de certificados
├── fotos/                 # Imágenes y retratos
└── transcripciones/       # Transcripciones .md generadas
```

Las transcripciones incluyen en frontmatter el campo `person` para vincular con la ficha correspondiente en `personas/`.

## Pipeline de Procesamiento

Cadena de herramientas OCR: OpenCode vision (único método)

### Clasificación de Documentos

| Cantidad | Descripción | Método OCR |
|----------|-------------|------------|
| — | Retratos, fotos grupales, edificios | Solo catálogo |
| — | Certificados, recortes de prensa, diplomas | OpenCode vision |
| — | Libros de registro, cartas, notas funerarias | OpenCode vision |
| — | Postales (frente/reverse), documentos anotados | Enfoque por capas |
