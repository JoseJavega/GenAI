---
name: genai-burial-spain
description: "Localizar defunciones, entierros y registros de cementerio en España. Trigger: defunción, entierro, cementerio, sepultura, esquela, registro civil."
license: MIT
metadata:
  author: JoseJavega
  version: "1.0.0"
---

## Activation Contract

Carga este skill cuando el usuario quiera:
- Buscar una defunción o entierro en España
- Localizar una tumba o sepultura
- Encontrar una esquela o necrológica
- Verificar fecha de defunción
- Consultar registros del Registro Civil
- Buscar en registros parroquiales de defunciones/entierros

## Hard Rules

1. SIEMPRE empezar por fuentes primarias (Registro Civil, parroquial) antes de secundarias.
2. Citar fuente completa: nombre del archivo, fondo, signatura, URL si aplica.
3. Registrar búsquedas negativas en Research_Log.md (resultado negativo también es resultado).
4. NO confiar en datos de Find a Grave sin corroboración con otra fuente.
5. **OBLIGATORIO**: Leer `vault-conventions.md` ANTES de crear cualquier archivo en el vault.

## Pipeline de Búsqueda

Ejecutar los pasos **en orden**. Si el paso 1 da resultado, NO continuar al paso 2 (salvo que se pida exhaustividad).

### Paso 1 — Registro Civil (1870+)

**Cuándo**: Para fallecidos después de 1870 (obligatorio llevar registro civil desde 1871).

**Qué buscar**: Acta de defunción → contiene: nombre, edad, fecha y hora de defunción, causa, estado civil, padres/cónyuge, lugar de sepultura.

**Dónde buscar**:
- **FamilySearch**: `https://www.familysearch.org/search/` → colección "España, defunciones, 1600-1920" o registros específicos por provincia
- **PARES**: `https://pares.cultura.gob.es` → Búsqueda por texto + filtro de fechas
- **Solicitud directa**: Web del ayuntamiento del municipio donde falleció → sección "Certificados" o "Registro Civil"
- **Búsqueda web**: `"acta de defunción" [NOMBRE] [MUNICIPIO] [PROVINCIA]`

### Paso 2 — Registros Parroquiales (1563-1985)

**Cuándo**: Para fallecidos anteriores a 1870, o si el Registro Civil no tiene el registro.

**Qué buscar**: Libro de defunciones/entierros → contiene: nombre, edad, fecha, causa, padres/cónyuge/hijos, lugar de sepultura.

**Dónde buscar**:
- **FamilySearch**: Colecciones diocesanas (Albacete, Cartagena, Lugo, Ávila, Santander, Segovia, etc.)
  - URL: `https://www.familysearch.org/search/collection/` → buscar por diócesis
  - Colección general: "España, registros parroquiales y diocesanos, 1307-1985"
- **PARES**: Fondo de la Dirección General de los Registros Civil y Notarial
- **Archivo Diocesano**: Contacto directo con la diócesis del lugar
- **Búsqueda web**: `"libro de defunciones" OR "entierro" [NOMBRE] [PARROQUIA] [MUNICIPIO]`

### Paso 3 — Cementerios Municipales (1850+)

**Cuándo**: Cuando se conoce el municipio pero no se encuentra en registro civil/parroquial, o para localizar la tumba física.

**Qué buscar**: Ubicación de sepultura, fecha de inhumación, datos del difunto.

**Dónde buscar**:
- **Búsqueda web**: `[MUNICIPIO] cementerio municipal búsqueda difuntos`
- **Web del ayuntamiento**: Muchos municipios tienen buscadores online (Alicante, Vitoria, Sevilla, Barcelona, Mérida, Murcia...)
- **Contacto directo**: Oficina de cementerios del ayuntamiento
- **Find a Grave**: `https://www.findagrave.com` → solo como fuente secundaria, verificar siempre

**Nota**: No existe base unificada nacional de cementerios. Cada municipio gestiona sus registros de forma independiente.

### Paso 4 — Hemerotecas y Esquelas (1800+)

**Cuándo**: Cuando no se encuentra en fuentes oficiales, o como complemento para obtener más datos biográficos.

**Qué buscar**: Esquelas, necrológicas, noticias de defunción.

**Dónde buscar**:
- **Hemeroteca Digital BNE**: `https://hemerotecadigital.bne.es`
- **Biblioteca Virtual Prensa Histórica**: `https://prensahistorica.mcu.es`
- **Hemeroteca ABC**: `https://hemeroteca.abc.es` (desde 1903)
- **La Vanguardia**: `lavanguardia.com` (esquelas)
- **Búsqueda web**: `"esquela" OR "necrológica" [NOMBRE] [CIUDAD] [AÑO]`

### Paso 5 — Find a Grave (Global, secundario)

**Cuándo**: Solo si los pasos anteriores no dieron resultado, o para emigrantes españoles en USA/UK.

**Búsqueda**: `"Find a Grave" "[NOMBRE]" [AÑO_MUERTE] [LUGAR]`

**IMPORTANTE**: Find a Grave tiene cobertura muy irregular en España. NO usar como fuente primaria. Siempre corroborar con otra fuente.

## Decision Gates

| Situación | Acción |
|-----------|--------|
| Acta de defunción (Registro Civil) | Añadir con `evidence_tier: strong` |
| Libro de defunciones (parroquia) | Añadir con `evidence_tier: strong` |
| Solo cementerio municipal | Añadir con `evidence_tier: moderate`, incluir ubicación |
| Solo esquela/hemeroteca | Añadir con `evidence_tier: moderate` |
| Solo Find a Grave (sin otra fuente) | Marcar `evidence_tier: speculative`, buscar corroboración |
| Sin ningún resultado | Registrar en Research_Log.md como búsqueda negativa |

## Output Contract

```markdown
## Búsqueda de Defunción/Entierro

### Resultado
| Campo | Valor |
|-------|-------|
| Persona | [nombre] |
| Fecha defunción | [fecha] |
| Lugar defunción | [municipio, provincia] |
| Fuente | [tipo de registro + signatura/URL] |
| Tier | [strong/moderate/speculative] |
| Ubicación tumba | [cementerio, nicho] (si disponible) |

### Fuentes Consultadas
| Fuente | Resultado | Notas |
|--------|-----------|-------|
| [fuente1] | [encontrado/no encontrado] | [detalles] |

### Búsquedas Negativas
- [registro no encontrado en ...]
```

## References

- `references/spanish-genealogy-resources.md` — Archivos y recursos españoles
- `references/vault-conventions.md` — Convenciones del vault
- `references/terminology.md` — Términos genealógicos
