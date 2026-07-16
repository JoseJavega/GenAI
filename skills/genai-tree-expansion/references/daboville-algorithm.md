# Algoritmo de Numeración d'Aboville

Aplicar al añadir hermanos o hijos de hermanos al árbol.

## Pasos

1. **Identificar Sosa del padre/madre común** del nuevo sujeto.
2. **Escanear `personas/`** para encontrar todos los siblings existentes (patrón `SSS-*.md` donde SSS es el Sosa del padre).
3. **Recopilar datos**: nombre, archivo, `born` del frontmatter de cada sibling.
4. **Para cada sibling nuevo sin fecha**: Preguntar al usuario su posición relativa ("¿Es mayor, mediano o menor que [X]?").
5. **Ordenar TODOS los siblings** por fecha de nacimiento (los sin fecha se ubican según respuesta del usuario).
6. **Asignar d'Aboville**: `.01` al mayor, `.02` al siguiente, etc.
7. **Posición existente**: La persona que ya existía MANTIENE su d'Aboville actual.
8. **Reasignar si necesario**: Si un nuevo sujeto es mayor que uno existente, reasignar el existente a `.02`, `.03`, etc. (renombrar archivo si procede).
9. **Hijos de hermanos**: Aplicar la misma lógica recursivamente (ej: `004-02.01`, `004-02.02`).

## Reglas clave

- d'Aboville siempre 2 dígitos: `01`, `02`, `12`.
- Separador Sosa-d'Aboville: guión (`-`).
- Separador generaciones d'Aboville: punto (`.`).
- Sin espacios en nombre de archivo: usar guiones bajos (`_`).
