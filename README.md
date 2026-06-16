# hermes-agent snap

Snap packaging for [Hermes AI Agent](https://github.com/NousResearch/hermes-agent) by Nous Research — an autonomous AI agent with 60+ built-in tools and 20+ platform integrations. The snap is named `hermes-agent` (matching the upstream PyPI package); the installed command is `hermes-agent`.

## Building

```
snapcraft
```

Snapcraft will install `hermes-agent` from PyPI via the Python plugin, install the `playwright` Python package, bake a pinned Chromium revision for browser automation, and bundle Node.js 22 for runtime browser tool invocation.

## Installing

```
sudo snap install --classic hermes-agent_<version>_amd64.snap --dangerous
```

## Usage

```
hermes-agent setup              # first-run setup wizard
hermes-agent                    # interactive CLI
hermes-agent.inference-snap     # pick a Canonical inference snap (interactive TUI)
```

The background gateway service is installed and enabled as a systemd user unit the first time any `hermes-agent` command is run:

```
systemctl --user status hermes-agent
systemctl --user stop hermes-agent
systemctl --user start hermes-agent
```

### Local AI with Canonical inference snaps

`hermes-agent.inference-snap` detects installed [Canonical inference snaps](https://snapcraft.io/search?q=inference)
such as `gemma4`, `gemma3`, `deepseek-r1`, `nemotron-3-nano`, or `qwen-vl`, probes
their OpenAI-compatible API, and lets you choose one as Hermes's default model.
It configures Hermes via `hermes config set` and restarts the gateway so the change
takes effect immediately. Re-run it any time to switch models.

```
sudo snap install gemma4
hermes-agent.inference-snap
```

## Design notes

**Python package** — hermes is distributed as a Python package (`hermes-agent` on PyPI). The snap installs it into a bundled virtualenv using the `nil` plugin with a manual `python3 -m venv` rather than the `python` plugin, because on core24 the python plugin's post-build payload check fails (python3.12 is filtered as a base-manifest package). The Python 3.12 stdlib is bundled into the venv and `pyvenv.cfg` is repointed at the snap path (with site-packages added via `PYTHONPATH`), so the snap is self-contained and does not depend on the host's Python. The interpreter is **not** patchelf'd — classic confinement uses the host's dynamic linker.

**Bundled Node.js** — A separate `nodejs` part stages Node.js 22 via the `npm` plugin (no npm packages installed — just the runtime). This allows hermes to use Node.js for browser automation tools at runtime without requiring it to be installed on the host.

**Playwright Chromium** — The Python `playwright` package is installed alongside `hermes-agent` and its pinned Chromium revision is baked in during build. Chromium runs with `--no-sandbox` via a wrapper script (classic confinement, no AppArmor policy).

**Classic confinement** — Hermes needs broad filesystem and process access for its agentic tool execution, so classic confinement is used.

## Links

- Upstream project: <https://github.com/NousResearch/hermes-agent> (https://hermes-agent.nousresearch.com)
- Snap packaging: <https://github.com/kenvandine/hermes-snap>
- Report a snap issue: <https://github.com/kenvandine/hermes-snap/issues>

## License

Hermes is licensed under **MIT**. This snap packaging lives in [kenvandine/hermes-snap](https://github.com/kenvandine/hermes-snap).
