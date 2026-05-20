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

## Local Research Project Workflow

This template adds a private project/export layer on top of iD:

1. Start the editor with `npm run id:start`.
2. Open `http://127.0.0.1:8080`.
3. Use the **Project** toolbar button to create or open a research project.
4. Enter imagery metadata:
   - imagery timestamp, such as `2012-06-30`
   - imagery CRS, such as `EPSG:3857`
   - optional custom tile/WMS template, such as
     `https://tiles.example.org/{z}/{x}/{y}.png`
5. Draw lines or polygons with the normal iD tools.
6. Use **Mark Destroyed** while a feature is selected to record destruction
   without deleting the footprint.
7. Use **Save** or the normal save toolbar button to write the active project.
8. Use **Export GeoJSON** to download the active project's `features.geojson`.

Project data is written locally under:

```text
projects/<project-folder>/
|-- metadata.json
`-- features.geojson
```

The exported GeoJSON properties include the original iD tags plus:

```text
objectID
project
projectFolder
imageryTimestamp
imageryCRS
imagerySource
imageryLayerID
idEditorEntityID
idEditorEntityType
changeType
destroyed
```

The normal iD Save button has been repurposed in this template: it writes to the
active local research project instead of opening the public OSM upload flow.

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
2. Add curated historical imagery layers for specific dates/locations.
3. Add domain-specific presets for cultural heritage sites, destruction events,
   confidence levels, source imagery, date ranges, and notes.
4. Add a richer export/review workflow so edits become reproducible research data
   rather than accidental public OSM uploads.
