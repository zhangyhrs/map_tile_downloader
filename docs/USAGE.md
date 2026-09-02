# Usage guide

**English | [简体中文](USAGE_ZH.md)** · [Home](../README.md)

## Set the area

Draw a rectangle, use the map view, enter bounds or import a vector. Import uses the bounding extent, not exact polygon clipping. Verify the rectangle visually and provide the correct source CRS.

## Choose a workflow

- **Tiles:** choose source/zoom, check estimated tile count, select output GeoTIFF/CRS and download.
- **Wayback:** refresh the release list, select a date and download. Release dates are not necessarily capture dates.
- **Satellite:** set dates/cloud threshold, search, inspect footprints, select scenes and bands, then download.
- **Vectors:** select a service and feature category; inspect exported attributes and geometry.
- **Land cover:** choose product/year and interpret the thematic raster using the appropriate class legend.
- **Field boundaries:** choose year and GeoJSON/GeoPackage; inspect logs for format fallback.
- **AW3D30:** enter your JAXA credentials privately, calculate required tiles, set output/extraction options and download.
- **GIF:** choose a local GeoTIFF folder and output/animation options. Check filename dates and ordering before enabling date labels.

## Troubleshooting

| Symptom | Action |
|---|---|
| Registration required | Contact the developer privately for a valid machine-bound authorization code |
| Missing runtime files | Re-extract the entire application package |
| Blank map/request failure | Check network, provider availability, credentials, limits and certificate configuration |
| Shifted output | Check source CRS and basemap offsets against known locations |
| Memory exhaustion | Reduce area/zoom and split the job |
| Gray patches | Inspect failed-tile logs and retry a smaller request |
| No satellite scenes | Check coverage, dates, cloud threshold and dataset |
| Unexpected extent | Imported polygons become rectangles; mosaics may extend to tile boundaries |

Stop requests are cooperative; ongoing network or processing steps may need to return first. Partial files are not complete outputs.

For [issue reports](https://github.com/zhangyhrs/map_tile_downloader/issues), include version, Windows version, module, reproduction steps and redacted logs. Never post credentials, machine codes, confidential coordinates or private paths.
