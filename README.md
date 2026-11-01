# Namma Space: 3D Reconstruction & Roaming

**Live demo:** https://aryan2159.github.io/namma-space-3d/

A real indoor space, filmed with a smartphone and reconstructed into a textured 3D mesh with open-source photogrammetry. You can explore it in the browser in three modes:

- **Explore**: orbit, zoom and pan, with the ceiling cut away (dollhouse view).
- **Walk**: first-person roaming with WASD keys or a touch joystick, drag-to-look, collision and click-to-step.
- **Tour**: replays the real camera trajectory recovered by Structure-from-Motion.

## Pipeline (CPU-only, free and open-source)
| Stage | Tool |
|---|---|
| Frame extraction + blur-based frame selection | ffmpeg (`blurdetect`) |
| Features, matching (sequential + vocab-tree loop closure), incremental SfM | COLMAP 4.2.1 |
| Gravity alignment, undistortion | COLMAP |
| Depth-map MVS, fusion, Delaunay/graph-cut meshing, texture atlas → GLB | OpenMVS 2.4.0 |
| Web optimisation (meshopt, WebP) | glTF-Transform 4.2 |
| Viewer | three.js r170 + three-mesh-bvh |

The viewer derives gravity from the SfM camera poses. It finds the floor from the densest low horizontal slab of mesh vertices and sets metric scale by assuming the phone was held ~1.35 m above the floor, so measurements are approximate.

Each scene's `assets/<id>/stats.json` and `cameras.json` are written by the pipeline and are shown in the viewer's **About** panel as provenance.
