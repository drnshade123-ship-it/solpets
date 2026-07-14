# Sol Pets Meadow Dash v12 stabilization

## Files

- `index.html` — surgical menu, startup, stats, rewards, shop, tutorial, and developer-test changes.
- `assets/js/three.module.js` — local Three.js 0.160.0 module (the same version previously loaded from the CDN).
- `assets/js/three.min.js` — matching Three.js 0.160.0 classic build used for direct `file://` launch compatibility.
- `CHANGELOG.md` — this change record.
- `TESTING.md` — executed checks, results, and remaining risks.

A pre-edit backup is preserved as `index.pre-stabilization-backup.html`.

## Phase 1 — DOM menus independent from Three.js

- Added one classic DOM menu controller before every Three.js module.
- Bound Start Run, Stats, Shop, Settings, Daily Missions, Characters, shop tabs, settings toggles, modal close, and menu Back buttons through one persistent delegated `click` listener.
- Kept menu navigation available when the Three.js module is missing or initialization fails.
- Disabled the full-screen offline/loading overlay so it cannot intercept taps.
- Added a small non-blocking engine status message for Start Run failures/loading.
- Moved the Three.js startup call to the end of its module so defaults and storage helpers exist before initialization.

## Phase 2 — Start Run

- Removed the old early Start Run click/pointer/touch bindings and the unused duplicate menu-routing function.
- Start Run now has one click path, an initialization lock, `aria-busy`, automatic unlock, and exception recovery.
- The persistent delegated listener survives the menu DOM being rebuilt after game over.

## Phase 3 — local Three.js

- Removed all Three.js CDN imports and kept `./assets/js/three.module.js` as the structured development module.
- Added the matching 0.160.0 classic build at `./assets/js/three.min.js` and load it before the three isolated inline game modules. This avoids browser CORS blocking when `index.html` is opened directly with `file://`.
- No data URL or version upgrade was used.
- Removed all runtime `http://` and `https://` dependencies from `index.html`.
- Native Gem purchasing and account syncing were intentionally not added.

## Direct-file launch repair

- Created `index.pre-file-launch-fix.html` before changing the loader.
- Replaced the three local ES-module fetch statements with references to the already loaded local `window.THREE` object.
- Preserved module isolation for the existing game scripts, avoiding cross-script declaration conflicts.
- Confirmed the menu and Start Run still load without console errors through local HTTP serving. The in-app browser security policy prevents automated inspection of a `file://` page, so a manual double-click test remains required.

## Visible developer controls

- Made the existing developer controls permanently visible in Settings instead of requiring `?soldev=1`.
- Exposed the real biome start picker, x1–x5 multiplier setter/test, and all four Meadow World near-completion/reward tests.
- Added a clear warning that these testing controls modify the device's current game save.
- Reused the existing developer functions and element IDs; no duplicate test system was added.

## Multiplier-scaled biome requirements

- Biome requirements now scale by the permanent Meadow World multiplier as well as score gain.
- At x2 every biome target doubles; x3 triples it; x4 quadruples it; and x5 multiplies it by five.
- Applied the same scaled values to transition triggers, the HUD biome tracker, daily biome goals, developer stage starts, developer threshold tests, and biome-reached lifetime stats.
- Scaled the tree-tunnel, blizzard, and rock-storm lead-in scores so their real-time spacing stays consistent at every multiplier.
- Cave entry now lands at the scaled Cave threshold when the blizzard finishes.
- Updated the tutorial to explain that higher multipliers increase final scores while biome timing remains balanced.

## Phase 4 — stats, saves, rewards, and multiplier

- Added displayed stats for Runs Completed and recorded run time; renamed the shield display to Shield Saves while retaining its existing ID.
- Added default fields for runs completed, shield saves, total run time, and best score without replacing healthy stored values.
- Added one idempotent run finalizer used for death, restart, manual exit, and page hide/refresh.
- Kept shield absorption and safe transitions out of death counts.
- Lane changes record only after moving into a different lane.
- Run time advances only during active gameplay and is finalized once.
- Synchronized legacy run/death keys with the lifetime object using the higher valid value for save compatibility.
- World claims are marked before currency is granted and count only the four known Meadow Worlds, preventing reload/duplicate rewards.
- Preserved the intended rewards: Meadow 25, Crystal 30, Cave 40, Volcano 50 Wispy Coins.
- Kept one permanent multiplier path: zero worlds x1, then x2 through x5 for one through four claimed worlds.
- Passive score applies that multiplier exactly once; biome checks use the resulting score.
- Added disabled-by-default developer tools (`?soldev=1` or `solPetsDevToolsEnabled=1`) for biome starts, score thresholds, multiplier math, nearly completed worlds, reward claims, and stats finalization.
- Added Pond to the developer biome picker.

## Phase 5 — shop and tutorial

- Preserved the four shop tabs and all existing item IDs, data attributes, ownership, prices, buy/equip/upgrade logic, and currencies.
- Kept distinct styles for Buy, Equip, Equipped, Upgrade, Locked, Gem pack, active tab, and Back controls.
- Strengthened the Wispy Coin currency card with a gold presentation and earning summary.
- Clarified attire cards with character, unlock condition, state, and honest placeholder labels where fitted transparent artwork does not exist.
- Equipped attire buttons now show a checkmark.
- Replaced overlapping tutorial pointer/touch handlers with one click path.
- Expanded the replayable Settings tutorial to eight concise pages covering lanes, jumping, Sol Meter, transitions, Meadow Worlds, Wispy Coins, multipliers, and attire rewards.

## Preserved systems

Characters, biomes, obstacles, transitions (including the original blizzard), Meadow Worlds, adaptive graphics, save keys, embedded images, rewards, progression, trail behavior, shop prices, and Gem test logic were not rebuilt or removed.

## Trail equip repair

- Reconnected the four existing trail shop buttons to their existing buy/equip functions through the single delegated menu click path.
- Preserved all trail ownership keys, prices, effects, and the `solPetsEquippedTrail` save key.
- Refreshes all trail card states whenever Shop opens so the saved equipped trail is shown immediately.
- Kept trail selection exclusive: equipping one trail removes the equipped state from the previous trail, and pressing the equipped trail removes it.
- Created the pre-edit backup `index.pre-trail-equip-fix.html`.

## Characters screen background repair

- Made the Characters overlay fully transparent and kept the real main-menu artwork active behind the character panel.
- Changed the panel and header from blue tones to warm cream and wood tones.
- Added safe-area padding, viewport height limits, and vertical scrolling so the character panel does not clip on shorter screens.
- Hides the live canvas only while Characters is open and restores it when leaving the screen.
- Fixed Close and `Play With` so both return to the real main menu instead of leaving a blue canvas-only screen.
- Created the pre-edit backup `index.pre-character-background-fix.html`.
- Created the follow-up backup `index.pre-character-menu-background-fix.html` before switching to the transparent main-menu background.

## Pond and Lava progression repair

- Promoted Pond Meadow into normal progression at 20,000 base score.
- Moved Lava Volcano from 20,000 to 30,000 base score so Pond no longer immediately triggers Lava.
- Added Pond to the gameplay biome tracker between Morning and Lava.
- Added the existing waterfall transition when Morning changes into Pond.
- Updated the developer Pond start to begin at Pond's real threshold and suppressed the already-passed rock storm for Night-or-later developer starts.
- All Pond and Lava requirements continue to scale through the shared x1-x5 biome multiplier system.
- Created `index.pre-pond-lava-threshold-fix.html` before the threshold change.

## Rainbow Trail and Nice Run reward chest

- Replaced the Rainbow Trail's blue-biased particle texture with neutral flame masks so every color remains saturated.
- Added the existing six-color rainbow texture as a moving ribbon inside each Rainbow Trail footprint while preserving the original trail timing and movement behavior.
- Added a lightweight CSS pixel-art chest with an apple and gems bursting from it on the Nice Run screen.
- Positioned the new chest between the daily-goal result and the Retry/Main Menu controls without changing reward calculations.
- Preserved all trail ownership, price, equip, reward, currency, and save keys.
- Created `index.pre-rainbow-trail-pixel-chest-fix.html` before editing.

## Daily Level Goal Gem reward

- Kept the existing rotating Daily Level Goal and its UTC daily reset countdown.
- Completing today's biome goal now unlocks a claimable `5 Gems` reward inside Daily Missions.
- Added a separate `solPetsRunGoalRewardClaimedDate` save key so completion and claiming remain independent and old saves stay compatible.
- The Daily Missions badge now includes an unclaimed Daily Level Goal reward.
- Claiming marks the reward before adding Gems, preventing repeated taps or page refreshes from duplicating it.
- Added pending and claimable states to the Daily Level Goal row; after claiming, the row disappears until the next daily goal becomes active.
- Created `index.pre-daily-goal-row-hide.html` before changing the claimed-row behavior.
- Updated the completion toast, goal information, and Nice Run result message to direct players to Daily Missions.
- Included the new claim state in the existing Delete Save Data reset path.
- Created `index.pre-daily-level-goal-reward.html` before editing.
