# Zerg Micro Bot (Ares Framework)

A micro bot for RL training in a combat arena.

This project uses the [ares-sc2](https://github.com/AresSC2/ares-sc2) framework with minimal logic:

- Fixed race: Zerg
- No economy and no macro
- No build order
- This project is built to train RL bots and will evolve gradually as those RL bots improve over time.

## Current behavior

In `bot/main.py`:

- The bot inherits from `AresBot`
- In `on_start`, the build runner is disabled using `set_build_completed()`
- In `on_step`, the bot uses the normal Ares loop (`super().on_step(...)`)
- Then it registers `AMove` for every unit

In `run.py`:

- Local map is forced to `MicroAIArenaT2R8`
- Default bot race is set to `Zerg`

## Requirements

- Python 3.11
- Poetry
- Git
- StarCraft II installed
- Micro map available in the SC2 maps folder

## Installation

With the project already cloned:

```bash
poetry install
```

The `ares-sc2` directory is included directly in this repository, not as a
submodule. It must be present in a fresh clone.

### Precompiled Ares helper

Ares imports `sc2_helper` when initializing its combat simulator, even when this
bot only uses A-move. The native modules in `ares-sc2/sc2_helper/` must therefore
be committed along with the Python files; `poetry install` does not rebuild them.

The repository includes these binaries for each CPython version from 3.10 to 3.13:

| Platform | Filename (`VERSION` is `310`, `311`, `312`, or `313`) |
| --- | --- |
| Windows x64 | `sc2_helper.cpVERSION-win_amd64.pyd` |
| Linux x86_64 | `sc2_helper.cpython-VERSION-x86_64-linux-gnu.so` |
| macOS | `sc2_helper.cpython-VERSION-darwin.so` |

Use Python 3.11 for the documented setup (the dependency manifests allow 3.11
and 3.12). Extra helper binaries do not extend the project's supported Python
versions, and a binary must match the target interpreter, OS, and architecture.

To check the helper after installing dependencies, run from the project root:

```bash
poetry run python -c "import run; from sc2_helper.combat_simulator import CombatSimulator; CombatSimulator(); print('Ares helper OK')"
```

The root `.gitignore` explicitly allows these native modules. Do not remove
them when preparing a clone or a new repository.

## Run locally

```bash
poetry run python run.py
```

If SC2 is installed in a different location, update `MAPS_PATH` in `run.py`.

## Basic configuration

You can set bot name and race in `config.yml`:

```yml
MyBotName: ZergMicroScripted
MyBotRace: Zerg
```

Note: `run.py` already sets Zerg by default. If you set another value in `config.yml`, it can override that.

## Relevant structure

- `bot/main.py`: bot logic
- `run.py`: local execution and map selection
- `config.yml`: name/race/general config
- `zerg_builds.yml`: present in the project, but its build order is not executed by this micro bot

## Publish to a new GitHub repository

Quick checklist:

```bash
git init
git add .
git commit -m "Initial commit: Zerg micro A-move bot"
git branch -M main
git remote add origin https://github.com/<your-username>/<new-repo>.git
git push -u origin main
```

If this project already has history and you only want to change the remote:

```bash
git add .
git commit -m "Prepare Zerg micro bot with precompiled Ares helper"
git remote remove origin
git remote add origin https://github.com/<your-username>/<new-repo>.git
git push -u origin main
```
