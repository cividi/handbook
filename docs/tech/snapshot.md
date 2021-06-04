title: Tech Stack/Snapshot

## Create your own Snapshot - Option 1: QGIS Plugin

The experimental QGIS Pugin "[Spatial Data Package Export](https://github.com/cividi/spatial-data-package-export)" is available via the [QGIS plugin repository](https://plugins.qgis.org/plugins/SpatialDataPackageExport/) or on [GitHub](https://github.com/cividi/spatial-data-package-export/releases/).

[Direct Download](https://github.com/cividi/spatial-data-package-export/releases/download/0.2.0/SpatialDataPackageExport.0.2.0.zip)

To install:

1. make sure to have [QGIS](https://qgis.org) 3.14 or later installed
2. Open Plugins -> Manage Plugins
3. Search for Spatial Datapackage Export
4. Click Install

Alternativly download the latest release from github.com/cividi/spatial-data-package-export/releases and manually install via zip or build from source.

## Create your own Snapshot - Option 2: Write your own exporter

Datapackage consists of metadata and data itself. For rednering a map you need to include the styling (Mapbox Simple Styles) to the exported GeoJSON.
Datapackage (Snapshot) = metadata + styled (geo)json

The full spec, a JSON schema and examples are available [here](https://github.com/cividi/spatial-data-export-spec).

####  Metadata. Here is recommended minimal schema to use:
```yaml
name: "{{ name }}"
license: "ODC-By-1.0"
licenses:
    - 
        url: "https://opendatacommons.org/licenses/by/1.0/"
        type: "ODC-By-1.0"
views:
    - 
        name: "mapview"
        specType: "gemeindescanSnapshot"
        spec:
            title: "{{ title }}"
            description: "{{ description }}"
            attribution: ""
            bounds: "{{ bounds_to_have }} "
            legend: "{{ legend }}""
        resources: ["data-layer", "mapbox-background"]
sources:
    -
        url: "https://www.openstreetmap.org/copyright"
        title: "Karte: Mapbox, © OpenStreetMap"
resources:
    - 
        name: "data-layer"
        mediatype: "application/vnd.geo+json" or "application/vnd.simplestyle-extended" see below
        data:
            name: "data"
            type: "FeatureCollection"
            features: "{{ STYLED DATA }}"
    -
        name: "mapbox-background"
        path: "mapbox://styles/gemeindescan/ckc4sha4310d21iszp8ri17u2"
        mediatype: "application/vnd.mapbox-vector-tile"
        data: 
```
In this schema you have variables in "{{ _variable_ }}", which you would rewrite to text: "{{ _variable_ }}" --> "desired variable"
{{ bounds_to_have }} - defines the frame  of the map.
                     - example: 
```python
"geo:47.310897, 8.458443", "geo:47.446897, 8.638687"
```
{{ legend }} - legend shown
             - example: 
```python
[{"label": 0, "size": "3.0", "shape": "circle", "primary": false, "fillColor": "#f30000", "fillOpacity": 0.7, "strokeColor": "#232323", "strokeWidth": 1.0, "strokeOpacity": 1.0},
{"label": 1, "size": "3.0", "shape": "circle", "primary": false, "fillColor": "#f30a00", "fillOpacity": 0.7, "strokeColor": "#232323", "strokeWidth": 1.0, "strokeOpacity": 1.0}, 
{"label": 2, "size": "3.0", "shape": "circle", "primary": false, "fillColor": "#f41400", "fillOpacity": 0.7, "strokeColor": "#232323", "strokeWidth": 1.0, "strokeOpacity": 1.0}]
```

####  Styled data

{{ STYLED DATA }} -- your data, each data point has to have style properties, depending on which type of data layer you want to add.

##### Mediatypes

A layer can be either of these three mediatypes:

- `application/geo+json` -> [Simple Style GeoJSON](https://github.com/mapbox/simplestyle-spec)
- `application/simplestyles-extended` -> Circles from points w/ a radius
- `application/vnd.mapbox-vector-tile` -> Mapbox style background layer

####  Examples

-example for a layer with points shown as circles
```python
{"type": "Feature", 
         "properties": 
             {
               "name": "2EtJFks9cv5AD1V7tOao2L", 
               "score": 13, 
               "category": "13", 
               "fill": "true", 
               "fillColor": "#f98500", 
               "fillOpacity": 0.7, 
               "stroke": "true", 
               "color": "#232323", 
               "opacity": "1.0", 
               "weight": 1.0, 
               "radius": "3.0", 
               "strokeColor": "#232323"
              }, 
              "geometry": {"type": "Point", 
                "coordinates": [8.5552764, 47.3757927]}
                }
                   
```

-example for a layer with polygons styled as [Simple Styles](https://github.com/mapbox/simplestyle-spec)
```python
{"type": "Feature", 
         "properties": {"name": "2EtJFks9cv5AD1V7tOao2L", 
               "score": 13, 
               "category": "13",  
               "fill": "#f98500", 
               "fill-opacity": 0.7, 
               "stroke": "true", 
               "color": "#232323", 
               "opacity": "1.0", 
               "weight": 1.0, 
               "radius": "3.0", 
               "strokeColor": 
               "#232323"}, 
        "geometry": {
              "type": "Polygon", "coordinates": [[[8.5552764, 47.3757927],[8.5452764, 47.3657927],[8.5552764, 47.3657927]]]}}               
```

More detailed mappings of the property names are available [here](https://hackmd.io/@n0rdlicht/HkaDNg_rL).


# Snapshot

- is a static [JSON](https://en.wikipedia.org/wiki/JSON) `document` for describing (geo)spatial data
- **mainly** to easily render maps, e.g. on the [Spatial Data Package Platform](https://github.com/cividi/spatial-data-package-platform)
- Full Specification: [Spatial Data Package Specification](https://github.com/cividi/spatial-data-package-spec) based on the [Frictionless Data Specification](https://specs.frictionlessdata.io) of [`Data Packages`](https://specs.frictionlessdata.io/data-package/) and [`Data Resources`](https://specs.frictionlessdata.io/data-resource/)
- Summary:
  - `has` 
    - a collection of `resources` (aka the data) to be rendered as a `map` or other visual representations
    - and metadata
  - For rendering a map: 
    - at least one (geo)spatial `resource` with styling information
    - referenced by a `view` object with a `view.specType` of `gemeindescanSnapshot`
    - `view.resources` should list the to be rendered resources by `resource.name` in order of display (first -> bottom, last -> top)
  - the `view.spec` should contain
    - `view.spec.bounds`: array of two geopoints (upper left and lower right corners of a rectangle) for rendering an initial viewpoint
    - `view.spec.legend`: array defining the legend entries for the map
    - `view.spec.title`: A title of the map
    - `view.spec.description`: (*optional*) a more detailed description of the map
    - the `datapackage.resource.name` referenced by `view.resources`
      - is a **[GeoJSON](https://en.wikipedia.org/wiki/GeoJSON)** object
      - is **inlined** with `resource.data` and not referenced as `resource.path`

### Example Snapshot

```json
{
  "name": "example-snapshot",
  "description": "Minimal Example Snapshot",
  "sources": [{
    "title": "Example Source",
    "url": "https://cividi.ch"
  }],
  "views": [{
    "name": "mapview",
    "resources": [
      "geojson-resource-name-1",
      "mapbox-resource-name"
    ],
    "specType": "gemeindescanSnapshot",
    "spec": {
      "title": "Snapshot Title",
      "description": "Snapshot Description",
      "bounds": [
        "geo:47.43668029143545,9.355459213256836",
        "geo:47.483104811626674,9.424123764038086"
      ],
      "legend": [{
        "shape": "square",
        "size": 0.5,
        "color": "#fff",
        "opacity": 0.2,
        "label": "Legend text",
        "primary": true
      }]
    }
  }],
  "resources": [{
    "title": "Example GeoJSON resource",
    "description": "Example GeoJSON resource",
    "mediatype": "application/geo+json",
    "name": "geojson-resource-name-1",
    "data": {
      "type": "FeatureCollection",
      "features": [{
        "type": "Feature",
        "geometry": {
          "type": "Polygon",
          "coordinates": [
            [
              [9.294214034, 47.396980325],
              [9.291692794, 47.403333351],
              [9.306849507, 47.414616277],
              [9.367107729, 47.445654958],
              [9.434770973, 47.43213103],
              [9.361936383, 47.404015763],
              [9.294214034, 47.396980325]
            ]
          ]
        },
        "properties": {
          "fid": 231,
          "fill": "#6a6a6a",
          "title": "St. Gallen",
          "stroke": "#fff",
          "fill-opacity": 0.2,
          "stroke-width": 5,
          "stroke-opacity": 1
        }
      }]
    }
  }, {
    "path": "mapbox://styles/gemeindescan/ck6qnoijj28od1is9u1wbb3vr",
    "mediatype": "application/vnd.mapbox-vector-tile",
    "name": "mapbox-resource-name"
  }]
}
```

