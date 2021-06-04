title: Maps/Map Engines Performance Test

# Map Engines Performance Test

***
Epic: As a developer, I want to evaluate Leaflet boundaries for loading larger datasets and understand if tiling will be needed, i.e., for temporal sets.
***
### Description
This is an evaluation of the web mapping data management services for Gemeindescan. With growing project sizes, the mapping should not be the limiting factor. For example, temporal data can change the amount of data dramatically. Various new services for mapping popped up recently in the open-source community.

### Abstract: 
The Engine has not a big impact on performance. For the Gemeindescan use case(2D graphics), all maps can have a smooth performance on big projects. The bigger impact on performance on the map is the styling of elements, especially markers. For example, is the default Leaflet marker with its shadow a performance killing object if you put more than 1000 in a map.

### Result:
My conclusion would be that we are still working with Leaflet but are going to the current Leaflet. The current used Leaflet-GL is deprecated. It has a huge community and contains many useful extensions.


### Test results:
I build for all platforms simple stand-alone examples and included two different big projects via JSON.
* **Project 1**: Parzellen 26MB, 20446 polygons with 409236 vertexes
* **Project 2**: LowerMillion 12.99 MB, 55432 points, with 110864 vertexes

Tested with Firefox and Chromium

|  platform name | base technology | performing on project 1 | performing on project 2 |
| --- | --- | --- | :--- |
|  Leaflet | Canvas | smooth/shaky | not usable<br/>*complex Markers with shadow |
|  Mapbox JS | old Leaflet | smooth | not usable<br/>*complex Markers with shadow |
|  Mapbox GL JS | WebGL | smooth | smooth |
|  OpenLayers | Canvas & WebGL | not usable | smooth/shaky |
|  deck.gl | WebGL (React) | smooth | smooth |
|  Kepler.gl | Mapbox & deck.gl | smooth | smooth/shaky |
|  nebula.gl | deck.gl (React) | not usable | not usable |


### further properties and ratings
|  platform name | style definition | workload to integrate styled GeoJSON | workload to integrate Mapbox Vector Tiles | workload to integrate time slider | workload to integrate TopoJSON | positive | negative | minzipped size in KB |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  Leaflet | like simplestyle | little (feature translator) | null (plugin) | null (plugin) | null (plugin) | - evolutive via plugins<br/>- strong community |  | 40 |
|  Mapbox JS | simplestyle | null (plugin) |  |  |  |  | - is no longer in active development | 60 |
|  Mapbox GL JS | like simplestyle |  | null (out of the box) | null (out of the box) | bigger (extern module) |  |  | 210 |
|  OpenLayers | like simplestyle | little (feature translator) | null (out of the box) | little (with extension) | null (out of the box) |  | - shaky performance with only Mapbox Vector Layer | 150 |
|  deck.gl | like simplestyle but a lot more Layerstyles | little (feature translator) | null (out of the box) | big (no good doc) | bigger (extern module) | - incredible looking visualizations (like 2.5D graphics)<br/>- style in units: meters or pixels | - optimized for GPUs | 310 |
|  Kepler.gl |  |  | null (out of the box) | null (out of the box) | bigger (no good doc) |  | - big app with a UI to edit styles | 1400 |
|  nebula.gl |  |  | null (out of the box) | big (no good doc) | bigger (no good doc) |  | - still a beta project | 190 |


