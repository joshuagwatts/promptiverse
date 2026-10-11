# Promptiverse — design sketch (2026-10-10)

Joshua's concept: a hub world (distinct from limboverse) where people paste links that
become portals. Flying through a portal loads the linked world *inside* your session —
you never leave the verse; your name tag and the people around you come with you.
Worlds you love get saved; your saved set travels in the link you text to people.

## The core loop

1. You open a promptiverse link → you spawn in the hub, see other people drifting
   around, see portals.
2. Someone pasted a cool world → a new portal ring appears with its name floating
   over it.
3. You fly through → the world fills your screen. Everyone who came through the
   same portal is there with you.
4. Tap the floating ⌂ button → you're back in the hub, exactly where you left.
5. You text the hub link to a friend → they arrive at the same hub with the same
   portals.

## Three primitives

**Portals.** A portal = { title, url, addedBy, addedAt }. A paste pedestal (or a
simple + button on mobile) takes a URL, validates it, and spawns a portal ring with
the title floating above it. Portals arrange in a ring around the hub plaza; new ones
slot into the next open position.

**The Dive.** Flying through a portal does not navigate away — it opens the world's
URL in a full-screen layer *over* the hub (iframe), with a persistent floating
"back to hub" button. Your identity (name, color) carries into the world via URL
params (`?pv_name=…&pv_color=…`) so worlds that read them keep your tag. Your hub
session stays alive underneath — the dive is a layer, not a departure.

**The Registry.** The hub's portal list is serialized into the URL hash
(`#w=<base64>`). Copying the URL *is* sharing your hub — no server, no accounts.
Your personal collection persists in localStorage; "add to my worlds" pins a portal
you found into every hub you open.

## Prompted worlds (the "prompt" in promptiverse)

A prompt pedestal in the hub: type a few words ("neon canyon at dusk") → a seeded
generator builds a world (palette, terrain, fog, ambient tone, floating geometry)
deterministically from prompt + seed. The world *is* a URL:
`promptiverse/world.html#s=<seed>&p=<prompt>`. Saving = keeping those bytes — the
same trick as the dawn-diffusion engine: the seed is the save file. Anyone with the
link regenerates the identical world. Saved worlds become portals like any other.

## Decisions so far (2026-10-10)

- **New repo**: promptiverse — not a limboverse wing.
- **Everything is promptable**: avatars and homeworlds included, not just worlds.

## Promptable avatar

Identity is generated, not picked. On first visit you type a prompt ("ember fox",
"glass jellyfish") and the seeded avatar engine renders your creature: body shape,
palette, glow, trail, idle motion — all derived from prompt + seed. Your avatar is
bytes: `name + avatarSeed` travels in every URL, so every world in the verse renders
*you* identically with zero accounts. v1 avatars are abstract generative creatures
(the prompt steers palette/mood/motion via keywords), not literal depictions —
literal text-to-image would need an API and kill the offline/serverless ethos.

## Homeworlds

Everyone gets a homeworld: a personal world generated from a prompt
(`world.html#home=<seed>`) — your front door in the verse. You place your own portals
there, set the vibe, and your homeworld link is your invite: text it and people land
in *your* space first, hub second. The hub keeps a "home" ring: one portal per
regular, addressed by their identity.

## Everything promptable (the ethos)

One seeded engine, multiple recipes: avatar, homeworld, prompt-pedestal worlds,
portal skins, ambient beds. Prompt + seed is the universal save format across all of
them — nothing in the verse needs a database.

## Presence

Trystero rooms keyed by portal: room id = `"pv-" + hash(worldUrl)`. Everyone through
the same portal lands in the same room with zero coordination. The hub itself is room
`"pv-hub"` (+ the registry hash, so different hub links = different hub instances).

## Honest caveats

- Some sites send headers that block iframe embedding — those portals fall back to
  opening in a new tab; the hub tab stays warm and one tap brings you back.
- Paste-anything means trust: launch is circle-of-friends; a remove/report control
  comes before any public link.
- Worlds that don't read the `?pv_name` params still work — they just don't show
  your tag. Graceful, not broken.

## Build order

- **v0.1** — SHIPPED 2026-10-10/11 (2D canvas hub — superseded).
- **v0.2** — IN BUILD (his 2026-10-11 feedback: "crazy but needs to be 3d and the
  avatar should be more than just stars"): three.js 3D hub (vendored, TAGTOWN-style)
  + real 3D generative creature avatars — bodies with volume, spikes/fins, eyes,
  readable silhouettes, not glow blobs. Deterministic prompt+seed so peers render
  each other identically.
- **v0.3** — The Dive: in-place iframe layer + floating back button + identity
  params (name, color, avatar seed). THIS is the togetherness fix: the session never
  breaks at the portal, and portal rooms (`"pv-" + hash(worldUrl)`) put everyone who
  flew through the same portal in the same instance automatically. Our worlds
  (limboverse, tagtown, promptiverse worlds) will read the `?pv_*` params and
  auto-join their portal room; third-party worlds that don't speak the protocol get
  the warm-hub + one-tap-return fallback.
- **v0.4** — Registry in the URL hash (textable hubs) + personal collection in
  localStorage + homeworlds (your front door + the hub's home ring).
- **v0.5** — Prompt pedestal: seeded world generator + `world.html`.

## Open questions for Joshua

- Hub aesthetic: same wisp-flight language as limboverse, or its own look?
- Prompt pedestal in v0.4 — deterministic seeded generator first (recommended:
  keeps everything offline and serverless), or real AI generation via API?
