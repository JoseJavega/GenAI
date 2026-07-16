# Patrones de Entidades en Documentos Genealógicos

## Tipos de Documentos

### Registro Civil

| Campo | Patrones comunes | Notas |
|-------|------------------|-------|
| Nombre completo | `NOMBRE APELLIDO APELLIDO` | Buscar en campos "Nombre", "Hijo de", "Casado con" |
| Fecha de nacimiento | `DD de MES de AAAA` | "el día quince de marzo de mil novecientos veinte" |
| Lugar de nacimiento | `Lugar, Municipio/Barrio` | "en la ciudad de Madrid, barrio de Malasaña" |
| Padres | `NOMBRE APELLIDO y NOMBRE APELLIDO` | "hijo legítimo de José García y María López" |
| Abuelos paternos | `NOMBRE y NOMBRE` | "nieto de Antonio García y Carmen Ruiz" |
| Testigos | `NOMBRE, ocupación` | "testigo: Juan Martínez, jornalero" |
| Oficiante | `CARGO NOMBRE` | "Antonio Ruiz, Juez Municipal" |
| Fecha de matrimonio | `DD de MES de AAAA` | En actas de matrimonio |
| Cónyuge | `NOMBRE APELLIDO` | "con María López González" |
| Hijos | Lista de nombres | "prole: José, María, Antonio" |

### Registro Parroquial (Bautismo)

| Campo | Patrones comunes | Notas |
|-------|------------------|-------|
| Nombre del bautizado | `NOMBRE APELLIDO` | "a José, hijo de..." |
| Fecha de bautismo | `DD de MES de AAAA` | Generalmente mismo día o día siguiente al nacimiento |
| Fecha de nacimiento | implícita | "nacido el día anterior" |
| Padres | `NOMBRE y NOMBRE` | "hijo legítimo de José García y María López" |
| Abuelos | `NOMBRE y NOMBRE` | "nieto de Antonio García y Carmen Ruiz" |
| Padrinos | `NOMBRE APELLIDO` | "padrino: Juan Martínez" |
| Sacerdote | `Pbro. NOMBRE` | "Pbro. Antonio Ruiz" |

### Registro Parroquial (Matrimonio)

| Campo | Patrones comunes | Notas |
|-------|------------------|-------|
| Novios | `NOMBRE y NOMBRE` | "Juan García con María López" |
| Fecha de matrimonio | `DD de MES de AAAA` | |
| Padres del novio | `NOMBRE y NOMBRE` | "hijo legítimo de José García y María López" |
| Padres de la novia | `NOMBRE y NOMBRE` | "hija legítima de Antonio López y Carmen Ruiz" |
| Testigos | `NOMBRE, ocupación` | "testigos: Pedro Sánchez, labrador" |
| Párroco | `Pbro. NOMBRE` | |

### Registro Parroquial (Defunción)

| Campo | Patrones comunes | Notas |
|-------|------------------|-------|
| Nombre del difunto | `NOMBRE APELLIDO` | "falleció José García" |
| Fecha de defunción | `DD de MES de AAAA` | |
| Fecha de sepultura | `DD de MES de AAAA` | "sepultado el día siguiente" |
| Edad | `NOMBRE` años | "a los sesenta y cinco años de edad" |
| Estado civil | `NOMBRE` | "casado con María López" |
| Cónyuge | `NOMBRE APELLIDO` | "viudo de Carmen Ruiz" |
| Padres | `NOMBRE y NOMBRE` | "hijo de Antonio García y Ana Martínez" |
| Causa de muerte | Descripción | "de accidente cerebrovascular" |

## Patrones de Fechas

### Formato textual

```regex
# Día en número o texto
(\d{1,2}|uno|dos|tres|cuatro|cinco|seis|siete|ocho|nueve|diez|...)
# Mes
de (enero|febrero|marzo|abril|mayo|junio|julio|agosto|septiembre|octubre|noviembre|diciembre)
# Año en número o texto
de (\d{4}|mil.*)
```

### Ejemplos comunes

| Texto OCR | Fecha normalizada |
|-----------|-------------------|
| `el día quince de marzo de mil novecientos veinte` | 1920-03-15 |
| `15 de Marzo de 1920` | 1920-03-15 |
| `a 15 del mes de Marzo del año 1920` | 1920-03-15 |
| `en 15 de Marzo de 1920` | 1920-03-15 |
| `el 15/03/1920` | 1920-03-15 |

## Patrones de Nombres

### Nombres propios comunes ( España s. XIX-XX)

| Masculinos | Femeninos |
|------------|-----------|
| José, Juan, Antonio, Manuel, Francisco, Pedro, Luis, Miguel, José María, Juan José | María, Ana, Carmen, Francisca, Teresa, Joaquina, Manuela, Petra, Pilar, Dolores |
| Antonio José, José Antonio, Juan Antonio | María José, María del Carmen, María de los Dolores |

### Apellidos comunes

| Apellido | Variantes frecuentes |
|----------|---------------------|
| García | García, Garçia |
| López | López, Lobes |
| Martínez | Martínez, Martinez |
| Rodríguez | Rodríguez, Rodriguez |
| Fernández | Fernández, Fernandez |
| Sánchez | Sánchez, Sanchez |
| Pérez | Pérez, Perez |
| González | González, Gonzalez |

### Abreviaturas frecuentes

| Abreviatura | Significado |
|-------------|-------------|
| `D.` / `D.ª` | Don / Doña |
| `Pbro.` | Presbítero (sacerdote) |
| `Ilmo.` / `Ilma.` | Ilustrísimo / Ilustrísima |
| `Dr.` / `Dra.` | Doctor / Doctora |
| `Sr.` / `Sra.` | Señor / Señora |
| `Sto.` / `Sta.` | Santo / Santa |

## Patrones de Relaciones

### Parentesco

```regex
# Padres
hijo(s)? (legítimo(s)?|natural(es)?|ilegítimo(s)?) de ([^y]+) y ([^.]+)
# Matrimonio
casad[oa]s? con ([^.]+)
# Viudo/a
viud[oa] de ([^.]+)
# Huérfano
huérfano de ([^.]+)
```

### Filiación completa

```regex
# Patrón estándar
(NOMBRE) (APELLIDO), hijo(s)? (legítimo(s)?) de (NOMBRE1) (APELLIDO1) y (NOMBRE2) (APELLIDO2)
# Con abuelos
nieto de (NOMBRE1) (APELLIDO1) y (NOMBRE2) (APELLIDO2)
```

## Patrones de Lugares

### Estructura típica

```
[Ciudad/Villa], [Barrio/Parroquia]
[Barrio/Parrioquia], [Ciudad/Villa]
[Parroquia], [Municipio], [Provincia]
```

### Ejemplos

| Texto | Lugar normalizado |
|-------|-------------------|
| `en la ciudad de Madrid` | Madrid |
| `en la parroquia de San Pedro, Granada` | San Pedro, Granada |
| `en el barrio de San Millán, Burgos` | San Millán, Burgos |
| `en este Registro Civil de Madrid` | Madrid |

## Patrones de Números

### Números en texto → dígitos

| Texto | Número |
|-------|--------|
| `uno` | 1 |
| `dos` | 2 |
| `diez` | 10 |
| `doce` | 12 |
| `quince` | 15 |
| `veinte` | 20 |
| `treinta` | 30 |
| `cuarenta` | 40 |
| `cincuenta` | 50 |
| `sesenta` | 60 |
| `setenta` | 70 |
| `ochenta` | 80 |
| `noventa` | 90 |
| `cien` | 100 |
| `mil` | 1000 |

### Años en texto

| Texto | Año |
|-------|-----|
| `mil novecientos veinte` | 1920 |
| `mil ochocientos noventa y cinco` | 1895 |
| `mil setecientos ochenta` | 1780 |
| `dos mil` | 2000 |

## Extracción de Entidades Clave

### Checklist por tipo de documento

#### Bautismo
- [ ] Nombre del bautizado
- [ ] Fecha de bautismo
- [ ] Fecha de nacimiento (si aparece)
- [ ] Lugar de nacimiento/bautismo
- [ ] Nombre del padre
- [ ] Nombre de la madre
- [ ] Nombre de los abuelos (si aparecen)
- [ ] Nombre del/los padrino(s)
- [ ] Nombre del sacerdote

#### Matrimonio
- [ ] Nombre del novio
- [ ] Nombre de la novia
- [ ] Fecha de matrimonio
- [ ] Lugar de matrimonio
- [ ] Padres del novio
- [ ] Padres de la novia
- [ ] Testigos
- [ ] Nombre del párroco

#### Defunción
- [ ] Nombre del difunto
- [ ] Fecha de defunción
- [ ] Fecha de sepultura
- [ ] Lugar de defunción
- [ ] Edad al fallecer
- [ ] Estado civil
- [ ] Nombre del cónyuge (si casado)
- [ ] Causa de muerte
- [ ] Padres (si aparecen)
