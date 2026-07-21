# NJORD Engine Handbook

The user guide for **NJORD Engine**, an AI-first focused engine. NJORD Engine lets you build 3D games in a single desktop app. Promt and describe what you want or use the in built functions to set up a game, test it instantly and export a double-clickable game.

> This is a living document and the source for the docs on the NJORD website.
> It describes the editor (NJORD Studio) and the games it produces. Sections
> marked _(coming soon)_ are on the roadmap.

## Contents
ö
- [Getting started](#getting-started)
- [The interface](#the-interface)
- [Moving around the viewport](#moving-around-the-viewport)
- [Building a scene](#building-a-scene)
- [Transforming objects (gizmos)](#transforming-objects-gizmos)
- [The player](#the-player)
- [Animations](#animations)
- [Importing models and textures](#importing-models-and-textures)
- [Materials and textures](#materials-and-textures)
- [Lights, sun and environment](#lights-sun-and-environment)
- [Sound](#sound)
- [Gameplay: goals, collectibles, enemies](#gameplay-goals-collectibles-enemies)
- [NPCs, dialogue and quests](#npcs-dialogue-and-quests)
- [Grouping and hierarchy](#grouping-and-hierarchy)
- [Testing your game (Play)](#testing-your-game-play)
- [The AI prompt and agents](#the-ai-prompt-and-agents)
- [The game front-end: splash, menu, pause](#the-game-front-end-splash-menu-pause)
- [Packaging: Build and export](#packaging-build-and-export)
- [Settings](#settings)
- [Controls reference](#controls-reference)

---

## Getting started

1. Open **NJORD Engine**.
2. **File ▸ New project…** creates a new game from a starter scene (a floor, a
   player and a goal). Set:
   - **Project name** , the folder name (letters, digits, `-`, `_`).
   - **Game name** , the title shown in-game and used for the exported `.exe`.
     You can change it later by right-clicking the game at the top of the asset
     tree and choosing **Rename**.
3. To open an existing game, use **File ▸ Change project**.

A game is a folder of plain, human-readable files (the world, gameplay rules,
visual style, and your imported models/textures). You never edit those by hand,
the editor does it for you, but it means your projects are portable and
version-control friendly.

## The interface

The window has three areas:

- **Asset tree** (left) , every object, light, material and file in your game,
  shown as a hierarchy. Select something here to edit it.
- **Viewport** (center) , the live 3D view of your game, powered by the real
  engine. What you see is what you ship.
- **Properties / Prompt** (right) , the inspector for the selected object, and
  the AI prompt panel.

Use **View ▸ Swap tree and prompt sides** to flip the left/right panels. Panels
can be resized and minimized; the layout is remembered.

The **menu bar** groups the main actions: **File** (new/open/save/package),
**Edit** (undo/redo/reset camera/settings), **Assets** (add lights, import,
create objects), **View**, and **Help**.

## Moving around the viewport

The editor camera orbits around your scene:

- **Orbit** , drag with the **left** or **middle** mouse button.
- **Pan** , drag with the **right** mouse button.
- **Zoom** , scroll wheel.
- **Frame selection** , select an object and press **F** to center the camera on
  it.
- **Reset camera** , **Edit ▸ Reset camera**.

The viewport has a ground grid and render modes (solid and wireframe) to help you
place things precisely.

## Building a scene

Promt or add objects from the **Assets** menu:

- **Create cube / cylinder / sphere / cone / plane** , primitive shapes, handy
  for blocking out a level.
- **Create empty** , an invisible transform-only object, useful as a group/parent
  (see [Grouping and hierarchy](#grouping-and-hierarchy)).
- **Create player / spawn point / goal** , gameplay objects (see below).
- **Add point / spot / directional light** and **Add sun**.

New objects appear in the asset tree and in the viewport. Select one to move it
and edit its **Properties** (transform, color/material, collision, gameplay).

The tree is organized under your game: **World** (the scene , the camera,
environment, sun, your objects and lights), **Materials**, **Models**,
**Textures**, and the data files. Right-click an object for **Rename**,
**Duplicate** and **Delete**.

## Transforming objects (gizmos)

Select an object and use the on-screen gizmo to move, rotate or scale it. Switch
tools with the keyboard:

- **W** , Move
- **E** , Rotate
- **R** , Scale

Drag the colored handles to transform along the X/Y/Z axes. Drag the **yellow
cube in the centre** for the free / uniform version of the active tool: free
move in the screen plane, uniform scale (whole object), or free trackball
rotation. **Hold Ctrl while dragging to snap** (a 0.5 grid for move, 0.25 steps
for scale, 15-degree steps for rotate). You can also type exact values in the
**Transform** section of the Properties panel. Changes are saved with your
project; use **Ctrl+Z** / **Ctrl+Y** to undo/redo.

## The player

Your game has one **player**: a first-class object you can select and edit like
any other (every game has one). Its **Properties** include:

- **Model** , the mesh the player uses. Pick an imported model from the dropdown
  (a `.glb`/`.gltf`/`.obj`), or a primitive. An animated glTF plays automatically.
- **Facing** , if your model runs the wrong way (some rigs face the "wrong"
  direction), set Facing to **Backwards 180°** (or 90° left/right) to turn the
  visual model. This only rotates the model, not the controls.
- **Animations** , map the model's clips to gameplay states (see below).
- **Spawn at** , the spawn-point marker where the player starts and respawns.
- **Transform ▸ Size** , scales the player model and its standing height. For the
  collision shape itself, use **Properties ▸ Collider** (see "Player collider"
  below) to set an explicit box that matches your model; otherwise the player uses
  a built-in fixed size. Place the player so it sits on the floor and it will rest
  in the same spot in-game.

The player faces the direction it moves (third-person: forward is where the
camera looks).

## Animations

When you set the player's **Model** to an animated glTF, NJORD drives it with a
locomotion system:

- It blends **idle ▸ walk ▸ run** by the player's actual speed, crossfades to
  **jump** in the air, and plays **die** on death, all smoothly.
- It plays **attack** as a one-shot when you swing a melee (left mouse): the
  swing takes over the whole body for its duration, then returns to locomotion.
- Hold **Shift** to sprint (run).

### Mapping clips

A model often ships several named clips. In the player's **Animations** section,
pick which clip plays for each state (**Idle / Walk / Run / Jump / Attack / Die**).
Each dropdown lists the clips found in the model. "Auto" matches by keyword
(idle/walk/run/jump/attack/die), so well-named clips work with no setup. A model
without an attack clip simply plays no swing animation; the attack still lands.

> Tip: export from your model tool as a **single `.glb` that contains all the
> clips**. FBX does not load, use glTF/GLB. If a model has no idle clip, the
> player holds a neutral pose when standing still.

### Shoot while running

The proper way to shoot while moving is a **shoot clip per stance**, so the legs
in the clip always match the movement , no upper/lower "mask" needed. Name them
with a stance word plus a shoot word and NJORD picks the right one automatically
from the player's speed:

- **`Idle_Shoot`** (or `Aim`) , idle legs + firing arms , used when **standing**.
- **`Walk_Shoot`** , walking legs + firing arms , used while **walking**.
- **`Run_and_Shoot`** , running legs + firing arms , used while **running**.

Map any one of them as the **Attack** clip (or just name them by convention and
Auto finds them). While you fire, the full-body clip for your current stance plays
(legs match, movement speed unchanged); between shots the plain locomotion plays.

If a stance has **no** matching shoot clip, that stance keeps its normal
locomotion and just shows the muzzle flash (no arm animation) , NJORD will **not**
slam in a running-legs clip while you stand or walk. So with only `Run_and_Shoot`,
running shots animate fully while standing/walking shots keep their idle/walk
legs. Add `Idle_Shoot` + `Walk_Shoot` (e.g. generate them in Meshy) to get firing
arms in those stances too.

A **pure** attack clip (e.g. `Shoot`, `Punch`, with no locomotion word in the
name) just plays full-body as-is, standing or moving , the right choice for melee.

> This replaces the earlier bone-mask / leg-freeze approaches (a Bevy animation
> mask collapsed the model on some rigs; both are disabled). Matching the clip to
> the stance is robust and needs no per-bone masking.

> Tip: map the states explicitly (Idle/Walk/Run/Jump/Die to the plain locomotion
> clips, Attack to the shoot clip); locomotion matching skips action clips so a
> clip named like `Run_and_Shoot` is never mistaken for the Run state.

### Custom animations (play any clip from a rule)

The states above are engine-driven (the engine decides when to play them). To
trigger **any other clip yourself**, use a `play_animation` behavior action in
`behaviors.json`:

```json
{ "trigger": { "type": "on_interact", "target": "chest" },
  "action": { "type": "play_animation", "target": "chest", "clip": "open" } }
```

- `target` is the entity id to animate. **Omit it to animate the player**, where
  the clip plays as a one-shot over locomotion (like attack).
- `clip` is matched case-insensitively against the model's clip names.
- By default it plays once and returns to the idle loop (or the player's
  locomotion). Add `"looping": true` to keep it playing (e.g. a spinning fan).

So known states (idle/walk/run/jump/attack/die) live in the engine, and any
extra movement you invent lives in your behavior rules, no engine changes.

## Importing models and textures

- **Assets ▸ Import model…** copies a `.glb`, `.gltf` or `.obj` into your game's
  `models/` folder. It then appears in the **Models** node and in any Model
  dropdown.
- **Assets ▸ Import texture…** copies an image into `textures/`.

Imported assets live inside your game folder, so projects stay self-contained.
Unused models/textures are flagged "(unused)" in the tree and can be deleted
(they move to a `.trash` folder, so deletes are undoable).

**Assets area (parking to the side).** A newly imported or AI-generated model is
dropped into an **assets area** to the right of your scene (a lane starting at
about x = 24, like a 3ds Max asset shelf), not into the middle of the active
scene. Generate or import several and they line up in a tidy grid there instead
of piling on top of each other. Drag one into place when you want to use it.

### Generating models with AI (Meshy)

The **3D Model** agent (see _The AI prompt and agents_) generates a textured but
**static** mesh via Meshy, it has no skeleton or animation. The result is a
normal `.glb` in your `models/` folder, so you can rig and animate it outside the
engine and bring the result back in:

1. Open the model's `.glb` from `models/` in **Blender** (right-click the model
   in the tree ▸ open externally), or upload it to **Mixamo** (free auto-rig +
   animations for humanoid characters) or Meshy's own rigging.
2. Add a skeleton and animation clips, then export a single `.glb` (with all the
   clips) back over the file in `models/`.
3. The editor **live-reloads** on save and the rigged glTF's clips play
   automatically (see _Animations_).

So "make a character that runs" is two steps today: generate the mesh, then rig
it externally and save it back.

**SOLO review before it enters the scene.** A generated model does not silently
land in your level. It opens in **SOLO mode**: the rest of the scene is hidden so
you see just the new model, with a review bar at the bottom of the viewport.
Choose **Approve** to keep it, or **Reject** to remove it (the `.glb` file stays in
`models/`, so nothing is lost, it is just not placed). If one run produces several
models they queue up and are reviewed one at a time (the bar shows how many are
left). Press **S** to toggle solo on the reviewed model, or on any selected entity,
to inspect it alone. The **Iterate** button (refine an existing model) is greyed
out for now, it needs a provider that supports refine (Tripo or Hyper3D), which is
coming. Limitation (current cut): the model is technically added to the scene while
you review it (hidden), so leaving solo without choosing keeps it placed; a stricter
"not placed until approved" pass comes with the multi-provider work.

**External editing:** right-click a model or texture to open it in an external
program (set your preferred apps in Settings). Saving in that program live-reloads
the asset in the editor.

## Materials and textures

- **Assets ▸ Create material** makes a reusable, named material. Edit its base
  color, metallic/roughness, emissive and texture in Properties; assign it to
  objects.
- Plain objects can also use an **inline** color/texture without a named
  material.
- glTF models bring their own baked materials, but you can **tint** them:
  Properties ▸ Material ▸ **Tint** multiplies the model's base color (white =
  unchanged), so it shifts a textured model or recolors an untextured one (e.g.
  a generated Meshy model). Applies on reload.

## Lights, sun and environment

- Add **point**, **spot**, **directional** lights or a **sun** from the **Assets**
  menu. Lights show as icons in the editor and are hidden in the game.
- Select a light to edit its color, intensity, range and (for spot) cone angle.
- The **Sun** and **Environment** nodes (under World) control the global sun
  (direction, color, intensity, shadows) and scene environment (shadows, ambient,
  ambient occlusion).

## Sound

Sounds live in an `audio.toml` next to your level, mapping a sound **id** to a
file in your game folder:

```toml
[sounds]
collect = "sounds/coin.ogg"
hurt = "sounds/ow.ogg"
win = "sounds/fanfare.ogg"
lose = "sounds/buzzer.ogg"
death = "sounds/thud.ogg"
```

Drop the audio files (`.ogg` or `.wav`) in your game folder (e.g. `sounds/`).
Two ways a sound plays:

- **Built-in moments** , the ids `collect` (pickup), `hurt` (took non-lethal
  damage), `win`, `lose`, `death` (the death sequence), `shoot` (fire a ranged
  weapon), `hit` (a shot hits a wall or target), `jump`, `reload` (a reload starts,
  on R or when the magazine empties) and `empty` (dry-fire click on an empty
  magazine) play automatically when those happen, just by being defined (each is
  silent until you map it).
- **Behavior rules** , a `play_sound` action (in your behavior rules, which the
  AI prompt can author) plays any id you choose, on any trigger.

**Background music** , add a `music` line (an audio file path) to `audio.toml` and
it loops while the game runs (stopped while editing):

```toml
music = "sounds/theme.ogg"

[sounds]
collect = "sounds/coin.ogg"
```

Set **per-sound volume** with a `[volumes]` table (and `music_volume` for the
music), 1.0 = full:

```toml
music = "sounds/theme.ogg"
music_volume = 0.6

[volumes]
collect = 0.8
```

The player's **Master volume** (Options menu) scales everything on top. Limitations
(first cut): one-shot sound effects + one looping music track, no music crossfade
yet.

## Gameplay: goals, collectibles, enemies

- **Spawn point** , where the player starts/respawns. Set the player's **Spawn
  at** to use it.
- **Goal** , reach it to win the level.
- **Collectible** , toggle **Collectible** on any object (Properties ▸ Gameplay)
  and give it a **Score value**. The player picks it up on touch.
- **Checkpoint** , toggle **Checkpoint** to move the respawn point when touched.
- **Enemy / AI** , in Properties ▸ **AI / Enemy**, set **Behavior** to **Chase**
  (hunts the player; set Speed/Damage/Health, and **Knockback** = how hard a touch
  shoves the player off, 0 = no shove), **Shooter** (a ranged enemy that keeps its
  distance and fires projectiles at the player; Damage = HP per shot), or
  **Patrol** (moves between waypoints). AI objects are shown in red in the tree. A
  Shooter takes the player's shots/melee just like a chaser. Place **several**
  shooters and each automatically gets a bit of variety (a different hold distance,
  fire rate and move speed, and it **strafes/circles** the player), and they push
  apart instead of stacking, so a squad reads as distinct enemies, not one AI.

- **Attacking (melee or ranged)** , the player attacks with the **attack** key
  (left mouse by default). Select the player and open **Properties ▸ Combat** to
  set it up (or edit `style.toml [combat]` directly). By default it is a **melee**
  swing that damages enemies within **Melee range**. Turn on **Ranged (shooter)**
  to fire a **projectile** along the player's facing instead, damaging the first
  enemy it hits then despawning. Tune **Damage** (per hit), **Cooldown** (fire
  rate), and the projectile **speed / life / size**. The attack animation (if the
  model has one) plays for both. Set **Muzzle R/U/F** to move where shots spawn
  relative to the player (**Right / Up / Forward** in world units), so they leave
  the weapon instead of the head, e.g. lower the Up value and add a little Right for
  a hip-fired gun. **Ranged shots converge on the crosshair** (screen centre),
  correcting for the camera being offset from the player, so what you point at is
  what you hit. Hold the **right mouse button** to **aim**: the camera pulls in
  over the shoulder and looks **along your aim line**, so you can aim fully **up or
  down** with the mouse (limited only by the camera's **min/max pitch**); the
  player turns to face your aim and a centered crosshair appears. If up-aim feels
  capped, raise **max_pitch** in the camera style (radians; ~1.2 gives a steep
  upward look).

- **Weapon pickups** , make any object a weapon the player picks up: select it and
  turn on **Properties ▸ Weapon ▸ Weapon pickup**. On touch the player equips it
  (the pickup disappears) and the ranged attack switches to its stats, so a melee
  game becomes a shooter once you grab a gun, and you can place stronger weapons
  later in a level. Each weapon sets **Name**, **Damage**, **Fire rate**,
  **Proj. speed**, **Range** (projectile life), **Proj. size** and **Spread**
  (scatter in degrees: **0 = dead straight**, higher = less accurate). So one
  weapon can shoot **straighter and farther** than another. Set **Ammo** to a
  magazine size (**0 = unlimited**, no reloading); with a magazine, each shot
  spends a round, an empty magazine **auto-reloads**, and **R** reloads manually
  (**Reload** = the wait). The engine shows a corner ammo readout when ammo is
  limited. The equipped weapon resets at the start of each level.

- **Health & lives** , the player has **health** (set **Max health** on the player)
  shown by a built-in **health bar** (bottom-left, green then amber then red as it
  drops) , this is automatic, every game shows it, no `ui.json` needed. On top of
  health, **lives** are how many times you can die before **game over**: set them
  in **Game settings ▸ Lives**. **Lives = 0 means unlimited** , a **health-only**
  game (a typical FPS): the player always respawns and only a `lose` rule (or a
  goal) ends the match. Any positive number is a life count that ticks down to a
  game over. (A game can still author its own `ui.json` HUD, e.g. lives hearts, on
  top of the built-in bar.) Each death also **flashes a message** (default "You
  died") , set **Game settings ▸ Death msgs** to your own text, or several
  separated by **|** to show a random one each time (e.g. `You died | Wasted |
  Try again`). This is separate from the final game-over **Lose text**.

- **Driving (racing)** , select the player and turn on **Properties ▸ Vehicle ▸
  Drive a vehicle** to make the player **drive an arcade car** instead of walking:
  forward/back = accelerate / brake / reverse, left/right = steer (only while
  moving, and inverted in reverse). It has momentum (it coasts to a stop , tune
  with **Grip**) and slides along walls. It also **drifts**: hold **Space** for
  the **handbrake** to break the back loose. Tune the slide with **Side grip**
  (sideways hold in corners , high = kart-like and planted, low = loose and
  drifty) and **Handbrake grip** (the side grip while Space is held , low = big
  slides). A hard drift automatically **kicks up grey tyre smoke and lays dark
  skid marks** on the road at the rear wheels (the marks hold, then fade after a
  few seconds); AI racers do the same. Toggle each off with **Drift smoke** and
  **Skid marks** in the Vehicle properties (both on by default). A drift also builds a **drift score** shown top-right while driving:
  the number climbs during a slide (the live combo shows as `+N`) and **banks**
  into the total shortly after you straighten out, so linking slides keeps a combo
  going. It resets each time you press Play. Set **Max speed / Accel / Brake / Reverse / Turn rate** to taste, and
  give the player a car model. Driving uses
  a **chase camera** that stays behind the car and follows where it points, so
  forward is up the screen. Press **C** to cycle the camera views (see below);
  this works on foot too. If the car looks like it drives sideways, its model's
  nose is not aligned with the drive direction: set the player's `model_yaw`
  (try 90 / 180 / 270).

- **Game camera views (cycle with C)** , the in-game camera is a LIST of views
  the player cycles with the **C** key. Select the **Game camera** (in the tree
  under your game) and open **Properties ▸ Views**: **Add view** for each camera,
  give it a **Mode** (**Follow** = third-person behind the player, or
  **First-person** = at the head/cockpit). A **Follow** view has **Distance**,
  **Height** and **Side** (a lateral over-the-shoulder offset), plus **Look at**
  (the point it aims at, as right / up / forward from the player , raise Forward
  to look down the track); a **First-person** view has **Eye height**, **Forward**
  and **Side** (place it in a cockpit or out of the model). Both have a **Look
  angle** (tilt the view further, + down / - up) and a
  **FOV**. **Look through** puts the editor view at the camera to preview what the
  player sees (with the game's aspect-ratio letterbox); move the mouse to jump
  back out. The selected camera is highlighted in the viewport. In Play, **C**
  steps through the cameras in order. With no views defined the engine uses a
  built-in third-person + first-person pair (the classic C toggle). Saved to
  `style.toml [[camera.views]]`; applies on reload.

- **Time limit (time-attack)** , set `[rules] time_limit` (seconds) in `style.toml`
  (or ask the AI for "a 60-second time limit") to make a beat-the-clock level: a
  corner **M:SS countdown** ticks down and the game is **lost at zero**. `0` (the
  default) = no limit.

- **Race track (generated)** , the easiest way to get a clean track is to ask the
  AI for one (e.g. "make an oval race track, 3 laps"): it emits a single **track**
  object and the engine generates a correct looping road plus evenly-spaced,
  oriented gates (a start/finish + the ordered checkpoints), so nothing comes out
  skewed or overlapping. The track's `size` is the loop's [x, z] extent, `width`
  the road width, and `gates` the number of gates besides the start/finish. Set
  **Vehicle ▸ Laps** (or let the prompt do it) to finish after N laps.

- **Race track (manual gates)** , or build a course by hand and mark gate objects
  as **Race gates** to make a lap.

- **Racing opponents** , add cars and set their **AI ▸ Racer** (or ask the AI for
  "3 opponents") to make them drive around the track's gates. They follow the same
  loop as you; face them toward the first gate with the object's rotation. Each
  opponent shows its **name on the finish leaderboard** , that is just its object
  name, so **rename it in the tree** (for example "Rival" or "Blue Team") to give
  it a proper racer name (unnamed ones fall back to "CPU 1", "CPU 2", ...). Select an object, open **Properties ▸ Gameplay ▸
  Race gate (track)** and give it a **Gate order**: **0 is the start/finish
  line** (place it where the player starts, facing gate 1), then **1, 2, 3, ...**
  around the loop in driving order. The player must pass the gates **in order**
  (so they cannot cut the track) and **re-reach the start/finish** to complete a
  lap; passing a gate also moves the respawn point there, so falling off puts you
  back at the last gate. Because the order is enforced, **driving backward never
  counts** (you would reach gates out of turn), so there is no separate "wrong
  way" rule to configure. Set **Properties ▸ Vehicle ▸ Laps** to the number of
  laps that **finishes (wins) the race** (**0 = endless**, laps are just counted).
  The engine shows a **Lap X / N** readout in the corner, with each lap's **time**
  under it (`L1 31.2`, the fastest lap marked `*`) plus the running current lap.
  A gate is a **single object** (no child posts to build): pick its **Shape** in
  **Properties ▸ Gameplay** , **Line** (a wide thin gate across the road),
  **Box** (an oriented zone), **Ring** (a flat circle on the ground) or
  **Sphere** (a 3D ball you fly through, for a flight game). The engine draws the
  shape in both the editor and while playing, so you can see and place the gates.
  Its **Scale** sets the size (default is a ~10-wide gate). Tip: lay a **looping**
  road of floor pieces first, then drop the gates around it.

- **Moving platform** , a solid object that moves along a route and **carries
  the player** standing on it (lifts, moving ground). Make one by giving a solid
  object **waypoints** but **no** enemy behavior (with a Chase/Patrol behavior the
  same waypoints make a moving enemy instead). Author the route in **Properties ▸
  Waypoints** (add/remove points, type each XYZ), or ask the AI ("a platform that
  moves up and down"). With the object selected, the route is drawn in the
  viewport and you can **drag the waypoint spheres** on the ground to reposition
  them (the first point is the object's own position, moved with the normal
  gizmo). The same applies to a **Patrol** enemy's route.

- **Hazard (spikes/lava)** , an object that **hurts the player** while they stand
  in or touch it. Turn on **Properties ▸ Gameplay ▸ Hazard** and set the
  **Damage**; like enemy contact, an invulnerability window stops instant death.
  A hit also **knocks the player back** off the hazard , tune the shove with
  **Knockback** (0 = damage only). Use a flat floor tile for a lava pool (make it
  non-solid to wade in) or a cube for spikes. Give a hazard **waypoints** to make
  it a **moving** hazard (sweeping spikes/saw). Give it a **Spin** (see below) to
  make a **rotating** saw/blade (combine both for a saw that sweeps AND spins).
  Author it in the UI or via the AI prompt ("a pool of lava that damages the
  player", "a spinning saw blade that patrols the corridor"). In the editor the
  **damage area shows as a red box** (matching exactly where a hit lands), so you
  can see and size it; a moving hazard's red box follows it.

- **Spin (rotating objects)** , any object can spin continuously in Play. Select
  it and set **Properties ▸ Transform ▸ Spin °/s** (degrees per second about X, Y
  and Z): a saw blade spins about X or Z, a turntable or fan about Y. The spin is
  **purely visual**, the collision/damage box stays put, so size the object to
  cover its spinning reach. Absent or all-zero = no spin.

- **One-way platform** , a platform the player can **jump up through from below**
  and **land on top** of when falling, and that never blocks sideways movement.
  Mark a thin object (`one_way: true`, e.g. a flat floor tile). Great for layered,
  ascending platforming. Author via the AI prompt or `world.json`.

- **Custom collider** , by default an object's collision box is derived from its
  mesh, so a model (a tree, a rock) gets a coarse cube. Turn on **Properties ▸
  Collider ▸ Custom collider** to set a tight box (half-extents + offset, scaling
  with the object); it shows as an orange box in the viewport. The box overrides
  the mesh collider for solid/ground collision, and it collides even if Solid is
  off (the collider IS the collision shape). Keep the offset at 0 to sit on the
  object; a Y offset floats the box. Shape options (sphere/capsule) and a
  low-poly collision mesh are planned.

- **Enemy hit shape** , a **skinned enemy** (an animated humanoid model) is hit
  against **per-bone hitboxes** built automatically from its skeleton: a shot is
  tested against the live bone positions, so a hit lands where you actually aimed on
  the moving body. Each region is **weighted**: a **headshot deals 2x**, a torso hit
  **1x**, and an arm/leg graze **0.5x**. This is the same idea as an Unreal **Physics
  Asset** / Unity per-bone colliders. A model **without a skeleton** (a static mesh)
  falls back to a single **body CAPSULE** (feet to head) so grazing a tall character
  still registers; you can CONTROL that capsule with a **Custom collider** (above) ,
  its X/Z half is the hit RADIUS, its Y half the HEIGHT, its offset the placement
  (the orange box shows it). **Press F4 while playing** to SEE the per-bone hitboxes
  as colored spheres on each enemy , red = head (2x), amber = torso (1x), blue =
  arm/leg (0.5x) , so you can check where shots register.

- **Tuning the per-bone hitboxes (sidecar file)** , the auto boxes are a good
  default, but you can OVERRIDE them per model with a **sidecar file** next to the
  `.glb`: `models/<name>.hitboxes.json`. It applies to **every** enemy using that
  model (author once, reuse everywhere , like an Unreal Physics Asset). List the
  boxes you want; each binds to a **bone** and sets its region / damage / size:
  ```json
  {
    "boxes": [
      { "bone": "Head",    "region": "head",  "damage_mult": 2.5, "radius": 0.18 },
      { "bone": "Spine02", "region": "torso", "damage_mult": 1.0 },
      { "bone": "LeftLeg", "region": "limb",  "damage_mult": 0.4 }
    ]
  }
  ```
  Only `bone` is required; `region` (`head`/`torso`/`limb`), `damage_mult`, `radius`
  (world units) and `offset` (`[x,y,z]` in the bone's frame) fall back to the region
  default when omitted. A model with a sidecar uses **only** the boxes you list (so
  you fully control the shape); a model **without** one keeps the auto boxes. The
  file is optional and hot-swappable , edit it and reload. Use **F4** in Play to see
  the result. (Editing the boxes visually in the editor is a later step.)

- **Shootable buttons / switches / explosives** , make any object react to being
  SHOT with an **`on_shot`** behavior rule (in `behaviors.json`, which the AI prompt
  can author for you): `{ "trigger": { "type": "on_shot", "target": "gate_button" },
  "action": { "type": "despawn", "target": "gate" } }` , shoot the button to remove
  the gate. It composes with **any** action (`win`, `set_state`, `play_sound`,
  `start_dialog`, ...), so a shootable target can win the level, open a door, arm a
  trap, and so on. Give the target a **Custom collider** so the shot has a box to
  hit. Fires once.

- **Explosive barrels / area damage** , the **`explode`** behavior action deals
  damage in a radius instead of to one target. Everything within `radius` of the
  blast centre takes flat `damage` (no falloff) and dies exactly like a shot; a
  short impact burst + an `explosion` sound play at the centre. `center` is an
  entity id whose **live** position is used (omit it to blast at the player), and
  `hurt_player` (default `true`) can be set `false` for a blast that only clears
  enemies. Combine it with `on_shot` on the **same** barrel id for a classic
  shootable explosive barrel: `{ "trigger": { "type": "on_shot", "target":
  "barrel_a" }, "action": { "type": "explode", "center": "barrel_a", "radius": 4,
  "damage": 100 } }` , shoot the barrel, the blast takes out the crowd around it.
  Add a second rule that `despawn`s the barrel if it should disappear. Map an
  `explosion` sound in `audio.toml` for the bang.

- **Player collider** , the **player** now has the same **Collider** section. By
  default the player uses a built-in size (a fixed radius + standing height), shown
  as a green cage when selected; that size does NOT scale with the model, so on a
  scaled or imported character the green cage will not match the art. Turn on
  **Custom collider** to set an explicit box you size and place (orange, scales +
  offsets with the player): its **X/Z half** becomes the wall/ground collision
  radius and its **Y half + offset** sets what the player is blocked by, so the box
  and the model finally line up. The character still stands on the ground at its
  normal height (the box is the wall-collision shape). Applies on reload.

- **Forgiving jump** , the platformer jump has built-in feel helpers (no setup
  needed): **coyote time** (you can still jump for a split second after walking
  off an edge), **jump buffering** (a jump pressed just before landing fires the
  instant you touch down), and **variable height** (tap for a short hop, hold for
  a full one). Tune via `jump_height`/`gravity` in `style.toml [movement]`.

More complex logic (triggers, conditions, actions, dialog) lives in the game's
behavior rules, which the AI prompt can author for you.

## NPCs, dialogue and quests

Characters can talk, and conversations can hand out quests. This lives in a
`dialog.json` file alongside your level. Author it three ways: the structured
editor in **Edit > Dialogue & quests...** (Dialogs / NPCs / Quests tabs), by
asking the AI prompt (see _The AI prompt and agents_), or by editing the file by
hand. All three validate against your level on save.

- **Speech bubbles (barks).** Bind a world entity to short lines that show in a
  bubble over its head. With `ambient` on, the character babbles to itself all
  the time; otherwise the bubble only appears when the player comes near, to draw
  them over.
- **Talking.** Give an NPC a `dialog` and walk up to it: a **Press E to talk**
  prompt appears, and **E** opens the conversation. Pick replies with the
  **number keys**.
- **Quests.** A dialog choice can start or complete a quest, or set a flag that
  your behaviors react to. Open the **quest log** in-game with **J**; it lists
  active and completed quests.

The pieces (in `dialog.json`): `dialogs` (the conversation trees), `npcs` (bind
an entity by id to a `dialog` and/or a `bark`), and `quests` (id + title). A
choice may carry `set_state`, `start_quest` or `complete_quest`.

## Grouping and hierarchy

Make one object **follow** another so they move together: in Properties ▸
**Hierarchy**, set **Follows** to a parent. The child nests under the parent in
the tree. A common pattern is to group props under an **empty** and have the
empty follow, e.g., the player or a moving platform.

**Multi-part models are grouped for you.** When you prompt the level designer to
build something made of several pieces (a car, an animal, a machine), it creates
an **empty (null) parent** and puts every part under it, with the parts
positioned relative to the null. So the whole model is **one group**: select the
empty and move/rotate it and every part follows. The parts stay non-solid
(visual composition); the null is the handle you grab.

## Testing your game (Play)

Press **Play** to run the game right inside the editor, then **Stop** to return
to editing (the player resets to its spawn). In-game controls:

| Action | Key |
| --- | --- |
| Move | W A S D |
| Sprint | Shift |
| Jump | Space |
| Look | Mouse |
| Attack | Left mouse button |
| Interact | E |
| Release cursor | Esc |

## The AI prompt and agents

The right-hand **Prompt** panel is how you build with words. Describe what you
want and the engine routes it to the right specialist agent, you do not pick by
hand (the **Auto** mode). There are manual **Level** and **3D Model** modes too,
for when you want to force one.

The agents:

- **Level designer** , edits the level (geometry, enemies, collectibles, lights,
  triggers, win/lose). Additive: it keeps what is already there. It can also turn
  on the **game mode** a request implies (e.g. "make a race track, 3 laps" turns
  on driving + laps; "a shooting arena" turns on ranged combat). For safety it may
  only change gameplay fields (vehicle, combat, player health/lives/respawn), never
  the rest of `style.toml`, so you can still tweak look/feel yourself afterwards.
- **Models** , generates a single 3D model from a description (via a 3D provider
  such as Meshy) and drops it into the scene. See _Importing models and
  textures_.
- **Dialogue & quests** , writes conversations, NPC barks and quests
  (`dialog.json`). See _NPCs, dialogue and quests_.
- **Extension** , when you ask for something the engine cannot do yet, it does
  not fail or guess; it writes a reviewable spec into the game's `extended/`
  folder for implementation.

Each agent is an **editable instruction file** in the `agents/` folder (the same
format as a markdown doc: a little front-matter plus the prompt). Open
**Edit > AI agents** to read and tweak any of them; changes take effect on Save.
You need an Anthropic API key in **Settings** for the agents, and a 3D-provider
key for model generation.

## The game front-end: splash, menu, pause

A packaged NJORD game launches with a proper front-end (this appears in the
exported game, not in the editor's Play):

1. **Splash screens** , the built-in **NJORD** splash, then any custom splash
   images you add, each shown for a few seconds (skip with Enter/Space/Esc).
2. **Main menu** , **New game** (Enter), **Load game** (L), **Options** (O), or
   **Quit** (Q/Esc).
3. **Pause** , press **Esc** in-game to pause; **Esc/Enter** resumes, **S** saves,
   **O** opens Options, **Q** quits.

### Options (volume, resolution, controls)

Press **O** from the main menu or the pause menu to open Options. Use **Up/Down**
to pick a row, **Left/Right** to change a value, **Enter** to rebind a control
(then press the new key), and **Esc** to go back. You can set:

- **Master volume** , scales all game sound.
- **Resolution** , cycles through common window sizes.
- **FPS counter** , toggle a small frames-per-second + frame-time readout in the
  top-right (off by default). The frame time makes stutter visible: a steady ms
  with occasional spikes is hitching, not a low average.
- **Controls** , rebind forward / back / left / right / jump / sprint / interact
  to any key.

Settings are saved to `settings.json` next to the game's exe and re-applied next
launch. (First cut: keyboard rebinds only, no mouse-button or gamepad rebind or
fullscreen toggle yet. Like save/load, Options is in the packaged game, not the
editor's Play.)

### Save and load

In the packaged game, press **S** in the pause menu to save, and **L** on the
main menu to load. The save (`savegame.json`, next to the game's exe) captures
the player's progression: position, score, lives, health, state flags, quests and
which collectibles you have already picked up (so they stay collected on load).
Limitations (first cut): it assumes the current level (multi-level "which level"
is not saved yet). Save/load is the packaged game only, not the editor's Play.

### Custom splash screens

Add intro images (DOOM/Quake style) in your game's `style.toml`:

```toml
[shell]
njord_splash = true                     # show the NJORD splash first (default)
splash_images = ["splash/intro.png"]    # your images, shown in order
splash_seconds = 2.0                    # seconds per splash
```

Put the images in your game folder (e.g. `splash/intro.png`).

### Window and resolution

Control the packaged game's window in `style.toml`:

```toml
[window]
width = 1280       # resolution
height = 720
vsync = true       # cap to the monitor refresh (smooth, low input lag)
fullscreen = false # borderless fullscreen
```

Keep `vsync = true` unless you have a reason not to, rendering uncapped feels
laggy even at very high frame rates. In the editor, pressing **Play** shows a
frame at this aspect ratio (dimming the rest) so you see exactly what the
packaged game will show.

**Frame-rate independent physics.** The gameplay simulation (movement, gravity,
jumping, enemies, platforms) runs at a fixed 60 Hz internally, separate from how
fast the screen draws. That means jump height and movement feel exactly the same
on a 60 Hz laptop and a 144 Hz monitor, and the game behaves reproducibly (the
groundwork for future multiplayer). Rendering is smoothly interpolated between
simulation steps, so motion stays fluid at any refresh rate above 60 fps. There
is nothing to configure; the fixed tick rate is not yet exposed in `style.toml`.

## Packaging: Build and export

**File ▸ Package game (Build & export)…** builds a portable, double-clickable
copy of your current game into a `dist/<game>/` folder:

- `<Your game>.exe` , double-click to play (named after your game, with the
  NJORD icon).
- `Play.bat` , a friendly launcher.
- your game's data and assets.
- `README.txt` , the controls.

The first build can take a few minutes; later ones are fast. When it finishes,
the `dist/<game>/` folder opens automatically. Hand it to anyone, no install or
dev tools required.

**Custom exe icon:** drop an `icon.ico` in your game's folder and it is embedded
into the packaged `.exe` (shown in Explorer and the taskbar). Without one, the
default NJORD icon is used.

## Settings

**Edit ▸ Settings**:

- **API keys** , NJORD uses your own AI keys (BYOK) for the prompt and content
  generation. Keys are stored securely in your OS credential manager, never in
  the project.
- **External editors** , choose which program opens models and images for
  right-click external editing.

## Controls reference

### Editor

| Action | Control |
| --- | --- |
| Orbit camera | Left / middle drag |
| Pan camera | Right drag |
| Zoom | Scroll wheel |
| Frame selection | F |
| Snap to a view (Top/Front/Side…) | Click an axis on the orientation gizmo, top-right |
| Move / Rotate / Scale tool | W / E / R |
| Snap while dragging a gizmo | Hold Ctrl |
| Undo / Redo | Ctrl+Z / Ctrl+Y |
| Save | Ctrl+S |

### In-game

| Action | Control |
| --- | --- |
| Move | W A S D |
| Sprint | Shift |
| Jump | Space |
| Look | Mouse |
| Attack / shoot | Left mouse button |
| Aim (ranged games) | Right mouse button |
| Reload (ranged games) | R |
| Camera: first/third person | C |
| Talk to a nearby NPC | E |
| Dialog choices | Number keys 1-9 |
| Quest log | J |
| Animation debug overlay (editor) | F3 |
| Per-bone hitbox overlay (editor, Play) | F4 |
| Pause (packaged game) | Esc |
| Options (packaged game) | O (main/pause menu) |

The move/jump/sprint/interact/attack keys are the defaults. A game can set them
with a `[controls]` section in `style.toml` (key names like `W`, `Space`,
`ShiftLeft`, `E`, or `Mouse:Left` for attack), and the player can rebind the
keyboard actions in the in-game **Options** menu (saved to `settings.json`).

---

_NJORD is a product of NJORD Studio AB._
