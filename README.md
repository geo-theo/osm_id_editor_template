# Simple Building Outline Editor

A lightweight geospatial editor for drawing building footprints from two years
of satellite imagery.

This project is intentionally **not** a copy of the OpenStreetMap iD editor.
OSM iD is useful inspiration for map editing interactions, but it includes far
more functionality than this workflow needs. This tool should stay small,
focused, and easy to use.

## Goal

Create a browser-based editor that lets a researcher:

1. Switch between two configured years of satellite imagery.
2. Draw polygon outlines around buildings visible in each year.
3. Export only the polygons they drew as GeoJSON, with one layer per year.

The exported data should be clean research data, not a dump of background map
features.

## Scope

### In Scope

- Two satellite imagery layers, one for each year.
- A simple year switcher.
- Polygon drawing for building outlines.
- Polygon editing and deletion.
- Separate user-drawn layers for each year.
- GeoJSON export.
- Optional local autosave in the browser.

### Out of Scope

- Public OSM data loading.
- OSM account login.
- Uploading edits to OpenStreetMap.
- Roads, POIs, boundaries, amenities, addresses, or other preset feature types.
- OSM tagging forms, validators, changesets, or conflict handling.
- Point and line drawing, unless a future research workflow requires them.

## User Workflow

The first usable version should support this flow:

1. Open the editor.
2. The map starts at the study area.
3. Select the first imagery year.
4. Draw building polygons visible in that year.
5. Switch to the second imagery year.
6. Draw building polygons visible in that year.
7. Export the results.

The active year's polygons should be visually prominent. The other year's
polygons can remain visible as faint reference outlines, but they should never
be confused with the active drawing layer.

## Interface

The editor should open directly to the map. No landing page is needed.

Expected controls:

- Year A
- Year B
- Draw polygon
- Edit polygon
- Delete polygon
- Export GeoJSON

The interface should be quiet and utilitarian. The main task is visual
comparison and careful tracing, so the map should get most of the screen.

## Data Model

The app should maintain one GeoJSON `FeatureCollection` per year:

```json
{
  "2012": {
    "type": "FeatureCollection",
    "features": []
  },
  "2024": {
    "type": "FeatureCollection",
    "features": []
  }
}
```

Each drawn polygon should have a minimal, predictable set of properties:

```json
{
  "id": "building-...",
  "year": "2012",
  "featureType": "building_outline",
  "imagerySource": "...",
  "createdAt": "...",
  "updatedAt": "..."
}
```

Only user-created polygons should be saved or exported.

## Export

The preferred export is a small bundle with one file per year plus one combined
file:

```text
buildings_2012.geojson
buildings_2024.geojson
buildings_all.geojson
```

The combined file should preserve the `year` property so the data can be
filtered, styled, or compared in GIS software.

If a zipped bundle is too much for the first version, downloading the three
files separately is acceptable.

## Recommended Technical Approach

Build a new standalone app with:

- Vite
- TypeScript
- OpenLayers

OpenLayers is a good fit because it already supports:

- XYZ and raster imagery sources.
- Vector layers.
- Polygon drawing.
- Geometry modification.
- GeoJSON import and export.

This avoids carrying the complexity of the full iD editor while still providing
solid geospatial primitives.

## Repository Direction

The existing `id-editor/` directory should be treated as reference material, not
the foundation for the final tool.

Recommended next step:

```text
simple-building-editor/
```

The new app can live in that directory or replace the root-level app structure.
Either option is fine, but the implementation should be independent from
`id-editor/`.

## Implementation Plan

1. Add a new simple app at the repository root or in
   `simple-building-editor/`.
2. Replace the current root scripts with `dev`, `build`, and `preview` for the
   new app.
3. Add an imagery configuration file with the two year labels and tile URLs.
4. Build the OpenLayers map with only satellite imagery and two vector drawing
   layers.
5. Add polygon draw, edit, and delete behavior, restricted to polygons.
6. Add local autosave so work survives browser refreshes.
7. Add GeoJSON export for the two year layers.
8. Keep the README focused on the stripped-down building outline workflow, not
   the iD/OSM workflow.
9. Smoke-test drawing, switching years, refresh recovery, and export contents.

## Configuration Needed

Before implementation, decide:

- The two imagery years.
- The tile URL or imagery source for each year.
- The study area and starting map view.
- Whether exports should be downloaded as a zip bundle or separate files.

The current Timbuktu study area can remain the default if it is still the target
research site.
