---
type: index
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [genealogy, family-history]
---

# Genealogía

Índice maestro de investigación familiar, ancestría genética y archivos digitalizados.

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

### Familia [Apellido]
- [[Apellido/Nombre_Persona]] — Breve biografía (n. YYYY, f. YYYY. Ocupación. Lugar.)

## Pipeline de Procesamiento

Cadena de herramientas OCR: Tesseract + ocrmypdf + ImageMagick + Claude multimodal

### Clasificación de Documentos

| Cantidad | Descripción | Método OCR |
|----------|-------------|------------|
| — | Retratos, fotos grupales, edificios | Solo catálogo |
| — | Certificados, recortes de prensa, diplomas | ocrmypdf (Tesseract) |
| — | Libros de registro, cartas, notas funerarias | Claude multimodal |
| — | Postales (frente/reverse), documentos anotados | Enfoque por capas |
