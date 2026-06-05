# hermes snap

Snap packaging for [Hermes AI Agent](https://github.com/NousResearch/hermes-agent) by Nous Research — an autonomous AI agent with 60+ built-in tools and 20+ platform integrations.

## Building

```
snapcraft
```

Snapcraft will install `hermes-agent` from PyPI via the Python plugin, install the `playwright` Python package, bake a pinned Chromium revision for browser automation, and bundle Node.js 22 for runtime browser tool invocation.

## Installing

```
sudo snap install --classic hermes_<version>_amd64.snap --dangerous
```

## Usage

```
hermes setup          # first-run setup wizard
hermes               # interactive CLI
```

The background gateway service is installed and enabled as a systemd user unit the first time any `hermes` command is run:

```
systemctl --user status hermes
systemctl --user stop hermes
systemctl --user start hermes
```

## Design notes

**Python package** — hermes is distributed as a Python package (`hermes-agent` on PyPI). The snap uses snapcraft's `python` plugin to install it into a virtualenv bundled with the snap.

**Bundled Node.js** — A separate `nodejs` part stages Node.js 22 via the `npm` plugin (no npm packages installed — just the runtime). This allows hermes to use Node.js for browser automation tools at runtime without requiring it to be installed on the host.

**Playwright Chromium** — The Python `playwright` package is installed alongside `hermes-agent` and its pinned Chromium revision is baked in during build. Chromium runs with `--no-sandbox` via a wrapper script (classic confinement, no AppArmor policy).

**Classic confinement** — Hermes needs broad filesystem and process access for its agentic tool execution, so classic confinement is used.
