---
name: godot-2-5d-environments
description: Build or repair side-scrolling 2.5D environments in Godot 4 from concept art or a written brief. Use for Blender-built playable scenery, painted parallax layers, or scenery that looks flat, slips, or opens seams as the gameplay camera moves.
---

# Godot 2.5D side-scrolling environments

## Hard rule for 2D artwork

**Never project a 2D image or surface onto a 3D mesh, or place an image-bearing surface against or in place of 3D scenery.** This applies at every depth, even for a fixed camera or a prototype. It covers concept art, photographs, screenshots, renders, crops, repaints and freshly generated artwork.

- Never transfer an image onto a solid through a camera or light projector, screen-space lookup, planar or triplanar image projection, decal, front-view UV unwrap, or a bake into albedo, emission or vertex colors. Baking the result into ordinary UVs or feeding a viewport texture to a mesh still counts.
- Never use an illustrated plane, billboard, `Sprite3D`, stack of cutouts, or image-backed extrusion to supply a building, rock, tree or other scenic form. A depth-map relief of the picture, thickness behind a painted façade, and invisible collision behind a cutout all leave the visible form dependent on a single view.
- Model playable contacts, moving parts and anything whose thickness, sides, shadows or occlusion the camera can reveal. Give these solids their own silhouettes and openings. Use ordinary mesh UVs for intrinsic surface detail such as wood grain, stone color, roughness and normals, or use a procedural 3D material. A timber grain map can finish a beam. The beam, its end and the opening it spans must exist in the mesh.
- Keep freshly painted sky, distant land or shallow foreground decoration in independent **2D canvas layers** when each layer stays wholly behind or in front of the 3D action. Use a `Sprite2D` under a `CanvasLayer`, for example. If a branch must pass behind a post and in front of a roof, model that branch. Keep the supplied illustration and any extracted pieces in authoring references only.

If a proposed shortcut breaks this rule, build the required geometry or place eligible artwork in the 2D canvas. Apply the same rule when repairing inherited assets.

## Start from the brief

- **Illustration.** Record the framing, ground line, light direction and one known or assumed length in `survey.md` beside the scene. Mark observed and inferred dimensions. On a working copy, trace occlusion boundaries and openings. Group each feature with the ground or structure that owns its contact. List what travel exposes behind each group. Reserve one landmark for the final fit check.
- **Written brief.** Establish scale, camera travel, playable contacts and visible depths from the brief and the existing project. Record significant assumptions. Use these landmarks to judge the blockout.
- **Repair.** Open the normal entry scene. Locate the camera pose and direction that expose the fault. If the scene has no travel driver, record the range used for a test sweep. Follow the affected instance to its Blender source, Godot material overrides, shader inputs or canvas driver as relevant. Change the smallest part that explains the fault. Preserve existing player controls, collisions and save behavior.

For an environment-only request, keep figures and their shadows out of scenery and delivery footage. Use an editor ruler for scale and an invisible collision probe or the game's existing controls to exercise the route.

## Build and review

1. **Block out the full route in Blender.** Use metres and the project's travel axis. Align square buildings to the set at 0° or 90°, model the thickness of walls and openings, and give moving parts a pivot and clearance. Where an attachment is unclear, resolve its plan, elevation and cross-section at the same scale. Check the import scale and orientation against known landmarks.
2. **Frame the blockout through the game's Godot camera.** Use the production viewport, projection, lens and travel limits. With a single opaque matte material on the solids and the canvas art hidden, inspect the start, middle, end and an oblique side view. A doorway should still have an opening, jambs and depth. Fix failures in the mesh. Show the gray set beside the supplied illustration, or against the brief's landmarks.
3. **Finish one representative section.** Unwrap the tops, sides, ends and reveals. For a new build, author surface swatches independently of the illustration. Choose a section where a solid meets its ground and the camera uncovers something behind it. Light it in Godot, move a light across a joint, and show a native frame and a short camera move before extending the treatment to the rest of the route.
4. **Compose the surrounding depths.** Give each depth its own named group and carry Blender group names through import. Paint each canvas layer through the regions hidden at the reference pose. Keep original artwork and, for generated assets, its prompt and inputs. Set explicit canvas order and drive each layer once from the gameplay camera. For axis conversion, canvas setup, scroll math, coverage, haze and texture budgets, use [Camera and layer setup](references/camera-and-layers.md).

## Verify and deliver

- **Inspect the actual material path.** Trace suspicious scenery from the final instance and surface override through shader coordinates, texture contents and any upstream bake or packed Blender/glTF image. Check what the image depicts even when its filename, graph or UV layout looks ordinary. Replace any projected or image-backed scenery found in scope, then repeat the matte and oblique views.
- **Check depth in motion.** Freeze animated effects and travel the route in both directions. Check each depth in isolation and restore the composite at the start, middle and end. Track a contact and a landmark per depth. Check for sliding foundations, duplicate silhouettes, gaps, tile seams and incorrect draw order. On returning to the same camera pose, landmarks should align with the starting frame. Re-enable effects and check their attachment points.
- **Hand over what was exercised.** For a full build, supply the editable Blender set, runnable Godot scene, materials and authored canvas layers, camera settings, matching gray and finished frames, and a short native travel clip for a moving shot. Reopen the handoff from a clean location and check the normal entry scene. For a repair, supply the focused change and evidence from the failing pose or move. Identify inferred geometry, checked resolutions and any view or run that remains unverified.
