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
| Shooter | Bullets, 40 per hit, every 0.9 s | 40 | Available from the start. Can overkill an almost-dead enemy |
| Beam | Continuous drain, 45 per second | 60 | Unlocked per colour with XP. Never wastes damage, shorter range |

- Placing: the first tap on a free tile shows a see-through preview with its range; a second tap on the same tile builds it.
- Choosing a colour shows which tower types that colour has. A locked beam shows as locked.
- Turrets aim at their target with a barrel snapped to 16 directions. Shooters recoil with a muzzle flash and their core dims while reloading. Beams pulse while draining. Idle turrets slowly scan.
- No cap on tower count. Limits are buildable space and coins.
- Selling refunds 70%.

## Economy

- 1 coin per 20 colour points drained, so overkill earns nothing.
- Wave clear bonus: 20 + 2 × wave number.

## Levels

Level N has N enemy colours. Each level has its own map and 10 lives.

| Level | Name | Colours | Waves | How the new colour arrives |
|-------|------|---------|-------|----------------------------|
| 1 | Red Alert | Red | 8 | Red only |
| 2 | Green Shift | Red, green | 10 | Waves 1–2 green alone, wave 3 red, then mixed |
| 3 | Full Spectrum | Red, green, blue | 12 | Waves 1–2 blue alone, wave 3 red and green, then all three |

- Clearing a level unlocks the next.
- Each enemy is still a single primary colour. Mixed-colour enemies (yellow, magenta, cyan, white) arrive once C/M/Y towers unlock.
- Waves get longer, faster and more tightly spaced as they go.
- The tower colour buttons only show the colours a level uses.

## Menu

- Main menu: Levels, Upgrades, Stats, How to play.
- Quitting mid-run takes two taps and still banks XP for the attempt.
- After a run: Retry, Next (after a win) or Menu.

## Progression

- XP for every attempt: 5 per wave cleared, 1 per enemy drained, plus 50 × level number for a clear.
- Beam unlocks bought with XP, one per colour: red 40, green 60, blue 80. A colour's beam becomes available once its level is open.
- Upgrades bought with XP, three tiers each (beam power needs a beam unlocked first):
  - Shot power: +5 damage per hit
  - Beam power: +8 drain per second
  - Long sight: +10% tower range
  - War chest: +25 starting coin
- Stats: runs, clears, kills, leaks, points drained per colour, XP earned, best wave per level.
- Heat map of where enemies died in the last run.
- Progress is saved in the browser.
- Planned unlocks: cyan, magenta and yellow shooters (each drains two channels at reduced strength).

## Smarter enemies (later)

- Start with a fixed path.
- Later, enemies that change the map, and enemies that route around danger.
- Approach: kill positions are already recorded. These feed the heat map first, then a danger-weighted flow field that later waves use to avoid hot zones (an ant-colony-style "pheromone" effect).

## Ideas parked for later

- Absorbers: enemies healed by one colour.
- Shifters: enemies that move health between channels.
- Leak damage proportional to the leaking enemy's remaining colour.
- Colour-blind mode: optional per-channel bars or patterns on enemies.
- Wrap as a phone app (Capacitor) or installable web app.

## Milestones

1. **Core loop** (done): one map, R/G/B shot and beam towers, 10 waves, coins, lives, win and lose.
2. **Menu, levels and XP** (done): three levels, upgrades, stats, heat map, saved progress.
3. **Next**: C/M/Y towers and mixed-colour enemies, then smarter enemies.

## Tech

- Plain JavaScript and canvas, no engine, no build step.
- Game logic kept separate from drawing, to make stats easy and porting to an app clean.
- Fixed 60 Hz simulation step; the speed button runs extra steps per frame.
