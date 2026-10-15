# ZPause T7

**Synced co-op pause for Black Ops III Zombies**

by Xep

[![Support the mod on Ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/F6L1285ROA)
[![Support the mod on Patreon](https://img.shields.io/badge/Patreon-Xeptix-F96854?logo=patreon&logoColor=white)](https://www.patreon.com/Xeptix)
[![Join the Discord](https://img.shields.io/badge/Discord-Join-5865F2?logo=discord&logoColor=white)](https://discord.gg/RE85HRwYZZ)

[**Download the latest release**](https://github.com/Xeptix/ZPauseT7/releases/latest)

A port of [ZPause](https://github.com/Xeptix/ZPause), the Black Ops II pause mod, to
Black Ops III. Same design, same settings, same version numbering - v1.4 here is feature
equal to v1.4 there.

Any player can pause. Any player can unpause. The state lives on `level`, so it's
identical for everyone - there's no per-client state that can desync.

- Zombies stop where they are, mid-stride, and stop spawning
- Boss fights and scripted encounters hold too
- Players are locked and can't be hurt
- The match clock, powerup timers, effect countdowns and bleedout all hold
- Resumes on a 3-2-1 countdown with a short grace period
- Optional vote-to-pause, so a pause has to carry the room

---

## Requirements

Black Ops III, zombies.

How ZPause loads depends on how you play. **The download carries every loose-script route**
- `install.bat` finds which of them you have and installs only to those. It is the same
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

**T7x loads compiled GSC rather than source** - its own scripts ship as compiled `.gsc`
files beside `.gsc_raw` sources, and a raw file in `custom_scripts` does not load. The
download carries a build made for it, so the installer treats T7x like any other route and
you do not need mod tools.

The one thing that route cannot do is take your settings from the installer. The config
editor works by rewriting defaults into the installed script, and a compiled one has no
text to rewrite - so it writes a `zpause.cfg` into T7x's own settings folder instead, and
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
| [T7 Patch](https://github.com/shiversoftdev/t7patch) | security patch - a `d3d11.dll`, game launches normally |
| [Clean Ops](https://github.com/notnightwolf/cleanopsT7/wiki) | the same, plus p2p hosting and an anti-cheat |
| [BO3Enhanced](https://github.com/shiversoftdev/BO3Enhanced) | lets the Windows Store build run on Steam files |

None of the three loads scripts, and none takes the mod slot - ZPause runs alongside any of
them by any route above. **Run T7 Patch or Clean Ops, not both:** Clean Ops carries all of
T7 Patch's security work and says plainly that it conflicts with it. Run one regardless of
ZPause, since unpatched Black Ops III has remote code execution exploits that get used in
public lobbies.

The one pairing to avoid is **the t7-compiler under a patch**. T7 Patch says it "uses a lot
of complicated techniques to patch the game that interfere with most third party tools",
and an external injector is exactly such a tool. Use BOIII, T7x or the Workshop build
instead - none of them injects anything.

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
- **configure ZPause** - every setting, grouped the way the script groups them, each with
  its default and a one-line description of what it does. See below.
- **remove** ZPause again - the loose scripts only; a Workshop subscription is Steam's
- **check GitHub** for a newer release and download it, with a progress bar
- **install a different version** - every download it makes is kept, so going back to an
  older build is the same two keystrokes as going forward. It can list what GitHub has and
  fetch any of those too.
- **put back a file it replaced** - it copies out whatever it is about to overwrite, so an
  install can be undone even over a script you had edited yourself
- **check my setup** - one key that looks for the handful of things that actually go
  wrong: copies at different versions, files something has edited since they were
  installed, settings that never reached the game, settings that cannot reach T7x
- **keep itself** on your PC with a Desktop or Start-menu shortcut, so you never have to
  go looking for the download again

Nothing in that keep-list is ever deleted behind your back. After a download it shows what
it is holding and offers to clear the older ones out - answering no keeps them all. It
also writes a plain-text log of everything it installs or removes.

### Configuring it from the installer

Every setting is a dvar, and the config editor is a way to set them without touching a
console. It reads the settings out of the script itself, so the list is always right for
the version you have, with the description of each one from the table further down.

Saving writes a **`zpause.cfg`** into your Black Ops III folder - plain `set zp_vote "1"`
lines, which is exactly what a dedicated server execs, so it is also the file to send
someone or reuse on another PC - and puts the values into the installed script, so they
take effect with no console step. You can pick either or both. Your settings are re-applied
automatically after an update, so a new version never quietly resets them.

**T7x is the exception**, because it runs a compiled script and there is nothing in one to
rewrite. Saving writes its cfg to T7x's own settings folder and you run
`exec zpause/zpause.cfg` in game - see above. The other three routes need no console step.

Settings that take a fixed set of values offer that list rather than a blank prompt, so a
typo cannot leave you with a combo the game silently ignores. Type a setting's name at any
config screen to jump straight to it. You can keep several **profiles** - a solo one and a
server one, say - and switch between them; each is its own shareable cfg. And when an
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

It asks before it touches the network, every run - answer no and it makes no connection
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
  README.md
  scripts/zbundle/zpause.gsc
  components/zpause.json
```

Both `Black Ops III` and `AppData` merge into folders you already have, so the whole folder
can go across at once. Original BOIII reads both its game-folder and its AppData script
folders, so either one works there.

**Ezz BOIII clears its AppData folder every time it launches**, taking anything it didn't put
there with it - so on Ezz BOIII, use the game folder. A copy in AppData is gone before the
game has started.

### Steam Workshop

**[Subscribe on the Workshop](https://steamcommunity.com/sharedfiles/filedetails/?id=3800204505)**,
then pick ZPause from the Mods menu. Nothing to build and nothing to copy.

**It takes your one mod slot.** Black Ops III only enables one mod at a time, so while
ZPause is selected you can't run another. If that matters, use one of the loose-script
routes above - they take no slot and coexist with any mod you like. To run it with Xep's
other Zombies mods, subscribe to
**[ZBundle](https://steamcommunity.com/sharedfiles/filedetails/?id=3802815997)**, which is
ZPause, [ZShare](https://steamcommunity.com/sharedfiles/filedetails/?id=3802815009),
[ZStats](https://steamcommunity.com/sharedfiles/filedetails/?id=3814909842) and
[ZTweaks](https://steamcommunity.com/sharedfiles/filedetails/?id=3814908789) in one mod. They
are all in
[Xep's Black Ops III Zombies collection](https://steamcommunity.com/workshop/filedetails/?id=3802560637).

**In your language.** The Workshop build draws every line in the language your game is set
to - English, French, Italian, Spanish, German, Brazilian Portuguese, Russian, Polish,
Japanese, and Traditional and Simplified Chinese - and in a mixed lobby each player reads
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
safe.

**Xep's other Zombies mods share this one project.** ZShare, ZStats and ZTweaks ship their
compiler folders in the same shape, each with its own file under `scripts/zbundle/` and the
same `gsc.conf`. Copy every mod's `t7-compiler` folder over one project and build it once:
the compiler joins them into one script and injects it over the one stock script, so they
all run together. Don't build them as separate projects and inject one after another - the
second replaces the first.

This is the one route not to use under a security patch - see above.

### Why the Workshop build wears a stock script's name

A mod's GSC doesn't run just for being in the fastfile; something already loaded has to
reach it. So the Workshop build stands in for `scripts/zm/gametypes/_clientids.gsc`, which
is the one worth taking: 72 lines, nothing in the game calls `clientids::` from outside it,
and `.clientid` is read only by campaign script. Everything it does is carried across
unchanged, so overriding it costs nothing.

The loose-script routes need none of that - BOIII, T7x and the compiler all load a script
directly, so there it keeps its own namespace and stands in for nothing.

You don't need to restart the game to load a new script - end the current match and start a
new one.

---

## Usage

| Action | Input |
|---|---|
| Pause / unpause | hold **crouch + melee** together for ~0.3s |
| Vote yes, while a vote is open | the same combo |
| Vote no, while a vote is open | hold **jump + melee** |

Crouching *or* prone counts - `stancebuttonpressed()` covers both.

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

### Pausing just yourself

Set **`zp_personal_pause 1`** and the pause input pauses only you. You're frozen, you can't
be hurt, and the zombies ignore you while everybody else plays on. The same input brings
you back, with the countdown the whole game would get.

The whole game pauses by itself once **nobody is left playing** - everybody else paused
as well, or everybody still in it down. A team that goes down around somebody who stepped
away is held for them rather than lost, bleedout and all. It comes back as soon as one of
the players who paused does; anybody else pressing resume is told it's waiting for them.

- **Not from the floor.** You can't pause yourself while you're down, or it would hold your
  bleedout while the team fought on.
- **Always frozen in place**, even with `zp_freeze_players 0` - walking around a running
  game where nothing can touch you would be a way through a round, not a pause.
- **It's yours alone,** so it never goes to a vote, the host's approval or the ready check,
  and `zp_host_only` doesn't stop it.
- **The limits still apply.** Each personal pause uses one of the match's `zp_max_pauses`,
  and `zp_max_pause_time` brings you back when it runs out.

On the Workshop build your banner and the messages around it are in your own language, the
same as the rest of the mod. Off by default.

### The game's own pause menu

Set **`zp_pause_menu own`** and opening Black Ops III's in-game menu pauses the game; closing
it resumes. Nothing to learn and nothing to press - pausing is what that menu looks like it
should do.

Opening it is the pause input and closing it is the resume input, so everything else here
still decides: `zp_host_only`, a vote, the host's approval, the ready check, the cooldown
and `zp_max_pauses`. With **`zp_personal_pause`** on as well, opening the menu takes you out
of the game on your own and closing it brings you back - which is the combination worth
having, since everybody else keeps playing while you're in there.

Closing the menu only resumes a pause your own menu started. A pause somebody else called
is not yours to end by walking out of a menu, and if you leave while it's open the game is
handed back for you.

- **Every route but the t7-compiler.** The menu is heard through Lua, which the Workshop
  build carries in its `core_mod` fastfile and BOIII, Ezz BOIII and T7x load from
  `ui_scripts` - the same files the lobby settings menu comes in, installed with the script
  beside them. An injected script reaches a stock client that loads no Lua at all, so on
  that route the combo and the chat commands are the pause input and this setting does
  nothing.
- **Each player's own game reports their own menu**, so a player without ZPause opening
  theirs pauses nothing.
- **Solo is left alone.** Black Ops III already pauses a solo game by itself when that menu
  opens, so ZPause stays out of it and you get no countdown on the way back.

#### The whole room's menus - `zp_pause_menu room`

With `own`, closing your menu only resumes a pause your own menu started, which is why
somebody else's open menu does nothing while the game is already held. **`zp_pause_menu room`**
makes the game follow the room instead: it is paused while *anybody's* pause menu is open,
and it comes back the moment the last one closes. The vanilla pause, working in a co-op game
the way it already works in solo.

For the game to mirror what the clients are doing, everything that can refuse or defer a
press has to stand down, and with `room` it does - no vote, no cooldown, no `zp_max_pauses`, no
`zp_round_pause`, and no countdown on the way back. `zp_personal_pause` stands down too,
since the whole point is that one player's menu pauses for everybody. **`zp_host_only` still
applies**: with it on, only the host's menu moves the game and anybody else opens theirs
without the room noticing.

The pause HUD names everyone who is in a menu rather than whoever got there first, and the
line changes as people come and go.

- **Leaving with your menu open** takes you off the list, and the game comes back if you
  were the last one on it.
- **A close that never arrives cannot strand the game.** An open menu says so twice a second,
  and one that has gone quiet for a few seconds is dropped from the list.
- **Solo is still left alone**, for the same reason as `own`: Black Ops III pauses itself when that
  menu opens, so ZPause stays out of it.
- **Every route but the t7-compiler**, the same as `own`. A player on a route with no Lua
  opening their menu is not on the list.

Off by default.

### The settings menu

While the game is paused, the host can change ZPause's settings without the console. Hold
**fire + melee** to open the menu:

| Button | Does |
|---|---|
| aim / fire | move up and down the list |
| grenade | change the setting |
| melee | close |

A switch flips, a list moves on to its next choice, and a number steps up through a few
common values and back round to the lowest - exact values are still the console's. A
change lands when play resumes, the same as one typed into the console, and lasts until the
game closes. The menu closes itself when a vote opens, since the host needs the buttons
back to vote. `zp_menu 0` turns it off.

On the Workshop build the menu speaks your language too; the setting names and their
values stay as the console spells them.

#### In the lobby, and from the game's own pause menu

The same settings have a page of their own, on the Workshop build and on BOIII, Ezz BOIII
and T7x. On the Workshop build, load ZPause from the Mods menu; on the other three the
installer puts the menu in the game folder's `ui_scripts`, beside the script.

There are two ways in, and both open the same page. ZPause's own settings are the host's, so
on anybody else's screen they are locked; a player who is not the host uses the page for
whatever is theirs on it, such as another Z mod's personal settings.

- the zombies lobby has a **ZPAUSE SETTINGS** button, above the gum and build kit buttons;
- the game's own pause menu has a **ZPAUSE SETTINGS** row, under RESUME GAME, during a
  match.

The pause menu also has a **PAUSE GAME** row - **UNPAUSE GAME** while the game is paused -
above that one, for every player. It presses pause exactly as the combo does, so a vote, the
host's approval, the cooldown and `zp_max_pauses` all still have their say, and
`zp_host_only` takes it off the menu of anybody it would turn away. Everybody in a Workshop
match has it, since everybody loads the mod; on BOIII, Ezz BOIII and T7x a player has it when
ZPause's menu is in their own game folder. With `zp_pause_menu` on it is not there at all:
opening the menu has already pressed pause, so the menu is the button.

Solo is a special case, and the row handles it. Black Ops III stops the game by itself
while the pause menu is open, and a ZPause pause underneath that would be one nothing could
lift - the script is not running to be asked. So the press leaves the game's own pause
first: the menu closes, the game runs, and what lands a moment later is ZPause's pause, with
the countdown, the HUD and the resume. **UNPAUSE GAME** does the same in reverse - the
menu closes and the game carries on. In co-op the menu pauses nothing, so nothing closes.

A category for each part of ZPause across the top - **INPUT**, **VOTING**, **TIMING**,
**FREEZING** and **PRESENTATION**. The bumpers move between them, or **Q** and **E**, or the
arrows either side of the strip, or a click on the category itself; the names either side and
`CATEGORY n / total` under the strip say where you are and what is next. The settings are
down the left, seven at a time with the rest scrolled, and the panel on the right says what
the one under the cursor does, what it is called in the console, what its default is, what a
change to it waits for and who it is for. Left and right change a setting. **DEFAULT** leaves
it to the script's own default. Opening it from the pause menu does not close the pause menu,
so it is not a resume with `zp_pause_menu` on.

**Y**, or **F** on a keyboard, takes everything still at its default out of the category
you are on, so a long one shows only what has been changed; press it again, or change
category, to put them back.

Along the bottom: **BACK**, **RESET TAB**, **RANDOMIZE** and **RESET ALL**. The last three
ask before they do anything, with **NO** already under the cursor, and RANDOMIZE says so
before it runs - DEFAULT is one of the values it can pick. RESET TAB is the category you are
on; RESET ALL is every ZPause setting there is.

A change lands on the next pause, whichever way in you used - the script re-reads its
settings every time a pause is asked for. Two are the exception and the panel says which:
`zp_show_hint` is drawn as a player spawns and `zp_round_pause` is read as a round ends. It
is saved as well, so it is still set the next time the game starts. On the Workshop build
that goes into the mod's own save data; on BOIII, Ezz BOIII and T7x, which have no mod to
keep it in, into the names of your offline zombies custom classes, which nothing in a zombies
game shows or uses. The t7-compiler route has neither way in: it injects into a game that
loads no menus of its own.

The page is the one Xep's other Black Ops III mods draw, so a bundle of them puts every mod's
settings on one screen with a category each and the title **ZBUNDLE SETTINGS**, which names
the mod owning the category you are on. Each mod keeps its own settings in its own place;
nothing here reads or writes another's. A bundle also gets a **MODS** category, first, which
switches each of those mods on and off - ZPause's row there is `zp_enabled`, and it lands on
the next match. Last, in every build, is **CHANGED**, which lists everything moved off its
default in one place, whichever mod owns it.

The Workshop build draws the page over a background image of its own. BOIII, Ezz BOIII and
T7x draw the same page over a half-transparent black panel - an image has to come out of a
mod's fastfile, and a loose script has none.

### Languages

The lobby page and the **ZPAUSE SETTINGS** row in the game's own pause menu follow the language
the game is set to: French, German, Italian, Spanish, Russian, Polish, Brazilian Portuguese,
Japanese, Korean, Czech, and Simplified and Traditional Chinese, with English for any other. That
holds on every route with a menu: the Workshop build, BOIII and T7x. The Workshop build also
gives the pause banner, the HUD and the chat messages in the same languages. Dvar names and the
values a console or the chat takes stay as they are in every language.

**The translations are made by AI and may contain mistakes.** If you find one, please report
it to Xep on the [Discord](https://discord.gg/RE85HRwYZZ), with the language and the line as it
reads in the game.

---

## Voting

Off by default. `zp_vote 1` and a pause has to carry the room instead of any one player
stopping the game.

Calling a vote is the same action as pausing. The vote runs 30 seconds and everyone gets a
tally: the count, the clock, and every player with how they voted.

The bar is whichever is higher, `zp_vote_min` or `zp_vote_percent` of the players in the
game, then clamped to how many are actually present - so a lobby can't set a threshold
nobody there can clear, and solo play skips the vote entirely. With the defaults that's
2 of 2, 2 of 3, 3 of 4.

A vote ends the moment it's decided either way. Disconnects take their vote with them. A
failed vote locks out the next one briefly so it can't be spammed. Resuming doesn't need a
vote by default, so one AFK player can't strand everyone in a paused game.

---

### Who decides

Five settings answer the same question - who may pause, and who has to agree. They can all
be on at once, so this is the order the script applies them in.

**Asking to pause:**

| | Setting | What happens |
|---|---|---|
| 1 | `zp_host_only` | Anybody but the host is turned away here. Nothing below runs for them. |
| 2 | - | Refused while the game is still starting. |
| 3 | `zp_round_pause` | If a pause is already waiting for the round to end, asking again calls it off. |
| 4 | `zp_max_pauses` | Refused once the match has spent its budget. |
| 5 | `zp_cooldown` | Refused if the last pause was too recent. |
| 6 | `zp_host_approve` | A non-host's ask goes to the host to answer. **Takes precedence over `zp_vote`.** |
| 7 | `zp_vote` | Otherwise, with voting on, it goes to a vote. |
| 8 | `zp_round_pause` | Once it is agreed - outright, approved or voted - it waits for the round to end instead of happening now. |

**Asking to resume:**

| | Setting | What happens |
|---|---|---|
| 1 | `zp_host_only` | Anybody but the host is turned away. |
| 2 | - | With a vote already open, the input is a yes instead. |
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
whenever a pause is requested, so a change takes effect **almost straight away** - no map
restart needed.

### Where settings are saved

Three routes in, and they keep a setting for different lengths of time:

| Set it here | How long it lasts |
|---|---|
| The installer's config editor | Permanently - it rewrites the default in the installed script. T7x runs a compiled script instead, so it gets a cfg you `exec`. |
| The ZPause settings page, from the lobby or from the pause menu | Permanently - it saves into the mod's own storage on the Workshop build, or your offline zombies class names on BOIII, Ezz BOIII and T7x, and puts it back once per game start. |
| The fire + melee menu, or the console | Until the game closes. |

Black Ops III is the one port with no settings file behind all this: its file functions are
all developer-only, so a released script cannot read one. That is why the settings page
saves in the game's own storage instead, and why the fire + melee menu cannot save for you
the way the Black Ops II, Black Ops and World at War builds do.

The periodic re-read is skipped while the game is paused: the HUD is built from these
settings when the pause starts and nothing rebuilds it in place, so moving them underneath
would leave elements where the old values put them. A change made mid-pause lands the
moment play resumes. It also means `zp_combo` can be changed by hand - before, that needed
a pause to take effect, and the combo is what asks for one.

`set zp_config_print 1` in the console prints every setting below with the value it is
currently holding, then puts the switch back so it can be used again. On Black Ops III it
needs a client running developer mode, because `println` is a dev-only call there and the
game refuses to load a script that makes one outside a devblock - which is the same mode
that gets you the console it prints to.

Black Ops III also only offers console completions for the dvars its own engine
registered, so the `zp_` names never appear in its suggestions. They still read and set by
their full name.

| Dvar | Default | What it does |
|---|---|---|
| `zp_enabled` | `1` | ZPause itself. Off, the script loads and does nothing at all - no pause, no HUD, no chat commands, no combos, nothing precached. **Read when the match loads**, so end the game and start a new one for a change to take. |
| `zp_menu` | `1` | Let the host change settings from a menu while the game is paused: hold fire and melee to open it. |
| `zp_host_only` | `0` | Only the host can pause or resume. Everyone else's chat command and combo are ignored, and a pause never goes to a vote. On a dedicated server there is no host, so it falls to whoever holds the first player slot. |
| `zp_personal_pause` | `0` | The pause input pauses only you, and the game carries on for everyone else; it pauses in full once nobody is left playing. See [Pausing just yourself](#pausing-just-yourself). |
| `zp_pause_menu` | `off` | Opening the game's own pause menu pauses, and closing it resumes. `own` follows your own menu, `room` the whole room's, `off` neither. **Every route but the t7-compiler.** See [The game's own pause menu](#the-games-own-pause-menu). |
| `zp_only_script` | `0` | Debug. With both a loose script and the Workshop mod installed, run only the loose one. |
| `zp_only_mod` | `0` | Debug. The same, the other way round. Both off - the default - is whichever loads first. Both on leaves nothing running. **Read when the script loads**, so end the game and start a new one for a change to take. |
| `zp_allow_short_words` | `0` | Also accept bare `p` / `u` / `pause` in chat. Off by default so normal conversation can't pause the game. |
| `zp_button_combo` | `1` | Enable the button combos. |
| `zp_combo` | `crouch_melee` | Which combo pauses: `crouch_melee`, `crouch_use`, `crouch_frag`, `crouch_ads`, `jump_melee`, `use_frag`, `frag_only`, `use_ads`, `use_attack`, `attack_ads`. |
| `zp_button_hold_time` | `0.3` | How long the combo must be held. |
| `zp_button_combo_dead` | `use_ads` | Combo used while downed or spectating, when stance and melee stop registering. Also takes `use_attack`, `attack_ads`, `use_frag`, `frag_only`. `none` = chat only. |
| `zp_vote_no_combo_dead` | `use_attack` | The same, for a no vote. |
| `zp_input_debug` | `0` | Print each player which buttons the server receives from them, for picking the two above. |
| `zp_host_approve` | `0` | The host pauses at once; anyone else has to ask and the host answers yes or no. It runs as a vote only the host can cast, so the yes/no input, the HUD and the timeout are a vote's. Pausing only - resuming still follows `zp_vote`. `zp_host_only` wins where both are set. |
| `zp_ready_check` | `0` | Resuming waits for the players to say they're back. Not a vote - nobody says no and it can't fail, so it needs no `zp_vote`, and it wins over `zp_vote_unpause` where both are set. |
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
| `zp_countdown` | `3` | Seconds of 3-2-1 before play resumes. |
| `zp_grace` | `2` | Seconds of invulnerability after resuming. |
| `zp_max_pauses` | `0` | How many times one match can be paused. `0` is no cap. Only a pause somebody asked for spends one - an automatic pause does not. |
| `zp_pause_on_disconnect` | `0` | Pause when somebody drops, so whoever is left isn't overrun while they rejoin. Nothing un-pauses on its own, so `zp_max_pause_time` is the way out if they don't come back. |
| `zp_round_pause` | `0` | Hold a pause until the round is over instead of freezing the game mid-horde. Asking again calls it off. |
| `zp_cooldown` | `2` | Minimum seconds between toggles. |
| `zp_max_pause_time` | `0` | Auto-resume after N seconds. `0` = unlimited. |
| `zp_engine_freeze` | `1` | Use `setpauseworld()` and the `world_is_paused` flag. |
| `zp_drift_guard` | `1` | Snap back any AI that still manages to move. |
| `zp_freeze_anims` | `1` | **No effect on this engine** - the world freeze already stops animation. |
| `zp_silence_zombies` | `1` | Stop zombies growling while paused. |
| `zp_godmode` | `1` | Make players invulnerable while paused. |
| `zp_freeze_players` | `1` | Lock players in place while paused. `0` lets them walk around with their weapons down, locked again for the countdown - not recommended, because doors, the box, perks, traps and pickups can all still be used while the zombies are held. |
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
| `zp_hud_timer` | `1` | Show who paused and how long it has been. Minutes, not mm:ss - see below. |
| `zp_pause_sound` | `zmb_bgb_killingtime_start` | Played when the game is paused. `none` = silent. |
| `zp_countdown_sound` | `zmb_finalcountdown_timer_marker` | Played on each countdown tick. `none` = silent. |
| `zp_resume_sound` | `zmb_bgb_killingtime_end` | Played when play resumes. `none` = silent. |

### Where the HUD sits

`zp_hud_position` places the pause banner, `zp_vote_hud_position` places the vote tally.
Both take the same six slots:

| Value | Where |
|---|---|
| `top` | Flush with the top edge, centred. |
| `center` | Centred horizontally, high enough to stay clear of the action - the classic pause banner spot. |
| `middle` | The actual centre of the screen, over the crosshair. |
| `bottom` | Above the bottom edge, centred. |
| `left` | Against the left edge, text left-aligned. |
| `right` | Against the right edge, text right-aligned. |

The left and right slots align their text to that edge instead of centring it, so a list
of voters reads as a clean column rather than a ragged stack.

The pause banner defaults to `center`, the vote tally to `top`. The two never share the
screen - the vote tally stands in for the pause banner while a vote is open - so you can
put both in the same slot without them colliding.

### The pause clock is in minutes without an auto-resume

With `zp_max_pause_time` set, the clock counts down to the auto-resume exactly as it does
on Black Ops II. Without one, it reports elapsed time in minutes rather than live mm:ss.

Black Ops III has `settimer()`, which counts down, and nothing that counts up - Black Ops
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

`setpauseworld()` is a true world freeze - AI, physics and animation all stop, so zombies
hold the pose they were in mid-stride rather than standing in an idle. That's a better
freeze than any other port gets: the Black Ops II build has to swap in a dormant pose, and
the Black Ops and World at War builds have to cancel scripted animations outright to stop
a zombie finishing a barrier teardown through the pause.

**The flag matters more than the builtin.** A large amount of stock zombies script already
watches `world_is_paused` and stops itself:

| Script | What it does while the flag is set |
|---|---|
| `_zm.gsc` | `round_spawning` blocks - the spawner stops at the source |
| `zm_castle_ee_bossfight` | boss phase timer waits it out |
| `zm_siegebot_nikolai` | three separate waits |
| `zm_stalingrad_dragon` | skips its think, refuses damage |
| `zm_stalingrad_zombie` | two spawn paths wait |
| `_zm_ai_napalm`, `_zm_ai_sentinel_drone`, `_zm_weap_shrink_ray` | early-out or wait |

So a pause here goes deeper than on the other engines - boss fights and scripted
encounters hold themselves, with nothing asked of ZPause.

**Players are exempted from it.** `setpauseworld()` freezes players too, which is why the
campaign calls `setignorepauseworld( 1 )` on anyone it wants to keep moving through a
paused scene. Here the reason is existential: a player who stops being simulated stops
reporting `usebuttonpressed()`, and nobody could ever unpause. So players are exempted from
the world freeze and locked the ordinary way with `freezecontrols()` - the same pairing
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
way out - so a GobbleGum popped during a pause, or a pause called during a GobbleGum,
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
| Black Ops III (T7) | ZPauseT7 - you are here |
| Black Ops II (T6) | [ZPause](https://github.com/Xeptix/ZPause) |
| Black Ops (T5) | [ZPauseT5](https://github.com/Xeptix/ZPauseT5) |
| World at War (T4) | [ZPauseT4](https://github.com/Xeptix/ZPauseT4) |

Versions are kept in step: the same version number means the same feature set, allowing
for what each engine can actually do.

---

## Changelog

### v1.8

- **The category strip stays where it was when a setting changes.** On Black Ops III, changing a
  setting on a later category kept that category selected but slid it to the other end of the
  strip; it now stays in the cell it was in.

### v1.7

- **The t7-compiler build runs alongside Xep's other mods.** Its folder now holds
  `scripts/zbundle/zpause.gsc` and the same `gsc.conf` ZShare, ZStats and ZTweaks ship, so
  copying their compiler folders over one project and building it once runs them all
  together over the one stock script.

- **PAUSE GAME is every player's.** The row in the game's own pause menu was the host's
  alone; every player has it now, and a press from anybody goes through exactly what the combo
  does - `zp_host_only`, the vote, the host's approval and a personal pause all still decide.
  With `zp_host_only` on, the row is not on the menu of a player it would turn away. An action
  another Z mod adds for each player's own game, such as ZStats' HUD toggle, is on every
  player's menu as well; one that acts on the whole match stays the host's.

- **The settings menu speaks twelve languages.** The lobby page and the **ZPAUSE SETTINGS** row
  in the game's own pause menu follow the language the game is set to: French, German, Italian, Spanish, Russian, Polish, Brazilian Portuguese, Japanese, Korean, Czech, and Simplified and Traditional Chinese,
  with English for any other. Every setting's name, its choices, its description and the reason
  a greyed-out row gives are translated, and so is the page around them - the tabs, the
  buttons, the confirmations and the details panel. What you type stays as it is: dvar names,
  and the values a console or the chat takes. The translations are made by AI and may contain
  mistakes; please report any to Xep on the Discord. Every route with a menu: the Workshop
  build, BOIII and T7x.

- **In a bundle, every mod's settings read in that mod's own words.** ZBundle draws all of its
  mods' categories on one screen, and each row is now translated by the mod it belongs to, so a
  ZShare setting reads in ZShare's translation on ZPause's screen. **CHANGED** reads in the
  same language too, with each row's mod still named in front of it.

- **Holding a shoulder button or Q/E moves through the categories cleanly.** The press moves
  one category, and holding it keeps going after a moment, in the lobby and in a match. It used
  to share each step with the game's own tab strip, which made a held key skip a category or
  stall.

- **A choice that happens to be the default stays picked.** Stepping onto a value that is also
  the setting's default - another mod's RANDOM, say - no longer snaps back to DEFAULT the moment
  you land on it. Reopening the screen or resetting shows DEFAULT again.

- **The screen opens on its own title**, the first category's, and a category's description
  fits on one line.

- **Action rows only act when you press them.** A, Enter or a click acts on the row the cursor
  is on, and never on a row you had already moved away from.

- **The pause names who paused.** With `zp_pause_menu room`, the pause screen named "someone"
  instead of the players whose menus were open; it names them now.

- **Two copies of ZPause loaded at once no longer split a player between them.** With the
  Workshop build and a script route both installed, the copy that started the match now looks
  after every player, rather than whichever reached each player first.

- **A pause asked for from the pause menu is asked for once.** The host's PAUSE GAME row
  reaches the game two ways, and both used to be served: with `zp_round_pause` on, the second
  called off the round-end pause the first had just set up, and with `zp_personal_pause` on it
  brought the host straight back. With `zp_host_only` and `zp_personal_pause` both on, a
  player's PAUSE GAME row now shows exactly when their combo would pause them.

### v1.6

- **`zp_enabled` - the mod's own switch, and two new categories with it.** On by default.
  Off, ZPause loads and installs nothing at all: no pause, no HUD, no chat commands, no
  combos, nothing precached. It is read as a match loads, so a change lands on the next one
  rather than the one being played. The settings screen gained the two categories that go
  with it: **MODS**, which switches each mod on the screen on and off and is only there when
  there is more than one of them, and **CHANGED**, which lists everything anybody has moved
  off its default. The game's own pause menu gained a **PAUSE GAME** row for the host -
  **UNPAUSE GAME** while the game is paused - and the title above each category now names
  the mod that owns it.

- **Xep's other mods can ask ZPause to pause.** ZPause puts itself on `level.zmods` as the
  match starts, with its version and the few things another mod may ask it to do: reload the
  settings, pause, resume, toggle, and say whether the game is paused. A pause asked for that
  way goes in at the same door a chat command or a combo does, so voting, the host's
  approval, the cooldown and `zp_max_pauses` all still have their say.

- **The settings page is now the one every Z mod draws.** Same title, same category strip,
  same seven-row list, same details panel, same footer - so a bundle of Xep's Black Ops III
  mods puts all of their settings on one screen, a category each, under **ZBUNDLE SETTINGS**,
  and each mod still keeps its own settings in its own place. On the way it gained the names
  of the categories either side and `CATEGORY n / total`, **Q** and **E** as well as the
  bumpers, **RESET TAB** and **RANDOMIZE** beside **RESET ALL** - each asking first, with
  **NO** under the cursor - and a panel that now also says what a change waits for and who
  the setting is for. The categories no longer split in two when a section runs long, since
  the list scrolls. See
  [In the lobby, and from the game's own pause menu](#in-the-lobby-and-from-the-games-own-pause-menu).

- **ZPause's settings from the game's own pause menu, and a page rebuilt around it.** The
  pause menu has a **ZPAUSE SETTINGS** row for the host, under RESUME GAME, and it opens the
  same page the lobby button does: a tab for each part of ZPause, the settings down the left,
  and a panel on the right saying what the one under the cursor does, what its default is and
  what it is called in the console. A change lands on the next pause, so it takes effect in
  the match you are in. Opening it does not close the pause menu, so it is not a resume with
  `zp_pause_menu` on. The Workshop build draws the page over a background image of its own;
  BOIII, Ezz BOIII and T7x draw it over a half-transparent black panel, since an image has to
  come out of a mod's fastfile and a loose script has none. The fire + melee menu is
  unchanged. See
  [In the lobby, and from the game's own pause menu](#in-the-lobby-and-from-the-games-own-pause-menu).

- **The pause-menu hook reaches BOIII, Ezz BOIII and T7x at last.** `zp_pause_menu` needs a
  Lua file that every build wrote and no download carried, so the menu asked for a file that
  was not there. It ships now, and the build fails rather than leaving it out.

- **`zp_pause_menu` - the game's own pause menu pauses the game.** Off by default, and on
  every route but the t7-compiler's, since the menu is heard through Lua and an injected
  script reaches a client that loads none. Opening it is the pause input and closing it is
  the resume input, so votes, approval and the rest still apply; with `zp_personal_pause`
  on it takes you out on your own. See
  [The game's own pause menu](#the-games-own-pause-menu).

- **`zp_personal_pause` - pause just yourself.** Off by default. With it on, the pause
  input takes only you out of the game: frozen, protected and ignored by the zombies while
  everybody else plays on. The whole game pauses once nobody is left playing, and comes back
  with the first player who paused. See [Pausing just yourself](#pausing-just-yourself).

- **The Workshop build no longer breaks the game when it is switched off.** It stands in
  for one of Black Ops III's own scripts, the one that gives every player their client id,
  and `zp_only_script 1` used to stop that script's work along with ZPause's. The game's
  own code reads those ids while it is scoring a kill. The stock half now always runs,
  whichever copy of ZPause is switched on.

- **The ready check can no longer strand a paused game.** With `zp_ready_check` on, the
  tally was only ever recounted when somebody pressed ready, so if the last player who had
  not pressed dropped out, nobody left could end the pause. A player leaving now recounts.

- **A pause that has already ended is not ended twice.** With `zp_max_pause_time` set, the
  automatic resume could fire while a resume vote was open; the vote then passed into a
  running game and played the whole resume again. The resume now leaves a running game
  alone, and closes an open vote on its way out.

- **A vote nobody carried no longer costs a pause**, with `zp_vote_hold` on.

- **The pause banner no longer sits on top of the vote**, with `zp_vote_hold` on - it was
  telling everybody to press the combo that was a yes vote.

- **`zp_round_pause` is respected by a vote.** With `zp_vote_hold` on as well, a passing
  vote froze the game where it stood instead of waiting for the round to end.

- **A pause waiting for the round to end can only be called off by whoever asked for it,
  or the host.**

- **Two-letter words no longer vote.** Chat is also read with its first character dropped,
  because BOIII and T7x put one in front; that turned "my" and "by" into a yes and "on",
  "in", "an" and "un" into a no while a vote was open. The character is only dropped now
  when it is not a letter or a digit.

- **The pause clock no longer leaves a thread behind for every vote.** It waited for a
  signal the script never sent, so each vote during a pause started another one.

- **The lobby menu keeps a change it could not save.** If writing to the save file failed,
  the change was dropped rather than written on the next try.

- **The lobby menu offers every value again.** A setting's own default was left out of its
  list, so once the installer's config editor had changed a default, the value it used to
  be could not be picked there any more.

### v1.5

- **The chat commands work here, on every route.** `!pause`, `!p`, `!unpause`, `!resume`,
  and `yes` / `no` while a vote is open - the same words as on Black Ops II. This port
  shipped without them because nothing in Black Ops III's own scripts listens for chat, and
  that turned out to prove nothing: the game raises the notify on the player, and BOIII and
  T7x raise it themselves after their own chat handling. `zp_allow_short_words` widens them
  to the bare words, and stops being a setting that does nothing here. The pause banner
  names `!unpause` beside the combo now, and the `while down:` line offers `!yes` / `!no`
  wherever a down combo is set to `none` - in every language the Workshop build ships.

- **A zombie paused on its way in through a window picks up where it was.** Resuming used
  to snap it to the window and straight into tearing the boards, from wherever it had been
  walking. The pause holds zombies with the world freeze and, behind it, pins each one's
  goal to where it stands - and a zombie's walk to the window ends the moment it is at its
  goal, with the board tear that follows lined up against the window. While the world
  freeze holds it, a zombie that isn't through its window yet keeps the goal the game gave
  it, which is how Killing Time holds it.

- **`zp_freeze_players`** - set it to `0` and players can walk around a paused game with
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
  still set the next time the game starts. See
  [In the lobby, and from the game's own pause menu](#in-the-lobby-and-from-the-games-own-pause-menu).

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
  - see [The pause clock is in minutes without an auto-resume](#the-pause-clock-is-in-minutes-without-an-auto-resume).
- **`zp_freeze_anims` does nothing here.** The world freeze already stops animation.
- **The screen blackout is a screen fade**, not a black HUD element - Black Ops III has no
  `precacheshader()`, so an injected script cannot pull a material in.
- **The `zp_` dvars do not appear in console completion.** Black Ops III only completes the
  dvars its own engine registered, and no GSC call adds one. `set zp_config_print 1` prints
  every setting and its current value instead.
- **T7x cannot take its settings from the installer.** It runs a compiled script and there
  is nothing in one to rewrite, so the config editor writes a cfg you exec - see
  [Configuring it from the installer](#configuring-it-from-the-installer). The other three
  routes take settings the usual way.

Four ways in, and the installer finds whichever you have: BOIII (and Ezz BOIII), T7x, the
Steam Workshop, and the t7-compiler. Only the Workshop build takes your one mod slot.

## Credits

- **Xep** - author
- **Treyarch** - `_zm_bgb_killing_time.gsc`, the pause recipe this is built on
- **[Serious](https://github.com/shiversoftdev)** - t7-source, and the compiler this
  builds with
- **D3V Team** - L3akMod, which the Workshop build's lobby menu is built with
