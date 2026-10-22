# WWE WrestleFest XBLA - DLC Unlock, Ground Controls & Complete Movelist

Patches and a complete roster reference for the WWE WrestleFest Xbox 360 prototype running in [Xenia Canary](https://github.com/xenia-canary/xenia-canary).

- **Unlock the embedded DLC wrestlers and rings/arenas**, making the full 38-wrestler roster available in the supported build.
- **Use RB for ground-move combinations**, including Yokozuna's Banzai Drop.
- **Explore the complete movelist**, with controls discoveries and setup conditions.

**Tap RB beside a grounded opponent to access the ground-move selection that normally requires simultaneous A+B presses.** The shortcut applies to the shared handler used by all 38 wrestlers, including the DLC characters made available by the separate DLC-unlock patch.

The project first unlocked the embedded DLC roster and rings, then investigated a seemingly missing move: Yokozuna's Banzai Drop. The move and animation were already present. Supplying both required input flags inside the ground handler made it execute successfully.

## Download

Open this repository's **Releases** section and download the attached patch ZIP. Extract it before installing.

The release contains:

| File | Purpose |
| --- | --- |
| `5841123D.patch.toml` | Unlocks embedded DLC availability for wrestlers and rings/arenas |
| `5841123D-Ground-Controls.patch.toml` | RB ground-move shortcut for all 38 wrestlers |
| `WrestleFest_Complete_Roster_Movelist.pdf` | 24-page guide with every wrestler's assigned moves, confirmed controls, and remaining uncertainties |
| `README.md` | Installation, compatibility, and usage instructions |

You must supply your own game copy. The download contains patches and documentation; it does not include game executables, game packages, or extracted assets. The DLC content is already embedded in the supported game build.

## Supported build

| Item | Tested value |
| --- | --- |
| Game | WWE WrestleFest, Xbox 360 prototype / XBLA build |
| Title ID | `5841123D` |
| Xenia module hash | `DB87FBA79C0275D9` |
| Emulator used during testing | Xenia Canary, build `e6cd2e4bc` |

Both patches target this exact game module. Compatibility with other game builds or emulator versions has not been established. Check your Xenia log for `Module Hash` if either patch does not apply.

## Installation

1. **Close Xenia completely.**
2. Open the `patches` folder in your Xenia Canary storage directory. In a portable setup, this is normally beside the emulator executable. Create the folder if needed.
3. Copy both `5841123D.patch.toml` (DLC unlock) and `5841123D-Ground-Controls.patch.toml` (ground controls) into that folder. You may install either patch independently if you only want that feature.
4. If you used the development patches, remove `5841123D-Move-Trace.patch.toml` and `5841123D-Banzai-Test.patch.toml` from the active patches folder. They share the injected code area and must not be loaded alongside this patch.
5. In your Xenia configuration, set the existing `apply_patches` setting to `true`. Both patches already have `is_enabled = true`. For the full-game setup used during testing, set the existing `license_mask` setting to `-1`. That setting alone did not unlock the embedded DLC; the DLC patch is also needed.
6. Save the configuration, restart Xenia, and launch the supported game build.

Both patches apply in emulator memory and do not rewrite your game package. To uninstall either feature, close Xenia, remove its patch file, and restart. Removing the DLC patch does not itself undo the separate `license_mask` configuration setting.

## DLC unlock: wrestlers and rings

`5841123D.patch.toml` changes the supported build's copied DLC availability flags so its embedded DLC wrestlers and rings/arenas become available. The Xbox roster is organized as 18 base wrestlers plus four packs of five, for **38 wrestlers total**. The patch makes the embedded arena content available through the same DLC availability system.

No iOS app, separate DLC asset download, or file conversion is needed. The patch does not add content absent from this build or change movesets. It works independently of the RB shortcut.

After restarting, check the wrestler and arena selection screens for the previously locked DLC choices.

## Ground controls: how to use RB

With your opponent lying on the mat, stand near the relevant part of their body and **tap RB**. The original game logic still decides which move is available based on positioning, the opponent's condition, match rules, and your wrestler's assigned moveset.

The patch supplies newly-pressed A+B flags only when the shared grounded-opponent handler receives a new RB press. It contains no diagnostic logging.

### Why does Banzai Drop happen before the opponent is nearly beaten?

That is how this build assigns the move:

| Move | Ground-move category |
| --- | --- |
| Yokozuna's Banzai Drop | Normal ground list, above the opponent's low-health threshold; head position |
| Yokozuna's Force Stomp | Low-health ground list |
| The Rock's People's Elbow | Low-health ground list |
| John Cena's Five Knuckle Shuffle | Low-health ground list |

The shortcut preserves those assignments. The corresponding tables in the supplied iOS 1.07 copy match. Whether the unusual Banzai assignment was intentional is unknown. Do not rely on health-bar color alone to identify the threshold.

## Movelist and controls discoveries

The PDF contains **38 wrestlers, 569 assigned move slots, and all 95 shared move definitions**, organized by running attacks, front grapples, running counters, ground attacks, rear attacks, turnbuckle attacks, and tag-in attacks.

Useful findings include:

- **Running counters:** B selects the first counter entry; A selects the last. With one entry, both select the same move.
- **Front grapples:** A or B advances the same alternating selector. Opponent grapple resistance and a special power flag determine whether it uses slots 1-2 or 3-4. A versus B does not directly select a particular slot in the checked routine.
- **Rear attacks:** stand behind a standing-tired opponent, face the same direction, and tap B. The rear list cycles. Orton's list contains Bulldog followed by RKO; RKO execution was confirmed in play.
- **Headlock tag attacks:** hold an opponent in a headlock near your own corner, wait for tag readiness / the tag hint, and press B. Your partner must be ready. Positioning is sensitive.
- **Animal and Hawk:** both are assigned Tag Elbow Drop. No Doomsday Device entry was found in the inspected move/tag tables or explicitly named animation data.

The guide distinguishes code findings from play-test confirmations. Some input details remain unaudited; a complete move inventory is not an exhaustive test of every move under every condition.

## Confirmed results and limits

Project testing confirmed the embedded DLC unlock. Gameplay testing also confirmed Yokozuna's Banzai Drop with the RB workaround, Orton's RKO, headlock tag attacks, and several front-grapple finishers, including Rock Bottom, Stone Cold Stunner, and Attitude Adjustment. The expanded ground shortcut was reported working across additional play.

The original ground handler checks for A and B both becoming newly pressed in the same update. Physical A+B attempts did not reach that selection path in the recorded tests. The RB injection supplied both flags together and succeeded. **The underlying cause has not been isolated to the game, Xenia, or controller input timing.** This is an input workaround, not a confirmed emulator-wide fix.

Running outside the ground handler keeps its existing controls. This patch does not provide a universal RB shortcut for every A+B action, import iOS code, add animations, or change damage, AI, health thresholds, or move assignments.

## Troubleshooting

**RB still runs or no signature move happens:** check that the patch applied to the supported module hash, then check positioning and the opponent's condition. RB does not force a specific signature move in every situation.

**Yokozuna stomps instead:** at/below the opponent's low-health threshold, his original move list switches to Force Stomp. Try Banzai Drop earlier in the match, at the opponent's head.

**Tagging produces a grapple instead:** the tag-readiness check may be failing. Confirm your corner position, your partner's readiness, and the headlock setup. The ground patch does not modify tagging.

**A DLC wrestler or ring is still locked:** verify that `5841123D.patch.toml` is installed and enabled, `apply_patches` is `true`, and the log reports the supported module hash and application of the embedded-DLC patch. The RB patch alone does not unlock DLC. `license_mask = -1` alone also did not unlock the DLC in our tests.

## Reporting an issue

Include your Xenia build, the game's module hash, active patch filenames, wrestler and opponent, match type, attempted input, and a short description or video of the setup. If attaching a log, check it for personal file paths first. Do not attach game packages or executables.

## Credits

- **Chris:** project direction, gameplay testing, reproduction steps, and confirmation of the recovered move behavior.
- **AI-assisted analysis:** executable and data-table inspection, patch development, and documentation with OpenAI Codex / ChatGPT.
- **Xenia contributors:** the emulator and its game-patch system.

This is an unofficial community research project, unaffiliated with WWE, the game's publishers or developers, or the Xenia project. Game names and trademarks belong to their respective owners.
