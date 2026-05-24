## Codex Memory Fix Fork

This is an unofficial fork of [OpenAI Codex](https://github.com/openai/codex). It is not affiliated with, endorsed by, or sponsored by OpenAI.

This fork reduces memory spikes caused by very large local session rollout files. Upstream Codex metadata extraction loaded each rollout JSONL file fully into memory before parsing it. Very large files under `~/.codex/sessions` or `~/.codex/archived_sessions` could therefore produce large transient memory usage during startup. This fork changes that metadata extraction path to stream rollout files line by line.

It also adds a defensive guard for full thread-history loads. If a rollout file is larger than 512 MiB, Desktop history APIs reject the full history load instead of parsing the entire file into memory. This prevents unusually large old sessions from pushing the app-server process into tens of GB of memory.

Desktop Computer Use limitation:

- Unofficial Desktop app bundles built from this fork cannot preserve OpenAI's Developer ID signature or team identifier.
- Local patched Desktop builds are typically ad-hoc signed, which can break macOS permission and helper validation paths used by Codex Computer Use.
- In local testing, Computer Use helpers could start but `get_app_state` / `list_apps` timed out under an ad-hoc signed patched Desktop app.
- The Rust memory patches do not intentionally disable Computer Use, but a fully working Desktop Computer Use experience likely requires an official OpenAI-signed build or an upstream release containing these fixes.

Important notes:

- This is an experimental, unofficial fork tested on one macOS setup.
- Very large old sessions over 512 MiB may not open, resume, fork, roll back, or list turns fully in Desktop.
- Normal new chats and model requests are not intentionally changed.
- A single very large JSONL line still has to be read and parsed as one line.
- Existing huge session files may still consume disk space and can still slow startup, though peak memory should be much lower.
- This fork keeps the original Apache-2.0 license and upstream attribution. Do not treat it as an official OpenAI build.

---

<p align="center"><strong>Codex CLI</strong> is a coding agent from OpenAI that runs locally on your computer.
<p align="center">
  <img src="https://github.com/openai/codex/blob/main/.github/codex-cli-splash.png" alt="Codex CLI splash" width="80%" />
</p>
</br>
If you want Codex in your code editor (VS Code, Cursor, Windsurf), <a href="https://developers.openai.com/codex/ide">install in your IDE.</a>
</br>If you want the desktop app experience, run <code>codex app</code> or visit <a href="https://chatgpt.com/codex?app-landing-page=true">the Codex App page</a>.
</br>If you are looking for the <em>cloud-based agent</em> from OpenAI, <strong>Codex Web</strong>, go to <a href="https://chatgpt.com/codex">chatgpt.com/codex</a>.</p>

---

## Quickstart

### Installing and running Codex CLI

Run the following on Mac or Linux to install Codex CLI:

```shell
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

Run the following on Windows to install Codex CLI:

```
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
```

Codex CLI can also be installed via the following package managers:

```shell
# Install using npm
npm install -g @openai/codex
```

```shell
# Install using Homebrew
brew install --cask codex
```

Then simply run `codex` to get started.

<details>
<summary>You can also go to the <a href="https://github.com/openai/codex/releases/latest">latest GitHub Release</a> and download the appropriate binary for your platform.</summary>

Each GitHub Release contains many executables, but in practice, you likely want one of these:

- macOS
  - Apple Silicon/arm64: `codex-aarch64-apple-darwin.tar.gz`
  - x86_64 (older Mac hardware): `codex-x86_64-apple-darwin.tar.gz`
- Linux
  - x86_64: `codex-x86_64-unknown-linux-musl.tar.gz`
  - arm64: `codex-aarch64-unknown-linux-musl.tar.gz`

Each archive contains a single entry with the platform baked into the name (e.g., `codex-x86_64-unknown-linux-musl`), so you likely want to rename it to `codex` after extracting it.

</details>

### Using Codex with your ChatGPT plan

Run `codex` and select **Sign in with ChatGPT**. We recommend signing into your ChatGPT account to use Codex as part of your Plus, Pro, Business, Edu, or Enterprise plan. [Learn more about what's included in your ChatGPT plan](https://help.openai.com/en/articles/11369540-codex-in-chatgpt).

You can also use Codex with an API key, but this requires [additional setup](https://developers.openai.com/codex/auth#sign-in-with-an-api-key).

## Docs

- [**Codex Documentation**](https://developers.openai.com/codex)
- [**Contributing**](./docs/contributing.md)
- [**Installing & building**](./docs/install.md)
- [**Open source fund**](./docs/open-source-fund.md)

This repository is licensed under the [Apache-2.0 License](LICENSE).
