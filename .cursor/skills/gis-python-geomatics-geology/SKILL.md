---
name: gis-python-geomatics-geology
description: Assist with GIS, geomatics, and geology tasks using Python, including vector/raster processing, spatial analysis workflows, and troubleshooting code with libraries like geopandas, shapely, rasterio, and pyproj. Use when the user mentions GIS, maps, shapefiles, GeoJSON, spatial analysis, coordinate systems, or geology-related spatial workflows in Python.
---

# GIS, Python, Geomatics, and Geology

## Quick Start

When the user asks for help with GIS, geomatics, or geology tasks in Python:

1. **Clarify the spatial data type**:
   - Vector (points, lines, polygons; e.g. shapefiles, GeoPackage, GeoJSON)
   - Raster (grids; e.g. GeoTIFF, NetCDF, DEMs)
   - Tabular data with coordinates (CSV, Excel, database tables)
2. **Identify the main goal**:
   - Data preparation/cleaning
   - Visualization and mapping
   - Spatial analysis (buffers, overlays, joins, distance, interpolation)
   - Coordinate reference system (CRS) / reprojection issues
   - Geology-specific workflows (outcrops, stratigraphy, lithology, structural data)
3. **Propose a Python-based workflow** using standard GIS libraries:
   - Prefer `geopandas`, `shapely`, `pyproj` for vector data
   - Prefer `rasterio`, `xarray`, `rioxarray` for raster data
   - Use `matplotlib`, `geopandas.plot`, or `contextily` for maps/visualization
4. **Show concise code snippets** tailored to the user’s data and environment.

Keep explanations focused on **practical steps and code**, not long theory, unless the user explicitly asks for conceptual background.

## Common Workflows

### 1. Loading and Inspecting Spatial Data

When the user works with shapefiles, GeoJSON, or similar vector formats:

```python
import geopandas as gpd

gdf = gpd.read_file("data/your_layer.shp")
print(gdf.head())
print(gdf.crs)
print(gdf.geometry.type.value_counts())
```

If the CRS is missing or incorrect, ask the user (or infer) which CRS should be used, then:

```python
gdf = gdf.set_crs("EPSG:4326")  # set if missing
gdf = gdf.to_crs("EPSG:3857")   # reproject if needed
```

For raster data (e.g. DEMs, satellite imagery):

```python
import rasterio

with rasterio.open("data/dem.tif") as src:
    dem = src.read(1)
    print(src.crs)
    print(src.transform)
```

### 2. Spatial Joins and Overlays

When combining geology polygons with points (e.g. wells, samples, stations):

```python
import geopandas as gpd

geology = gpd.read_file("data/geology_units.gpkg")
samples = gpd.read_file("data/samples.gpkg")

# Ensure a common CRS
samples = samples.to_crs(geology.crs)

# Point-in-polygon join
joined = gpd.sjoin(samples, geology, how="left", predicate="within")
```

For overlay operations (intersection, union, difference):

```python
import geopandas as gpd

layer1 = gpd.read_file("data/geology_units.gpkg")
layer2 = gpd.read_file("data/faults.gpkg")

layer1 = layer1.to_crs("EPSG:3857")
layer2 = layer2.to_crs(layer1.crs)

intersections = gpd.overlay(layer1, layer2, how="intersection")
```

### 3. Buffers, Distances, and Proximity

Example: find features within a certain distance of a fault or road:

```python
import geopandas as gpd

faults = gpd.read_file("data/faults.gpkg").to_crs("EPSG:3857")
wells = gpd.read_file("data/wells.gpkg").to_crs(faults.crs)

buffer_distance_m = 1000  # 1 km
fault_buffer = faults.buffer(buffer_distance_m)
fault_buffer_gdf = gpd.GeoDataFrame(geometry=fault_buffer, crs=faults.crs)

near_wells = gpd.sjoin(wells, fault_buffer_gdf, how="inner", predicate="within")
```

### 4. Basic Raster Analysis (e.g. DEM)

When the user wants to extract elevation or sample raster values at point locations:

```python
import geopandas as gpd
import rasterio
from rasterio.features import geometry_mask

points = gpd.read_file("data/stations.gpkg")

with rasterio.open("data/dem.tif") as src:
    points = points.to_crs(src.crs)
    coords = [(geom.x, geom.y) for geom in points.geometry]
    values = list(src.sample(coords))

points["elevation"] = [v[0] for v in values]
```

For masking/clipping a raster with a geology polygon:

```python
import geopandas as gpd
import rasterio
from rasterio.mask import mask

geology = gpd.read_file("data/geology_unit.gpkg")

with rasterio.open("data/dem.tif") as src:
    geology = geology.to_crs(src.crs)
    geoms = [geom for geom in geology.geometry]
    out_image, out_transform = mask(src, geoms, crop=True)
```

### 5. Coordinate Reference Systems (CRS) and Projections

When the user has misaligned layers or distance/area problems:

1. Always **inspect CRS**:

```python
print(gdf.crs)
```

2. If computing distances/areas, use an **appropriate projected CRS** (not geographic lat/lon):

```python
projected = gdf.to_crs("EPSG:3857")  # or a local UTM / national grid
projected["area_m2"] = projected.area
projected["length_m"] = projected.length
```

3. When in doubt, suggest using **UTM** or a local projection suitable for the study area, and ask for the approximate location (country/region) to pick a good EPSG code.

### 6. Geology-Specific Patterns

When the task is geology-focused, prefer examples like:

- Mapping lithological units and formations
- Associating field measurements (strike/dip, station points) with mapped units
- Cross-matching borehole or well logs with surface geology polygons
- Highlighting areas within distance of faults, contacts, or lineaments

Example: attach lithology from surface geology to boreholes:

```python
import geopandas as gpd

boreholes = gpd.read_file("data/boreholes.gpkg")
geology = gpd.read_file("data/geology_units.gpkg")

boreholes = boreholes.to_crs(geology.crs)
boreholes_with_units = gpd.sjoin(boreholes, geology, how="left", predicate="within")
```

## Troubleshooting Guidance

When the user encounters errors, follow this checklist:

1. **Library imports**: Confirm the correct libraries are installed and imported (`geopandas`, `shapely`, `rasterio`, etc.).
2. **File paths**: On Windows, recommend using raw strings or forward slashes:

```python
gdf = gpd.read_file(r"C:\data\geology\units.shp")
```

3. **CRS mismatches**: If results look wrong (misaligned layers, strange distances), check and harmonize CRS before spatial operations.
4. **Geometry validity**: For topology errors, try fixing geometries:

```python
gdf["geometry"] = gdf.buffer(0)
```

5. **Large datasets**: If performance is an issue, suggest simplifying geometries, filtering to smaller extents, or using bounding boxes before full operations.

## How to Respond Using This Skill

When applying this skill:

1. **Ask for minimal but essential context**: data type (vector/raster), file formats, CRS if known, and the user’s final objective.
2. **Propose a concrete workflow** with 3–7 clear steps.
3. **Provide concise, runnable Python snippets** targeted to that workflow, using the libraries above.
4. **Explain only what is necessary** to help the user run and adapt the code.
5. When multiple approaches are possible, **recommend a default approach**, and briefly mention alternatives only if they are clearly better for the user’s case.

