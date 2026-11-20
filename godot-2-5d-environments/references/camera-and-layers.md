# Camera and layer setup

## Place and measure the set

With Blender's usual metre scale, X along the route, Y into depth and Z up, the default glTF +Y Up export maps Blender `(x, y, z)` to Godot `(x, z, -y)`. Check the imported root and two landmarks at different depths before applying any further conversion. Confirm an architectural turn in the final world transform, including parents and instances.

Calibrate in the active Godot viewport. For a landmark whose support plane is known, intersect the ray from `Camera3D.project_ray_origin(pixel)` and `project_ray_normal(pixel)` with that plane. Check the intersection exists and lies in front of the camera, then round-trip it with `unproject_position(point)`. Use the result to place or measure geometry. Check the held-back landmark after fixing the pose. [Godot Camera3D](https://docs.godotengine.org/en/stable/classes/class_camera3d.html)

## Composite painted depths in 2D

Reuse the game's existing canvas composition where available. For a new single-viewport scene, put the background art on `CanvasLayer.layer = -1`. In the active `Environment` resource assigned to `WorldEnvironment.environment`, set `background_mode = Environment.BG_CANVAS` and `background_canvas_max_layer = -1`. The 3D set then draws over that canvas. Put foreground art on a higher canvas layer and put gameplay UI above it. Test the ordering with a solid that occludes the background. [Godot Environment](https://docs.godotengine.org/en/stable/classes/class_environment.html) · [Godot CanvasLayer](https://docs.godotengine.org/en/stable/classes/class_canvaslayer.html)

Build each painted depth around its own occlusion boundary. For example, paint a whole sky behind a ridge. Keep the ridge contour exclusively in the land layer. A distant building and the meadow it stands in can share one canvas layer if the entire group stays behind play. Isolate each group against a neutral color, then restore groups from far to near. Compare the composite at both travel limits.

## Drive parallax once

Use a fixed lens and orientation for a straight rail unless the game deliberately changes them. Take the aligned reference pose as the origin. Recompute offsets from that pose every frame so travel, reversal and reset produce the same placement.

| What moves | How to place it |
|---|---|
| Static geometry under a perspective camera | World depth supplies parallax. For camera motion parallel to screen X, a point at forward depth `d` shifts by `-f_px * travel_m / d` pixels. With vertical FOV, `f_px = viewport_height_px / (2 * tan(vertical_fov_rad / 2))`. |
| Static geometry under an orthographic camera | Depth alone gives all fixed points the same lateral screen displacement. Keep the playable set and anything attached to it fixed in world space. If an independent decorative 3D group needs rate `r` on a straight X rail, set `group_x = initial_group_x + (1 - r) * (camera_x - initial_camera_x)`. |
| Independent 2D canvas artwork under either camera | Measure `delta_play_px`, the **signed** screen displacement of a fixed play-layer landmark since alignment. In a screen-fixed canvas using matching viewport units, set `layer_x = initial_layer_x + r * delta_play_px`. A rate of 0 holds the sky still, 1 follows the play layer, and 0.4 gives two fifths of its movement. Apply that offset in one place. |

Use the actual viewport transform if canvas units and viewport pixels differ. Recalculate alignment when the viewport or lens changes. Track one landmark per depth over the full move, including a return to the starting pose.

## Cover reveals and tile edges

Let `D` be the play landmark's horizontal range in pixels from one travel extreme to the other. For constant rates, two adjacent layers can expose a strip up to `abs(r_near - r_far) * D` pixels wide. Paint those hidden regions and enough outer margin for filtering and animated sway. For a changing lens or curved move, measure the actual extrema along the shot.

For an art width of `B` layout units when `W` units fill a viewport `V` pixels wide, `ceil(B * V / W)` gives one source texel per output pixel. Check both axes at the closest view. Work across the complete panorama before cutting tiles. Carry neighboring edge texels into each tile's sampling margin through the mip levels in use. Test a fractional-pixel crossing and a reversal on the join. Check transparent edges over light and dark backgrounds at delivery size.

## Materials, atmosphere and effects

- Author evenly lit surface swatches at a stated scale. Tile a sample 2 by 2 and inspect how grain turns at modeled corners. Treat albedo as color data. Import normals as normal maps and sample roughness, metallic and masks as linear data. Match texture filtering and mipmaps to the smallest projected detail. [Godot shader texture hints](https://docs.godotengine.org/en/stable/tutorials/shaders/shader_reference/shading_language.html)
- Record the art color profile, renderer, exposure and tonemapper when matching a reference. Sample a lit surface, its shadow, the air and one accent. Apply haze once per depth. For a linear-light mix, use `T = exp(-optical_depth)` and `result = T * surface + (1 - T) * air`. Keep silhouette alpha independent of haze. Compare with haze off at the same exposure.
- Give soft edges a matching alpha import and blend mode. Inspect mip transitions during motion. Anchor wind at the stem and water to the channel it follows. With the camera and clock frozen, toggle an effect and compare its contact patch with an adjacent unaffected patch.
- Check each requested output at native resolution and compare matched poses across sizes. When target hardware is specified and available, measure frame time there and inspect large transparent layers for overdraw.
