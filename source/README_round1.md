# NammaSpace: Round 1 prototype (Techfest, IIT Bombay)

Indoor photographs → 3D reconstruction → explorable web digital twin (orbit, zoom, first-person WASD walking with collision, minimap).

**Demo video:** `NammaSpace_Round1_Demo.mp4` (in this folder)

## Dataset (public, not our own capture)
Deep Blending, "Dr Johnson" scene: 263 photographs of a historic house interior, with COLMAP camera poses.
- Paper: P. Hedman et al., *Deep Blending for Free-Viewpoint Image-Based Rendering*, SIGGRAPH Asia 2018.
- Download used: `tandt_db.zip` from the 3D Gaussian Splatting release (Kerbl et al., 2023):
  https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/datasets/input/tandt_db.zip (folder `db/drjohnson`)

## Run the viewer locally
```
node video/serve.mjs site 8765
```
Open http://localhost:8765/?scene=drjohnson
- **Explore**: drag = orbit · wheel = zoom (to cursor) · right-drag = pan · double-click = focus
- **Walk**: W A S D = move · drag = look · Shift = run · Q/E = down/up · M = minimap · K = input overlay
- **Layers**: Sparse / Dense / Mesh / Textured · **F** = fullscreen · **About** = data source + run statistics

## Rebuild the reconstruction (Windows, CPU only; tools in `tools/`)
```
bash pipeline/reconstruct_posed.sh datasets/deepblending/db/drjohnson drjohnson   # OpenMVS densify → mesh → texture → GLB
bash pipeline/publish_drjohnson.sh                                                 # copy into site/ and register the scene
```
The phone-video pipeline from the earlier capture is `pipeline/reconstruct.sh <video> <job>`.

## Re-record the video
```
cd video && node tts.mjs DENSE_M=<millions> && node record.mjs all && node assemble.mjs
```
`record.mjs` drives the real viewer in headless Chrome (GPU) with a virtual 30 fps clock and real mouse/keyboard events. Narration is neural TTS (Edge voice "Andrew").
