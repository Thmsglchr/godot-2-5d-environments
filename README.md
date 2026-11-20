# Godot 2.5D Environments

A ChatGPT Codex skill for creating side-scrolling 2.5D environments. It guides Codex from a visual reference or written brief through an editable Blender blockout, then into Godot 4 for materials, lighting, parallax, camera setup and in-engine verification.

The workflow models playable scenery as actual 3D geometry and checks it through the game's camera as it moves. Painted sky, distant scenery and foreground decoration can be composed as separate 2D layers. The skill forbids projecting illustrations onto meshes or using image-covered surfaces to substitute for dimensional scenery.

## Use

Copy the `godot-2-5d-environments` folder into your Codex skills directory, then ask Codex to build or repair a Godot 4 side-scrolling environment, or invoke `$godot-2-5d-environments` explicitly. Blender and Godot are needed to carry out the full workflow.

The skill includes a camera and layer reference for axis conversion, parallax, coverage, materials and atmosphere.

## Example prompt

A reference image of a 2D side-scrolling scene is important for defining the composition, style and visual details to recreate. Attach an existing image or use `$imagegen` to create one.

```text
Use $godot-2-5d-environments to recreate the attached reference as a 2.5D side-scrolling scene in Godot, aiming for AAA production quality.

Polish the scene to match the reference as closely as possible, then rate the match from 0 to 10. Repeat the polish and assessment until the score reaches 10.
```
