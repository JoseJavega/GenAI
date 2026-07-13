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

Cada archivo de persona sigue el formato: `SSS-HH_Nombre_Apellido.md`

| Sosa | d'Aboville | Nombre | Relación |
|------|------------|--------|----------|
| 001 | - | Luis Jávega | Yo (de cujus) |
| 002 | - | Padre de Luis | Padre |
| 003 | - | Madre de Luis | Madre |
| 004 | - | Abuelo paterno | Abuelo paterno |
| 004 | 01 | Tío paterno | Hermano del abuelo 4 |
| 004 | 01.02 | Primo paterno | Hijo del tío 4-01 |
| 005 | - | Abuela paterna | Abuela paterna |
| 006 | - | Abuelo materno | Abuelo materno |
| 006 | 01 | Tío materno | Hermano del abuelo 6 |

### Ejemplo de Estructura

```
001_Luis_Jávega.md
├── 002_Padre_de_Luis.md
│   ├── 004_Abuelo_Paterno.md
│   │   ├── 008_Bisabuelo_1.md
│   │   ├── 009_Bisabuela_1.md
│   │   ├── 004-01_Tío_Paterno.md
│   │   │   └── 004-01.02_Primo_Paterno.md
│   │   └── 004-02_Tía_Paterna.md
│   └── 005_Abuela_Paterna.md
│       ├── 010_Bisabuelo_2.md
│       └── 011_Bisabuela_2.md
└── 003_Madre_de_Luis.md
    ├── 006_Abuelo_Materno.md
    │   ├── 012_Bisabuelo_3.md
    │   ├── 013_Bisabuela_3.md
    │   └── 006-01_Tío_Materno.md
    └── 007_Abuela_Materna.md
        ├── 014_Bisabuelo_4.md
        └── 015_Bisabuela_4.md
```

## Pipeline de Procesamiento

Cadena de herramientas OCR: Tesseract + ocrmypdf + ImageMagick + Claude multimodal

### Clasificación de Documentos

| Cantidad | Descripción | Método OCR |
|----------|-------------|------------|
| — | Retratos, fotos grupales, edificios | Solo catálogo |
| — | Certificados, recortes de prensa, diplomas | ocrmypdf (Tesseract) |
| — | Libros de registro, cartas, notas funerarias | Claude multimodal |
| — | Postales (frente/reverse), documentos anotados | Enfoque por capas |
