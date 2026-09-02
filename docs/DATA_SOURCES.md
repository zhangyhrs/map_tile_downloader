# Data sources, resolution & dates

**English | [简体中文](DATA_SOURCES_ZH.md)** · [Home](../README.md)

This catalogue describes the author-supplied implementation and configuration, not a live availability test. The published download remains v2.5.1; the separately supplied source filename says v2.5.2. Their equivalence has not been verified. Source code, private URLs and credentials are not published.

## Read this before choosing data

- **Zoom/pixel spacing is not native image resolution or positional accuracy.** Upsampling cannot recover details absent from the source.
- **Acquisition date, mosaic release date and product reference year are different.** Record the scene/product metadata, not just the date selected in the interface.
- **Important export limitation:** the inspected satellite export path limits the long edge to 4096 pixels and stretches each band to 8-bit using its 2nd–98th percentiles. These GeoTIFFs are visualization outputs, not preserved scientific values. Do not use them directly for quantitative reflectance indices, temperature or elevation analysis. Copernicus DEM through this satellite path has the same limitation; the separate AW3D30 download workflow is different.

## 1. Basemap tiles

The following zoom ceilings are **current configuration values**, not service guarantees. Coverage, detail and access can change.

| Configured source | Content | Configured maximum zoom |
|---|---|---:|
| Amap / 高德 | Street map, satellite, satellite labels | 18 |
| Esri | World Imagery, street map, topographic | 19 |
| Esri National Geographic | Rendered thematic basemap; legacy service needs validation | 16 |
| Google | Satellite, hybrid, street map, terrain | 20 |
| Tianditu / 天地图 | Imagery, rendered vector map, imagery labels | 18 |
| Tianditu terrain | Rendered terrain map | 14 |
| OpenStreetMap | Rendered street-map tiles | 19 |
| OpenTopoMap | Rendered topographic map | 17 |
| CyclOSM | Rendered cycling map | 19 |
| Carto | Light, dark, Voyager | 19 |
| Tencent / 腾讯 | Satellite | 18 |
| Custom URL | User-authorized service | Default 18; source-dependent |

“Vector map” in a tile menu means a **raster-rendered map**, not downloadable vector features. Amap/Tencent entries use GCJ-02 handling; approximate conversion must not be treated as survey-grade correction. Tianditu may require a valid key.

Ordinary basemap mosaics do not have one guaranteed capture date or uniform ground resolution. Adjacent areas can use different imagery. A high zoom option does not guarantee high-resolution coverage. Provider attribution, download/offline-use rules, quotas and licenses still apply; an entry in the application is not permission for bulk downloading.

### What zoom means

For standard 256-pixel Web Mercator tiles, approximate ground pixel spacing is
`156543.033928 × cos(latitude) / 2^zoom` metres/pixel. This is a tile-grid calculation, not a sensor-resolution claim. [Microsoft tile-grid documentation](https://learn.microsoft.com/en-us/azure/azure-maps/zoom-levels-and-tile-grid)

| Zoom | Equator, approximate m/pixel | At 30° latitude, approximate m/pixel |
|---:|---:|---:|
| 14 | 9.55 | 8.27 |
| 15 | 4.78 | 4.14 |
| 16 | 2.39 | 2.07 |
| 17 | 1.19 | 1.03 |
| 18 | 0.60 | 0.52 |
| 19 | 0.30 | 0.26 |
| 20 | 0.15 | 0.13 |

Reprojection/resampling changes output pixel spacing. In EPSG:4326 the raster grid is expressed in degrees, not directly metres.

## 2. Historical imagery & time-series GIF

Esri Wayback archives World Imagery releases from February 2014 onward. A selected date identifies a **basemap release**, not necessarily a new local observation. Some releases may show identical imagery in your area. [Esri Wayback introduction](https://www.esri.com/arcgis-blog/products/arcgis-living-atlas/mapping/use-world-imagery-wayback)

For actual capture date, source and resolution, inspect the location-specific imagery metadata where available; the date slider alone is insufficient. [Esri imagery metadata](https://www.esri.com/arcgis-blog/products/arcgis-living-atlas/imagery/wayback-with-world-imagery-metadata)

The GIF module combines local GeoTIFFs. Frame dates/intervals do not establish satellite revisit frequency. Compare co-registered images with consistent extent, scale and valid acquisition dates; seasonal differences, clouds and mosaic changes can resemble land-use change.

## 3. Satellite scenes & DSM in the satellite tab

These are **native product characteristics**, not guaranteed exported file resolution. Apply the 4096-pixel/8-bit export warning above.

| Dataset | Native resolution / bands | Time meaning and practical use |
|---|---|---|
| Sentinel-2 L2A | MSI bands span 10/20/60 m; configured RGB B04/B03/B02 and B08 are 10 m; B05 and B12 are 20 m | Mission began in 2015; nominal two-satellite revisit is 5 days, not a cloud-free guarantee. Query actual L2A catalogue coverage and scene date. Vegetation and land-surface visualization |
| Landsat 8 Collection 2 Level-2 | Multispectral 30 m; thermal sensor sampling 100 m, delivered resampled to 30 m | Landsat 8 begins in 2013, **not 1972**. Nominal revisit 16 days; actual usable scenes depend on clouds and coverage |
| Landsat 9 Collection 2 Level-2 | Multispectral 30 m; thermal products require native-vs-delivered resolution distinction | Mission-era coverage from 2021 onward; query available scenes rather than assuming all dates |
| MODIS Terra MOD09GA | Surface-reflectance bands 500 m; ancillary layers include 1 km | Daily product, not continuous cloud-free observation. Regional-scale visualization, not parcel detail |
| Copernicus DEM GLO-30 | Approximately 30 m / 1 arc-second **surface model** | Primarily 2011–2015 acquisition; not annual imagery or bare-earth terrain. Satellite-path export does not preserve physical elevations |

References: [Sentinel-2 bands](https://dataspace.copernicus.eu/data-collections/copernicus-sentinel-missions/sentinel-2), [Sentinel-2 revisit](https://sentiwiki.copernicus.eu/web/s2-mission), [Sentinel-2 history](https://www.esa.int/Applications/Observing_the_Earth/Copernicus/Sentinel-2), [Landsat 8](https://science.nasa.gov/mission/landsat-8/), [Landsat 9](https://www.usgs.gov/landsat-missions/landsat-9), [MOD09GA](https://ladsweb.modaps.eosdis.nasa.gov/missions-and-measurements/products/MOD09GA), [Copernicus DEM](https://dataspace.copernicus.eu/explore-data/data-collections/copernicus-contributing-missions/collections-description/COP-DEM), [DEM handbook](https://dataspace.copernicus.eu/sites/default/files/media/files/2024-06/geo1988-copernicusdem-spe-002_producthandbook_i5.0.pdf).

RGB, near-infrared and short-wave-infrared combinations serve different visualization purposes; a vegetation color composite is not automatically an NDVI product. Cloud percentage is a scene-level filter and may not represent cloud cover inside your selected rectangle.

## 4. Land cover, vector features & elevation

| Source/module | Scale / configured dates | Interpretation & limits |
|---|---|---|
| IO/Esri/Microsoft annual land cover | 10 m, 9 classes; configuration lists 2017–2023 | Annual classification reference years, not exact capture dates; not a claim about the provider's newest release. [Dataset](https://planetarycomputer.microsoft.com/dataset/io-lulc-annual-v02) |
| ESA WorldCover | 10 m, 11 classes; 2020 and 2021 | The two products use different algorithm versions; apparent differences are not all real change. [Data access](https://esa-worldcover.org/en/data-access) |
| MODIS land-cover entry | Intended MCD12Q1: 500 m, annual | **Requires configuration correction/validation.** The supplied entry points to MCD64A1 burned area, not MCD12Q1 land cover. Do not advertise it as verified land-cover downloading. [MCD12Q1](https://ladsweb.modaps.eosdis.nasa.gov/missions-and-measurements/products/MCD12Q1), [MCD64A1](https://www.earthdata.nasa.gov/data/catalog/lpcloud-mcd64a1-061) |
| OpenStreetMap / Overpass | Roads, railways, water, buildings, land use, POIs and boundaries | Vector data has no single raster resolution; completeness, positional accuracy and edit dates vary |
| Tianditu WFS | Configured transport, water and settlement features | Access and coverage depend on service permissions; not a uniform accuracy or capture-date guarantee |
| OpenBuildingMap | Building vectors | Aggregated sources; no single resolution/date. Inspect source metadata and local completeness |
| Fields of the World | Predicted field polygons; configured year 2024 | Dataset year is not necessarily image capture year; model boundaries are not legal cadastral boundaries |
| JAXA AW3D30 | Approximately 30 m / 1 arc-second DSM | Primarily ALOS PRISM 2006–2011 with supplementary data; later product updates are not annual new surveys. Buildings/vegetation can influence surface heights. [JAXA](https://www.eorc.jaxa.jp/ALOS/en/dataset/aw3d30/aw3d30_e.htm), [Product guide](https://www.eorc.jaxa.jp/ALOS/en/dataset/aw3d30/data/aw3d30v4.1_product_e_1.0.pdf) |

The separate Microsoft Buildings entry is deprecated in the supplied configuration and is not advertised as an independent working module.

## 5. Choosing and checking an output

- **Background map:** choose an authorized basemap and a modest zoom; test a small area before a large mosaic.
- **Historical comparison:** use Wayback for reference, verify local acquisition dates, and avoid interpreting release dates as observations.
- **Scene-based comparison:** query satellite scenes by acquisition date and inspect local clouds. Use original provider products and an appropriate scientific workflow for quantitative analysis.
- **Annual thematic comparison:** check class definitions, algorithm versions and product years before comparing land-cover maps.
- **Terrain analysis:** use original elevation rasters with verified units, vertical datum and NoData; do not use stretched satellite-tab DSM previews.

Keep a record of source/product, scene ID, acquisition/reference date, download date, bands, CRS, pixel spacing, processing and attribution. Imported vector extents are bounding rectangles, not exact polygon masks. Check output alignment, missing tiles and values in GIS before use.
