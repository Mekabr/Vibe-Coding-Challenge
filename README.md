# Aureus – Surviving the CF Lung

A small educational arcade game for the browser, made for the Vibe Coding Challenge.
You steer a colony of *Staphylococcus aureus* through the thick mucus of a lung with
cystic fibrosis (CF), collect nutrients and dodge the immune system.

## Play

Open `index.html` in a browser – that's it. No installation, no dependencies.

With GitHub Pages (Settings → Pages → Branch `main`) the game can also be played online.

## Controls

| Action | Keyboard | Mouse / touch |
|---|---|---|
| Move | Arrow keys or W A S D | Pointer / finger is followed |
| Dash | Space (or Shift) | ⚡ button, or double-tap on a phone |
| Pause | P or Esc | Pause button at the top right |
| Start | Space or Enter | “Start infection” |

The dash is a short burst of speed with a 3-second cooldown (shown as a ring around the colony).

## Rules

**Collect** – anything that sparkles
- **Nutrient** (teal): +10 points
- **Iron (Fe)** (orange): +50 points
- **Heme jackpot** (golden star “200”): +200 points – very rare and 1.3× faster than anything else
- **SCV** (small pale cocci): for 5 seconds your colony becomes a *small colony variant* –
  much smaller and harder to hit, but a little slower
- **Biofilm** (green): 5-second shield – immune cells you ram give bonus points
- **EPO** (pink drop): ultra rare – every level has a 0.1% chance. Triggers 3 seconds of
  **heme rain**: no new enemies and 18 hemes fall from the sky. Never appears during a boss fight.

**Streak bonus:** every 10 pickups in a row without getting hit give +25 extra points.

**Level bonus:** every completed level pays 10 × the level number (level 5 → +50, level 15 → +150).

**Dodge** – anything that glows red; five enemies from small to large:
antibody → complement → neutrophil → macrophage → NET (neutrophil extracellular trap).
Each hit costs a life; you have three.

Every 25 seconds the level goes up: more immune enemies arrive, they get faster,
and the share of large ones grows. From level 16 on the difficulty stays on a plateau.

**Boss fight** – every 5 levels: after a warning (flashing + siren, sound can be muted with 🔊)
a boss slides in from the top. Survive for 30 seconds – you get **double points** meanwhile,
and the level clock stands still. Four bosses take turns; from level 25 on they come back stronger:

| Level | Boss | Attack |
|---|---|---|
| 5, 25, 45 … | *Pseudomonas aeruginosa* | spits pyocyanin, LasA and HQNO right at you |
| 10, 30, 50 … | *Burkholderia cepacia* | jumps around, fires fan volleys |
| 15, 35, 55 … | *Aspergillus fumigatus* (mould) | rains spores |
| 20, 40, 60 … | Antibiotic therapy | rotating β-lactam spiral |

**Reward after each boss:** missing hearts are refilled to three. If you still have all three,
you get a **golden heart** instead. Golden hearts are used up first when you get hit,
don't come back, and stack.

**Game over:** you see your stats – level, bosses beaten, time survived, nutrients, iron and heme
collected, total level bonus, longest time without a hit, best streak and number of dashes.

Your personal best is stored in the browser.

## Leaderboard

After game over you can enter your name; the game shows the **top 3** of all players
(the best score per name counts). Scores are stored in a [Supabase](https://supabase.com)
database – players don't need an account, just an internet connection.
Without a connection the game runs normally, just without the leaderboard.

Entries can be viewed and deleted in the Supabase dashboard under *Table Editor → scores*.

## The science behind it (simplified)

- In CF the CFTR channel is defective; airway mucus becomes thick and is poorly cleared.
- *S. aureus* is one of the earliest and most common pathogens in CF lungs, especially in children.
- The golden pigment staphyloxanthin protects against the reactive oxygen species of neutrophils.
- *Small colony variants* (SCVs) grow slowly, persist for a long time and tolerate antibiotics better.
- Inside a biofilm, the bacteria survive antibiotics and immune attacks better.
- The body keeps iron scarce (“nutritional immunity”), which makes it precious for bacteria.
- EPO (erythropoietin) boosts red blood cell production – and with it hemoglobin and heme.
- *P. aeruginosa* often displaces *S. aureus* in CF lungs over time – among other things with the
  protease LasA (“staphylolysin”), the respiration blocker HQNO and the toxic pigment pyocyanin.
- *Burkholderia cepacia* is highly resistant and can cause the dangerous “cepacia syndrome”.
- *Aspergillus fumigatus* is a mould whose spores are inhaled; some people with CF develop an
  allergic reaction (ABPA).
- β-lactam antibiotics such as flucloxacillin block cell wall synthesis in *S. aureus*; MRSA is resistant.

The game is heavily simplified and not to scale.

## Tweaking the balance

All values are collected in the script of `index.html` under the comment `Game balance`:

- `LEVEL_SECONDS` – length of a level
- `enemyInterval` – time between immune enemies
- `enemySpeed` / `itemSpeed` – speeds
- `SHIP_SPEED` / `SHIP_ACCEL` – speed and acceleration of your colony
- `DASH_SPEED` / `DASH_TIME` / `DASH_COOLDOWN` – the dash
- `LEVEL_BONUS` – bonus per completed level (× level number)
- `IRON_CHANCE` – how often iron appears
- `SCV_CHANCE` / `SCV_SECONDS` – the SCV power-up
- `STREAK_EVERY` / `STREAK_BONUS` – the streak bonus
- `BOSS_EVERY` / `BOSS_SECONDS` / `WARN_SECONDS` – when and how long bosses appear
- `BOSSES` – the four bosses with movement, attack and fire rate; `shotSpeed` – projectile speed
- `EPO_CHANCE` / `EPO_SECONDS` / `EPO_HEMES` – the EPO event
- `HEME_CHANCE` / `hemeSpeed` – frequency and speed of the heme jackpot
- `ENEMY_SIZES` / `sizeRatio` – enemy sizes and how quickly large ones become more common
