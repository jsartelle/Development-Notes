# Notes

## General

- <kbd>F3</kbd>: command search
- <kbd>n</kbd>: show properties sidebar (includes view options)
- <kbd>Ctrl+Space</kbd>: maximize editor pane under pointer
- <kbd>m</kbd>: move to collection
- <kbd>F2</kbd>: rename selected object
- <kbd>Shift+a</kbd>: open Add menu
    - when adding a mesh, use the lowest resolution you can get away with (less vertices to deal with), and try to keep the faces roughly square
    - circles have a Fill Type, set to Ngon to add a face
- <kbd>Shift+d</kbd>: Duplicate
    - canceling while using tools with a movement step after (like Duplicate and Extrude) does not cancel the tool, only the movement, so there will still be duplicated vertices (undo to avoid this)
- <kbd>Alt+d</kbd>: Duplicate Linked
    - in Edit mode both the original and any linked duplicates will be affected, but in Object mode only the selected instance will
- <kbd>Tab</kbd>: switch between Object and Edit modes
- <kbd>Ctrl+Tab</kbd> (hold) - open mode select menu

## Viewport

- <kbd>Mouse3</kbd>: orbit (rotate) camera
- <kbd>Shift+Mouse3</kbd>: pan camera
- <kbd>~</kbd>: open view selection menu
- <kbd>Numpad 1</kbd>: front orthographic view
- <kbd>Numpad .</kbd>: focus on selected object
- <kbd>z</kbd> (hold) - open rendering mode menu

## Cursor

- the cursor controls where new objects are added, and affects how some tools work
- <kbd>Shift+Mouse2</kbd>: move cursor
- <kbd>Shift+s</kbd>: cursor movement menu
    - Cursor to Selected will move the cursor to the center of the selected object
- <kbd>Shift+c</kbd>: center scene and reset cursor

## Selection

- <kbd>a</kbd>: select all
    - <kbd>Alt+a</kbd>: deselect all
    - <kbd>Ctrl+i</kbd>: invert selection
- <kbd>w</kbd>: activate Select tool if another tool is active, cycle through Select modes if already active
    - <kbd>Shift+Mouse1</kbd>: add to selection
    - <kbd>Alt+Mouse1</kbd>: select loop
- when working in orthographic views, drag select instead of just clicking to select so that you get the vertices behind the front-most one
- <kbd>Alt+z</kbd>: toggle x-ray mode
    - allows you to select "through" a mesh
- <kbd>h</kbd>: hide selection
    - <kbd>Alt+h</kbd>: unhide
- <kbd>x</kbd>: delete selection (can also use <kbd>Delete</kbd>)
    - Dissolve will merge the surrounding geometry to fill in the space
- <kbd>Numpad /</kbd>: toggle local view (show only selection)
- <kbd>p</kbd>: se**p**arate selection
- <kbd>Ctrl+p</kbd>: parent objects
    - select the child first, then the parent-to-be, and select Keep Transform

## Camera

- <kbd>Numpad 0</kbd>: toggle camera view
- <kbd>Ctrl+Alt+Numpad 0</kbd>: move camera to current viewport position
- *Properties sidebar > View > Lock Camera to View* lets you move the camera by moving the view
    - or with the camera selected use <kbd>g</kbd> as usual, and use <kbd>Mouse3</kbd> to "zoom" (move along local z axis)
- <kbd>F11</kbd>: bring up most recent render
- <kbd>F12</kbd>: render

## Tools

- <kbd>g</kbd>: Move (g for grab)
- <kbd>r</kbd>: Rotate
- <kbd>s</kbd>: Scale
- <kbd>t</kbd>: Transform
- <kbd>Shift+Space</kbd>: open tools menu
- While a tool is in use:
    - <kbd>x/y/z</kbd>: restrict to that axis
    - <kbd>Shift+x/y/z</kbd>: exclude that axis
    - hold <kbd>Mouse3</kbd>: snap to nearby axis
    - hold <kbd>Ctrl</kbd>: snap to increments
    - hold <kbd>Shift</kbd>: slow down
    - can enter numeric values, and - for negatives

## Object mode

- *Right click > Shade Smooth/Flat*
- *Right click > Set Origin > Origin to Geometry* to move origin to center of each selected object

## Edit mode

- <kbd>1/2/3</kbd>: enter vertex/edge/face select mode
- <kbd>Ctrl+l</kbd>: select all connected (**l**inked) vertices
- <kbd>o</kbd>: toggle proportional editing
    - affects the area around the selection (scroll bar to adjust radius)
    - use menu in toolbar to change falloff type
- <kbd>Shift+Tab</kbd>: toggle snapping (use menu in toolbar to change what it snaps to)
    - Project Individual Elements allows all the vertices to snap when using proportional editing
- *Right click > Subdivide* (can adjust smoothness from popup)

### Edit mode tools

- <kbd>Ctrl+r</kbd>: Loop Cut (adds another "line" of vertices around the shape)
- <kbd>i</kbd>: Inset (like loop cuts but for the vertices of a single face)
- <kbd>e</kbd>: Extrude
    - <kbd>Alt+e</kbd>: open Extrude tools menu
    - <kbd>s</kbd> while extruding along normals - make extrusion even
    - extruding duplicates vertices - to move existing vertices, use the Move tool with only those vertices selected
- Spin: extrudes vertices in a circle around the 3D cursor
    - after spinning, can adjust center coordinates from popup menu
    - <kbd>Scroll wheel</kbd>: increase loop count
    - right click when moving to cancel and place in middle
- <kbd>Shift+e</kbd>: add crease to selected loop
- <kbd>f</kbd>: add face between selected edges
    - to do the same between edge loops use Bridge Edge Loops (<kbd>Ctrl+e</kbd>)

## Sculpting

- <kbd>f</kbd>: change brush size
    - *Right click on header > Tool Settings* to see brush settings
- <kbd>Shift+f</kbd>: change strength
- <kbd>Ctrl</kbd> (hold): push in instead of out

## Particles

- takes an object or collection and scatters it over the surface of another object (ex. sprinkles on a donut)
    - if using a collection, check Use Count to adjust the frequencies of different objects
    - checking Whole Collection will treat the collection as if it is one object
- use Emitter for animated particles, Hair for static
- to use an object as the particle, change *Render > Render As* to Object, and then select the object under Instance Object (same with a collection)
- *Render > Scale Randomness* will randomly scale, but along all three axes
- check the Advanced box to make rotation & other settings appear
    - under Rotation, Randomize adjusts rotation along all three axes, Randomize Phase adjusts rotation along relative X and Y (I think)
- check *Emission > Source > Use Modifier Stack* to improve results
- to limit the particles to the weight painted area, under Vertex Groups change Density to Group (the name of the vertex group created when you start weight painting)
    - disable particle rendering while weight painting to avoid slowdown

## Modifiers

- found under the wrench icon in Properties (bottom right)
- applied top to bottom, order matters
- can control visibility of modifiers in different modes
- use Apply in dropdown (next to delete button) to apply the changes to the mesh (good idea to make a copy first)

### Available Modifiers:

- Subdivision Surface: non-destructively smooths the surface by subdividing faces
- Solidify: adds thickness (extrudes)
    - offset controls direction
    - use crease (under Edge Data) to adjust generated shape
- Array: creates arrays of the same object (use multiple Array modifiers for multi-dimensional arrays)

## Materials

- Roughness defines how reflective the material is
- Subsurface allows light to enter and bounce around within the object, like skin or icing (need to set the subsurface color)
    - Subsurface Radius defines how deeply the light penetrates the object (separate values for R/G/B)
    - for the donut icing, set the subsurface color to a bit deeper shade than the base color, and set the Subsurface Radius for the channel matching the icing color to 0.3 and the others to 0.15
- Emission makes a material "glow"

## Shading

<kbd>Ctrl+Shift+Mouse1</kbd> on a node: "preview" by connecting to Material Output/Surface

- to randomize base color of a material: *Object Info/Random > ColorRamp/Fac (customize gradient as desired), ColorRamp/Color > Principled BDSF/Base Color*
    - change Gradient Type to Constant (from Linear) to avoid interpolation
- to apply bump mapping: *Texture Coordinate/Object > Noise Texture/Vector, Noise Texture/Fac > Displacement/Height, Displacement/Displacement > Material Output/Displacement*
    - To actually affect the mesh, in Material properties change *Settings > Surface > Displacement* to Displacement & Bump (change Scale in Displacement node if it looks like a porcupine)
- use a MixRGB node to combine multiple textures (Fac slider controls the "strength" of the second input)
    - use Blend type Add with Fac 1.00 to "merge" B&W textures

## Texture Paint

- <kbd>n</kbd>: bring up brush options & color picker
    - can change blend types too
- <kbd>x</kbd>: swap colors
- <kbd>s</kbd>: sample color
- <kbd>Alt+s</kbd> with cursor over texture: save image
- purple is the "missing texture" color
- use a Texture Mask in the brush options to paint with a texture - set the mask type in Texture properties on the right
    - in brush options set Mask Mapping to Random to avoid perfectly tiling the texture
- use Shading mode to hook up the texture (as an Image Texture node) to the Base Color (or any other property) of a material

## Rendering

- hiding objects from viewport does not disable them in rendering
- use the Cycles rendering engine for path tracing (but much slower and noisier preview than Eevee)
- sun lights do not have any fall off
- light angle affects shadow sharpness (0 = sharp, higher = smoother)
- can adjust exposure in Render Properties tab under Color Management
- change *Color Management > View Transform* to False Color to check brightness - everything should be gray or green (see https://blender.stackexchange.com/a/195862)

## Compositing

- denoising through compositor seems to give better results than using checkbox in Properties

# Donut steps

## Level 1

1. Make torus
2. Move some vertices to make it "lumpy"
3. Add subdivision modifier
4. Duplicate top to make icing
5. Add solidify modifier
6. Drag icing edges down to be uneven and add drips
7. Apply modifiers to donut
8. Use Draw tool in Sculpting to indent ring around middle
9. Apply modifiers to icing
10. Use Inflate tool in Sculpting to create raised drips on icing
11. Use Grab tool to make icing around hole uneven
12. Use Draw tool to add a bit of bumpiness to icing
13. Pull icing back towards center of donut
14. Apply materials & subsurface to donut and icing
15. Render and use Compositor to denoise

## Level 2

1. Add a low poly sphere (sprinkle)
2. Extrude top half to stretch out, and scale caps along Z axis
3. Add hair particles to the icing, using the sprinkle as the instance object
4. Change rotation Orientation Axis to Normal and tweak Phase (to make sprinkles lay on surface in random directions)
5. Use Weight Paint mode to paint where the sprinkles should appear
6. Use Shading mode to randomize the sprinkle colors (and turn up Roughness to make them look chalky)
7. Make more sprinkles with different shapes (including a ball), and change the icing particle effect to render a Collection with all of them
8. Adjust relative counts of different sprinkle shapes in Particle properties
9. Create a rectangular texture in Texture Paint mode (so that the faces are roughly square)
10. Use Shading mode to assign the texture to the donut's Base Color
11. Use Texture Paint to paint details on the donut, using a Texture Mask set to Clouds to add variance to the brush
12. In Shading mode, apply a Noise Texture (Fac output) to the donut using NodeWrangler
13. Add bump mapping, and change Displacement to Displacement & Bump so it affects the mesh
14. Duplicate the noise texture with a larger scale to add larger bumps, and use a MixRGB node set to Add to merge them
15. Connect the large scale noise texture to a ColorRamp and move the black slider up to only take the "deeper" (whiter) details from the noise texture
16. Connect the large scale noise ColorRamp to input 1 of the MixRGB node and the smaller scale noise to input 2, then use the Fac slider to control how much of the smaller detail to "let through"
17. Add another MixRGB node set to Overlay and connect the donut texture into input 1, set input 2 to a dark brown (burnt donut) color, then the merged noise textures into Fac, and use this as the base color to darken the bumpy areas
18. (optional) Add some holes and less saturated areas in Weight Paint to make the sprinkle distribution more realistic
19. (optional) If you overdid the variance in the texture painting, use another MixRGB node to add a solid color overlay to the donut texture to help even it out
20. (optional) Make the icing glow by setting its Emission

## Level 3

1. Add coffee cup reference image
2. Add a cylinder for the cup and scale it to line up with the reference image
3. Scale down the bottom of the cup to make a funnel shape
4. Create and scale edge loops to form the curved cup shape
5. Add Solidify and Subdivision Surface modifiers
6. Add another loop cut to the top to make the rim less tapered
7. Use Inset tool a couple times on the bottom of the cup to avoid the strange "pinched in" look, then scale out to create a flat bottom
8. In top down view, rotate so the face where the handles will attach is flat
9. Apply Solidify modifier (to avoid solidifying the inside of the handle)
10. Use Extrude, Rotate and Spin tools to shape the handle, using Bridge Edge Loops to finish it off
    1. To create the bulk of the handle, move the 3D cursor to the center of the handle loop and use Spin to extrude the handle around it
11. Add creases where the handle joins the cup, and increase Subsurface modifier in render to get rid of the jagged shading around the creases
12. Add a circle with Ngon fill for the plate, lined up with where the top of the plate should be
13. Inset the plate where it meets the cup, then add a couple edge loops
14. Use sphere proportional editing to pull the plate down to the bottom of the cup in stages
15. Add Solidify and Subsurface modifiers to plate, and then add a loop cut to make the edge less thin/sharp
16. Create the circular raised "ripple" inside the plate by creating a couple insets on the plate around the bottom of the cup
    2. apply the Solidify modifier first because you only want to apply the ripple to the top
    3. create loop cuts and insets around the ripple to emphasize it

# Links

[Blender Beginner Tutorial - Part 1](https://www.youtube.com/watch?v=TPrnSACiTJ4&list=PLjEaoINr3zgEq0u2MzVgAaHEBt--xLB6U)

[Become a 3D illustrator in one hour!](https://polygonrunway.com/p/become-a-3d-illustrator-in-one-hour)
