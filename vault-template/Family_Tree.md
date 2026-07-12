---
type: reference
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [genealogy, family-tree]
---

# Árbol Genealógico

Árbol familiar completo fusionado. Todas las fechas están sourced; fechas sin fuente se marcan con `(sin verificar)`. Las personas vivas o posiblemente vivas deben marcarse como `Living` y no deben incluir fechas exactas de nacimiento ni datos de contacto.

## Línea Directa

```
TÚ (Living)
├── [PADRE-1] (n. YYYY, f. YYYY)
│   ├── [ABUELO-1] (n. YYYY, f. YYYY)
│   │   ├── [BISABUELO-1] (n. YYYY, f. YYYY)
│   │   └── [BISABUELO-2] (n. YYYY, f. YYYY)
│   └── [ABUELO-2] (n. YYYY, f. YYYY)
│       ├── [BISABUELO-3] (n. YYYY, f. YYYY)
│       └── [BISABUELO-4] (n. YYYY, f. YYYY)
└── [PADRE-2] (n. YYYY, f. YYYY)
    ├── [ABUELO-3] (n. YYYY, f. YYYY)
    └── [ABUELO-4] (n. YYYY, f. YYYY)
```

## Sharding e Índice de Archivos

Un solo `Family_Tree.md` está bien para unas pocas personas. A medida que el árbol crece, mantén este archivo en el núcleo reciente (ej. Generaciones 1–N, más este índice) y divide las ramas más antiguas o por región en archivos shard `Family_Tree_<Región>_<Rama>.md` — un rango de generaciones contiguo por shard, agrupado por geografía o linaje. Elimina esta sección si tu árbol es pequeño.

La tabla a continuación es tanto navegación humana como manifiesto legible por máquina. La columna **Región** permite agrupar personas por linaje.

| Archivo | Región | Contenido |
|---------|--------|-----------|
| [[Family_Tree_Paternal_Coastal]] | Costa | [APELLIDO-1], [APELLIDO-2] ([lugar]; Gen 6–9) |
| [[Family_Tree_Maternal_Highland]] | Montaña | [APELLIDO-3] ([lugar]; Gen 6–10) |
| [[Family_Tree_Colonial_North]] | Colonial | [APELLIDO-4], [APELLIDO-5] ([lugar]; Gen 8–14) |

## Línea [Paterna/Materna]

### Familia [APELLIDO-1]

**[ANCESTRO-1]** (n. [FECHA], [LUGAR]; f. [FECHA], [LUGAR])
- Casó con [CÓNYUGE] el [FECHA] en [LUGAR]
- Hijos: [HIJO-1] (YYYY), [HIJO-2] (YYYY)
- Ocupación: [ocupación]
- Fuentes: [[Apellido/Archivo_Persona]], Find a Grave #[número_memorial]

### Familia [APELLIDO-2]

**[ANCESTRO-2]** (n. [FECHA], [LUGAR]; f. [FECHA], [LUGAR])
- Padres: [PADRE] y [MADRE]
- Emigró de [PAÍS] a [DESTINO] en [AÑO]
- Fuentes: [[archivo_transcripción]], [[archivo_persona]]

## Discrepancias de Datos

| Persona | Campo | Fuente A | Fuente B | Resolución |
|---------|-------|----------|----------|------------|
| [ANCESTOR] | fecha_nacimiento | YYYY (Captura árbol) | MM/DD/YYYY (Certificado) | Certificado es autoritativo |

## Orígenes Geográficos

| Región | Líneas Familiares | Período |
|--------|-------------------|---------|
| [País/Región] | [APELLIDO-1], [APELLIDO-2] | 1800s a 1900s |
