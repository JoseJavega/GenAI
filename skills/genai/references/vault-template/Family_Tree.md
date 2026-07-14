---
type: reference
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [genealogy, family-tree]
---

# Árbol Genealógico

Árbol familiar completo fusionado. Todas las fechas están sourced; fechas sin fuente se marcan con `(sin verificar)`. Las personas vivas o posiblemente vivas deben marcarse como `Living` y no deben incluir fechas exactas de nacimiento ni datos de contacto.

## Línea Directa (Numeración Sosa-d'Aboville)

```
001 [TU_NOMBRE] (Living)
├── 002 [PADRE] (n. YYYY, f. YYYY)
│   ├── 004 [ABUELO_PAT] (n. YYYY, f. YYYY)
│   │   ├── 008 [BISABUELO_1] (n. YYYY, f. YYYY)
│   │   ├── 009 [BISABUELA_1] (n. YYYY, f. YYYY)
│   │   ├── 004-01 [TÍO_PAT] (n. YYYY, f. YYYY)
│   │   │   └── 004-01.02 [PRIMO_PAT] (n. YYYY)
│   │   └── 004-02 [TÍA_PAT] (n. YYYY)
│   └── 005 [ABUELA_PAT] (n. YYYY, f. YYYY)
│       ├── 010 [BISABUELO_2] (n. YYYY, f. YYYY)
│       └── 011 [BISABUELA_2] (n. YYYY, f. YYYY)
└── 003 [MADRE] (n. YYYY, f. YYYY)
    ├── 006 [ABUELO_MAT] (n. YYYY, f. YYYY)
    │   ├── 012 [BISABUELO_3] (n. YYYY, f. YYYY)
    │   ├── 013 [BISABUELA_3] (n. YYYY, f. YYYY)
    │   └── 006-01 [TÍO_MAT] (n. YYYY, f. YYYY)
    └── 007 [ABUELA_MAT] (n. YYYY, f. YYYY)
        ├── 014 [BISABUELO_4] (n. YYYY, f. YYYY)
        └── 015 [BISABUELA_4] (n. YYYY, f. YYYY)
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

**[[004_Abuelo_Paterno]]** (n. [FECHA], [LUGAR]; f. [FECHA], [LUGAR])
- Casó con [[005_Abuela_Paterna]] el [FECHA] en [LUGAR]
- Hijos: [[004-01_Tío_Paterno]] (YYYY), [[004-02_Tía_Paterna]] (YYYY)
- Ocupación: [ocupación]
- Fuentes: [[004_Abuelo_Paterno]], Find a Grave #[número_memorial]

### Familia [APELLIDO-2]

**[[006_Abuelo_Materno]]** (n. [FECHA], [LUGAR]; f. [FECHA], [LUGAR])
- Padres: [[012_Bisabuelo_3]] y [[013_Bisabuela_3]]
- Emigró de [PAÍS] a [DESTINO] en [AÑO]
- Fuentes: [[archivo_transcripción]], [[006_Abuelo_Materno]]

## Discrepancias de Datos

| Persona | Campo | Fuente A | Fuente B | Resolución |
|---------|-------|----------|----------|------------|
| [ANCESTOR] | fecha_nacimiento | YYYY (Captura árbol) | MM/DD/YYYY (Certificado) | Certificado es autoritativo |

## Orígenes Geográficos

| Región | Líneas Familiares | Período |
|--------|-------------------|---------|
| [País/Región] | [APELLIDO-1], [APELLIDO-2] | 1800s a 1900s |
