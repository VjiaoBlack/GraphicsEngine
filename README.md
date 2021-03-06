GraphicsEngine
==============
Final Project for CS2377 @UChicago

Here is a sample rendering of test-map3, with grass and texture-mapping enabled but not rain.
![sample rendering of test-map3][sample]


FINAL PROJECT - grp1411
=======================

Controls
--------
Feature toggle controls
  w: toggle wireframe
  l: toggle lighting
  f: toggle fog
  g: toggle grass (generates when pressed, not before, so may be slow on first click)
  b: toggle rain
Movement controls
  u, h, j, l: movement (think wasd controls but shifted along the keyboard [u = w, etc])
  arrow keys: camera direction
  r         : movement speed increased while held
  space     : move upwards while held (works like thrust on a jetpack)
  v         : return to the ground
Miscellanious controls
  +: increase screen-space error tolerance
  -: decrease screen-space error tolerance
  q: quit

Features by Victor Jiao
-----------------------
* View Frustum
    - Only draws tiles that are at least partly visible by the view frustum.
    - Attempted optimizing collision algorithm
* Skybox
    - Cleared the 'edges' of the skybox by pushing in each side a tiny amount
    - Incorporates blending of the skybox into the horizon fog color
    - Looked hard for a good skybox texture online smile emoticon
* Terrain
    - Includes a 'detail' texture for rendering of very close triangles
* Grass
    - Includes support for billboard display
        > Uses instancing to increase rendering speed of billboards.
        > For displaying many many identical meshes, instancing is considered
          faster than using a geometry shader (due to parallelism, I believe).
    - Supports grass 'wind' animation
    - Is generated at correct heights on the terrain
    - Has a function (but is slow; not used in final version) for
      the procedural generation of grass. Uncomment the update
      function in the grass.cxx file to see.
* Water
    - Simple animation for 'waves' through constructive interference of two
      rotated 'wave crest' textures
    - Simple reflection of skybox and nearby terrain. I do this by drawing a
      copy of the entire map essentially upside-down, using the stencil-buffer
      to speed things up (and render alpha-values correctly).
* Rain
    - You can see rain
    - The fog becomes more prevalent with rain
* Fog
    - You can see fog and toggle it on and off.
* View Animation and Movement
    - Supports collisions ('walking on top of') the mesh
    - Supports physics (using the 'jetpack')
    - Supports smooth movement of camera

* For grass and rain, I used a combined Texture Buffer Object to hold data for BOTH
  particle systems. This lets me refresh things a lot faster, without having to switch
  between TBO's. (Switching between TBOs is exceedingly slow)

* Coded original function to find heightAt a certain point in mesh. Later updated by Noah Krim

Features by Noah Krim
---------------------
* Buffer-Cache
    - Made use of the cache-buffer through pointers in tiles and a vector of tiles to render
    - Optimized in tandem with recursive level-of-detail calculations and view frustum collision detection to reduce number of operations prior to rendering
* Morphing
    - Implemented height morphing during the vao creation stage
      > Includes the ability to morph backwards while already morphing
      > Will only skip the morphing phase if the camera goes fast enough to pass two LODs in the time of one morph
      > All achieved through interpretation of one float value in the Tile class between [-1, 1]
    - Diffuse textures morph with the tiles via linear interpolation between the child and parent textures
* Optimizations
    - Organizational and algorithmic optimizations performed across the board
      > Major areas include in the FindHeight function, rendering operation, and selecting tiles to be rendered

Comments / Issues
-----------------
- Wireframe mode may be slow due to GPU branching since one shader is used for all mesh rendering
- Grass only exists in a square area around the starting position of the camera and it is generated on the first press of the "g" key so that action may be slow, but subsequent presses will not be (there were attempts at procedurally generated or whole-terrain grass, but issues arose with avoiding areas of water and large height increases that expose the imposters as well as being extremely slow for a moderate amount of grass, but the code for this is still in the project, but commented out)
- Large maps may be slowed by the fact that water reflections are rendered by rendering the mesh again, reversed, underneath itself
- Some maps with larger bodies of water will have some artifacts from the skirts of the reflected mesh poking through the water
- Details from the detail texture and the waves from the water textures may increase in size as the camera goes up (or as the LOD of the tile changes), and while attempts were made to fix this, it became too inconvenient considering how small of an effect it had




[sample]: https://github.com/VjiaoBlack/GraphicsEngine/raw/master/sample_image.png "Sample Rendering"
