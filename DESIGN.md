# Lena Balloon — Design Document
**Round 20 | game_name: lena-balloon**

---

## IDENTITY

**Game Name:** Lena Balloon
**Tagline:** *The sky has roads. Learn to read them.*

---

### Protagonist

**Name:** Lena Vasari
**Species:** Human (late 20s, sun-freckled, compact and precise)
**Personality:** Methodical, quietly joyful, the kind of person who stops mid-conversation to watch clouds change shape. She is never flustered — she is *reading*. She narrates nothing aloud; her competence speaks through the altitude she chooses.

**Backstory:** Lena is the last wind-cartographer of the Archimedes Guild, an academy that once charted the invisible corridor-roads above the Lithos Archipelago — a cluster of floating basalt islands held aloft by ancient thermal columns. The Guild dissolved when steam-ships replaced the old sky routes; the charts were lost in a fire. Lena inherited her grandmother's balloon and one handwritten note: *"The winds remember. You just have to listen at the right height."* She is rebuilding the lost Wind Atlas, one flight at a time.

---

### World

**Setting:** The Lithos Archipelago — a chain of floating basalt islands layered across a warm inland sea, perpetually bathed in golden morning light. Islands range from the size of a house to the size of a city block; some are bare rock, some are dense with dry grass and leaning olive trees. Below, the sea shimmers turquoise and distant. Above, cumulus towers catch amber light at their edges.

**World Feel (2 sentences):**
The Lithos Archipelago feels like the Mediterranean in a daydream — warm stone, salt air, the creak of old rope, and absolute silence except for wind and the occasional distant gull. Every altitude Lena occupies is a different world: at the low bands you can smell the sea and hear waves; at the high bands the air is thin and blue and the only sound is the slow exhale of the balloon's burner.

---

### Emotional Experience

The player feels like an expert reader of an invisible language. There is no scrambling, no reflexive panic — only the quiet satisfaction of positioning yourself correctly *before* the gap arrives, of trusting your altitude choice and watching the wind carry you exactly where you intended. The primary emotional state is **contemplative mastery**: a slow build from confusion to fluency, with occasional heart-in-throat moments when a fast band sweeps you toward a cliff faster than expected.

---

### Reference Games with DNA Notes

| Game | DNA extracted |
|---|---|
| **Inside** | Absolute silence of narration; world legibility through art alone; dread at the right moment, not constantly |
| **Thumper** | Every mistake is the player's misread, never the game's fault; rhythm of decision-making has a physical feel |
| **R-Type** | Reading the screen as a whole before reacting; memorization rewarded; elegant death |
| **Gran Turismo** | Mastery is about *understanding physics*, not about reflexes alone; the car (balloon) has rules you learn to love |
| **Metal Slug** | World is vivid and readable at a glance; silhouette is everything; warmth and craft in every frame |
| **Tony Hawk** | The line you pick before you execute it; setting up the next move while still in the current one |

---

## VISUAL SPEC

### Color Palette (all RGB channels > 30)

| Role | Hex | RGB | Use |
|---|---|---|---|
| **Background Sky** | `#7FA8C9` | 127 / 168 / 201 | Ambient sky mid-tone, warm blue |
| **Sky Horizon Fade** | `#C4935A` | 196 / 147 / 90 | Gradient bottom strip (sea-glow) |
| **Primary** | `#F0A832` | 240 / 168 / 50 | Balloon envelope, UI highlights |
| **Secondary** | `#4A7C59` | 74 / 124 / 89 | Island flora, basket rope accents |
| **Accent** | `#E04F35` | 224 / 79 / 53 | Lena's jacket, goal markers, danger |
| **Wind Band Warm** | `#F0C87A` | 240 / 200 / 122 | Rightward-blowing bands (warm tint) |
| **Wind Band Cool** | `#6EAAC4` | 110 / 170 / 196 | Leftward-blowing bands (cool tint) |
| **Wind Band Neutral** | `#9AABB8` | 154 / 171 / 184 | Calm / minimal wind bands |
| **Island Rock** | `#8C6E4B` | 140 / 110 / 75 | Basalt cliff silhouettes |
| **Balloon Highlight** | `#FDE082` | 253 / 224 / 130 | Backlit envelope bloom ring |

### Bloom

- **Enabled:** Yes
- **Strength:** 0.35 (subtle — only the balloon envelope and goal beacons catch full bloom)
- **Threshold:** 0.72 (only near-white and the bright gold of the balloon trigger glow)
- **Effect:** The balloon reads as a warm lantern against the open sky. Wind band tints do NOT bloom — they must remain readable, not decorative.

### Camera Angle

**Side-scrolling, fixed vertical frame.** The screen is 800px wide × 600px tall. The world scrolls horizontally. Lena's balloon is pinned at x=200 (left 25% of screen), so the approaching world is always visible ahead. Vertical position reflects true altitude — no camera pan on Y axis. The full height of the playfield (600px) is always visible, so the player can read all wind bands simultaneously at any moment.

### Player Silhouette (5 words)

**Round envelope, trailing wicker basket**

- The envelope is a perfect teardrop (wider at top). Diameter: 44px. Warm amber with thin dark panel seams.
- The basket hangs 20px below on four visible rope lines. 16px × 12px wicker rectangle.
- Lena is visible as a silhouette from the waist up above the basket rim: dark jacket, short hair, looking right.
- Total vertical footprint: 76px. Total horizontal footprint: 44px.
- Burner flame (8px tall, `#FFF0A0`) pulses between 4–8px height when holding (rising). Off when releasing.

### Wind Band Visualisation

This is the central design challenge. Wind bands must be **instantly readable while the player watches the balloon and the approaching terrain simultaneously.**

**Structure:**
Each wind band occupies a horizontal strip of the playfield. Bands are **120px tall** each. With a 600px playfield, 5 bands fit on screen at all times (numbered 0=lowest to 4=highest). The band Lena currently occupies is subtly brighter (5% luminosity lift).

**Visual language — 3 simultaneous readable cues per band:**

1. **Background tint:** Warm amber tint (`#F0C87A` at 18% opacity) for rightward bands. Cool blue tint (`#6EAAC4` at 18% opacity) for leftward bands. Neutral grey-blue (`#9AABB8` at 10% opacity) for near-calm bands. Applied as a full-width horizontal strip. No blending between bands — hard edges at band boundaries.

2. **Animated streak lines:** Each band contains 6 horizontal streak lines. Each streak is 2px tall, 40–80px long (randomised per streak), color matching the band tint at 60% opacity. Streaks scroll continuously in the wind direction at the band's exact wind speed in px/frame. New streaks spawn at the leading edge (right for rightward wind, left for leftward wind) and despawn at the far edge. Streak density and scroll speed are the primary speed cue.

3. **Direction arrow strip — left margin HUD:** A 20px-wide vertical strip on the far left of the screen (not part of the world — overlaid UI). Each band's row in this strip shows a single flat arrow: `→` for rightward, `←` for leftward, `—` for calm. Arrow color matches band tint. Arrow size: 14px. This is the cheat-sheet — players glance left to plan, then look right to react.

**Band boundary lines:** 1px solid line at `#FFFFFF` at 15% opacity. Visible but not dominant. Lena's balloon crossing a boundary triggers a brief 3-frame scale-pulse (1.0 → 1.04 → 1.0) to confirm the transition — tactile without being noisy.

---

## SOUND SPEC

### Music Identity

**Genre:** Pastoral ambient folk — the feel of a late-morning radio programme from a country where time moves slowly. Acoustic textures with a subtle electronic underpinning that pulses with the wind.

**Character:** Unhurried, curious, warm. The music should feel like the sky itself: open, enormous, and forgiving in the low bands, crisper and more alert in the high bands.

**THE HOOK — The Lena Motif:**
A five-note ascending phrase on plucked dulcimer: **D – F# – A – D – E** (in D major). It plays in the first 4 bars and recurs as a 2-bar fragment any time Lena successfully threads a narrow gap. The phrase sounds like a question asked with confidence — it goes up and hangs on the E, unresolved, waiting.

### Arrangement (Tone.js)

| Instrument | Tone.js Synth | Role | Entry | Exit |
|---|---|---|---|---|
| **Plucked Dulcimer** | `PluckSynth` (attackNoise: 1.2, dampening: 3800, resonance: 0.96) | Lead melody — the Lena Motif and variations | Immediate, bar 1 | Fades on game over — last note held |
| **Warm Bass Pad** | `Synth` with triangle oscillator + 1.8s release, reverb (room: 0.7) | Harmonic foundation, one chord per 2 bars | Bar 2, crossfade in | Crossfades out on level end |
| **Hand Pan Percussion** | `MetalSynth` (harmonicity: 8, modulationIndex: 10, envelope: {attack: 0.01, decay: 0.4}) | Rhythmic pulse — quarter notes with syncopated accents on the off-beat | Bar 1 | Drops out in danger state |
| **Whistling Flute** | `Synth` with sine oscillator, vibrato (depth: 0.3, rate: 5.2), high reverb | Countermelody in high-altitude state — airy, exposed | Enters in state: HIGH_ALTITUDE | Fades when Lena descends |
| **Vinyl Texture Layer** | `Noise` (type: pink, volume: -28 dB) + low-pass filter (freq: 800) | Warmth and analogue grit underneath everything | Always on, very low | Never off |
| **Distant Wind Bell** | `MetalSynth` (very low volume, long decay: 3s) | Accent hit when crossing a band boundary | Triggered on band transition event | One-shot, no exit |

**BPM:** 76
**Bar length:** 4 beats
**Loop length:** 16 bars (≈ 50.5 seconds per loop)
**Time signature:** 4/4. The hand pan accents beat 2 and the "and" of 3, giving a gentle lilt.

### Dynamic Music State Table

| State | Trigger | Active Layers | Change |
|---|---|---|---|
| `IDLE_DRIFT` | Default / low stress | Dulcimer, Bass Pad, Vinyl, Hand Pan (soft) | Full arrangement at -3 dB master |
| `HIGH_ALTITUDE` | Lena in band 3 or 4 (top two bands) | Dulcimer, Bass Pad, Vinyl, Whistling Flute (enters) | Hand Pan mutes. Flute enters bar boundary. Bass Pad shifts to Bm (minor relative) |
| `APPROACHING_OBSTACLE` | Obstacle within 180px ahead | Dulcimer, Hand Pan (tighter, faster subdivisions: 8th notes), Vinyl | Bass Pad volume –6 dB. Tempo nudges to 82 BPM over 2 bars (subtle) |
| `DANGER` | Less than 40px clearance from obstacle | Hand Pan only + Vinyl + single held Bass note (C#, tension) | Dulcimer drops. Flute drops. BPM stays at 82. Heartbeat-like hand pan on beats 1 and 3 only |
| `LEVEL_CLEARED` | Reach goal marker | Full arrangement + Lena Motif plays complete 5-note phrase + brief bell chord | BPM returns to 76. Dulcimer volume +3 dB for 4 bars |

### Start Screen Music

A single looping section: Dulcimer playing the Lena Motif at half tempo (38 BPM effective feel) with long reverb (decay: 4s), Bass Pad holding a warm Dmaj7 chord, Vinyl texture. No percussion. Sounds like a morning before departure — quiet anticipation. Loop is 8 bars long.

### Sound Effects (8+)

| Sound | Event | Tone.js Implementation | Why It Fits |
|---|---|---|---|
| **Burner Fire** | Hold input active (rising) | `Noise` type:brown, low-pass filter sweeping 200→600Hz over 0.3s, volume oscillates ±2dB at 8Hz | Brown noise is physically accurate to gas combustion; the filter sweep mimics the burner igniting and sustaining |
| **Band Transition** | Balloon crosses band boundary | `MetalSynth` single hit: freq 440Hz, harmonicity:5, envelope {attack:0.01, decay:0.25, sustain:0, release:0.1} | Clean, percussive confirmation — feels like clicking into a gear |
| **Rightward Wind** | Lena in a rightward band | `Noise` type:white filtered at 900Hz, panned slightly right, volume scales with wind speed (–24 to –16 dB) | White noise at mid-freq = rushing air; right pan reinforces direction read |
| **Leftward Wind** | Lena in a leftward band | Same as rightward wind but panned slightly left | Immediate directional audio confirmation |
| **Obstacle Scrape** | Balloon within 20px of cliff edge | `Synth` triangle osc, freq: 180Hz, rapid amplitude tremolo (rate: 22Hz) + pink noise burst –18dB | Rumble + flutter = rope/wicker stress; never used unless near-miss |
| **Collision / Death** | Balloon contacts solid obstacle | `Noise` burst (white, 0.4s) + `Synth` descending glide 440→110Hz over 0.6s | The envelope rupturing — sharp pop then the air rushing out |
| **Goal Beacon Reached** | Touch goal marker | `PluckSynth` playing D–F#–A–D–E (the Lena Motif, full phrase), long release | Motivic payoff — you hear the hook as reward |
| **Map Stamp** | Level complete screen | `MetalSynth` bright strike (freq: 880, short decay: 0.15s) + 3-note descending `PluckSynth` (A–F#–D) | Satisfying physical stamp sound — Lena marking the chart complete |
| **Altitude Warning** | Balloon within 15px of ceiling or floor | `Synth` sine osc, freq: 660Hz, staccato pulse every 0.4s, vol: –20dB | Gentle alert tone; urgent but not alarming |
| **Level Start** | "Go" moment at start of each level | `PluckSynth` single note D5, followed by hand pan strike on beat 2 | Clean, confident departure — this is the moment Lena releases the mooring |

---

## MECHANIC SPEC

### Core Loop (one sentence)

Lena holds to rise and releases to fall, repositioning her balloon between five wind bands whose directions are visible as coloured streaks, threading waypoints and avoiding cliff faces that only the correct wind can carry her past.

---

### Primary Input

| Action | Value | Notes |
|---|---|---|
| **Hold (rise)** | +2.8 px/frame | Upward velocity, applied every frame the input is held |
| **Release (fall)** | –1.6 px/frame | Gravity drift downward; the balloon has residual lift so fall is slower than rise |
| **Terminal velocity upward** | 4.2 px/frame | Cannot exceed regardless of hold duration |
| **Terminal velocity downward** | 2.8 px/frame | Balloon always fights gravity slightly |
| **Band transition smoothing** | None | Wind direction changes are instantaneous at band edge — no lerp. This is intentional: the band edge is a hard decision point |
| **Frame rate target** | 60 fps | All px/frame values assume 60 fps |

**Altitude physics:** Balloon y-position is a simple accumulator. No momentum or inertia on vertical axis — the balloon responds immediately to input. This is a deliberate design choice: the challenge is *reading* (knowing which altitude to be at) not *reacting* (catching a ball in free fall). Horizontal movement is purely wind-driven: the balloon has zero horizontal agency.

---

### Wind Bands

**Number per screen:** 5 bands, always visible simultaneously.
**Band height:** 120px each (5 × 120px = 600px = full playfield height).
**Band labelling (internal):** Band 0 (bottom) through Band 4 (top).

**Wind direction display** (three simultaneous cues, see Visual Spec):
1. Background tint (warm = right, cool = left)
2. Animated streaks scrolling in wind direction
3. Left-margin arrow HUD

**Wind speed per band (Level 1 defaults, px/frame, rightward positive):**

| Band | Default Speed | Direction |
|---|---|---|
| 0 (bottom) | +1.2 | Right |
| 1 | +0.6 | Right (slow) |
| 2 | 0.0 | Calm |
| 3 | –0.8 | Left |
| 4 | –1.4 | Left (fast) |

Wind speeds are constants per band per level — they do not shift during a level (they begin shifting in Level 4). This predictability is what allows mastery: players *memorize* the band map.

---

### Difficulty Curve (exact per-level values)

| Parameter | L1: Velarde's Rise | L2: Cliff Coast | L3: Storm Belt | L4: The Shifting | L5: Chart Run |
|---|---|---|---|---|---|
| Band speeds (R+/L–) px/frame | 0→[1.2, 0.6, 0, –0.8, –1.4] | [1.8, 0.9, 0, –1.1, –2.0] | [2.2, 1.2, 0, –1.4, –2.5] | [2.5, 1.4, 0, –1.8, –3.0] | [3.0, 1.6, 0, –2.2, –3.5] |
| Band width (px) | 120 | 120 | 120 | 100 | 100 |
| Obstacle frequency | 1 per 900px | 1 per 700px | 1 per 550px | 1 per 420px | 1 per 320px |
| Obstacle gap height (px) | 260 | 220 | 180 | 160 | 140 |
| Turbulence (random vertical nudge) | None | None | ±0.4px/frame, changes every 90 frames | ±0.6px/frame, changes every 60 frames | ±0.8px/frame, changes every 45 frames |
| Band shift events | None | None | None | 1 band reverses direction every 1800px | 2 bands swap speeds every 1200px |
| World scroll speed (px/frame) | 2.0 | 2.4 | 2.8 | 3.0 | 3.4 |
| Waypoints to collect | 3 | 5 | 5 | 6 | 8 |
| Level length (px of world) | 6000 | 8000 | 9000 | 10000 | 12000 |

**Band shift events (Levels 4–5):** When a band shift event triggers, the affected band's colour, streak direction, and HUD arrow all transition over exactly 60 frames (1 second). The new wind speed takes effect immediately but visually fades in over 60 frames so the player has a 1-second read window. A brief audio cue (wind bell cluster: 3 notes descending) plays at the moment of shift.

---

### Win / Lose Conditions

**Win (level):** Reach the goal marker (a glowing red lantern on a rope — the Guild's traditional waypoint beacon) at the far right of the level. The balloon must physically overlap it (within 30px radius).

**Win (game):** Complete all 5 levels. A final screen shows the completed Wind Atlas — a hand-drawn map of the Lithos Archipelago with Lena's route marked in ink.

**Lose:** The balloon envelope contacts any solid surface (cliff face, island edge, ceiling, or floor). Contact is defined as the 44px-diameter envelope circle intersecting any solid geometry rectangle. On loss: collision sound plays, balloon deflation animation (envelope shrinks to 0 over 0.6s, basket falls off-screen), level restarts from beginning with same wind configuration.

**No lives system.** Instant restart. No penalty score. Failure is information.

---

### Score

Score is purely distance-based and exists only for the player's own satisfaction (no leaderboard in v1).

- **Distance score:** 1 point per 10px of rightward world scroll (minimum, even if wind pushes left)
- **Waypoint bonus:** 500 points per waypoint collected
- **Precision bonus:** If Lena completes a level without touching any obstacle within 60px (tracked as "clean pass"), +1000 points per level
- **Speed bonus:** Time under par → +100 points per second saved (par per level: L1:55s, L2:70s, L3:80s, L4:90s, L5:100s)

Score is displayed post-level on the map screen as a handwritten number in Lena's style. No HUD score during play — nothing to distract from reading the sky.

---

## LEVEL DESIGN

### Level 1 — "Velarde's Rise"

**What's new:** The core mechanic. First flight. Wind bands are wide (120px), slow, and the gap in obstacles is generous (260px). No turbulence. No time pressure.

**Setting:** Dawn over the outer islands. The sea is visible below (animated waves, turquoise, parallax layer at 0.4× speed). Three small islands in the distance serve as visual depth. The sky is clear — maximum band readability.

**Exact parameters:** World scroll 2.0px/frame. Band speeds [1.2, 0.6, 0, –0.8, –1.4]px/frame. 1 obstacle per 900px. Gap 260px. 3 waypoints.

**Duration:** ~50s at par. Approx 6000px of world.

**Goal/teaching:** Player discovers that holding causes the balloon to drift right in the upper bands and left — they must release to drop back into rightward bands. The single calm band (band 2) acts as a "pause button" and is always available as a safe refuge. First waypoint is at band 2 height, second at band 4, third at band 1 — teaches the full vertical range.

---

### Level 2 — "Cliff Coast"

**What's new:** Obstacles become cliffs (tall basalt columns rising from below, or hanging from above). Some obstacles require the player to be in a specific band — not just *through* the gap but at a specific altitude to thread the gap with the correct wind.

**Setting:** Afternoon. The sea is replaced by rocky coastline below. Cliffs are dark brown-ochre (`#8C6E4B`), textured with horizontal striations. Gull silhouettes (static, 3px wide) at mid-altitude for depth.

**Exact parameters:** World scroll 2.4px/frame. Band speeds [1.8, 0.9, 0, –1.1, –2.0]px/frame. 1 obstacle per 700px. Gap 220px. 5 waypoints.

**Duration:** ~65s at par. Approx 8000px of world.

**Goal/teaching:** Some gaps are only reachable from band 3 or 4 (leftward wind), meaning the player must first overshoot rightward and use leftward bands to position — introduces the idea of intentional "backtracking" by altitude choice.

---

### Level 3 — "Storm Belt"

**What's new:** Turbulence (random vertical micro-nudges). Wind bands remain constant in direction but the balloon wobbles. Players learn to anticipate and compensate.

**Setting:** Overcast, late afternoon. Background sky shifts to a deeper, cloudier blue-grey (`#5D7E96` — all channels >30). Cumulus shadows pass across the screen (dark semi-transparent ellipses, 200px wide, scrolling at 1.2px/frame, independent of world). Lightning flash in background every 30–45 seconds (single white frame, 1/8 opacity).

**Exact parameters:** World scroll 2.8px/frame. Band speeds [2.2, 1.2, 0, –1.4, –2.5]px/frame. Turbulence ±0.4px/frame every 90 frames. 1 obstacle per 550px. Gap 180px. 5 waypoints.

**Duration:** ~75s at par. Approx 9000px of world.

**Goal/teaching:** Turbulence is never enough to kill you on its own — it only kills you if you're already in a marginal position. Teaches that *comfortable margins* matter. Players who were riding band edges start centering their altitude within bands.

---

### Level 4 — "The Shifting"

**What's new:** Band shift events. Every 1800px of world travel, one band's wind direction reverses (full visual transition over 60 frames). A Guild signal bell sounds 2 bars before the shift (the player hears the audio cue and has ~1.6 seconds to plan). The shift affect cycles through bands 0, 2, 4 (the first, middle, and top bands) so the safe band structure changes dramatically mid-level.

**Setting:** Golden hour. The sky is warm amber at the bottom, transitioning to deep orange-red at the top. Islands are silhouetted completely — just flat black shapes. The contrast maximises band readability against the coloured sky.

**Exact parameters:** World scroll 3.0px/frame. Band speeds [2.5, 1.4, 0, –1.8, –3.0]px/frame. Turbulence ±0.6px/frame every 60 frames. 1 obstacle per 420px. Gap 160px. 6 waypoints. 1 band reversal event per 1800px of world (≈5 events total).

**Duration:** ~85s at par. Approx 10000px of world.

**Goal/teaching:** Players must hold a mental model of the current band map, not the default one. When a band shifts, the HUD arrow and streak direction flip — the player must re-read the sky immediately. Shifts mid-obstacle approach are the highest-pressure moments in the game.

---

### Level 5 — "Chart Run"

**What's new:** Everything. Highest speeds, smallest gaps, turbulence ±0.8px/frame, 2 bands swap speeds every 1200px, and 8 waypoints that must be reached in sequence — each waypoint is at a *specific* altitude (shown as a thin horizontal dotted line at the correct band center, color-matched to the band). Reaching a waypoint at the wrong altitude misses it entirely (no partial credit).

**Setting:** Pre-dawn. The sky is dark blue-purple (`#3D5E7A` — all channels >30) at top, transitioning to warm rose-gold at the horizon. This is Lena's final chart run, completing the atlas. Stars are visible as static white dots (40 total, 2px radius, scattered in the top 200px of background). The goal marker at the end is not a single lantern but an array of seven lanterns — the Guild's old relay station, lit for the first time in decades.

**Exact parameters:** World scroll 3.4px/frame. Band speeds [3.0, 1.6, 0, –2.2, –3.5]px/frame. Turbulence ±0.8px/frame every 45 frames. 1 obstacle per 320px. Gap 140px. 8 ordered waypoints. 2 band swap events per 1200px.

**Duration:** ~95s at par. Approx 12000px of world.

**Goal/teaching:** This is the test of everything. The player who has internalized band reading, turbulence compensation, and shift anticipation will flow through it. A player who has not will restart multiple times — and the knowledge gained from each run compounds visibly.

---

## THE MOMENT

The moment the player releases the input at exactly the right altitude to drop from a leftward band into a rightward band, the balloon traces a clean diagonal arc that threads the 140px gap in a cliff face while the wind carries it precisely through — no correction, no near-miss, just the Lena Motif ringing out as the waypoint beacon floods gold.

---

## EMOTIONAL ARC

**First 30 seconds:** Confusion followed by the first *aha*. The player holds, the balloon rises, and then they see the streaks change direction — and then they feel the balloon change direction horizontally without touching anything. The sensation of passive horizontal motion is genuinely surprising. "Oh. *Oh.* I'm not steering. The sky is steering me."

**After 2 minutes:** Fluency settling in. The player has died 2–3 times and now recognizes the band map. They're planning 2 bands ahead. When they hit a waypoint cleanly they feel it in their chest — not adrenaline, but the precise satisfaction of being right about something invisible.

**Near win (Level 5 final approach):** Lena is approaching the guild relay station. The music has built to its full arrangement with the Lena Motif cycling. The band map has shifted twice. The player is threading the final gap with 3.4px/frame scroll speed. There is no panic — only reading. The seven lanterns come into view and the player adjusts their altitude one last time, drops into band 1, and the rightward wind pulls them directly into the goal marker. The stamp sound fires. The map completes. Silence — then the full Lena Motif, complete, for the first time without the loop.

---

## GAME IDENTITY

**"This is the game where you learn to let the sky drive — and discover that reading is faster than steering."**

---

## START SCREEN

### Idle Animation

The start screen shows a still dawn sky (background gradient: `#C49060` at bottom → `#7FA8C9` at top, full 600px height). Three layers of activity, all looping:

**Layer 1 — Slow island drift (parallax background):**
- 4 basalt island silhouettes, all flat black (`#2A2520` — within rule since it's the world geometry, not the background)
- Island A: 280px wide × 60px tall, at y=480. Moves left at 0.15px/s.
- Island B: 180px wide × 40px tall, at y=510. Moves left at 0.10px/s.
- Island C: 120px wide × 30px tall, at y=530. Moves left at 0.08px/s.
- Island D: 90px wide × 20px tall, at y=545. Moves left at 0.06px/s.
- All islands loop: when off-screen left, teleport to x=820.
- Creates a gentle sense of depth without animation complexity.

**Layer 2 — The balloon at rest (bobbing):**
- Lena's balloon at x=200, y=310 (screen center, slightly left).
- Vertical bobbing: amplitude ±6px, period 3.8s, sine wave.
- Horizontal micro-sway: amplitude ±2px, period 5.2s, sine wave (slightly offset from vertical).
- Burner flame: pulses between 4px and 7px height at 1.2Hz (inhale-exhale rhythm) — this is the only light source on screen.
- Balloon envelope bloom: soft glow ring, radius 34px, `#F0A832` at 25% opacity, pulses with the same 1.2Hz as the burner.
- Rope lines between envelope and basket: 4 lines, each 18px long, sway ±1.5px amplitude following the horizontal micro-sway with a 0.3s lag (mechanical string feel).

**Layer 3 — Wind band preview streaks (5 bands visible):**
- The background sky shows all 5 wind bands with their default Level 1 streak animation, at 40% opacity.
- Band 0 (bottom, y:480–600): rightward streaks at 1.2× visual speed.
- Band 1 (y:360–480): rightward streaks at 0.6× visual speed.
- Band 2 (y:240–360): no streaks (calm band).
- Band 3 (y:120–240): leftward streaks at 0.8× visual speed.
- Band 4 (top, y:0–120): leftward streaks at 1.4× visual speed.
- Streaks: 2px tall, 50px long, 6 per band, opacity 35%. This teaches the mechanic before the player touches anything.

### SVG Overlay

**Option A (required) — Glow Title:**

SVG centered at x=400, y=150. Text: "LENA BALLOON" in two lines.

- "LENA" — uppercase, bold condensed sans-serif. Font-size: 72px. Fill: `#F0A832`. Stroke: none.
- "BALLOON" — uppercase, regular condensed sans-serif. Font-size: 48px. Fill: `#FDE082`. Stroke: none.
- Behind both lines: SVG `<filter id="glow">` using `<feGaussianBlur stdDeviation="8" result="blur"/>` + `<feMerge>` combining blur and source graphic. The glow color is `#F0C87A` at full intensity. The result: the title appears to emanate warm amber light, as if the balloon envelope is casting it.
- Both lines sit on the same glow filter. Glow animates: stdDeviation oscillates between 6 and 10 over 2.8s (sine), synced with the balloon's burner pulse. The title breathes.

**Option B — Subtitle Scroll:**

Below the title, at y=220: a single line in small caps, 18px, `#FDE082` at 70% opacity:

*"The sky has roads. Learn to read them."*

This line does not animate. It appears on a gentle fade-in 0.8s after the title appears. Below it, at y=248, in 13px regular weight, `#FDE082` at 45% opacity:

*"Hold to rise · Release to fall · Read the wind"*

This three-part instruction is the only UI on the start screen. It teaches the complete mechanic in one line. No button labels. No menu items other than a single "HOLD TO BEGIN" prompt at y=560, in 16px, `#E04F35`, pulsing opacity between 50% and 100% at 0.9Hz.

---

## IMPLEMENTATION NOTES FOR PROGRAMMER

These notes address the non-obvious implementation details so there are no ambiguities:

1. **Balloon y-position** is clamped to [22px (top: half balloon radius above band 4 upper edge) — 578px (bottom: half balloon below band 0 lower edge)]. Never allow clipping into ceiling or floor geometry.

2. **Horizontal balloon x-position** is fixed at x=200. The *world* scrolls. The balloon's horizontal position in world-space is tracked separately for scoring and obstacle collision, but the render position is always x=200.

3. **Wind application:** Every frame, read Lena's current y-position, compute which band she's in (floor(y/120)), look up that band's wind speed, add to world scroll speed accumulator. World x-offset increases by (worldScrollSpeed + bandWindSpeed) per frame. Positive = world moves left (Lena moves right). Negative bandWindSpeed = world moves right (Lena moves left).

4. **Obstacle collision:** Check envelope circle (center: balloon x=200, y=Lena.y, radius: 22px) against each obstacle rectangle in viewport. If any point of the circle's circumference intersects the rectangle, trigger death.

5. **Waypoint collection:** Check Lena's world-space position against waypoint world-space position. Waypoint collected if distance < 30px. In Level 5, waypoints are ordered: waypoint N is only collectable after N–1 is collected (draw uncollected waypoints at 40% opacity until active).

6. **Band shift animation:** During a 60-frame transition, the band's visual tint and streak direction linearly cross-fade. The wind speed does NOT lerp — it snaps immediately on frame 1 of the transition. The visual is catching up to physics, not the other way around.

7. **Touch input:** Hold = any touch on the screen. Release = no touch. This is a single-button game. For keyboard: spacebar or any key held = rise, released = fall.

8. **World generation:** Obstacles and waypoints are procedurally placed per level with a fixed random seed (per-level seed: L1=1001, L2=1002, L3=1003, L4=1004, L5=1005). This ensures reproducible difficulty and enables score comparison.

---

*End of Design Document — Lena Balloon, Round 20*
