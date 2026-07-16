# Validación de Entidades Extraídas

Esta guía se usa para **validar** los datos extraídos por los prompts estructurados (`ocr-prompts/`). NO se usa para extraer — la extracción la hace el modelo directamente en YAML.

## 1. Validación de Coherencia Interna

### Reglas temporales (bautismo)

| Regla | Descripción | Si se viola |
|-------|-------------|-------------|
| Bautismo ≤ nacimiento + 30 días | El bautismo suele ocurrir días después del nacimiento | Marcar `baptism_date` como `~` |
| Bautismo ≥ nacimiento | No se puede bautizar antes de nacer | Error grave, pedir revisión manual |
| Edad del padre ≥ 14 años | Edad mínima razonable para paternidad (históricamente) | Marcar datos como `~` |
| Edad de la madre ≥ 12 años | Edad mínima razonable para maternidad histórica | Marcar datos como `~` |

### Reglas temporales (matrimonio)

| Regla | Descripción | Si se viola |
|-------|-------------|-------------|
| Novio ≥ 14 años (histórico) | Edad mínima para matrimonio canónico histórico | Marcar como `~` |
| Novia ≥ 12 años (histórico) | Edad mínima histórica | Marcar como `~` |
| Padres del novio/novia deben ser ≥ novio+14 | Los padres deben ser al menos 14 años mayores | Marcar datos parentales como `~` |

### Reglas temporales (defunción)

| Regla | Descripción | Si se viola |
|-------|-------------|-------------|
| Defunción ≥ nacimiento | No se puede morir antes de nacer | Error grave |
| Defunción ≥ matrimonio (si casado) | La muerte ocurre después del matrimonio | Marcar fecha como `~` |
| Edad en años ≈ año_muerte − año_nacimiento | Coherencia entre edad declarada y fechas | Si difiere > 5 años, marcar |

### Reglas de nombres

| Regla | Descripción |
|-------|-------------|
| El apellido del hijo coincide con el del padre | Al menos el primer apellido del hijo = apellido del padre |
| El apellido de la madre aparece como segundo apellido en el hijo | Patrón español: hijo lleva apellido1 del padre + apellido1 de la madre |
| Los abuelos paternos comparten apellido con el padre | El abuelo paterno tiene el mismo apellido que el padre |
| Los abuelos maternos comparten apellido con la madre | La abuela materna tiene el apellido de la madre |

## 2. Validación Contra el Vault

### Coincidencia de personas

Cuando extraes un nombre, búscalo en el vault existente:

```
1. Buscar en personas/*.md por nombre exacto o parcial
2. Si existe: comparar fechas con las extraídas
   - ¿Coinciden? → validado, usar datos del vault como fuente cruzada
   - ¿Difieren? → marcar como discrepancia, no sobrescribir sin verificación
3. Si no existe: crear nueva persona
```

### Coincidencia de fuentes

```
1. Buscar en fuentes/transcripciones/ por nombre de archivo similar
2. Si ya existe transcripción para ese documento → saltar, ya procesado
3. Si no existe → continuar con guardado
```

### Matriz de verificación cruzada

| Situación | Acción |
|-----------|--------|
| Persona existe + fechas coinciden | Usar `evidence_tier: strong`, añadir fuente a `sources` |
| Persona existe + fechas NO coinciden | NO modificar. Registrar en Research_Log.md como discrepancia |
| Persona existe + fecha nueva (campo que faltaba) | Añadir campo con fuente, mantener `evidence_tier` existente |
| Persona NO existe | Crear nueva ficha con `evidence_tier` según calidad OCR |

## 3. Evaluación de Confianza por Campo

Cada campo en el YAML extraído lleva un `confidence` (0.0-1.0). Usa esta tabla para determinar la acción:

| Confidence | Acción en persona | Acción en transcripción |
|------------|-------------------|------------------------|
| ≥ 0.90 | Añadir a persona sin revisión | Marcar como `good` |
| 0.70 - 0.89 | Añadir con nota "pendiente de verificar" | Marcar como `fair` |
| 0.50 - 0.69 | NO añadir a persona, incluir en notas de transcripción | Marcar como `fair` |
| < 0.50 | NO añadir ni a persona ni a notas — pedir re-escaneo | Marcar como `poor` |

### Confianza combinada del documento

```
ocr_confidence_final = min(
    ocr_confidence,
    avg(field_confidence for each extracted field)
)
```

Si `ocr_confidence_final < 0.60` → todo el documento requiere revisión manual.

## 4. Validación de Formato de Fechas

### Formatos aceptados en YAML extraído

Las fechas DEBEN llegar en formato `YYYY-MM-DD` desde la extracción. Verificar:

| Formato esperado | Ejemplo | ¿Válido? |
|------------------|---------|----------|
| `YYYY-MM-DD` | `1920-03-15` | ✅ |
| `YYYY-??-??` | `1920-??-??` | ✅ (año conocido, día/mes no) |
| `~YYYY-MM-DD` | `~1920-03-15` | ✅ (incierto, marca `~`) |
| `YYYY-MM-DD` con año < 1000 | `0950-03-15` | ❌ (muy antiguo para registro) |
| Texto sin convertir | `quince de marzo` | ❌ (debió convertirse en prompt) |

### Conversión de números en texto a dígitos (referencia)

| Texto | Dígito |
|-------|--------|
| uno | 1 |
| dos | 2 |
| tres | 3 |
| cuatro | 4 |
| cinco | 5 |
| seis | 6 |
| siete | 7 |
| ocho | 8 |
| nueve | 9 |
| diez | 10 |
| once | 11 |
| doce | 12 |
| trece | 13 |
| catorce | 14 |
| quince | 15 |
| veinte | 20 |
| treinta | 30 |
| cuarenta | 40 |
| cincuenta | 50 |
| sesenta | 60 |
| setenta | 70 |
| ochenta | 80 |
| noventa | 90 |
| cien / ciento | 100 |
| mil | 1000 |

## 5. Referencia de Abreviaturas y Títulos

| Abreviatura | Significado | Contexto |
|-------------|-------------|----------|
| `D.` | Don | Tratamiento caballero |
| `D.ª` / `Da.` | Doña | Tratamiento señora |
| `Pbro.` | Presbítero | Sacerdote oficiante |
| `Ilmo.` / `Ilma.` | Ilustrísimo/a | Autoridad eclesiástica |
| `Excmo.` / `Excma.` | Excelentísimo/a | Alta autoridad |
| `Dr.` / `Dra.` | Doctor/a | Título académico |
| `Sr.` / `Sra.` | Señor/a | Tratamiento general |
| `Sto.` / `Sta.` | Santo/a | En nombres de iglesias/parroquias |
| `V.` | Véase | Referencia a otro folio |
| `L.` | Libro | En signaturas |
| `F.` / `Fol.` | Folio | Número de página del libro |
| `Vto.` | Visto | Aprobado |

## 6. Nombres Propios Comunes (s. XIX-XX España)

Para validación de extracción — si el modelo devuelve un nombre poco común y con baja confianza, puede ser un error de OCR.

| Masculinos comunes | Femeninos comunes |
|--------------------|-------------------|
| José, Juan, Antonio, Manuel, Francisco | María, Ana, Carmen, Francisca |
| Pedro, Luis, Miguel, Rafael, Diego | Teresa, Joaquina, Manuela, Petra |
| Santiago, Andrés, Tomás, Pablo, Ángel | Pilar, Dolores, Rosario, Concepción |
| Vicente, Ramón, Enrique, Joaquín | Josefa, Isabel, Catalina, Lucía |

### Variantes ortográficas frecuentes

| Apellido | Variantes documentadas |
|----------|----------------------|
| García | García, Garçia, Garsía |
| López | López, Lopes, Lobes |
| Martínez | Martínez, Martinez, Martines |
| Rodríguez | Rodríguez, Rodriguez, Rodrigues |
| Fernández | Fernández, Fernandez, Fernandes |
| Sánchez | Sánchez, Sanchez, Sanches |
| Pérez | Pérez, Perez, Peres |
| González | González, Gonzalez, Gonçales |

## 7. Flujo de Validación (resumen)

```
YAML extraído del prompt
       │
       ▼
┌─────────────────────────────┐
│ 1. Validar coherencia       │ ← Reglas temporales y de nombres
│    interna del documento    │
└──────────┬──────────────────┘
           │ ¿Pasa?
           │
      ┌────┴────┐
      │  SÍ     │  NO
      │         ▼
      │    ┌──────────────────┐
      │    │ Ajustar campos   │
      │    │ con baja conf    │
      │    └────────┬─────────┘
      │             │
      ▼             ▼
┌─────────────────────────────┐
│ 2. Validar contra vault     │ ← Coincidencia de personas/fuentes
└──────────┬──────────────────┘
           │
      ┌────┴────┐
      │ Nueva   │ Existente
      │ persona │     │
      │    │    │     ▼
      │    ▼    │ ┌────────────────┐
      │ ┌─────┐ │ │ ¿Coinciden     │
      │ │Crear│ │ │ fechas?        │
      │ └─────┘ │ └──┬────────┬────┘
      │    │    │  SÍ│        │NO
      │    │    │    ▼        ▼
      │    │    │ ┌────────┐ ┌─────────────┐
      │    │    │ │Añadir  │ │Registrar    │
      │    │    │ │fuente  │ │discrepancia │
      │    │    │ └────────┘ └─────────────┘
      │    │    │
      ▼    ▼    ▼
┌─────────────────────────────┐
│ 3. Evaluar confianza final  │ ← evidence_tier
│    y asignar evidence_tier  │
└──────────┬──────────────────┘
           │
           ▼
┌─────────────────────────────┐
│ 4. Guardar transcripción    │
│    y actualizar persona     │
└─────────────────────────────┘
```
