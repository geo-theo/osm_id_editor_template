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
2. Decide whether the research edits should write to GeoJSON, OSM XML, an
   `.osc` changeset file, or a private OSM-compatible API.
3. Add curated historical imagery layers for specific dates/locations.
4. Add domain-specific presets for cultural heritage sites, destruction events,
   confidence levels, source imagery, date ranges, and notes.
5. Add an export/review workflow so edits become reproducible research data
   rather than accidental public OSM uploads.
