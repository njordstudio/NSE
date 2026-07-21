# NJORD Engine , Game Builds & Bug Reports

Public downloads and bug tracker for games built with **NJORD Engine**.
This repository contains **finished builds only** , the engine and game source
code are **not** public.

> © 2026 NJORD Studio AB. All rights reserved. "NJORD Engine" is a trademark of
> NJORD Studio AB. Use of the builds is governed by [EULA.txt](EULA.txt).

## Download & play

1. Open the [**Releases**](../../releases) page.
2. Download the latest `njord-<game>-v<version>.zip`.
3. Unzip it anywhere.
4. Double-click **`njord-engine.exe`** (or **`Play.bat`**).

Windows may warn about an unknown publisher (the build is not code-signed yet) ,
choose **More info -> Run anyway**. See `README.txt` inside the zip for controls;
in-game, press **O** for Options (volume, resolution, key rebinding).

## Report a bug

Found a problem? Please [**open an issue**](../../issues/new/choose) using the
**Bug report** template. The more detail (build version, steps, your OS version,
a screenshot), the faster it gets fixed. Thank you for testing!

## Documentation

The user guide for NJORD Engine and the games it makes lives here:
[**docs/handbook.md**](docs/handbook.md) , how everything works, its limits, and how to use
it. (It is a snapshot of the engine's handbook, refreshed with each release.)

## API keys (bring your own)

NJORD Engine is **bring-your-own-keys (BYOK)**: you supply the AI, so costs go
straight to the provider with no markup from us, and your keys stay on your machine
(stored in your OS keyring, never in project files, never sent to us). Add them under
**Settings** in the app.

**Required , the AI agents**

- **Anthropic (Claude)** , powers the agents that build your game from a prompt. Get a
  key at [console.anthropic.com](https://console.anthropic.com). Without it you can
  still open the app and edit everything by hand.

**Optional , 3D model generation**

- **Mesh (geometry + texture):** we recommend **Tripo** (best geometry), or **Meshy**.
- **Rigging + animation:** we recommend **Meshy** (600+ motion-clip library).
- **Hyper3D (Rodin)** , coming.
- You can skip these entirely and import your own glTF models instead.

The best results come from mixing providers: one for the mesh (e.g. Tripo) and another
to rig + animate (e.g. Meshy). Pick each under Settings. See the
[handbook](docs/handbook.md) for the full workflow.

## Roadmap & feature requests

See what is planned in the [**Roadmap**](ROADMAP.md). Got an idea or a "it should
also do X"? Open a [**Feature request**](../../issues/new/choose) , requests feed
straight into the roadmap and are prioritised from real feedback.

## License

The builds are proprietary. You may download and run them to play and test, and
report issues. You may **not** redistribute them, use them commercially, or
reverse engineer them. Full terms: [EULA.txt](EULA.txt). Third-party open-source
components used inside a build are listed in `THIRD-PARTY-LICENSES.txt` (included
in each release zip) under their own licenses.
