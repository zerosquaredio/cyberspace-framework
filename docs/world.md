# Cyberspace Framework: One World at Every Level

Status: Ideas (discussed 2026-11-25)
Last updated: 2026-11-25

How one shared world can be seen, used, changed and programmed by everyone
in it, whatever their client: a text terminal, a 2D map in a browser, 3D in
a browser, or full VR. It expands the [vision](vision.md)'s sections on
assets (§7) and on the common space (§8), and changes one direction there:
the text level of the common space is a MUD rather than IRC (§4 below).

## 1. The goal: a MOO at every level

MUDs, and especially MOOs and MUSHes such as LambdaMOO, were shared worlds
that their own people built and programmed from inside, in text: `@dig` a
room, `@create` an object, write its verbs. The framework wants the same at
every level up to full VR, as far as possible:

- **One world,** shared by everyone in it, whatever their client.
- **Everyone can take part** at the level their client supports, and nobody
  is shut out of what is happening.
- **People change and program the world** from inside it, at any level,
  within the permissions its owners set (vision §8.1, §8.4).
- **Assets come from anywhere:** BBSes, their users, other VR platforms,
  open-source games, and are made to fit together.

## 2. One world, drawn many ways

The rule underneath everything: **meaning lives in the hub's model; looks
live in each view.** The hub holds one model of the world (the live model,
vision §8.3, extended with the world's places and objects). Each client
draws that model at its own level.

### 2.1 Places, plans and positions

The world needs a shape every level can show:

- **Places** (rooms, areas, a leaf's zone) and the **exits** between them.
  This is all the text level needs.
- **A floor plan with heights** inside each place, as Doom's maps were.
  The 2D view draws it from above; the 3D views raise it into floors and
  walls. The map and the 3D world are then the same data, not two things to
  keep in step. Overhangs, bridges and anything else a plan can't express
  are 3D objects placed on it.
- **Objects and people placed on the plan.** People whose client has no
  positions, such as text callers, still have one: the hub places them
  (beside whoever they are talking to, or at the place's entrance), and they
  can move to named spots.

### 2.2 Levels

Levels differ in two ways: the **shape** of what's drawn (text, flat or 3D)
and the **budget** of the device drawing it. "Basic 3D in a browser",
"low-quality VR" and "full VR" use the same kind of content, glTF models and
textures, at different budgets; VR draws twice, at 72 to 90 frames a second,
so it uses a budget one class lower than the same device on a screen, and
adds comfort rules (no flashing, true scale).

The level is **chosen per element, not per scene.** Doom drew a 3D
environment with sprite people; modern engines draw far-away people as flat
"impostors". So a client picks a version for each thing it draws, from its
budget and the distance, and the levels below are presets:

| Preset | Runs in | Environment | People and objects |
|---|---|---|---|
| **Text** | A terminal, a BBS door, a MUD client | Places, exits and descriptions | Names, descriptions, ANSI |
| **Map** | Any browser, phones | The floor plan from above | Icons, sprites |
| **Doom-style** | Any browser with WebGL | Simple 3D from the plan | Sprites facing the viewer |
| **3D Light** | Phone browsers, standalone headsets | 3D, small budget | Models, sprites at a distance |
| **3D Standard** | Desktop browsers | 3D, medium budget | Models |
| **3D Full** | PC VR, Overte | 3D, large budget | Models, full avatars |

Budget classes are figures, not hardware names (vision §8.3): triangles,
texture memory, draw calls, download size. VRChat's performance ranks are a
ready-made starting point: a good avatar on a standalone headset is about
10,000 triangles, and on a PC about 70,000. A client drops a class when its
frame rate falls.

The Doom-style preset runs on almost anything and suits the grid style
(§8), so it may be the best first 3D view.

### 2.3 Choosing a version

For each thing it draws, a client uses the first of:

1. the asset's own version for this level;
2. a version the baker derived (§7);
3. a lift of a lower version: a sprite on a billboard, an emblem on a
   default shape, a picture on a default body;
4. the slot's default (§6.5).

A themed view has its own order, through the theme's forms (§6.4).
Either way, the client then applies the style cascade (hub, zone, object, viewer; vision §8.8)
and the budget: the avatar budget (vision §7.4), sprites at a distance, a
lower class when it struggles. A place never has holes for some visitors.

## 3. Shared reality

Two people at different levels never see the same picture. They can agree
completely on what is there and what is happening. **Shared reality is
shared facts and meaning, not shared pixels.**

- **One set of facts, from the hub:** who is present and where, what
  objects exist and their state, the environment's state (lights down for a
  film, a door open). One clock; the hub orders events. Views only draw
  them.
- **One coordinate system for everyone,** text callers included (§2.1). So
  everyone agrees who is near whom, and what depends on that works alike at
  every level: groups, nearby talk, spatial voice. When a VR visitor walks
  up to a text caller, the caller reads "Ana walks up to you", and Ana sees
  the caller standing there.
- **Expression translates both ways:**
  - Rich to simple: a VR user's wave becomes a `waves` event, which a text
    caller reads as "Ana waves at you"; pointing becomes "Ana points at the
    chessboard". References are always to things in the model, never to
    pixels.
  - Simple to rich: a text caller's `wave` plays a wave on their avatar for
    3D viewers, and what they type appears as a speech bubble.
  - Full body poses from VR tracking go only to nearby 3D clients; everyone
    else gets the named gestures, which are small and readable at every
    level.
  - Voice is positioned for 3D viewers. Transcribing it for text callers
    makes a record, so it needs the speakers' consent.
- **Identity stays recognizable:** each person has anchors every version
  keeps: name, color, and an emblem or icon. The baker derives versions from
  one source, so a person's sprite, model and ANSI read as the same person,
  and restyling never removes the anchors.
- **Everyone can see how others see.** Each person's level is a fact in the
  model, shown as a small marker ("in text", "on the map", "in VR"), so a VR
  user knows a text caller won't see their gesture in detail.
- **Personal changes never change facts.** A personal view can restyle
  anything but can't move, add or remove what others see; hiding someone is
  blocking, which has its own rules (vision §8.7). A client shows when its
  user is looking through a personal style, so nobody mistakes it for what
  everyone sees.
- **One authority per object** (vision §8.8): every client sends verbs to
  it, shows the result at once, and corrects it if the authority refuses.
  Two people can't both take the same chess piece, whatever their levels.
- **Nothing that matters exists at only one level.** Anything people need
  in order to take part (a door, a sign, a game piece, a status) must be
  readable down to text. Higher levels add beauty, never meaning. This also
  makes the world accessible.

Experiences of the same place won't be equal: a text caller can't
appreciate a sculpture, and fast physical play (catching a thrown ball)
belongs in games with their own sessions (vision §8.3, §9). The aim is that
nobody is shut out, and everyone agrees on what happened.

Precedents: Radegast, a text-only client for Second Life; Mozilla Hubs,
with phone, desktop and VR users in one room; VRChat, which shows desktop
users as such; MUD clients with maps.

## 4. The text level: a MUD

The text view of the world is a MUD, and it makes a better model for the
hub's chat than IRC alone: a MUD already contains IRC.

| MUD | The world |
|---|---|
| `say` | Talk heard in this place (spatial voice is the same idea in 3D) |
| Channels | Hub-wide channels: the hub's chat room today |
| `tell` | Direct messages |
| `emote` (IRC's `/me` came from MUDs) | Named gestures, translated to other levels (§3) |
| `look`, `look Ana` | A place's description, a person's description |
| `go north`, exits | Moving between places, through the same exits the 3D views walk through |
| `who` | Presence |

**Descriptions are the text version of every asset** (§6): a person's short
line on arrival ("a lanky figure in a neon jacket") and longer one for
`look`, an object's description and state ("the screen shows a chess game,
white to move"), a place's room description. The same text is what screen
readers read in the 2D and 3D views. The baker can draft descriptions from
renders, for the author to edit (§7). Descriptions are assets like any
other: rated, length-limited, and reportable if they misrepresent what they
describe.

**How it fits the hub's chat.** The hub's chat today is one room on an IRC
server (Nexus Lounge hub plan H15), reached from browsers and BBS doors. IRC
can stay the transport: each place gets its own IRC room, the hub moves
people between rooms as they move between places, and today's room becomes
a hub-wide channel. The MUD view is commands and descriptions on top: `look`
and `go` read the live model, `say` goes to the place's room. If IRC gets in
the way later, the hub can carry talk itself without the views changing.

**MUD clients and BBSes.** MUDs send structured data beside the text with
GMCP; clients such as Mudlet use it to draw maps, so a MUD client could show
the hub's places with no work on its side. A BBS door into the hub becomes
a MUD, which suits BBS callers.

## 5. Building and programming

As in a MOO, changing the world is editing the model, and each level edits
what it can show:

| Level | Editing |
|---|---|
| Text | Naming and describing, settings, `@dig`/`@create`-style commands, writing code |
| Map | Drawing the floor plan, placing and moving objects, choosing assets, fills and effects. Doom's levels were always made in a 2D editor |
| 3D and VR | Placing by hand, height and scale, previewing exactly |

- **Objects declare verbs** (use, open, enter, sit, talk, take), following
  the vision's split of an object into an interface and a backend (§8.8).
  Each level offers the same verbs its own way: a command or menu in text,
  a click on the map, point and press in 3D, reach and grab in VR. Movement
  likewise: `go` in text, click a spot on the map, walk in 3D.
- **Code written once works at every level:** an object's rules are
  portable WebAssembly (vision §8.8), and its looks respond to **named
  states and events** (§6.3), never to drawing code. Where an author needs
  logic in its looks, the code produces events and settings ("set glow to
  0.7") and each level draws them.
- **Permissions** follow the vision: personal changes need nobody's
  permission; shared ones need the owner's, the hub admin for the common
  space and the sysop for a leaf's zone (§8.1, §8.4). Code from other people
  is sandboxed and metered (§8.8).

## 6. Assets

Who provides what: leaves (BBSes) provide their own looks and art, and can
offer assets to users; users provide their own avatars; the hub provides
the common space's look and the defaults. Where assets live, how a person
chooses avatars per place, ratings and budgets are in the vision (§7).

### 6.1 Kinds of asset, by how well they scale

| Kind | Native at | Up | Down |
|---|---|---|---|
| **Parameters:** a palette, a named effect with settings ("glow", "pulse", "scanlines", "glitch"), a named shape from a shared library ("tower", "terminal", "hex") | Every level | Each level's renderer implements the vocabulary: a glow is a colored border in text, a filter on the map, an emissive material in 3D | |
| **ANSI art** | Text | Drawn with a CP437 font on the map; a texture on signs, walls and screens in 3D, or each character cell raised into a block | |
| **Images:** icons, emblems, textures, patterns, panoramas | Map | Textures, signs, billboards; a panorama becomes a skybox | ANSI by conversion (chafa), worse than hand-made |
| **Models:** glTF, VRM, voxels, splats | 3D | | Simplified for lighter classes; rendered to sprites and icons for the map; a splat world rendered to a panorama |
| **Descriptions** | Text | Alt text and screen readers at every level | |
| **Code:** shaders, view code | Its own runtime | Doesn't port; prefer named effects | |

**Lowering is automatic; raising isn't.** A model renders down to a good
icon, but an icon doesn't become a good model by itself. Going up, the
system uses defined lifts (§2.3); AI image-to-3D tools can be an optional
step an author reviews, never automatic. The rule for authors: **make the
highest level you care about; the tools derive the lower ones, and anything
missing above falls back to a lifted default.** Every asset has at least a
flat image and a description, made by hand or derived.

### 6.2 ANSI art

BBSes have a great deal of ANSI art: every board has logon screens and a
logo, and ENiGMA½ ships over a hundred files. It should be used liberally
across the hub:

| Slot | ANSI use |
|---|---|
| Common space | The hub's header and border art; a gallery rotating leaves' art through it |
| A leaf | Its logo as its emblem; its logon screen shown when someone walks up or hovers, so it shows itself before anyone calls it |
| A person | A small ANSI signature or portrait, like the old taglines |
| Objects | Billboards, murals, a gallery wall of a leaf's art pack |

- **Sanitized:** an ANSI file is a stream of terminal commands. The hub
  parses each one and keeps only characters, colors and cursor movement;
  anything else (window titles, terminal queries) is dropped.
- **SAUCE** metadata gives the author, group, width, font and iCE colors,
  which the art needs to look right, and credit. Much BBS art comes from art
  groups (ACiD, iCE and others), so a leaf publishing art it didn't make
  names the artist. SAUCE has no licence field, so the record carries one.
- **Small:** a few KB each, so blobs in the leaf's AT repo, published with
  its listing (whether PDSes accept the media type is to be checked).
- **Animations** play back at a set rate rather than a modem's speed.
- **Rating** is by the sysop, plus reports: nothing can judge what ANSI art
  depicts automatically.

Tools: libansilove (BSD) renders ANSI to images; the hub's web terminal
already draws it live.

### 6.3 Versions, states and effects

One asset can carry several hand-made versions: a person's avatar might
have an icon for the map, an animated sprite sheet for low-budget 2D, a VRM
model for 3D and VR, a description and an ANSI portrait. They work together
through **named states and events**, not shared code:

- **States and events** come from the model: `idle`, `talking`, `typing`,
  `away`, `mentioned`, `entered`, plus named gestures (`waves`, `nods`).
  Where a standard exists, the names follow it: VRM's expression names for
  faces and speech (`happy`, `blink`, `aa`, `ih`, …).
- **Each version responds in its own way.** On `talking`, the sprite plays
  its own mouth loop and the VRM moves its lips with the voice. Animations
  unique to one version are expected. A version with nothing for a state
  shows `idle`.
- **Effects are declared:** "on `mentioned`: glow, cyan, 2 s" uses the
  shared vocabulary (§6.1), and each level draws its own glow. So one effect
  works on all versions.

Most people will make one version, so the baker makes the rest (§7): from a
3D model it renders the sprite sheet (each animation from 8 directions, as
Diablo's and Donkey Kong Country's sprites were made) and icon, so they
match; from a sprite it makes a flat "paper" figure for 3D (as Paper Mario
does). Hand-made versions always override derived ones.

### 6.4 Roles, forms, events and themes

**An object has one role and many forms.** A BBS in the world has the role
of an entrance to a leaf; its form differs by level, and can differ by zone
or theme: an exit or door in text, a portal or door sprite on the map, a
building with a door, a server rack or a portal in 3D. The role is what the
model knows, following the vision's rule that every extension falls back to
a core type (§8.3). Forms are assets.

Roles are a small, fixed set, grown like the model's core: person,
entrance, exit, seat, screen, sign, container, light, decoration, and a few
more.

**A role defines its events and states.** An entrance has `online`,
`offline`, `busy`, `full`, `someone-enters` and `someone-leaves`; a place
has environment events such as `lights-down`, `film-starts` and `night`.
They are facts in the model, carrying the hub's time and a duration, so
every level plays them together.

**Each form says how it reacts,** best first:

1. made by hand for that form: the building's door slides open, the portal
   flares;
2. derived by the baker where that works: a 3D door animation rendered into
   a sprite animation (§7);
3. the role's defaults: every role ships a reaction to each of its events at
   every level, from the shared effect vocabulary, so a sysop who picks
   "building" gets the door and the lights with no work.

Someone going through a BBS's entrance:

| Level | Presentation |
|---|---|
| Text | "Ana steps through the door to Cool BBS." |
| Map | Her dot moves into the portal, which flashes |
| 3D | She walks up, the door opens, she goes in, it shuts |

The BBS going offline: "The door to Cool BBS darkens and locks"; the portal
sprite greys out; the building's lights go out.

- **Every event has a text version,** which is also its caption for screen
  readers (§3: nothing exists at only one level).
- **Environment events go through the style sheet:** place and zone styles
  react to events, as CSS does to hover and transitions. One rule for
  `lights-down` dims the text ("The lights dim"), the map and the 3D
  lighting.
- **Sound crosses levels:** MUDs have a sound protocol (MSP), so a door's
  sound can play at nearly every level and tie the presentations together.
- **A preview** shows one event at every level side by side, so an author
  can see whether it makes sense everywhere; it belongs in the baker's
  report (§7.2).

As with assets, lowering is automatic and raising isn't: a text event never
becomes a good 3D animation by itself, which is why every role carries
defaults at the upper levels.

**Themes.** Views can be themed, by a zone's owner for everyone or by a
viewer for themselves (vision §8.4): fantasy, cyberpunk, anything. A theme
doesn't need a form for every object, only **one form per role,** at each
level it supports. It is a role pack, like a roguelike's tileset (NetHack's
and Dungeon Crawl's tilesets fit any game and fall back to ASCII) or a
Minecraft resource pack (missing textures keep their defaults). In a themed
view, an object's form is the first of:

1. the object's own form for that theme, if its author made one;
2. the theme's form for the object's role;
3. the object's own form, restyled by the theme's palette and shading, so
   it at least blends in;
4. the theme's neutral placeholder for the role.

Rules that keep shared reality intact:

- **Forms fit the object's footprint.** Size and collision are facts in the
  model, so a theme's barrel stands where the original crate stood, and
  someone sitting on it in 3D sits on something in every view.
- **Forms show the role's states and events.** A themed entrance still
  shows online, offline and busy. Parts marked "don't restyle" (a sign's
  text, a logo, a game piece) stay as they are (vision §8.8); a themed
  chessboard is still readable chess.
- **People keep their identity anchors** (§3): a theme may give everyone a
  fantasy body in one's own view, but each keeps their name, color and
  emblem, and the client shows that the view is personal.
- **Text themes are cheapest:** description templates per role. The same
  entrance is "a stone archway glowing with runes" in fantasy and "a server
  rack, humming, its door lit green" in cyberpunk.

Decoration is the long tail; steps 3 and 4 handle it harmlessly. Themes are
assets like any other: made, shared and remixed, with credit through their
source links. AI-made themed versions of whole objects (the vision's cached
restyling, §8.8) could fill gaps later, but role packs made by artists are
the dependable path.

### 6.5 Slots and owners

Each thing in the world that can be decorated is a **slot**. Its owner sets
it for everyone; anyone can override it in their own view (vision §8.4).

| Slot | Owner | Text | Map | 3D |
|---|---|---|---|---|
| The common space: frame, background, environment | Hub admin | Border art, palette, room descriptions | Frame, background image or pattern | Skybox, ground, environment |
| A leaf | Its sysop | Label, color, ANSI logo, description | Shape, fill, emblem, effect | A model, or a default shape with the emblem |
| A leaf's zone | Its sysop | Room descriptions | Its plan and style | Its environment and style sheet |
| A person | Themselves | Name color, description, ANSI | Avatar image in a shape, fill, effect, sprite | VRM, or a default body with their image |
| Links, the hub itself | Hub admin | Line style | Line style, effect | Beams, effect |

**Leaves as libraries.** A leaf can publish a catalogue of assets that
people may use under each asset's licence, and items only its members may
wear (a badge, an avatar frame, a "guild tabard"), checked by the
membership the leaf vouches for (vision §6.4). It gives leaves a reason to
make assets, and people something to wear.

### 6.6 Records, delivery and checks

- **An asset is an AT record** in its author's or leaf's repo, listing its
  versions, each with its level, format, size and budget, plus a
  description, rating, licence, and a link to its source if derived (vision
  §8.8). Where it fits, follow at3d.app's record types for avatars rather
  than defining our own (vision §7.2).
- **Files:** small ones are blobs in the AT repo; large ones are hosted by
  the leaf, through its tunnel (vision §7.1).
- **The hub fetches, checks and caches** each asset by hash, and serves it
  to clients, which never load from people's PDSes. It checks that the
  format is valid, that the size and budget are under its caps, and that
  the rating fits; it sanitizes ANSI (§6.2) and strips scripts from SVG, or
  rasterizes it.
- **Licences** decide what's allowed (vision §8.8): a hub can refuse assets
  with no licence for anything other people see, while using anything in
  one's own view is a personal matter.

## 7. The baker

A tool that takes a model, image, ANSI file or other source, from
wherever it came, and derives every level's version, measures each, and
writes the asset record. It is also the vision's cached restyling (tier 2,
§8.8), so they can be one tool.

### 7.1 Where assets come from

| Source | Format | Notes |
|---|---|---|
| VRChat, ChilloutVR | Unity: FBX, textures, materials | From the **source files**, often sold on Booth and Gumroad with sources included; never ripped from uploads, which breaks VRChat's terms |
| VRoid Hub, other VRM avatars | VRM | The best fit; VRM carries its licence terms in the file |
| Sketchfab | glTF, FBX | Much is Creative Commons, labelled per model |
| Open game art | glTF, FBX, OBJ | Kenney, Quaternius and Poly Haven are CC0; OpenGameArt varies by item |
| Open-source games | Various | Luanti (formerly Minetest) and Veloren are grid-style; licences per game and asset |
| Minecraft-style skins | 64×64 PNG | A large library, and a ready-made grid-style avatar template |
| Grid art | MagicaVoxel `.vox`, sprite sheets, ANSI | Native to the grid style (§8) |
| Scans and generated worlds | Splats, photogrammetry meshes | Marble, Polycam, and the like; backdrops only |

### 7.2 What it does

1. **Import** into one canonical form: glTF, with VRM's extensions for
   humanoid avatars. Blender, run headless, handles most formats; a Unity
   package is an archive, so its FBX and textures come out without Unity.
2. **Translate** what other platforms use: Unity humanoid skeletons to VRM
   bone names; VRChat's speech shapes (`vrc.v_aa` …) and blinks to VRM's;
   PhysBones to VRM spring bones; toon shaders (Poiyomi, lilToon) to MToon,
   keeping the main texture and glow.
3. **Measure** triangles, materials, texture memory and bones against each
   budget class.
4. **Derive:**
   - lighter 3D classes: simplify meshes (meshoptimizer, gltf-transform),
     merge materials into an atlas (draw calls often matter more than
     triangles), compress textures to KTX2, drop bones and shape keys;
   - the map: render an icon and a sprite sheet, using a shared animation
     set that fits any humanoid rig, since most VR avatars bring no
     animations of their own;
   - text: ANSI from the icon, and a draft description;
   - optionally the grid style: a voxel version, a palette-mapped one.
5. **Report** each level's budget, what was dropped, previews of every
   level, and warnings ("the face lost detail at 3D Light"). The author
   reviews it, replaces what they want by hand, and publishes the record
   with its licence and source.

### 7.3 Limits

- **Automatic simplification has a floor.** A 70,000-triangle anime avatar
  cut to 10,000 mangles faces and hair; VRChat creators often make headset
  versions by hand for this reason. The baker shows the damage and accepts
  a hand-made version instead.
- **Looks shift between engines.** Shaders don't transfer; toon styles
  translate well, custom shader effects don't.
- **Platform features are mostly lost:** VRChat's menus, contacts, animator
  logic and Udon code. Expressions, speech, blinks and spring bones carry
  over, which covers what presence needs.
- **Rigging** a model with no skeleton is semi-automatic at best (Mixamo's
  and AccuRIG's auto-riggers do humanoids reasonably).
- **Licences are the hard limit.** Booth models often use the VN3 licence,
  which can restrict platforms. The baker requires a licence and source for
  every import and reads VRM's built-in terms.

**Where it runs:** mainly on the author's own computer, a desktop tool with
Blender behind it; a web version for simple cases (images, ANSI, small
glTF); on a hub only if its admin allows, since baking is heavy (vision
§8.8). A first version: glTF, VRM and FBX in; the 3D classes, icon, sprite
sheet, description draft and report out.

## 8. Cohesion and quality

Open-ended user content, as in Second Life and VRChat, looks uneven. The
places that look cohesive got there by **constraint**: Minecraft skins are
a 64×64 template; Habbo and Animal Crossing give people kits, not a blank
canvas. The levers, strongest first:

1. **A house style that forgives mismatches.** In a stylized, low-fi look,
   mismatched sources read as intentional; realism exposes every flaw.
2. **Everything drawn through the hub's style** (vision §8.8, live styles):
   palette mapping, outlines and an emissive neon treatment applied to every
   model while drawing. It does more than anything else to unify assets from
   different sources, and costs almost nothing. Avatars are restyled only by
   consent; "colors only" may be a sensible default.
3. **Kits and templates:** a base body and rig to customize (VRoid Studio
   does this for VRM), a sprite template, a library of shapes for leaves.
4. **Zones:** each leaf's zone has its own style, the common space the
   hub's. Cohesion inside a zone and contrast between them is the appeal,
   like a city's neighborhoods.
5. **Curation:** the shared slots are the admin's; featured assets and
   defaults made by good artists; contributor awards for good work (vision
   §8.8).
6. **The viewer's last say:** a "uniform style" setting that draws everyone
   in the house style.

None of this makes bad art good; automatic restyling beyond palettes and
outlines often looks poor; and every constraint costs some expression. Each
hub chooses its trade-off in its style sheet.

### 8.1 The grid style, as a default

ANSI, pixel art and voxels are one look at three levels: a voxel model
renders down to a pixel sprite, a pixel sprite to ANSI half-blocks, and ANSI
art stands up into blocks. As the default house style, it makes cohesion,
budgets and moving between levels easier at once, and ties the 3D world to
BBS art. Its resolution rises with the level, which makes it a natural
level of detail:

| Level | Grid look |
|---|---|
| Text | ANSI cells |
| Map | Pixel-art sprites |
| 3D Light | Chunky voxels, near Minecraft or Crossy Road |
| 3D Standard | Finer voxels, as in Cube World or MagicaVoxel art |
| 3D Full | Small voxels with real lighting, glow and reflections, closer to Teardown |

- Fine voxels need a capable renderer (merged faces, "greedy meshing", or
  ray-marching) to stay within budget.
- Voxel avatars animate either as rigid parts (blocky in motion) or as a
  skinned mesh made from the voxels (smooth, but the voxels stretch).
- The grid needn't be geometry: low-poly models with pixel textures (the
  PS1 look), or neon grid lines and wireframes (Tron, the old pictures of
  cyberspace), are lighter and animate normally.

So the default is a **grid style**, not "voxels": ANSI, pixels and voxels
for what people make, tied together by neon grid lines and glow from the
hub's style sheet. Hubs and leaves can lean further toward voxels or toward
smooth models.

## 9. Order

Most of the early work needs no 3D:

1. **ANSI and images on the map:** the asset record; leaves publish an ANSI
   logo with their listing; the hub sanitizes and caches it, and the map
   draws boards with their emblems and people with their Bluesky pictures.
   The effect and shape vocabulary, on the map first, and the entrance role
   with its events (online, offline, someone enters).
2. **Places in the live model, and the MUD view:** places, exits and
   descriptions; a room per place on the chat server; `look`, `go`, `say`,
   `emote`; the BBS door as a MUD.
3. **Building in text and on the map:** creating places and objects,
   drawing floor plans, verbs; then object rules in WebAssembly (vision
   §8.8's chessboard).
4. **The baker,** first version (§7.3).
5. **The Doom-style view,** then the 3D classes and VR.

## 10. Open questions

- The world model's shape: places, plans and objects as an extension of
  the live model's core (vision §8.3), and how a hub stores the world it
  holds.
- The vocabulary: roles and their events, effect, shape and state names,
  and how they grow (as model extensions do).
- The theme format: a role pack at each level, and how a zone's theme and
  a viewer's combine.
- The asset record's fields, and how far at3d.app's types cover avatars.
- The style sheet format (vision §11).
- Whether Overte can draw this model (places, plans, the shared states)
  through an adapter, or whether 3D views are our own (three.js and WebXR)
  with Overte as one client among them.
- Whether PDSes accept ANSI files' media type as blobs.
- How a text caller's position is chosen, and how they move within a
  place.
