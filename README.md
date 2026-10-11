# promptiverse

The hub world. Anyone pastes a link → it becomes a portal. Fly through a portal
and you land in that world — your prompt-generated creature travels with you.
No servers, no accounts: WebRTC P2P presence via Trystero, and your identity is
just bytes (name + avatar prompt + seed).

Live: https://joshuagwatts.github.io/promptiverse/

## v0.2 — the hub, in 3D

- three.js r160 vendored (`js/vendor/three.module.js`, TAGTOWN-style import map) —
  no CDN dependency for the engine. No shadows, low poly, capped pixel ratio:
  phones are the target.
- Real 3D creature avatars: displaced-icosahedron bodies with volume, seeded
  spikes/fins/horns, 1–3 eyes with blink, emissive shimmer, glow halo, fading
  sprite trail, mood-driven idle motion (bob/pulse/jitter/flicker). Same
  prompt + seed builds the identical creature on every device — peers render
  each other from the pose bytes. Spawn preview is a live 3D turntable.
- 3D hub: starfield points, nebula sprites, glowing hub core with orbit rings,
  upright torus portals in a ring with swirl discs + spawn animation, floating
  canvas-texture labels. Camera-relative WASD/arrows/touch-drag flight on the
  XZ plane with smooth follow camera, banking, and creature heading.
- Portals, identity, fly-through (`?pv_name=&pv_ap=&pv_avatar=`), and Trystero
  `pv-hub` presence unchanged from v0.1 — v0.1 identities keep their colors.

## v0.1 — the hub (2D, superseded)

- Canvas-2D prototype: wisp-flight hub, glow-blob avatars, same portal/presence
  protocol. Replaced by v0.2's 3D build.

## Protocol notes

- Portal room protocol (for the v0.3 Dive): peers inside the same world share
  room id `"pv-" + hash(worldUrl)` — automatic co-presence, no coordination.
- Trystero strategy: pinned `esm.sh/@trystero-p2p/torrent@0.25.4` +
  `esm.sh/@trystero-p2p/nostr@0.25.4`, dynamic import, parallel rooms
  (same approach as limboverse). v0.2 ships STUN-only; add TURN creds to
  `rtcConfig` in `bootNet()` if phone-to-phone P2P fails on carrier NAT.

## Roadmap

- v0.3 — The Dive: in-place portal travel (iframe layer) + back-to-hub button
- v0.4 — Registry in the URL hash (textable hubs) + personal collection + homeworlds
- v0.5 — Prompt pedestal: seeded world generator

Design: [DESIGN.md](DESIGN.md)
