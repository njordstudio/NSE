# NJORD Engine , Game Builds & Bug Reports

Public downloads and bug tracker for games built with **NJORD Engine**.
This repository contains **finished builds only** , the engine and game source
code are **not** public.

> © 2026 NJORD Studio AB. All rights reserved. "NJORD Engine" is a product of
> NJORD Studio AB. Use of the builds is governed by [EULA.txt](EULA.txt).

## Download & install

1. Open the [**Releases**](../../releases) page.
2. Download the latest `NJORD Studio_<version>_x64-setup.exe`.
3. Run the installer.

Windows may warn about an unknown publisher (the app is not code-signed yet) ,
choose **More info -> Run anyway**. Then open **NJORD Studio**, add your AI key(s)
under **Settings**, and describe the game you want to build. See the
[handbook](docs/handbook.md) for the full workflow.


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

**Recommended , the AI agents (the CREW)**

- **Anthropic (Claude)** , recommended; best results for the agents that build your
  game from a prompt. Key: [console.anthropic.com](https://console.anthropic.com).
- **OpenAI-compatible** , or point the agents at any OpenAI-compatible endpoint,
  including a local model on your own machine.
- Not required , without any AI key you can open the app and build/edit by hand.

**Optional , 3D model generation**

- **Mesh (geometry + texture):** **Tripo** (best geometry) or **Meshy**.
- **Rigging + animation:** **Meshy** (600+ motion-clip library).
- **Hyper3D (Rodin)** , text-to-3D.
- Skip these and import your own glTF models instead.

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