# Creating Levels with REPOLib SDK

::: warning NOTICE
This page is currently Work in Progress!
:::

::: info NOTE
**This guide assumes you have a patched Unity project set up for REPOLib modding.\
If not, follow [Getting Started](./start.md) first.**
:::

The patcher outputs vanilla level data to `Assets/REPO/Game/ScriptableObjects/Level`, and their modules to `Assets/REPO/Game/Resources/level`.

## Creating the Level Asset

1. Create a `Level` asset by right-clicking in your mod folder and choosing `Create > Level > Level Preset`. The default name for this will be "Level - _____"; replace the underscores with your level's name.
2. Fill in the following fields. Most of them are required, otherwise your level will generate errors.
   - `Narrative Name`: The name that will appear in-game to the players in the loading screen.\
     For this guide, we will use "Level - Example".
   - `Loading Graphic 01`: The building sprite that the truck crashes into on the loading screen.
   - `Loading Graphic 02`: The background sprite for terrain like trees/mountains.
   - `Loading Graphic 03`: The far background sprite for further terrain and the horizon/moon/sun.
   - `Valuable Presets`: The valuables you want to appear in your level. You can choose to create your own [valuable preset](#valuable-preset), or leave it empty to use the Generic preset.\
      If you want to use vanilla valuable presets, do *not* add them directly; instead, see [Proxy Valuable Presets](#proxy-valuable-presets).
   - `Music Preset`: The "music" that can play randomly during the level. You can create your own [music preset](#music-preset), or use a placeholder vanilla preset (such as `Level Music - Arctic`).
   - `Ambience Presets`: The ambient noises that play randomly during the level. You can create your own [ambience preset](#ambience-preset), or use a placeholder vanilla preset (such as `Level Ambience - Global`).
3. Configure and experiment with the following fields to taste. You can refer to vanilla's `Level` assets for ideal settings.
   - **Fog**:
     - `Fog Color`: The color of the fog that fades and obscures distant objects.
     - `Fog Start/End Distance`: The distances from the player, in units, where the fog falloff will start and end respectively. The default end distance, 15, is the size of one module. The player cannot see anything past the end distance.
   - **Environment**:
     - `Ambient Color`: The ambient color that illuminates the level.
     - `Ambient Color Adaptation`: The ambient color used while the player is crouching, which should be *slightly* brighter than the former to compensate for the player not having their flashlight enabled.
   - **Color**:
     - `Color Temperature`: Sets the post-processing white balance. Positive values are warmer, while negative values are colder.
     - `Color Filter`: The color that is multiplied in post-processing to tint the render.
   - **Bloom**:
     - `Bloom Intensity`: The strength of the bloom effect.
     - `Bloom Threshold`: The pixel brightness threshold which the bloom effect is applied to. Pixels brighter than the threshold contribute to the effect, while pixels below don't. This value is in gamma-space.
   - **Vignette**:
     - `Vignette Color`: The color of the vignette, which is the darkening around the screen edges.
     - `Vignette Intensity`: The strength of the vignette.
     - `Vignette Smoothness`: The smoothness of the vignette borders. Lower values will induce "banding" in the vignette layers.

Leave all the Module and other unmentioned fields empty or default.

### Valuable Preset

The `Level Valuables` asset is used to determine which valuables can spawn in your level, which can include your own.

1. Right-click in your mod folder and choose `Create > Level > Level Valuables Preset` to create a new `Level Valuables` asset.
<!-- 2. Drag-and-drop your valuable prefabs into their respective size categories. Be sure not to mix any. -->
<!-- Is this correct? -->

Again, do *not* use vanilla valuable presets directly; see [Proxy Valuable Presets](#proxy-valuable-presets).

### Music Preset

The `Level Music` asset is used to play random, occasional music/ambient tracks.

1. Right-click in your mod folder and choose `Create > Level > Level Music Preset` to create a new `Level Music` asset.
2. Populate the **Tracks** list:
   - `Name`: The name of the track. This is currently not used in-game.
   - `Audio Clip`: The reference to your track file.
   - `Volume`: The volume of the track. All vanilla presets have this set to 0.2.

Vanilla levels typically have five tracks, spanning between 1-2 minutes. It is **highly** recommended to use the OGG format; you can use Audacity or FFmpeg to convert your tracks to OGG.

### Ambience Preset

The `Level Ambience` asset is used to play a variety of ambient noise loops and ambience breakers (short sounds which interrupt the ambience).

1. Right-click in your mod folder and choose `Create > Level > Level Ambience` to create a new `Level Ambience` asset.
2. Configure the following fields:
   - `Loop Clip`: The reference to the constant ambience loop audio.
   - `Loop Volume`: The volume of the ambience loop.
   - **Breakers**:
     - `Sound`: The reference to the ambience breaker audio.
     - `Volume`: The volume of the ambience breaker.

As with level music, it's recommended to use OGG for the `Loop Clip`. However, ambience breakers can be WAV; this format has less decompression overhead, making it more suitable for short, instantaneous sound effects.

## Creating the REPOLib Level Content Asset

- Right-click in your mod folder and choose `Create > REPOLib > Level` to create a `Level Content` asset. Rename this to your level's name.
- Fill in the fields:
  - `Level`: Drag your `Level` asset into this field (in our case, `Level - Example`).
  - `Connect Object`: An optional reference to a [door](#doors) prefab, which is spawned between **all** connected modules (like McJannek Station, Headman Manor and Swiftbroom Academy).
  - `Block Object`: The prefab that always spawns between unconnected modules (optional).

## Modules

- There are 5 types of Modules (rooms):
  - `Start Room`: Module containing the truck that the players spawn in and the first extraction. These do not have a difficulty level and always spawn exactly 1.
  - `Normal`: Most common module that contains [Module Prop Switches](#module-prop-switches), and can connect to adjacent modules on all 4 sides. Has 3 difficulty levels.
  - `Passage`: Always has two entrances on opposite sides. Has 3 difficulty levels.
  - `Dead End`: Rarest module that has a chance to spawn connected to any of the other above modules. Has 3 difficulty levels.
  - `Extraction`: Required modules that contain the extraction points. Has 3 difficulty levels.
- Module Size:
  - The maximum size a vanilla module can be is 3x3 (15x15 units/meters), with the coordinates spanning `-7.5` to `7.5` in both the X and Z axes.
  - Anything extending past the module bounds will have a chance to overlap with neighboring modules, except for `Start Rooms` that can expand as far back as you'd like.
  - Using this, we know the doorways will always spawn at these coordinates:

    | Normal Modules |   X   |   Y   |   Z   |
    | -------------- | :---: | :---: | :---: |
    | Top            |   0   |   0   |  7.5  |
    | Right          |  7.5  |   0   |   0   |
    | Bottom         |   0   |   x   | -7.5  |
    | Left           | -7.5  |   0   |   0   |

    | Module Type  |   X   |   Y   |   Z   |
    | ------------ | :---: | :---: | :---: |
    | Starter Room |   0   |   0   |   0   |
    | Dead End     |   0   |   0   | -7.5  |
    | Extraction   |   0   |   x   | -7.5  |
    | Passage TOP  |   0   |   0   |  7.5  |
    | Passage BOT  |   0   |   0   | -7.5  |

### Module Prop Switches

- Module Prop Switches are only used in `Normal` modules and are required. These are what are used to change how a room looks depending on if that side is connected to another module or not.
- Typically, we have 4 parents for each side of a module. Each parent should have the `Module Prop Switch` component where you can drag in your `Connected` and `Not Connected` parents respectively, and set the connection side.

  ![Screenshot](/repolib/sdk/levels/ModulePropSwitch.png)

### Level Path Points

- `Level Points` need to be on the [navmesh](#navmesh) and are used by enemies as wander points and general navigation.
- `Level Points` should not be in extraction modules.
- Connected path points need to be correctly connected and referenced to one another on both objects. For example, if `TOP` has a connected point `Middle`. The `Middle` path point should also have a connected point to `TOP`.
- To assign path points, click and drag the desired point in to the `Connected Points` field on a `Level Point` object.
- There are two kinds of `Level Points` which are used for enemy navigation. A straight line is drawn between all connected path points, and there should be no objects obstructing that path.
    - `External`: These are marked as `Module Connect` objects and are required on every module type, except for Extraction Modules. External path points should NOT be connected to each other, only to `Internal` path points. External path points should be placed near/on these coordinates:

      <!-- BLOKBUSTR TODO: The rotation of Level Points also matters. -->

      | External Path Points |   X   |   Y   |   Z   |
      | -------------------- | :---: | :---: | :---: |
      | Top                  |   0   |   0   |   5   |
      | Right                |   5   |   0   |   0   |
      | Bottom               |   0   |   0   |  -5   |
      | Left                 |  -5   |   0   |   0   |

    - `Internal`: These are optional path points that are not marked as `Module Connect` objects and are used for adding extra navigation within a module, like going around a corner or up/downstairs.

### DirtFinder

- `DirtFinder` is the object used to display the in-game minimap. There are 3 categories of meshes we use for maps:
  - `Floor`: The dark green shapes that display the floor.
  - `Wall`: The lighter green lines that outline the walls.
  - `Door`: The white lines that are attached to doors.
- Each `DirtFinder` mesh type has a subset of different shapes to properly display your physical level to the minimap.

  ![Screenshot](/repolib/sdk/levels/DirtFinderTypes.png)

- You can get a better visual aid by assigning your `DirtFinder` object with a `Mesh Filter` and `Mesh Renderer` with the appropriate mesh. Make sure to disable this afterwards, or else it will appear in-game.

### Navmesh

- The `Navmesh` is used by Enemies to pathfind between modules.
- In the Editor, the `NavMesh` will show up as a blue highlight displaying where enemies are able to navigate.
- `NavMeshModifiers` are typically assigned to the parent of your floors/walls which applies the modification to all the colliders in the children.\
  `NavMeshModifiers` have 3 different area types:
  - `Walkable`: Allows navigation on all nested colliders.
  - `Not Walkable`: Overwrites collider navigation and removes walkability.
  - `Jump`: Forces enemies to pathfind by jumping over selected colliders.

  ![Screenshot](/repolib/sdk/levels/NavMeshModifier.png)

- Typically, we want to assign `Walkable` to all our floors, and `Not Walkable` to all our walls and any props (like tables, decorations, etc.).

### Room Volume

- `Room Volumes` are used for ambient sound logic, as well as displaying the undiscovered rooms on the minimap.
- `Room Volume` objects should include a `Box Collider` and the `RoomVolume` script, and must be assigned to the `RoomVolume` layer at the top of the Inspector.
- `Room Volumes` can overlap each other and should cover the entire walkable area, but should not extend outside of the module boundaries.

  ![Screenshot](/repolib/sdk/levels/RoomVolume.png)
  ![Screenshot](/repolib/sdk/levels/RoomVolumeExample.png)

### Doors

- Doors are optional prefabs we can use to connect between modules.
- They should be centered on `0, 0` in their prefab.
- For our example, we can copy the `Manor Door` prefab and reference it to make our own doors.
- To make sure our door fits in our doorways, we want to make sure the `PhysGrabObject` `Collider` is slightly smaller around the edges than the actual door mesh.
- Make sure the collider also has the `Phys Grab Object` tag and is under the `PhysGrabObjectHinge` layer. Our collider should also have the `PhysGrabObject` material assigned.

  ![Screenshot](/repolib/sdk/levels/Door.png)

### Proxy Valuable Presets

To use vanilla `Level Valuables` presets in your levels, you should not simply reference them in the `Level` asset. This is because the bundle will then contain duplicates of all the vanilla valuables from that preset. Instead, you have to create a "proxy" preset:

- Create a `Level Valuables` asset anywhere in your project by right-clicking and choosing `Create > Level > Level Valuable Preset`.
- Name the asset exactly as the vanilla one you want to include (see [Creating Valuables with REPOLib SDK](valuables.md)).
- Add your newly created preset to `Level Valuables` in your `Level` asset.

At runtime, REPOLib will match the name and replace your proxy with the real preset.
