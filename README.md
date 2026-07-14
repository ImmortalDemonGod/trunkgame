# TRUNK!

A ground-up TypeScript rebuild of Team Ratateam's game-jam project *ElephantGame*
(Unity). Not a port — a redesign built from the original's ideas ("the spirit"),
with its failure modes designed out. See `DESIGN.md` for the full redesign
rationale and the fun audit.

**Play**: open `dist/index.html` in a browser (one self-contained file, ~40KB,
no dependencies, works offline).

## The pitch

You are an elephant girl whose trunk is a multitool: gun, jump, key, and hands.
Bullets ricochet; every bounce near you launches you — there is no jump button,
**recoil is the jump**. 3 shots per flight; land, grab a ring, or bounce off
green to reload. Journey: tutorial ledge → gap field → minecart over spike
alley → tower ladder → box-and-lever puzzle room → pulsing wind shaft → rooftop
boss who dodges shots aimed at his face (bank shots off the mirrors).

## Structure

- `src/game.ts` — pure deterministic logic: no DOM, no `Date`, no
  `Math.random`. `step(state, input, dt)` is the whole game.
- `src/render.ts` — canvas renderer, pure function of state.
- `src/main.ts` — browser shell: input, fixed-step loop, procedural WebAudio
  SFX, trajectory preview, screenshot/demo query params.
- `test/bot.test.ts` — 16 bot playtests (`bun test`), including a waypoint bot
  that completes the whole game and a casual-player difficulty sim.
- `build.ts` — `bun run build.ts` bundles everything into `dist/index.html`.

## Verification

- 19/19 bot tests green; full-run bot finishes spawn→boss-kill with 0 deaths.
- 36,000-step random-input fuzz: no NaN, no leaks, no out-of-world.
- Casual-player sim (reaction lag + aim error): median 1 death on the gap field.
- Honest boss duel (no healing): bot-optimal kill ≈ 15s taking 10 damage;
  humans ≈ 2-3x.
- Every level beat screenshot-reviewed via Firefox headless
  (`?x=&y=`, `?demo=launch|boss|win` query params).

## Query params (testing hooks)

`?x=100&y=10` teleport spawn · `?checkpoint=3` spawn at checkpoint ·
`?demo=launch|boss|win` canned action states for screenshots.
