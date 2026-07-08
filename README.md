# OSM iD Editor Template

This repository is a local template wrapper around the open-source
[OpenStreetMap iD editor](https://github.com/openstreetmap/iD). The upstream
source is included as the `id-editor` git submodule, currently pinned to
`v2.40.0`.

The goal of this template is to get a known-good local copy running first.
After that, it can be adapted for cultural heritage destruction research, custom
imagery timestamps, private exports, or a private OSM-compatible API.

## Why This Is Possible

iD is open source and available under the ISC License. That means you can run,
study, modify, and redistribute your own version as long as you preserve the
license terms.

Important research note: the stock iD editor is designed for editing current
OpenStreetMap data. For historical cultural heritage destruction work, you
probably do **not** want to upload experimental or historical edits directly to
the public OSM API. A safer first target is a private dataset, a private
OSM-compatible API, or an export workflow that writes GeoJSON/OSM change files
for analysis.

## Requirements

- Git
- Node.js 22 or newer
- npm

## First-Time Setup

Clone this template with its submodule:

```powershell
git clone --recurse-submodules <your-template-repo-url>
cd osm_id_editor_template
```

If you already cloned it without submodules:

```powershell
git submodule update --init --recursive
```

Install iD's dependencies:

```powershell
npm run id:install
```

Generate the local build assets:

```powershell
npm run id:prepare
```

Start the development server:

```powershell
npm run id:start
```

Then open:

```text
http://127.0.0.1:8080
```

## Useful Commands

```powershell
npm run id:install    # Install dependencies in id-editor
npm run id:prepare    # Build generated assets used by the editor
npm run id:start      # Run the local development server
npm run id:build      # Create a production build in id-editor/dist
npm run id:test       # Run upstream iD tests once
```

## Local Dataset Export Workflow

This template is locked to one study location: Timbuktu. Within that location,
you can create multiple exportable datasets, for example one dataset per year or
imagery date.

1. Start the editor with `npm run id:start`.
2. Open `http://127.0.0.1:8080`.
3. Use the **Dataset** toolbar button to create or open an export dataset.
4. Enter imagery metadata:
   - imagery timestamp, such as `2012-06-30`
   - imagery CRS, such as `EPSG:3857`
   - optional custom tile/WMS template, such as
     `https://tiles.example.org/{z}/{x}/{y}.png`
5. To use Esri Wayback imagery, pan/zoom to the study area and use:
   - **Load Local Wayback Dates** for versions with local changes near the map
     center
   - **Load All Wayback Dates** if the local list is empty or you want the full
     archive
   - **Use Wayback** to switch the basemap to the selected release
6. To use licensed/custom imagery, enter the tile/WMS template and click
   **Use Custom Tiles**.
7. Draw points, lines, or polygons with the normal iD tools. The editor now
   includes cultural heritage presets near the top of the point, line, and area
   preset lists:
   - **Cultural Heritage Site**
   - **Destroyed Heritage Building**
   - **Heritage Damage Event**
8. Use **Mark Destroyed** while a feature is selected to record destruction
   without deleting the footprint.
9. Use the Dataset panel's OSM feature focus shortcuts when needed:
   - **All** restores all OSM feature types.
   - **Buildings** shows buildings and building parts.
   - **Roads** shows traffic roads, service roads, and paths.
   - **POIs** shows point features and address points.
   - **Land/Water** shows landuse and water features.
   - **Boundaries** shows boundary features.
10. For more detailed feature filtering, open the normal iD **Map Data** panel
    and expand **Map Features**.
11. Use **Save** or the normal save toolbar button to write the active dataset.
12. Use **Export GeoJSON** to download the active dataset's `features.geojson`.

Exports are limited to the Timbuktu study area bounding box:

```text
west -3.06, south 16.72, east -2.94, north 16.82
```

Only features whose geometry intersects that box are included in saved/exported
GeoJSON. The map itself can still pan outside the box while you are working, but
the dataset export stays locked to Timbuktu.

Dataset data is written locally under:

```text
datasets/<dataset-folder>/
|-- metadata.json
`-- features.geojson
```

The exported GeoJSON properties include the original iD tags plus:

```text
objectID
dataset
datasetFolder
project
projectFolder
studyArea
studyAreaName
imageryTimestamp
imageryCRS
imagerySource
imageryLayerID
imagerySourceType
waybackReleaseNum
idEditorEntityID
idEditorEntityType
changeType
destroyed
```

The normal iD Save button has been repurposed in this template: it writes to the
active local dataset instead of opening the public OSM upload flow.

## Cultural Heritage Presets

The template adds private research-oriented presets on top of the upstream iD
tagging schema. These presets do not change public OSM upload behavior; they are
intended for the local dataset export workflow.

The custom fields include:

```text
heritage:object_id
heritage:site_type
heritage:status
heritage:damage_type
heritage:destruction_date
heritage:destruction_start_date
heritage:destruction_end_date
heritage:confidence
source:imagery
source:imagery:date
source:imagery:start_date
source:imagery:end_date
heritage:evidence
research:note
```

## Template Structure

```text
.
|-- id-editor/      # Upstream OpenStreetMap iD source as a pinned submodule
|-- package.json    # Convenience commands for working from the template root
|-- .gitmodules     # Submodule pointer to openstreetmap/iD
`-- README.md       # This guide
```

## Next Customization Milestones

1. Run the stock editor locally.
2. Add a richer export/review workflow so edits become reproducible research data
   rather than accidental public OSM uploads.
