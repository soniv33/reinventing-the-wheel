# Reinventing the Wheel

A 3D side-on physics racer where your vehicle rides on a **single wheel whose
cross-section you draw by hand**. The shape you sketch is extruded into a real
3D wheel, and its silhouette decides how fast and grippy you are on whatever
terrain is under it. Redraw at any time to adapt, and race a pack of AI drivers
left-to-right to the finish line.

It's one self-contained `index.html` — no build step, no bundler, no framework.
[Three.js](https://threejs.org/) renders, [Rapier3D](https://rapier.rs/)
(the `compat` build, with inline WASM) handles physics. Both load from a CDN via
an ES-module import map, so it deploys to GitHub Pages as a static file and runs
in a mobile browser.

## Play

1. Watch the **telegraph strip** at the top — it shows the upcoming terrain and
   weather so you can plan.
2. Hit **REDRAW WHEEL**. Time slows and a sketch overlay opens. Draw a single
   closed loop — your wheel's side profile — and **Materialise** it.
   - **Round & smooth** → fast on tarmac, hopeless in mud/ice.
   - **Spiky / treaded** → bites mud, snow, ice, and climbs; wasteful on tarmac.
   - **Wide & fat** → floats on sand; narrow shapes sink.
3. Redrawing costs ~1.2s of cut torque, so swap tactically, not constantly.
4. Beat the AI to the finish on the right by matching shape to ground.

## How the shape matters

The whole game balance lives in two heavily-commented functions near the top of
the `<script type="module">` block:

- `geometryToProfile()` reads three properties off the drawn polygon —
  **roundness**, **tread/spikiness**, and **width/footprint**.
- `shapeTerrainMultiplier()` turns *(profile × terrain × weather)* into a single
  speed multiplier, one readable line per surface. Tune balance there and
  nowhere else.

## Deploy to GitHub Pages

Push to your repo and enable **Settings → Pages → Deploy from branch**, pointing
at the branch root. Because everything is a single static `index.html` with
CDN-loaded modules, there's nothing to build. Open the published URL on a phone.

If Rapier's WASM ever misbehaves on a very old device, the physics layer is
isolated enough to swap for the lighter `cannon-es`.
