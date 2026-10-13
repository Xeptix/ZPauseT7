# ZBundle for t7-compiler

This project uses overlay-safe Z Mod fragments. Copying ZPause, ZShare, ZStats,
and ZTweaks compiler folders into the same project is supported: t7-compiler
concatenates their `scripts/zbundle/*.gsc` files into **one** compiled script,
so every installed component shares one stock injection slot without collision.

Copy this directory into the upstream t7-compiler workspace (or point the
compiler at it), launch Black Ops III Zombies, then compile/inject the project.
The compiler route has no Z Mod settings menu; configure supported dvars by
console/config as documented by each component.

Do not run these as separate compiler projects and inject them successively.
Merge/overlay the folders first, then compile once. Each `components/*.json`
receipt records the exact source and generated hashes for that component.
