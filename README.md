# GensPlugin

A plot-based Gens core for Paper/Spigot with 120 generator levels, no cap on
generators per plot, and a custom unlimited-storage "Vacuum Collector" item.

## What's included

- **120 generator levels**, formula-driven (not hand-typed) — cost, sell
  price, and speed all scale exponentially with level. Tune the whole
  economy from `config.yml` (`economy` section) without editing 120 lines.
- **Plot system** — built-in, no PlotSquared/WorldEdit needed. Each player
  gets a square plot in a grid; auto-assigned on first join.
- **No generator limit** — `economy.generator-limit-per-plot: -1` by
  default. Set it to a number if you ever want a cap.
- **Vacuum Collector** — a custom hopper item. Keep it in your inventory
  while you're online and it silently collects every drop from every
  generator on your plot into its own unlimited storage (no item-count cap,
  no despawning). Right-click it to view contents and sell everything at
  once.
- **Vault support** (optional) — hooks into Vault + any economy plugin
  (EssentialsX, etc.) if present; otherwise falls back to a built-in simple
  economy so it works standalone.

## Building the jar

You have two options — pick whichever's easier for you.

### Option A: GitHub Actions (no Java/Maven install needed)

This project already includes `.github/workflows/build.yml`, which builds
the jar automatically in the cloud every time you push code. You never run
`mvn` yourself; GitHub's servers do it and hand you a downloadable jar.
This is safe — it's the exact same `mvn clean package` command running in
an isolated, disposable GitHub-hosted machine, and it never touches your
own computer or credentials beyond your GitHub login.

1. Go to [github.com/new](https://github.com/new) and create a new
   repository (public or private, doesn't matter) — e.g. `gens-plugin`.
   Don't add a README/gitignore/license on GitHub's side, since this
   project already has them.
2. On the new repo's page, click **"uploading an existing file"** and drag
   in everything from the unzipped project folder (or use `git` from the
   command line if you're comfortable with it — see below).
3. Commit the files to the `main` branch.
4. Click the **Actions** tab at the top of the repo. You should see a
   "Build Plugin" run start automatically (or click **Run workflow** if it
   doesn't).
5. Once it finishes (green checkmark, usually under a minute), click into
   that run, scroll to **Artifacts**, and download **GensPlugin** — that's
   a zip containing your `GensPlugin.jar`.
6. Upload that jar to Minehut's Plugins tab.

Command-line version of steps 2-3, if you'd rather:
```bash
cd gens-plugin
git init
git add .
git commit -m "Initial gens plugin"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/gens-plugin.git
git push -u origin main
```

Every time I hand you updated code later (adding woodcutting, mining,
PvE, etc.), you just repeat the upload/push step and a fresh jar builds
automatically — no local setup ever needed.

### Option B: Build locally

```bash
mvn clean package
```

The finished jar will be at `target/GensPlugin.jar`.

## Installing on Minehut

1. Build the jar as above (or ask me and I'll walk you through it step by
   step if `mvn` isn't set up on your machine yet).
2. In the Minehut panel, go to your server's **Plugins** tab and upload
   `GensPlugin.jar`.
3. Start the server once so it generates its config files under
   `plugins/GensPlugin/`.
4. **Create the plots world**: in-game (as an op) run a world-creation
   plugin/command to make a **superflat or void world** named `gens_world`
   (matches `gens-world:` in config.yml — change either to match). This is
   the one manual step; the plugin builds plot floors inside that world but
   doesn't generate the world itself.
5. (Optional but recommended) Install **Vault** + an economy plugin like
   **EssentialsX** so balances are shared with the rest of your server. If
   you skip this, GensPlugin's built-in economy kicks in automatically.

## Commands

| Command | Description |
|---|---|
| `/plot claim` | Claim your plot (also happens automatically on first join) |
| `/plot home` | Teleport to your plot |
| `/plot info` | See your plot's generator count |
| `/gens buy` | Buy a Level 1 generator |
| `/gens upgrade` | Upgrade the generator item held in your hand by one level |
| `/gens sell` | Sell loose generator drops sitting in your inventory |
| `/gens gui` | Open your Vacuum Collector's storage (must be holding/carrying one) |
| `/gens info` | Look at a placed generator to see its owner/level/timer |
| `/vacuum give <player>` | (admin) Give someone a Vacuum Collector |

## How the vacuum item works

Every Vacuum Collector is tagged with its own unique ID under the hood.
When one of your generators is due to drop an item, the plugin checks if
you're online and have a vacuum anywhere in your inventory — if so, the
drop is credited straight to that vacuum's storage (a per-level counter
with no upper bound) instead of spawning a physical item. No vacuum in
your inventory? It falls back to a normal physical item drop next to the
generator, same as vanilla hopper/chest setups.

Right-click the vacuum to open a GUI listing everything it's holding, with
a "SELL ALL" button that cashes it all out at once.

## Next steps

This covers the core gens + plot + vacuum loop you asked for first. Ready
whenever you want to add the RPG layer on top — woodcutting, mining
skills/XP, and a PvE combat system for swords. Just tell me how you want
each one to work (leveling curve, tools/enchants, mob types, rewards) and
I'll build it into this same plugin.
