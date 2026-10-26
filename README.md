# Atlas ZBE · datos

Dataset abierto de las **Zonas de Bajas Emisiones (ZBE) de España**: dónde están,
qué restringen hoy, a quién, desde cuándo, cuánto es la multa, qué excepciones
hay y qué pasa con los vehículos extranjeros e históricos. Cada dato lleva su
fuente oficial y la fecha en que se comprobó.

- Datos al día: <https://datos.atlaszbe.es/v1/> (se actualizan cada mañana).
- Este repositorio es una copia de lo publicado: cada cambio en los datos es un
  commit, así que el historial de git es el historial del dataset.
- Web: <https://atlaszbe.es>

> Los datos se cuidan, pero no son asesoramiento legal. Antes de entrar en una
> ZBE, comprueba la fuente oficial que se cita en cada zona.

## Ficheros

| Fichero | Qué tiene |
|---|---|
| `data/zonas.json` | Zonas con su estado para quien conduce, fechas clave, confianza, reglas y capa editorial (sin preguntas frecuentes), y la geometría simplificada (~5 m). Pensado para apps y webs. |
| `data/zonas.geojson` | Las mismas zonas en GeoJSON, con la geometría completa, el texto de la fuente, las correcciones aplicadas con sus fuentes y la capa editorial entera. |
| `data/municipios.json` | Municipios del inventario del MITECO con su estado (ZBE vigente, en trámite o pendiente) y sus zonas. |
| `data/zonas.csv`, `data/municipios.csv` | Lo mismo en tablas, para hojas de cálculo (UTF-8 con BOM). |
| `data/cambios.json` | Cambios de los últimos 90 días: altas, bajas y cambios de reglas, perímetro, estado o capa editorial. |
| `data/osm-zonas.geojson` | **Pistas sin verificar**: ZBE que voluntarios han dibujado en OpenStreetMap y que ninguna fuente oficial publica (ver abajo). |
| `data/meta.json` | Versión de los datos, fecha, recuentos y estado de cada fuente. |
| `schema/*.schema.json` | JSON Schema (draft 2020-12) de cada fichero JSON y GeoJSON. |

`v1` solo gana campos: no se quitan ni cambian de significado. Los esquemas no
cierran los objetos, así que un campo nuevo no rompe a quien valide con ellos.

### Sobre `osm-zonas.geojson`

No forma parte de los datos verificados. Son perímetros que alguien ha dibujado
en OpenStreetMap (etiqueta `boundary=low_emission_zone`) en municipios para los
que ninguna fuente oficial publica todavía la ZBE. Pueden estar desactualizados,
ser zonas solo previstas o no coincidir con la ordenanza, y no llevan reglas,
estado ni ficha: solo el perímetro aproximado y las etiquetas de OpenStreetMap
tal cual. Sirven como pista. Cuando una se confirma con fuentes oficiales, pasa a
`zonas.json` como zona propia y sale de este fichero. © colaboradores de
OpenStreetMap, ODbL.

## Lo esencial de cada zona

- `estadoSancion`, el estado para quien conduce hoy:

  | Valor | Significa |
  |---|---|
  | `sanciona` | Hay restricciones en vigor y se multa. |
  | `aviso` | Hay restricciones, pero en periodo de aviso: no se multa. |
  | `aprobada_sin_aplicar` | Aprobada, pero las restricciones empiezan más adelante. |
  | `solo_episodios` | Solo restringe durante episodios de contaminación. |
  | `suspendida` | Anulada o suspendida (judicialmente o por el ayuntamiento). |
  | `sin_restricciones` | La zona existe pero no restringe el acceso. |
  | `sin_verificar` | No está confirmado. |

- `fechas`: inicio de las multas, fin del periodo de aviso y próximo cambio.
- `confianza`: `alta` (confirmado en fuentes oficiales), `media` (alguna fecha o
  detalle solo por prensa, o fuentes oficiales que se contradicen) o `baja` (sin
  confirmar).
- `reglas`: cada regla dice el acceso (`prohibido`, `solo_autorizados`…), las
  etiquetas DGT afectadas, a quién (`todos`, `no_domiciliados`…), qué vehículos,
  si rige siempre o solo en episodios, si se multa, desde y hasta cuándo, y el
  horario.
- `editorial`: resumen, calendario, multa, excepciones y dónde se piden,
  vehículos extranjeros, vehículos históricos, vías excluidas, episodios,
  estado judicial y preguntas frecuentes. Cada bloque lleva sus fuentes y su
  fecha de verificación; lo que lleva más de 90 días sin comprobarse se revisa.

Cada municipio es una zona propia, aunque comparta registro con otros (por
ejemplo, las ZBE del área metropolitana de Barcelona, que se indican en el
bloque `ambito`).

## Fuentes

El punto de partida es el dataset de ZBE de la DGT (Punto de Acceso Nacional,
CC BY 4.0), el inventario del MITECO y servicios municipales de datos abiertos.
Todo se contrasta con las ordenanzas y las webs municipales y se corrige donde
hace falta, siempre con fuente oficial. La prensa solo se usa para fechas y
estado judicial, y se marca como tal. La lista completa de fuentes y licencias
está en `ATRIBUCION.md` y en `data/meta.json`.

## Licencia y cita

**Open Database License (ODbL) 1.0** (`LICENSE`); los contenidos, bajo la
Database Contents License (DbCL) 1.0 (`LICENSE-DbCL`). Puedes usar, copiar y modificar los datos, también con fines
comerciales, siempre que:

- cites a «Atlas ZBE» y a las fuentes (`ATRIBUCION.md`);
- publiques bajo la misma licencia las bases de datos derivadas que hagas
  públicas.

Para citarlo, usa `CITATION.cff` (GitHub muestra «Cite this repository»).

## Errores y sugerencias

Si ves un dato mal o desactualizado, abre un issue en este repositorio con la
zona y, si puedes, la fuente oficial que lo corrige.

---

## English

Open dataset of Spain's **Low Emission Zones (ZBE)**: perimeters, current
restrictions, who they apply to and since when, fines, exemptions, rules for
foreign and historic vehicles, and the official source and verification date
of every item. Updated daily at <https://datos.atlaszbe.es/v1/>; this
repository mirrors it, so the git history is the dataset history. Licensed
under the **ODbL 1.0** (contents under DbCL 1.0): credit «Atlas ZBE» and the
sources listed in `ATRIBUCION.md`, and share derived databases alike. Field names and
texts are in Spanish; JSON Schemas are in `schema/`. Report errors through
GitHub issues.
