# Sol Pets Meadow Dash v12 testing

Tested July 14, 2026 against `index.html` through a local HTTP server using the in-app browser. Developer test controls are now visible in Settings without a query parameter. The original `file://` tab was not automated because the browser security boundary blocks scripted control of local-file tabs. A later direct-launch repair removed all external module fetches from the inline game modules and added the matching local Three.js classic build.

## Required test sequence

1. **Open main menu — PASS.** Menu rendered with local Three.js and also with the engine module intentionally unavailable.
2. **Stats and Back — PASS.** Lifetime Stats opened, displayed the expanded fields, and returned to the menu.
3. **Shop and every tab — PASS.** Trails, Attires, Wispy, and Gems each became active and showed their matching section. The test was repeated with Three.js unavailable.
4. **Settings and tutorial replay — PASS.** How to Play opened at page 1/8, advanced to the final attire page, closed, and replayed with the engine unavailable.
5. **Daily Missions and Back/Close — PASS.** The normal mission view opened; the engine-free fallback opened a non-blocking saved-progress message and closed correctly.
6. **Start a run once — PASS.** One click started one run.
7. **Die and return to menu — PASS.** Natural death showed the reward flow and returned to the rebuilt menu.
8. **Start a second run — PASS.** Start continued working after the menu DOM was rebuilt.
9. **Pause and resume — PASS.** Paused, resumed, paused again, and used Main Menu.
10. **Saves after refresh — PASS.** The serialized lifetime object before refresh matched the object loaded after refresh.
11. **Old save compatibility — PASS.** Existing legacy run/death values were retained and merged into the expanded lifetime object; missing fields received defaults without lowering healthy values.
12. **World rewards do not duplicate — PASS.** First/second claims produced: Meadow 25/0, Crystal 30/0, Cave 40/0, Volcano 50/0 Wispy Coins.
13. **Multiplier changes score exactly once — PASS.** One simulated second gained 10, 20, 30, 40, and 50 points at x1, x2, x3, x4, and x5 respectively.
14. **Each biome threshold — PASS.** Browser starts verified Snow 2,500; Cave 5,000; Night 7,000; Ice Cave 10,000; Morning 15,000; Pond 20,000; Lava 30,000; Rainbow 50,000.
15. **No blocking overlay — PASS.** The offline/loading overlay remained hidden and pointer-inert; menus stayed clickable during engine failure.
16. **Offline menu operation — PASS by engine-failure simulation; literal network toggle unavailable.** The local Three.js file was temporarily renamed, the page reloaded, and Stats, Shop/tabs, Settings/tutorial, Daily Missions, Back, and Start error handling were exercised. The file was restored afterward. The browser harness does not expose a network-disable switch.
17. **Offline game with local Three.js — PASS on local-only serving.** `index.html` loads `./assets/js/three.min.js`, the three game modules use that local `window.THREE` object, Start Run initialized without console errors, and the HTML contains no HTTP(S) runtime URLs. The structured `assets/js/three.module.js` development file is also retained. A literal device airplane-mode test remains recommended before packaging.

## Additional verification

- **Run accounting — PASS.** A natural-death test changed runs started, runs completed, and deaths by exactly +1 each. A later input run recorded one jump and one completed lane change.
- **Manual exit finalization — PASS.** A second run was paused and returned to Main Menu without double-finalization.
- **Start lock — PASS by audit and repeated run testing.** One delegated click route owns Start Run; no Start pointer/touch listener remains.
- **Syntax — PASS.** All 15 inline/module scripts parsed with zero syntax errors using Node VM module parsing.
- **Rendered duplicate IDs — PASS.** The live DOM contained no duplicate IDs. Naive text scans report ID-shaped strings inside JavaScript templates, so the live DOM result is authoritative.
- **Duplicate declarations — PASS at parser level.** No same-scope declaration errors occurred in any script.
- **Startup order — PASS.** Engine startup occurs after defaults and helpers are defined; the expanded stats object survived refresh.
- **Startup console — PASS.** No console errors were present after a clean reload with the restored local module.
- **Three.js file integrity — PASS.** Size 1,272,972 bytes; SHA-256 `76DEA8151BC9352AEF3528B4262E249B2604F62543828328DB978D060D61A495`.
- **Direct-launch loader regression — PASS through local HTTP serving.** The repaired loader opened the main menu and started a run with zero console errors. Three.js emits its own r160 deprecation warning for the classic build; this is informational and does not stop the game.
- **Visible Settings developer controls — PASS.** Settings shows the biome picker, multiplier x1–x5 controls, and Meadow/Crystal/Cave/Volcano world testing controls without a query parameter. The panel continues to call the existing real multiplier and reward functions.
- **Scaled biome requirements — PASS.** Developer output returned Pond/Lava targets of 20,000/30,000 at x1, 40,000/60,000 at x2, 60,000/90,000 at x3, 80,000/120,000 at x4, and 100,000/150,000 at x5. All earlier biome and Rainbow targets continued scaling by the same multiplier.
- **Scaled stage start and tracker — PASS.** Starting Snow at x3 began at approximately 7,500 and the tracker correctly showed Cave at 15,000, with 7,499 points remaining after the first frame. No console errors occurred.

## Remaining risks and intentionally unfinished work

- Shield-save counting was code-path audited but not forced through a collision in the browser test; it should receive a dedicated gameplay QA pass.
- Page-refresh finalization uses `pagehide`, which is the best available browser path but cannot guarantee a write after an operating-system force kill.
- Attire visuals without fitted transparent character art remain explicitly labeled placeholders.
- Native Gem purchases, restoration, receipt verification, paid-Gem account sync, and Capacitor wrapping were intentionally not implemented.
- Performance was not benchmarked. The adaptive graphics system was preserved and no new continuous particle/blur system was added.
- The automated browser cannot inspect `file://` pages. Manually double-click `index.html` once to confirm the direct-file loader in the target browser; the blocked local module fetch has been removed.
- A final physical-phone pass should cover touch gestures, airplane mode, WebGL context loss, and multiple background/foreground cycles.

## Trail equip repair verification

- **Trail button routing - PASS.** The existing Rainbow, Blue, Fire, and Gold controls each changed to `Equipped` when selected in the live Shop.
- **Exclusive selection - PASS.** Switching from Blue to Fire, then Rainbow, then Gold returned the previous trail to `Equip`; only one card showed `Equipped` at a time.
- **Unequip - PASS.** Pressing the equipped Gold control returned all four owned trails to `Equip`.
- **Save persistence - PASS.** Fire still showed `Equipped` after a full page reload and reopening Shop > Trails.
- **Syntax regression - PASS.** All 15 inline/module scripts parsed successfully after the repair.
- Tests used an isolated localhost test save with all four trails owned. The player's real `file://` save was not read or modified.

## Characters screen background verification

- **Background - PASS.** The Characters overlay rendered transparently with the complete Sol Pets main-menu artwork visible behind it; the blue WebGL canvas stayed hidden.
- **Panel rendering - PASS.** The character art, five character thumbnails, ownership labels, close control, and `Play With` control remained visible and functional.
- **Close navigation - PASS.** Closing Characters restored the complete main menu instead of leaving the canvas-only background.
- **Play navigation - PASS.** Pressing `Play With Wispy` restored the complete main menu and cleared the Characters-open state.
- **Syntax regression - PASS.** All 15 inline/module scripts parsed successfully after the repair.
- A physical-phone portrait/landscape pass is still recommended for device-specific safe-area behavior.

## Rainbow Trail and reward chest verification

- **Rainbow color rendering - PASS.** A live Pond run showed a saturated red, orange, yellow, green, cyan, blue, and purple trail behind Wispy instead of a blue-tinted effect.
- **Trail behavior - PASS.** The repaired Rainbow Trail continued spawning and fading behind the moving character through the existing trail system.
- **Nice Run chest placement - PASS.** After a natural death, the pixel chest rendered after `Daily level goal not reached yet.` and before Retry.
- **Pixel loot artwork - PASS.** The chest visibly showed an apple and two gems bursting from its open lid.
- **Retry interaction - PASS.** Retry remained unobstructed, started a new run, removed the result screen, and advanced the score.
- **Rendered duplicate IDs - PASS.** The Nice Run DOM contained 166 IDs and no duplicates.
- **Syntax regression - PASS.** All 15 inline/module scripts parsed successfully after the repair.

## Daily Level Goal reward verification

- **Claim row - PASS.** A completed Daily Level Goal appeared at the top of Daily Missions with a `5 Gems` reward and its own Claim button.
- **Ready badge - PASS.** The main-menu Daily Missions badge included the unclaimed level-goal reward.
- **Exact reward - PASS.** Claiming changed the isolated localhost test balance from 4 Gems to 9 Gems.
- **Claimed state - PASS.** The level-goal row and its Claim button disappeared after claiming, and the ready badge count decreased by one.
- **Duplicate prevention - PASS.** After a full refresh, the balance remained 9 Gems and the already-claimed level-goal row stayed absent.
- **Rendered duplicate IDs - PASS.** No duplicate IDs were present while the Daily Missions reward row was open.
- **Daily reset - PASS by code-path audit.** Completion and claim availability are both keyed to the existing UTC day key; the next goal becomes available at the displayed daily reset.
- **Syntax regression - PASS.** All 15 inline/module scripts parsed successfully after the reward change.
