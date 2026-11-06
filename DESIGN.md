# RGBTD design

A tower defence game for phones, built web-first. Enemies walk from the top of the screen to the bottom. An enemy's colour is its health.

## Core rule

- Every enemy has three channels: red, green and blue, each 0–255.
- Its fill colour is exactly `rgb(r, g, b)`, so damage is visible instantly.
- Towers each drain one channel. An enemy dies when all three channels reach 0 (black).
- A red tower cannot hurt a pure green enemy. Building the right mix of colours is the main decision.
- Towers only target enemies that still have their channel, so shots are never wasted on immune enemies.

## Look

- 8-bit, minimal, clear short text. Graphics stay simple while the mechanics are worked out.
- The map is the phone screen in portrait. The path is plain black.
- Enemies have a thin white outline so they stay visible as they darken towards black.
- Internal resolution 180×280, scaled up in whole-number steps for crisp pixels.

## Towers

| Type | Behaviour | Cost | Notes |
|------|-----------|------|-------|
| Shot | Bullets, 40 per hit, every 0.9 s | 40 | Can overkill an almost-dead enemy |
| Beam | Continuous drain, 45 per second | 60 | Never wastes damage, shorter range |

- No cap on tower count. Limits are buildable space and coins.
- Selling refunds 70%.

## Economy

- 1 coin per 20 colour points drained, so overkill earns nothing.
- Wave clear bonus: 20 + 2 × wave number.

## Waves (map 1)

- Early waves are a single colour (red first, then green, then blue).
- Later waves mix single-colour enemies.
- Mixed-colour enemies (yellow, magenta, cyan, white) arrive once C/M/Y towers unlock.
- 10 waves to clear the map. 10 lives.

## Progression (planned)

- Players beat maps and earn XP for every attempt, including losses.
- Between rounds, XP unlocks:
  - Cyan, magenta and yellow shooters (each drains two channels at reduced strength)
  - Tower upgrades
  - Stats screen
  - Heat map of where enemies died
- Enemy complexity is gated behind unlocks, so the unlock tree doubles as the tutorial.

## Smarter enemies (later)

- Start with a fixed path.
- Later, enemies that change the map, and enemies that route around danger.
- Approach: kill positions are already recorded. These feed the heat map first, then a danger-weighted flow field that later waves use to avoid hot zones (an ant-colony-style "pheromone" effect).

## Ideas parked for later

- Absorbers: enemies healed by one colour.
- Shifters: enemies that move health between channels.
- Colour-blind mode: optional per-channel bars or patterns on enemies.
- Wrap as a phone app (Capacitor) or installable web app.

## Milestones

1. **Core loop** (current prototype): one map, R/G/B shot and beam towers, 10 waves, coins, lives, win and lose.
2. **Between-round layer**: XP, unlock screen, saved progress.
3. **Stats and heat map**, then C/M/Y towers, then smarter enemies.

## Tech

- Plain JavaScript and canvas, no engine, no build step.
- Game logic kept separate from drawing, to make stats easy and porting to an app clean.
- Fixed 60 Hz simulation step; the speed button runs extra steps per frame.
