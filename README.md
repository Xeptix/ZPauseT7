# ZPause T7

**Synced co-op pause for Black Ops III Zombies**

by Xep

[**Download the latest release**](https://github.com/Xeptix/ZPauseT7/releases/latest)

A port of [ZPause](https://github.com/Xeptix/ZPause), the Black Ops II pause mod, to
Black Ops III. Same design, same settings, same version numbering — v1.4 here is feature
equal to v1.4 there.

Any player can pause. Any player can unpause. The state lives on `level`, so it's
identical for everyone — there's no per-client state that can desync.

- Zombies stop where they are, mid-stride, and stop spawning
- Boss fights and scripted encounters hold too
- Players are locked and can't be hurt
- The match clock, powerup timers, effect countdowns and bleedout all hold
- Resumes on a 3‑2‑1 countdown with a short grace period
- Optional vote-to-pause, so a pause has to carry the room

---

## Requirements

Black Ops III, zombies.

How ZPause loads depends on how you play. **The download carries every loose-script route**
— `install.bat` finds which of them you have and installs only to those. It is the same
installer that ships with every ZPause download: it knows all five games, and a copy kept
on your PC can install any of them, fetching a game's release from GitHub when the files
are not beside it.

| You play with | ZPause loads as | Mod slot |
|---|---|---|
| **BOIII** / **Ezz BOIII** | a loose script in `boiii\custom_scripts` | free |
| **T7x** | a compiled script in `t7x\custom_scripts` | free |
| **Steam**, mods from the Workshop | the Workshop build | **takes it** |
| the **t7-compiler** | an injected script | free |

The two loose routes are the same file. BOIII compiles raw GSC and accepts "a hybrid of
BO2-style and BO3-style syntax", and the t7-compiler is where that dialect came from. Ezz
BOIII is a BOIII fork with the same `boiii.exe` and the same folders, so it is the same
route.

**T7x loads compiled GSC rather than source** — its own scripts ship as compiled `.gsc`
files beside `.gsc_raw` sources, and a raw file in `custom_scripts` does not load. The
download carries a build made for it, so the installer treats T7x like any other route and
you do not need mod tools.

The one thing that route cannot do is take your settings from the installer. The config
editor works by rewriting defaults into the installed script, and a compiled one has no
text to rewrite — so it writes a `zpause.cfg` into T7x's own settings folder instead, and
in game you run:

```
exec zpause/zpause.cfg
```

Once per session, or from a bind. Everything else is the same script the other routes
run.

**The Workshop build takes your one mod slot**, because Black Ops III only enables one mod
at a time. That is the whole trade: easiest to install, and it cannot share the game with
another mod. Every other route is a loose script and takes no slot.

### Things that are not routes in

| | What it is |
|---|---|
| [T7 Patch](https://github.com/shiversoftdev/t7patch) | security patch — a `d3d11.dll`, game launches normally |
| [Clean Ops](https://github.com/notnightwolf/cleanopsT7/wiki) | the same, plus p2p hosting and an anti-cheat |
| [BO3Enhanced](https://github.com/shiversoftdev/BO3Enhanced) | lets the Windows Store build run on Steam files |

None of the three loads scripts, and none takes the mod slot — ZPause runs alongside any of
them by any route above. **Run T7 Patch or Clean Ops, not both:** Clean Ops carries all of
T7 Patch's security work and says plainly that it conflicts with it. Run one regardless of
ZPause, since unpatched Black Ops III has remote code execution exploits that get used in
public lobbies.

The one pairing to avoid is **the t7-compiler under a patch**. T7 Patch says it "uses a lot
of complicated techniques to patch the game that interfere with most third party tools",
and an external injector is exactly such a tool. Use BOIII, T7x or the Workshop build
instead — none of them injects anything.

---

## Install

Run **`installer\windows\install.bat`**. It finds your Black Ops III install, works out
which script loaders you actually have, ticks those, and copies to only what you leave
ticked:

```
  Where ZPause can go

    [x] 1. BOIII / Ezz BOIII  installed   loose script, no mod slot
    [ ] 2. BOIII (AppData)    not found   original BOIII's other script folder -- Ezz BOIII clears it on launch
```

Type a number to change its mind, Enter to go ahead. It writes `zpause.gsc` and nothing
else.

That is the ZPause T7 Manager, and it does more than a first install needs:

- **install or update** to the routes you tick, after showing the changelog for the
  version you are about to get
- show **what's installed**, and which version each copy is
- **configure ZPause** — every setting, grouped the way the script groups them, each with
  its default and a one-line description of what it does. See below.
- **remove** ZPause again — the loose scripts only; a Workshop subscription is Steam's
- **check GitHub** for a newer release and download it, with a progress bar
- **install a different version** — every download it makes is kept, so going back to an
  older build is the same two keystrokes as going forward. It can list what GitHub has and
  fetch any of those too.
- **put back a file it replaced** — it copies out whatever it is about to overwrite, so an
  install can be undone even over a script you had edited yourself
- **check my setup** — one key that looks for the handful of things that actually go
  wrong: copies at different versions, files something has edited since they were
  installed, settings that never reached the game, settings that cannot reach T7x
- **keep itself** on your PC with a Desktop or Start-menu shortcut, so you never have to
  go looking for the download again

Nothing in that keep-list is ever deleted behind your back. After a download it shows what
it is holding and offers to clear the older ones out — answering no keeps them all. It
also writes a plain-text log of everything it installs or removes.

### Configuring it from the installer

Every setting is a dvar, and the config editor is a way to set them without touching a
console. It reads the settings out of the script itself, so the list is always right for
the version you have, with the description of each one from the table further down.

Saving writes a **`zpause.cfg`** into your Black Ops III folder — plain `set zp_vote "1"`
lines, which is exactly what a dedicated server execs, so it is also the file to send
someone or reuse on another PC — and puts the values into the installed script, so they
take effect with no console step. You can pick either or both. Your settings are re-applied
automatically after an update, so a new version never quietly resets them.

**T7x is the exception**, because it runs a compiled script and there is nothing in one to
rewrite. Saving writes its cfg to T7x's own settings folder and you run
`exec zpause/zpause.cfg` in game — see above. The other three routes need no console step.

Settings that take a fixed set of values offer that list rather than a blank prompt, so a
typo cannot leave you with a combo the game silently ignores. Type a setting's name at any
config screen to jump straight to it. You can keep several **profiles** — a solo one and a
server one, say — and switch between them; each is its own shareable cfg. And when an
update changes a default you had been getting implicitly, it says so before installing.

### Checksums

Every download from v1.4 on carries a **`SHA256SUMS`**, and the installer checks the whole
download against it before touching anything. It is a plain coreutils manifest, so you can
check it yourself in the extracted folder:

```bash
sha256sum -c SHA256SUMS
```

### Without the menu

```
install.bat -Install -Yes        install to every loader it finds
install.bat -Uninstall -Yes      remove every copy it can find
install.bat -Find                show what it detects, change nothing
```

`install.sh` takes the same things as `--install --yes`, `--uninstall --yes` and `--find`.

It asks before it touches the network, every run — answer no and it makes no connection
at all. It only ever writes or removes `zpause.gsc`, at paths it found itself: no deletes,
no folders removed, nothing else touched.

**On Linux**, `installer/linux/install.sh` does all of the same things, and knows that
Black Ops III runs under Proton there: the game folder is an ordinary path, but
`%localappdata%` lives inside the Proton prefix at
`steamapps/compatdata/311210/pfx/drive_c/users/steamuser/`. It looks in every Steam library
including a Steam Deck's SD card, and in Heroic's shared prefix for DeckOps installs.

```bash
./install.sh          # or  bash install.sh
./install.sh --find   # show what it detects, change nothing
```

### On a Steam Deck

Switch to Desktop Mode and use **`installer/linux/Install ZPause.desktop`**. KDE will not
run a desktop entry until you allow it: right-click it → **Properties** →
**Permissions** → tick **Is executable** → **OK**, then double-click.

It looks for Black Ops III in every Steam library including an SD card, and for BOIII's
script folder inside the Proton prefix.



### By hand

Everything in the download is laid out to match where it goes:

```
Black Ops III/                          drop into your Black Ops III folder
  boiii/custom_scripts/zpause.gsc
AppData/                                drop into %localappdata%
  boiii/data/custom_scripts/zpause.gsc
t7-compiler/                            a compiler project - build it
  gsc.conf
  scripts/zpause/main.gsc
```

Both `Black Ops III` and `AppData` merge into folders you already have, so the whole folder
can go across at once. Original BOIII reads both its game-folder and its AppData script
folders, so either one works there.

**Ezz BOIII clears its AppData folder every time it launches**, taking anything it didn't put
there with it — so on Ezz BOIII, use the game folder. A copy in AppData is gone before the
game has started.

### Steam Workshop

**[Subscribe on the Workshop](https://steamcommunity.com/sharedfiles/filedetails/?id=3800204505)**,
then pick ZPause from the Mods menu. Nothing to build and nothing to copy.

**It takes your one mod slot.** Black Ops III only enables one mod at a time, so while
ZPause is selected you can't run another. If that matters, use one of the loose-script
routes above — they take no slot and coexist with any mod you like. To run it with ZShare,
subscribe to **[ZBundle](https://steamcommunity.com/sharedfiles/filedetails/?id=3802815997)**,
which is both in one mod. All three are in
[Xep's Black Ops III Zombies collection](https://steamcommunity.com/workshop/filedetails/?id=3802560637).

**In your language.** The Workshop build draws every line in the language your game is set
to — English, French, Italian, Spanish, German, Brazilian Portuguese, Russian, Polish,
Japanese, and Traditional and Simplified Chinese — and in a mixed lobby each player reads
their own. The loose-script routes stay in English: there is no fastfile to carry
translations in.

### The t7-compiler

`t7-compiler/` is a [t7-compiler](https://github.com/shiversoftdev/t7-compiler) project.
Put it wherever you keep those and build it:

```
DebugCompiler.exe --build
```

`gsc.conf` names the stock script it's injected over:

```
script=scripts/shared/duplicaterender_mgr.gsc
```

That's the compiler's own default and it does nothing in zombies, which is what makes it
safe — but it's also the slot every tutorial-following script uses, and two scripts can't
share one. If something else already occupies it, point `gsc.conf` at another script you're
not using and rebuild.

This is the one route not to use under a security patch — see above.

### Why the Workshop build wears a stock script's name

A mod's GSC doesn't run just for being in the fastfile; something already loaded has to
reach it. So the Workshop build stands in for `scripts/zm/gametypes/_clientids.gsc`, which
is the one worth taking: 72 lines, nothing in the game calls `clientids::` from outside it,
and `.clientid` is read only by campaign script. Everything it does is carried across
unchanged, so overriding it costs nothing.

The loose-script routes need none of that — BOIII, T7x and the compiler all load a script
directly, so there it keeps its own namespace and stands in for nothing.

You don't need to restart the game to load a new script — end the current match and start a
new one.

---

## Usage

| Action | Input |
|---|---|
| Pause / unpause | hold **crouch + melee** together for ~0.3s |
| Vote yes, while a vote is open | the same combo |
| Vote no, while a vote is open | hold **jump + melee** |

Crouching *or* prone counts — `stancebuttonpressed()` covers both.

The combo keeps working while you're frozen: `freezecontrols()` blocks movement and weapon
use, but button state still reaches the server. That's what lets a frozen player resume.

**The chat commands work here too**, on every route. Type `!pause` or `!p` to pause,
`!unpause` or `!resume` to resume, and `yes` / `no` while a vote is open.
`zp_allow_short_words` widens them to the bare words. The combos matter all the same: a
downed player's buttons change, and chat is not always at hand.

### While you're down

You can't crouch from the floor, and once you've bled out the engine stops delivering
those buttons at all. Downed and spectating players switch to:

| Action | Input |
|---|---|
| Pause / unpause / vote yes | hold **use + aim** |
| Vote no | hold **use + fire** |

The HUD shows a `while down:` line whenever anybody is in that state.

### The settings menu

While the game is paused, the host can change ZPause's settings without the console. Hold
**fire + melee** to open the menu:

| Button | Does |
|---|---|
| aim / fire | move up and down the list |
| grenade | change the setting |
| melee | close |

A switch flips, a list moves on to its next choice, and a number steps up through a few
common values and back round to the lowest — exact values are still the console's. A
change lands when play resumes, the same as one typed into the console, and lasts until the
game closes. The menu closes itself when a vote opens, since the host needs the buttons
back to vote. `zp_menu 0` turns it off.

On the Workshop build the menu speaks your language too; the setting names and their
values stay as the console spells them.

#### In the lobby

The same settings are in the lobby too, on the Workshop build and on BOIII, Ezz BOIII and
T7x. On the Workshop build, load ZPause from the Mods menu; on the other three the installer
puts the menu in the game folder's `ui_scripts`, beside the script. Either way the zombies
lobby gets a **ZPAUSE SETTINGS** button for the host: a page for each part of ZPause, with
what the setting under the cursor does and what its default is beside it. Left and right
change a setting. **DEFAULT** leaves it to the script's own default, and **RESET TO
DEFAULTS**, at the bottom of the first page, puts every setting back there at once.

A change made there is saved, so it is still set the next time the game starts, and a match
picks it up as it loads. On the Workshop build it goes into the mod's own save data. On
BOIII, Ezz BOIII and T7x, which have no mod to keep it in, it goes into the names of your
offline zombies custom classes, which nothing in a zombies game shows or uses. A change made
from the pause menu still lasts until the game closes. The t7-compiler route has no lobby
button: it injects into a game that loads no menus of its own.

---

## Voting

Off by default. `zp_vote 1` and a pause has to carry the room instead of any one player
stopping the game.

Calling a vote is the same action as pausing. The vote runs 30 seconds and everyone gets a
tally: the count, the clock, and every player with how they voted.

The bar is whichever is higher, `zp_vote_min` or `zp_vote_percent` of the players in the
game, then clamped to how many are actually present — so a lobby can't set a threshold
nobody there can clear, and solo play skips the vote entirely. With the defaults that's
2 of 2, 2 of 3, 3 of 4.

A vote ends the moment it's decided either way. Disconnects take their vote with them. A
failed vote locks out the next one briefly so it can't be spammed. Resuming doesn't need a
vote by default, so one AFK player can't strand everyone in a paused game.

---

### Who decides

Five settings answer the same question — who may pause, and who has to agree. They can all
be on at once, so this is the order the script applies them in.

**Asking to pause:**

| | Setting | What happens |
|---|---|---|
| 1 | `zp_host_only` | Anybody but the host is turned away here. Nothing below runs for them. |
| 2 | — | Refused while the game is still starting. |
| 3 | `zp_round_pause` | If a pause is already waiting for the round to end, asking again calls it off. |
| 4 | `zp_max_pauses` | Refused once the match has spent its budget. |
| 5 | `zp_cooldown` | Refused if the last pause was too recent. |
| 6 | `zp_host_approve` | A non-host's ask goes to the host to answer. **Takes precedence over `zp_vote`.** |
| 7 | `zp_vote` | Otherwise, with voting on, it goes to a vote. |
| 8 | `zp_round_pause` | Once it is agreed — outright, approved or voted — it waits for the round to end instead of happening now. |

**Asking to resume:**

| | Setting | What happens |
|---|---|---|
| 1 | `zp_host_only` | Anybody but the host is turned away. |
| 2 | — | With a vote already open, the input is a yes instead. |
| 3 | `zp_cooldown` | Refused if the last toggle was too recent. |
| 4 | `zp_ready_check` | The input marks you ready rather than resuming. **Takes precedence over `zp_vote_unpause`.** |
| 5 | `zp_vote_unpause` | Otherwise, with `zp_vote` on as well, it goes to a vote. |

Three things sit outside all of that:

- **`zp_max_pause_time` ends a pause whatever else is set.** It is the way out of a ready
  check nobody answers, or a request the host never sees. Leave it at `0` and there is no
  way out but somebody pressing something.
- **A pause nobody asked for skips the lot.** `zp_pause_on_disconnect` pauses immediately:
  it does not wait for the round, does not spend the budget, and asks nobody.
- **`zp_host_only` with `zp_host_approve` is just `zp_host_only`.** The first turns the
  request away before there is anything left to approve.

## Configuration

Every setting is at the top of the file, and each one is also a dvar of the same name. The
script creates each dvar with its default on load, so you can set them straight from the
console:

```bash
zp_countdown 5
```

The config is re-read every five seconds while the game is running, and again
whenever a pause is requested, so a change takes effect **almost straight away** — no map
restart needed.

### Where settings are saved

Three routes in, and they keep a setting for different lengths of time:

| Set it here | How long it lasts |
|---|---|
| The installer's config editor | Permanently — it rewrites the default in the installed script. T7x runs a compiled script instead, so it gets a cfg you `exec`. |
| The lobby menu | Permanently — it saves into the mod's own storage on the Workshop build, or your offline zombies class names on BOIII, Ezz BOIII and T7x, and puts it back once per game start. |
| The in-game settings menu, or the console | Until the game closes. |

Black Ops III is the one port with no settings file behind all this: its file functions are
all developer-only, so a released script cannot read one. That is why the lobby menu saves
in the game's own storage instead, and why the in-game menu cannot save for you the way the
Black Ops II, Black Ops and World at War builds do.

The periodic re-read is skipped while the game is paused: the HUD is built from these
settings when the pause starts and nothing rebuilds it in place, so moving them underneath
would leave elements where the old values put them. A change made mid-pause lands the
moment play resumes. It also means `zp_combo` can be changed by hand — before, that needed
a pause to take effect, and the combo is what asks for one.

`set zp_config_print 1` in the console prints every setting below with the value it is
currently holding, then puts the switch back so it can be used again. On Black Ops III it
needs a client running developer mode, because `println` is a dev-only call there and the
game refuses to load a script that makes one outside a devblock — which is the same mode
that gets you the console it prints to.

Black Ops III also only offers console completions for the dvars its own engine
registered, so the `zp_` names never appear in its suggestions. They still read and set by
their full name.

| Dvar | Default | What it does |
|---|---|---|
| `zp_menu` | `1` | Let the host change settings from a menu while the game is paused: hold fire and melee to open it. |
| `zp_host_only` | `0` | Only the host can pause or resume. Everyone else's chat command and combo are ignored, and a pause never goes to a vote. On a dedicated server there is no host, so it falls to whoever holds the first player slot. |
| `zp_only_script` | `0` | Debug. With both a loose script and the Workshop mod installed, run only the loose one. |
| `zp_only_mod` | `0` | Debug. The same, the other way round. Both off — the default — is whichever loads first. Both on leaves nothing running. **Read when the script loads**, so end the game and start a new one for a change to take. |
| `zp_allow_short_words` | `0` | Also accept bare `p` / `u` / `pause` in chat. Off by default so normal conversation can't pause the game. |
| `zp_button_combo` | `1` | Enable the button combos. |
| `zp_combo` | `crouch_melee` | Which combo pauses: `crouch_melee`, `crouch_use`, `crouch_frag`, `crouch_ads`, `jump_melee`, `use_frag`, `frag_only`, `use_ads`, `use_attack`, `attack_ads`. |
| `zp_button_hold_time` | `0.3` | How long the combo must be held. |
| `zp_button_combo_dead` | `use_ads` | Combo used while downed or spectating, when stance and melee stop registering. Also takes `use_attack`, `attack_ads`, `use_frag`, `frag_only`. `none` = chat only. |
| `zp_vote_no_combo_dead` | `use_attack` | The same, for a no vote. |
| `zp_input_debug` | `0` | Print each player which buttons the server receives from them, for picking the two above. |
| `zp_host_approve` | `0` | The host pauses at once; anyone else has to ask and the host answers yes or no. It runs as a vote only the host can cast, so the yes/no input, the HUD and the timeout are a vote's. Pausing only — resuming still follows `zp_vote`. `zp_host_only` wins where both are set. |
| `zp_ready_check` | `0` | Resuming waits for the players to say they're back. Not a vote — nobody says no and it can't fail, so it needs no `zp_vote`, and it wins over `zp_vote_unpause` where both are set. |
| `zp_ready_percent` | `100` | How much of the room has to be ready. `100` is everybody. |
| `zp_vote` | `0` | Put pauses to a vote. See Voting. |
| `zp_vote_min` | `2` | Minimum yes votes, whatever the player count. |
| `zp_vote_percent` | `51` | Percent of players who must vote yes. |
| `zp_vote_time` | `30` | Seconds a vote stays open. |
| `zp_vote_unpause` | `0` | Resuming needs a vote too. |
| `zp_vote_hold` | `0` | Freeze the game while the vote runs, and resume it if the vote fails. |
| `zp_vote_initiator_yes` | `1` | Whoever called the vote counts as a yes. |
| `zp_vote_lockout` | `10` | Seconds before another vote can be called after one fails. |
| `zp_vote_hud` | `1` | Show the vote tally on screen. |
| `zp_vote_show_voters` | `1` | List each player and how they voted. |
| `zp_vote_hud_position` | `top` | Where the vote tally sits. See Where the HUD sits. |
| `zp_vote_alive_only` | `1` | Leave bled-out spectators out of the threshold and the count. |
| `zp_vote_result_time` | `2` | Seconds the result stands on the tally after a vote resolves. `0` = clear at once. |
| `zp_vote_no_combo` | `jump_melee` | Combo for a no vote: `jump_melee`, `crouch_use`, `crouch_frag`, `crouch_ads`, `crouch_melee`. |
| `zp_ease` | `1` | Ease time down into the pause and back out, instead of cutting to a stop. |
| `zp_ease_time` | `0.35` | Seconds of ramp at each end. |
| `zp_countdown` | `3` | Seconds of 3‑2‑1 before play resumes. |
| `zp_grace` | `2` | Seconds of invulnerability after resuming. |
| `zp_max_pauses` | `0` | How many times one match can be paused. `0` is no cap. Only a pause somebody asked for spends one — an automatic pause does not. |
| `zp_pause_on_disconnect` | `0` | Pause when somebody drops, so whoever is left isn't overrun while they rejoin. Nothing un-pauses on its own, so `zp_max_pause_time` is the way out if they don't come back. |
| `zp_round_pause` | `0` | Hold a pause until the round is over instead of freezing the game mid-horde. Asking again calls it off. |
| `zp_cooldown` | `2` | Minimum seconds between toggles. |
| `zp_max_pause_time` | `0` | Auto-resume after N seconds. `0` = unlimited. |
| `zp_engine_freeze` | `1` | Use `setpauseworld()` and the `world_is_paused` flag. |
| `zp_drift_guard` | `1` | Snap back any AI that still manages to move. |
| `zp_freeze_anims` | `1` | **No effect on this engine** — the world freeze already stops animation. |
| `zp_silence_zombies` | `1` | Stop zombies growling while paused. |
| `zp_godmode` | `1` | Make players invulnerable while paused. |
| `zp_freeze_players` | `1` | Lock players in place while paused. `0` lets them walk around with their weapons down, locked again for the countdown — not recommended, because doors, the box, perks, traps and pickups can all still be used while the zombies are held. |
| `zp_control_guard` | `1` | Re-apply the player freeze every tick, so a map script can't hand controls back mid-pause. |
| `zp_freeze_clock` | `1` | Hold the match timer. |
| `zp_freeze_powerups` | `1` | Stop ground powerups timing out. |
| `zp_freeze_effects` | `1` | Hold insta-kill / double-points countdowns. |
| `zp_freeze_bleedout` | `1` | Stop downed players bleeding out. |
| `zp_blackout` | `1` | Dim everyone's screen while paused, which keeps the pause text readable over a bright skybox. Raise `zp_blackout_alpha` for the anti-scouting blackout this used to be. |
| `zp_blackout_alpha` | `0.2` | How far it dims. `0.2` is a light darkening; `1` is fully black. |
| `zp_blur` | `1` | Blur everyone's screen while paused. Clears when play resumes. |
| `zp_blur_amount` | `2` | Blur strength. `4` is the blur the game runs when you buy a perk. |
| `zp_show_hint` | `1` | Tell players how to pause when they spawn. |
| `zp_hud` | `1` | Draw the pause block at all. The vote HUD is separate and still draws. |
| `zp_hud_position` | `center` | Where the pause banner sits. See Where the HUD sits. |
| `zp_hud_binds` | `1` | Draw combos as each player's bound buttons instead of words. |
| `zp_hud_glow` | `1` | Black glow behind the HUD text, to carry it over a bright skybox. |
| `zp_hud_panel` | `0` | Black slab behind the whole block. Heavier than the glow. |
| `zp_hud_panel_alpha` | `0.45` | How opaque that slab is. `1` is solid black. |
| `zp_hud_panel_width` | `340` | How wide it is, in HUD units. |
| `zp_hud_timer` | `1` | Show who paused and how long it has been. Minutes, not mm:ss — see below. |
| `zp_pause_sound` | `zmb_bgb_killingtime_start` | Played when the game is paused. `none` = silent. |
| `zp_countdown_sound` | `zmb_finalcountdown_timer_marker` | Played on each countdown tick. `none` = silent. |
| `zp_resume_sound` | `zmb_bgb_killingtime_end` | Played when play resumes. `none` = silent. |

### Where the HUD sits

`zp_hud_position` places the pause banner, `zp_vote_hud_position` places the vote tally.
Both take the same six slots:

| Value | Where |
|---|---|
| `top` | Flush with the top edge, centred. |
| `center` | Centred horizontally, high enough to stay clear of the action — the classic pause banner spot. |
| `middle` | The actual centre of the screen, over the crosshair. |
| `bottom` | Above the bottom edge, centred. |
| `left` | Against the left edge, text left-aligned. |
| `right` | Against the right edge, text right-aligned. |

The left and right slots align their text to that edge instead of centring it, so a list
of voters reads as a clean column rather than a ragged stack.

The pause banner defaults to `center`, the vote tally to `top`. The two never share the
screen — the vote tally stands in for the pause banner while a vote is open — so you can
put both in the same slot without them colliding.

### The pause clock is in minutes without an auto-resume

With `zp_max_pause_time` set, the clock counts down to the auto-resume exactly as it does
on Black Ops II. Without one, it reports elapsed time in minutes rather than live mm:ss.

Black Ops III has `settimer()`, which counts down, and nothing that counts up — Black Ops
II's `settenthstimerup()` has no equivalent here. Drawing a live second counter as text
instead would cost a configstring a second until the pool ran dry, which is what took the
Black Ops II build down before it was fixed. Minutes bound the whole set to about sixty
strings, all reused.

---

## How it works

**Treyarch already wrote this pause, and it's in zombies.** The Killing Time GobbleGum
freezes the world for twenty seconds, in
`scripts\zm\bgbs\_zm_bgb_killing_time.gsc`:

```gsc
level flag::set( "world_is_paused" );
setpauseworld( 1 );
```

ZPause uses that rather than inventing one.

`setpauseworld()` is a true world freeze — AI, physics and animation all stop, so zombies
hold the pose they were in mid-stride rather than standing in an idle. That's a better
freeze than any other port gets: the Black Ops II build has to swap in a dormant pose, and
the Black Ops and World at War builds have to cancel scripted animations outright to stop
a zombie finishing a barrier teardown through the pause.

**The flag matters more than the builtin.** A large amount of stock zombies script already
watches `world_is_paused` and stops itself:

| Script | What it does while the flag is set |
|---|---|
| `_zm.gsc` | `round_spawning` blocks — the spawner stops at the source |
| `zm_castle_ee_bossfight` | boss phase timer waits it out |
| `zm_siegebot_nikolai` | three separate waits |
| `zm_stalingrad_dragon` | skips its think, refuses damage |
| `zm_stalingrad_zombie` | two spawn paths wait |
| `_zm_ai_napalm`, `_zm_ai_sentinel_drone`, `_zm_weap_shrink_ray` | early-out or wait |

So a pause here goes deeper than on the other engines — boss fights and scripted
encounters hold themselves, with nothing asked of ZPause.

**Players are exempted from it.** `setpauseworld()` freezes players too, which is why the
campaign calls `setignorepauseworld( 1 )` on anyone it wants to keep moving through a
paused scene. Here the reason is existential: a player who stops being simulated stops
reporting `usebuttonpressed()`, and nobody could ever unpause. So players are exempted from
the world freeze and locked the ordinary way with `freezecontrols()` — the same pairing
Treyarch uses.

**A zombie on its way in keeps its goal.** Behind the world freeze, ZPause pins each
zombie's goal where it stands, as the Black Ops II build does. A zombie walking to a window
is the exception: its walk ends the moment it is at its goal, and what it plays next is
lined up against the window, so a goal at its feet would read as arrival and the resume
would snap it there. It keeps the goal the game gave it, the way Killing Time leaves it.

Everything else is the Black Ops II build:

- **`locktimer()`** holds the match clock. It's byte-for-byte the same function on both
  engines.
- **Ground powerups.** `powerup_timeout()` is a plain `wait()` chain and can't be paused,
  so the thread is cut and restarted on resume.
- **Powerup effects.** Every timed powerup keeps an `_on` flag and a `_time` countdown;
  pinning the countdowns holds the on-screen timers.
- **Bleedout** is pinned so a downed player doesn't bleed out.
- **Late joiners and respawns** are frozen on spawn, and everything is torn down on
  `end_game` so nobody is left frozen at the scoreboard.

### Sharing the world pause with Killing Time

Killing Time uses the same flag and the same builtin. If the world is already paused when
ZPause arrives, ZPause leaves it exactly as it found it and doesn't clear anything on the
way out — so a GobbleGum popped during a pause, or a pause called during a GobbleGum,
can't have one release the other's freeze.

---

## Notes

- **`zp_freeze_anims` does nothing here.** The world freeze already stops animation. The
  setting exists so one config works across every port.
- **Team chat is not listened for.** `say_team` and `chat` are raised the same way as
  `say`, and ZPause binds only `say`, the one Black Ops II binds.
- **The screen blackout is a screen fade**, not a black HUD element. Black Ops III has no
  `precacheshader()`, so a material can't be pulled in from an injected script;
  `lui::screen_fade_out()` takes `"black"` and is called from zombies script already.
- **Not held:** the magic box close timer, teleporter cooldowns, trap durations and Easter
  egg step timers.

---

## Ports

| Game | Repo |
|---|---|
| Black Ops 4 (T8) | [ZPauseT8](https://github.com/Xeptix/ZPauseT8) |
| Black Ops III (T7) | ZPauseT7 — you are here |
| Black Ops II (T6) | [ZPause](https://github.com/Xeptix/ZPause) |
| Black Ops (T5) | [ZPauseT5](https://github.com/Xeptix/ZPauseT5) |
| World at War (T4) | [ZPauseT4](https://github.com/Xeptix/ZPauseT4) |

Versions are kept in step: the same version number means the same feature set, allowing
for what each engine can actually do.

---

## Changelog

### v1.5

- **The chat commands work here, on every route.** `!pause`, `!p`, `!unpause`, `!resume`,
  and `yes` / `no` while a vote is open — the same words as on Black Ops II. This port
  shipped without them because nothing in Black Ops III's own scripts listens for chat, and
  that turned out to prove nothing: the game raises the notify on the player, and BOIII and
  T7x raise it themselves after their own chat handling. `zp_allow_short_words` widens them
  to the bare words, and stops being a setting that does nothing here. The pause banner
  names `!unpause` beside the combo now, and the `while down:` line offers `!yes` / `!no`
  wherever a down combo is set to `none` — in every language the Workshop build ships.

- **A zombie paused on its way in through a window picks up where it was.** Resuming used
  to snap it to the window and straight into tearing the boards, from wherever it had been
  walking. The pause holds zombies with the world freeze and, behind it, pins each one's
  goal to where it stands — and a zombie's walk to the window ends the moment it is at its
  goal, with the board tear that follows lined up against the window. While the world
  freeze holds it, a zombie that isn't through its window yet keeps the goal the game gave
  it, which is how Killing Time holds it.

- **`zp_freeze_players`** — set it to `0` and players can walk around a paused game with
  their weapons down, and are locked again for the countdown back in. Players are still
  locked by default, and roaming isn't recommended: doors, the box, perks, traps and
  pickups can all still be used while the zombies are held.

- **The Workshop build is in every language Black Ops III ships.** Every line the pause
  draws or prints now reaches each player in the language their own game is set to, so a
  lobby can mix them. It was built for English alone before. The loose-script routes stay
  in English, since they have no fastfile to carry translations in.

- **A settings menu for the host.** While the game is paused, hold **fire + melee** to
  change ZPause's settings without the console: aim and fire move through the list,
  grenade changes the setting, melee closes it. See
  [The settings menu](#the-settings-menu); `zp_menu` turns it off.

- **The same settings in the lobby, on the Workshop build, BOIII, Ezz BOIII and T7x.** The
  zombies lobby has a **ZPAUSE SETTINGS** button for the host, with a page for each part of
  ZPause, what each setting does and its default. What you set there is saved, so it is
  still set the next time the game starts. See [In the lobby](#in-the-lobby).

- **Ezz BOIII, and one Black Ops III folder per client.** Ezz BOIII clears its AppData
  folder every time it launches, so ZPause goes in its game folder there. A player who keeps
  a copy of the game for each client -- `Call of Duty Black Ops III EzzBOIII` beside
  `Call of Duty Black Ops III` -- gets each copy offered as a place to install, and
  `install.bat -Find` lists them.

- **`none` empties a setting from the console.** `set zp_pause_sound ""` was put back to
  its default the next time the config was read, so a silent sound or a downed combo left
  on chat alone only ever worked from the installer. `none` does it from the console, a
  config file or the settings menu.

- **Changing `zp_engine_freeze` or `zp_godmode` during a pause** no longer leaves the
  zombies frozen or the players invulnerable once it ends. Resuming undoes what the pause
  did, rather than what the setting says by then.

- **The installer's `-To` works when it already remembers a folder.** It was only read when
  the installer had to ask where the game is, so once it remembered one, `-To` was ignored.
  It comes first now, for that run, and a `-To` that is not the game's folder says so. On
  Linux, the question itself now shows when the installer has to ask.

### v1.4

First release. Feature equal to ZPause v1.4 for Black Ops II, except where the engine
doesn't allow it:

- **No chat commands.** Everything is on the button combos, and `zp_allow_short_words` is
  carried so one config reads the same on every port. Both changed in v1.5.
- **The pause clock is in minutes** unless `zp_max_pause_time` is set. Black Ops III has no
  `settenthstimerup()`, so an open-ended pause counts up in minutes rather than live mm:ss
  — see [The pause clock is in minutes without an auto-resume](#the-pause-clock-is-in-minutes-without-an-auto-resume).
- **`zp_freeze_anims` does nothing here.** The world freeze already stops animation.
- **The screen blackout is a screen fade**, not a black HUD element — Black Ops III has no
  `precacheshader()`, so an injected script cannot pull a material in.
- **The `zp_` dvars do not appear in console completion.** Black Ops III only completes the
  dvars its own engine registered, and no GSC call adds one. `set zp_config_print 1` prints
  every setting and its current value instead.
- **T7x cannot take its settings from the installer.** It runs a compiled script and there
  is nothing in one to rewrite, so the config editor writes a cfg you exec — see
  [Configuring it from the installer](#configuring-it-from-the-installer). The other three
  routes take settings the usual way.

Four ways in, and the installer finds whichever you have: BOIII (and Ezz BOIII), T7x, the
Steam Workshop, and the t7-compiler. Only the Workshop build takes your one mod slot.

## Credits

- **Xep** — author
- **Treyarch** — `_zm_bgb_killing_time.gsc`, the pause recipe this is built on
- **[Serious](https://github.com/shiversoftdev)** — t7-source, and the compiler this
  builds with
- **D3V Team** — L3akMod, which the Workshop build's lobby menu is built with
