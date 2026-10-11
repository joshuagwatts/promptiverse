# promptiverse

The hub world. Anyone pastes a link → it becomes a portal. Fly through a portal
and you land in that world — your prompt-generated creature travels with you.
No servers, no accounts: WebRTC P2P presence via Trystero, and your identity is
just bytes (name + avatar prompt + seed).

Live: https://joshuagwatts.github.io/promptiverse/

## v0.1 — the hub

- Hub scene (canvas 2D, wisp-flight feel): WASD / arrows / touch-drag to fly,
  camera follows, starfield + nebulae + a glowing hub core.
- Promptable avatar: describe your creature → a deterministic generative avatar
  (same prompt + seed renders identically on every device). Remix for a new seed.
  Identity persists in localStorage; returning visits skip the spawn flow.
- Paste-a-link portals arranged in a ring; drifting through one opens the world
  in a new tab with your identity attached (`?pv_name=&pv_ap=&pv_avatar=`) —
  the hub tab stays warm underneath.
- Serverless presence in Trystero room `pv-hub` (~10Hz pose broadcast, avatar +
  name tags for peers). Fully usable solo offline — net failure is silent.

## Protocol notes

- Portal room protocol (for the v0.2 Dive): peers inside the same world share
  room id `"pv-" + hash(worldUrl)` — automatic co-presence, no coordination.
- Trystero strategy: pinned `esm.sh/@trystero-p2p/torrent@0.25.4` +
  `esm.sh/@trystero-p2p/nostr@0.25.4`, dynamic import, parallel rooms
  (same approach as limboverse). v0.1 ships STUN-only; add TURN creds to
  `rtcConfig` in `bootNet()` if phone-to-phone P2P fails on carrier NAT.

## Roadmap

- v0.2 — The Dive: in-place portal travel (iframe layer) + back-to-hub button
- v0.3 — Registry in the URL hash (textable hubs) + personal collection + homeworlds
- v0.4 — Prompt pedestal: seeded world generator

Design: [DESIGN.md](DESIGN.md)
