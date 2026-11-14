# 13 — Client and art pipeline

Status: draft spec. The client stack is a recommendation with a recorded alternative; see [DECISIONS.md](DECISIONS.md).

## 1. Recommended stack

**A web-first client.**

| Part | Technology | Why |
|---|---|---|
| World rendering | PixiJS 8 on WebGL2 | Runs on any WebGL2 device; mature 2D batching |
| Menus and tools | HTML and CSS with a small component library | Wardrobe, inventory, housing, and market screens are document-shaped. HTML gives text layout, scaling, localisation, and screen-reader support for free |
| Game logic shared with server | Rust compiled to wasm (`client-core`) | One implementation of the protocol, movement prediction, and craft simulation |
| Desktop | The same client in a thin desktop wrapper | Optional; the browser is the primary target |
| Phones | The same client, touch layout | After launch |

### Alternatives considered

| Option | For | Against |
|---|---|---|
| **Godot 4.7** with Rust bindings | Native performance; good 2D; text scene files; runs on 2012 integrated graphics | Rust on the web export is experimental (nightly toolchain, two wasm builds, cross-origin isolation); engine UI is weaker than HTML for dress-up and housing screens |
| **Bevy** | All Rust | No editor; weakest UI; frequent breaking releases |
| Defold, MonoGame | Light runtimes | No Rust sharing; weaker web or UI story |

Godot is the fallback if browser performance on the target hardware fails the M2 measurement.

### Costs of the web choice, stated plainly

- A browser tab uses more memory than a native 2D client.
- Garbage-collection pauses must be managed: object pools, and hot loops in wasm.
- The client source is readable by anyone. This changes nothing, because the client is not trusted ([12](12-security.md)).
- WebTransport support must be tested per browser; the WebSocket fallback is required.

## 2. Performance budget

| Target | Value |
|---|---|
| Reference low-end machine | Dual-core CPU from about 2013, integrated graphics, 4 GB RAM |
| Frame rate | 30 fps minimum, 60 fps typical |
| Memory | Under 600 MB in a town |
| First load | Under 15 MB to reach character select; the rest streams |
| Bandwidth | Under 10 kB/s typical, under 30 kB/s in a World Hunt |
| Fully animated characters on screen | 40; others drawn as simplified figures with nameplates, prioritised party, target, friends, nearest |

Every budget has an automated measurement in CI from M2 onward.

## 3. Perspective

Top-down three-quarter view.

| View | Drawn facings | Relative art cost | Housing | Telegraph clarity |
|---|---|---|---|---|
| Side-scrolling | 1 | 1× | Awkward | Good on one axis |
| **Top-down ¾** | 3 (side is mirrored) | 3× | Good | Good; floor shapes are undistorted |
| Isometric | 5 | 5× | Good | Shapes are skewed |

This choice multiplies across every garment and animation and is effectively permanent.

## 4. World sprites: a cutout rig

Frame-by-frame layered clothing costs (garments × animations × frames × facings). That does not scale to hundreds of garments. Instead:

- Characters are **cutout puppets**: a small skeleton per facing (head, torso, hips, upper and lower arms, hands, upper and lower legs, feet, plus attachment points).
- Each garment supplies **part sprites** for the bones it covers, per drawn facing: typically 6–12 small images × 3 facings.
- **Animations belong to the skeleton**, not the garment. A new animation costs nothing per garment; a new garment costs nothing per animation.
- World sprites are **stylised and small**, with one shared proportion across frames (frames differ by modest scale and part swaps). Every garment therefore fits every frame with no extra art.
- Asymmetric garments set a flag that swaps a part instead of mirroring.
- Skirts, capes, and hair use a few extra bones with simple secondary motion.

The rig format is the project's own plain-text data, so tools and agents can read and validate it, and there is no runtime licence.

### Rendering

- When an outfit changes, the client **bakes** all layers for that character into one region of a shared atlas. A character then draws as a handful of quads from one texture.
- Dyes use **mask channels**: each part has a greyscale base and a mask whose channels select up to three dye colours, resolved at bake time. No per-character shader cost afterwards.
- Bakes are cached by outfit hash. Distant or over-budget characters use a low-detail bake.

## 5. Atelier figures

- One front-facing, high-detail figure per frame, with subtle idle motion (breathing, blink, cloth sway).
- Each garment has one high-detail drawing authored on a reference frame, as a deformable mesh.
- Fitting to other frames is done by a **warp** defined once per frame pair, with manual correction only where needed. A checker flags garments whose warp distorts beyond a threshold.
- Layer order, hide-masks, and dye masks follow the same data model as world sprites.

## 6. Authoring pipeline

```
artist files ─▶ export ─▶ validator ─▶ packer ─▶ content pack + texture bundles
                             │
                             ├─ every part present for every drawn facing
                             ├─ anchor points within tolerance
                             ├─ hide-mask covers underlayers on all frames
                             ├─ dye masks valid
                             └─ atelier warp distortion within limit
```

- The validator generates a **contact sheet** for each garment: every frame, every facing, key poses, three dye sets. A human approves the sheet.
- Textures ship in a GPU-compressed container format transcoded per device.
- Bundles are per garment family and load on demand, with a neutral placeholder until they arrive.

## 7. Art production: the hard part

Six hundred patterns, each with world parts in three facings and one atelier drawing, plus fabrics, hair, faces, furniture, tiles, enemies, and effects, is thousands of hours of skilled illustration. Agents cannot do this to a shippable standard.

| Option | Notes |
|---|---|
| Commissioned artists working to the rig spec | The default assumption. Needs a style guide and budget |
| A small in-house art team | Best consistency; highest fixed cost |
| Licensed or open asset bases to start | Useful for the slice; check licences carefully, as some open sets carry share-alike terms |
| Generative tools with human clean-up | An owner decision with legal, ethical, and quality dimensions; not assumed |
| Procedural variation | Already in the design: fabric, trim, and dye multiply each drawn pattern |

The vertical slice needs about 60 patterns and is the point at which the true per-garment cost is measured.

## 8. Client structure

```
client/
  src/
    net/        connection, wasm bridge
    world/      PixiJS scene: map, entities, effects, telegraphs
    avatar/     rig, baker, atlas cache
    ui/         HTML screens: wardrobe, inventory, craft, housing, market, social
    craft/      minigame views over craft-sim
    input/      keyboard, gamepad, touch; remapping
    a11y/       accessibility settings
  tests/        unit, browser-driven, screenshot
```

Game rules never live in TypeScript. If a screen needs to know a rule, it asks `client-core`.

## 9. Testing

- Unit tests for UI logic.
- Browser-driven tests for every screen and flow, against a local server.
- Screenshot comparison with one pinned rendering backend; baselines are generated in CI only, because software rendering differs between machines.
- The headless `botclient` (Rust) covers protocol and gameplay without a browser.
- Performance tests on a throttled profile that approximates the reference machine.
