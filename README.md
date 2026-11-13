# Mapping-Scheme-Ontology

Esquema de correspondencias entre el **esquema de etiquetado de catálogos** del proyecto
**Complexhibit** (PID2021-125037NB-I00) y la ontología **OntoExhibit**, ampliada con
**CIDOC-CRM**, que permite convertir las anotaciones en un grafo de conocimiento (RDF/Turtle).

Web del proyecto: https://complexhibit.eu · Plataforma: https://complexhibit.uma.es

## Contenido

| Fichero | Qué contiene |
| --- | --- |
| `README.md` | Esquema de correspondencias completo, con las tablas de roles y tipos y los supuestos aplicados |
| `mapeo_esquema_ontologia.csv` | La tabla de correspondencias en formato reutilizable |

El código que aplica este esquema y genera el RDF está en el repositorio
[Automatic-Tagged-System](https://github.com/Complexhibit-Project/Automatic-Tagged-System):
`v2-2026-transformer/complexhibit_rdf.py` y el cuaderno `v2-2026-transformer/cuadernos/04_exportacion_rdf.ipynb`.

## Esquema de correspondencias

OntoExhibit modela exposiciones, actores, roles, instituciones, lugares, fechas y obras. Lo que no
modela (técnica, material, dimensiones, tipo de obra y número de inventario) se expresa con
CIDOC-CRM.

| Manual de etiquetado | OntoExhibit / CIDOC-CRM |
| --- | --- |
| `exhibition` | `ox:Exhibition` · `ox:hasTitle` · `ox:hasTimeSpan` · `ox:hasVenue` · `ox:takesPlaceAt` · `ox:displays` |
| `person` + `role` | `ox:E21_Person` · `ox:hasRole` → `ox:Creator`, `ox:Lender`, `ox:Curator`, `ox:Editor`, `ox:Designer`, `ox:Member`… |
| `institution` + `type` / `role` | `ox:Institution` / `ox:Gallery` / `ox:Company` · `ox:hasOrganizer`, `ox:hasLender`, `ox:hasVenue` |
| `placeName` · `address` | `ox:Territorial_Entity` · `ox:Physical_Site` + `ox:address` |
| `date` (`when`, `from`/`to`) | `ox:Exact_Date` · `ox:Time_Range` + `ox:hasStartingDate` / `ox:hasEndingDate` |
| `catalogueItem` / `work` | `ox:Artwork` · `ox:hasCreation` → `ox:E65_Creation` (`ox:hasCreator`, `ox:hasTimeSpan`) |
| `work` `type` | `crm:P2_has_type` → `crm:E55_Type` |
| `technique` | `crm:P32_used_general_technique` → `crm:E55_Type` |
| `material` | `crm:P45_consists_of` → `crm:E57_Material` |
| `dimensions` (`height`, `width`, `depth`, `weight`, `diameter`) | `crm:P43_has_dimension` → `crm:E54_Dimension` (`P2` tipo, `P90` valor, `P91` unidad) |
| `idno` | `crm:P1_is_identified_by` → `crm:E42_Identifier` |
| catálogo | `ox:Catalog` · `crm:P67_refers_to` (cada entidad mencionada) |

### Roles de persona

| Valor de `role` en el manual | Clase de OntoExhibit |
| --- | --- |
| creator, photographer | `ox:Creator` |
| exhibitionCurator, museumCurator, curator | `ox:Curator` |
| lender | `ox:Lender` |
| owner | `ox:Owner` |
| funder | `ox:Funder` |
| assistant | `ox:Assistant` |
| editor | `ox:Editor` |
| designer | `ox:Designer` |
| author | `ox:Author` |
| trustee | `ox:Member` |

### Roles y tipos de institución

| Valor de `role` en el manual | Correspondencia |
| --- | --- |
| organiser / organizer | `ox:Organizer` (`ox:hasOrganizer`) |
| lender | `ox:Lender` (`ox:hasLender`) |
| owner, depository | `ox:Owner` |
| funder | `ox:Funder` |
| client | `ox:Commissioner` |
| host | Sede de la exposición (`ox:hasVenue`) |

| Valor de `type` en el manual | Clase de OntoExhibit |
| --- | --- |
| artGallery, gallery | `ox:Gallery` |
| company | `ox:Company` |
| museum, foundation, artCenter, academy, institute, university y demás | `ox:Institution` |

### Supuestos

- Las entidades que aparecen fuera de un bloque se vinculan a la exposición principal del catálogo.
- Si un catálogo es monográfico (un creador concentra al menos el 50 % de las menciones), ese
  creador se asigna a las obras cuya ficha no indica autor.
- En el gold standard se conservan todas las anotaciones, también las que se superponen.

## Resultado

Aplicado a los 100 catálogos del gold standard, el esquema produce un grafo de **108.996
tripletas** que describen 2.754 obras, 1.757 personas, 376 exposiciones y 527 instituciones. El
grafo se publica en Zenodo junto con las anotaciones: [DOI].

## Uso

El esquema se aplica con el sistema de extracción del proyecto, en
[Automatic-Tagged-System](https://github.com/Complexhibit-Project/Automatic-Tagged-System), que
convierte en RDF tanto el gold standard anotado en GATE como la salida del etiquetado automático de
catálogos nuevos. Allí está también el
[manual de etiquetado](https://github.com/Complexhibit-Project/Automatic-Tagged-System/tree/main/docs),
que define los elementos y atributos de la columna izquierda de las tablas.

## Desarrollo y autoría

La herramienta se ha desarrollado a partir de una metodología de etiquetado de textos diseñada específicamente en el marco del proyecto, que incluye una guía de anotación y un esquema de categorías propio. El equipo investigador ha realizado el etiquetado manual de los textos que constituyen la base de entrenamiento de la herramienta, permitiendo automatizar el etiquetado de nuevos textos conforme a los criterios establecidos. El código se ha generado con asistencia de inteligencia artificial (Claude, de Anthropic), a partir de las especificaciones y directrices proporcionadas por el equipo investigador.

- **Diseño del sistema y de su evaluación, y dirección del desarrollo:** M.ª Luisa Díez Platas (Universidad Internacional de La Rioja, UNIR).
- **Guía de anotación y esquema de categorías:** María Ortiz Tello y M.ª Luisa Díez Platas.
- **Validación:** el esquema se ha aplicado a los 100 catálogos del gold standard y el grafo resultante se ha comprobado con las cifras de obras, personas, exposiciones e instituciones indicadas arriba.

## Cómo citar

Rodríguez Ortega, N., Díez Platas, M. L. Ortiz Tello, M. (2026). *Mapping-Scheme-Ontology: correspondencias entre el esquema de etiquetado de catálogos y OntoExhibit*. Proyecto Complexhibit. https://github.com/Complexhibit-Project/Mapping-Scheme-Ontology

## Licencia

Esquema de correspondencias y documentación: CC BY 4.0.

## Financiación

Proyecto Complexhibit (PID2021-125037NB-I00), financiado por MCIN/AEI/10.13039/501100011033 y
por FEDER, Una manera de hacer Europa. Universidad Internacional de La Rioja (UNIR) y
Universidad de Málaga (UMA).
