# BPSRStarmodPlugins

The **plugin registry** for the [Stellar launcher](https://github.com/StellarResonance/BPSRStarmodLauncher)
(Blue Protocol: Star Resonance) — the dedicated home of the curated plugin catalog,
in the spirit of Dalamud's plugin repo.

This repo owns the registry; its CI publishes the index + DLLs to the public MinIO
bucket the launcher reads (`https://minio.revette.io/starmod/plugins.json`). It is
**decoupled from the framework's releases** — plugins ship on their own cadence.

## Layout

```
plugins/
  <id>/
    manifest.json     # id, name, description, version, dll, author
    <Plugin>.dll      # the built plugin assembly
tools/build-registry.py   # validates manifests, computes sha256, builds + publishes plugins.json
.github/workflows/publish.yml  # PR = validate; push to main = validate + publish to MinIO
```

`plugins.json` (generated) is the launcher's curated registry:
each entry = `{ id, name, description, version, dllUrl, sha256, author }`.

## Submitting a plugin (community)

1. Fork + add `plugins/<your-id>/manifest.json` and your built `<Plugin>.dll` beside it.
2. Open a PR. CI validates the manifest + DLL.
3. On merge, CI publishes it to the registry the launcher sees.

The launcher can also add **third-party registry URLs** directly (any `plugins.json`),
so you can host your own registry instead of submitting here.
