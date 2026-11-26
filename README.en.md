# Aruhan — International edition / 国际版

**Version v2.1.30** · **International edition** · [Download](https://github.com/xizhihao1/Aruhan/releases) · [Install](INSTALL.md) · [License](LICENSE) · [Disclaimer](DISCLAIMER.md)

> **First international release.** This is the first public build of the
> International edition. Feedback, bug reports, and provider requests are
> especially welcome.

**Your codebase. Your machine. Your models. An AI engineer that actually runs
the loop.**

Aruhan is a local-first AI engineering workspace. It reads your repository,
plans the change, edits files, runs commands and tests, reviews the diff, and
carries the work through a gated delivery pipeline. The same engine powers a
CLI, a terminal UI, a desktop app, a VS Code extension, and the Factory control
plane — so you can start in one surface and continue in another without losing
context.

There is no Aruhan-hosted backend, no mandatory account, and no telemetry by
default. Bring your own API key, point Aruhan at the provider you already
trust, and keep prompts, keys, sessions, memory, and artifacts on your machine.

## Why Aruhan

Most AI coding tools stop at a suggestion. Aruhan closes the engineering loop:

1. **Understand.** Explore the repository, read the relevant files, and inspect
   tests, history, and project rules.
2. **Plan.** Break the task into steps and choose the tools and files involved.
3. **Execute.** Edit code, run commands, apply patches, and work in isolated git
   worktrees when a task should not touch your main checkout.
4. **Verify.** Run tests, linters, and builds; inspect the diff; react to
   failures instead of declaring success.
5. **Deliver.** Record artifacts and evidence, pass quality gates, and hand off
   a result that a teammate or customer can audit.

You stay in control at every step: permission modes decide what may run, and
approval prompts show the exact command or file change before it happens.

## What you can do with it

- **Fix a failing test without babysitting it.** Point Aruhan at the failure,
  let it read the trace, patch the code, run the suite, and show you the diff.
- **Ship a feature across a real repository.** The agent follows project rules,
  reuses existing patterns, updates tests and docs, and keeps changes reviewable.
- **Standardize delivery with Factory.** Turn a repeatable process into a YAML
  pipeline of stations with prompts, inputs, outputs, models, timeouts, and
  quality gates. Every run leaves artifacts, evidence, and a retrospective.
- **Keep sensitive work local.** Use a local model through the built-in Ollama adapter, or a provider endpoint inside your own network. Nothing is sent to the publisher.
- **Use the model you already pay for.** Switch providers and models without
  changing your workflow; the catalog stays current with official provider
  lists, and credentials are encrypted at rest.
- **Work from the surface you prefer.** Start in the terminal, open the desktop
  app for long sessions, or stay inside VS Code — it is one runtime and one
  configuration directory.

## One engine, five surfaces

| Surface | Best for | Entry point |
|---|---|---|
| **CLI / REPL** | Scripts, automation, quick one-shot tasks | `aruhan --plain`, `aruhan query "..."` |
| **Terminal UI** | Long interactive sessions without leaving the shell | `aruhan` (default on a terminal), `aruhan chat` |
| **Desktop** | Visual review, settings, sessions, and Factory console | `aruhan-desktop` |
| **VS Code extension** | Keeping the agent beside the code you are editing | the `.vsix` in this release |
| **Factory** | Repeatable, auditable delivery pipelines | `factory serve` |

On Windows the desktop is a native window. On Linux and macOS it is headless:
it serves the same UI on loopback and opens your default browser.

## Cross-process collaboration

Aruhan sessions are not isolated chat windows. With peer discovery enabled (the
default), local processes can find each other, hand over work, and return
results without copy-pasting between terminals.

- **Discover sessions.** `aruhan peers list` shows registered CLI/TUI/engine
  sessions with their workspace and busy/idle status.
- **Send and track work.** `aruhan peers send <name> "<message>"` supports
  `text`, `task`, `result`, and `ask` message kinds. `inbox`, `read`, `ack`,
  and `accept` list, inspect, close, or start a turn for a queued message. A TUI
  wakes on arrival and queues messages while it is busy.
- **Pair machines on a LAN.** `aruhan peers identity` prints this machine's
  fingerprint; `aruhan peers remote add` pairs another machine;
  `aruhan peers bridge` listens for peers (default `0.0.0.0:8788`); send with
  `--remote <name> --to <session>` to reach a session on the paired machine.
- **Deliberate boundaries.** The registry stores routing metadata only — never
  conversation content. Messages land in a local mailbox and do not run tools on
  the receiving side by themselves. Set `ARUHAN_PEER=0` to opt out for one
  process.

## Factory: from one-off chats to repeatable delivery

Factory is the control plane for work that must be repeatable. A pack defines
the delivery process; a pipeline is a sequence of stations. Each station
declares its prompt, inputs, outputs, timeout, model, and gates, so the same
process can run again next week with a different repository and the same
quality bar.

- **Machine checks block; advisory checks warn; human gates park the run** until
  someone with authority approves it.
- **Evidence is recorded per run** — artifacts, ledger entries, retrospectives,
  and worktree isolation — instead of being scattered across chat logs.
- **The control plane stays separate from execution.** `factory serve` accepts
  and tracks runs; a detached worker executes the stations, so an HTTP request
  never becomes the runtime.
- **The console is local** at `http://127.0.0.1:8787/console/`, with SQLite by
  default and PostgreSQL for larger deployments.

## Capability map

| You need | What Aruhan gives you |
|---|---|
| **Change a real repository** | An agent loop that reads the code, edits files, runs commands and tests, inspects the diff, and works in isolated git worktrees when needed. |
| **Stay in control** | Permission modes, work modes, file-access scope, approval prompts, MCP and desktop-control grants, plus audit records. |
| **Keep context** | Persistent sessions, project memory, and reusable skills instead of re-explaining the repository every time. |
| **Work beyond one turn** | `aruhan goal "<goal>"` drives a long-running autonomous task; subagents split focused work; local peers can hand work between processes. |
| **Automate** | `aruhan query "<task>"` with `--json` or `--jsonl`, pipe mode, and machine-readable envelopes for status, doctor, session, and config commands. |
| **Integrate** | MCP client and a read-only `aruhan mcp-serve` server, lifecycle hooks, skills, and an Agent Client Protocol server (`aruhan acp --stdio`). |
| **Deliver repeatably** | Factory packs and pipelines: stations with prompts, inputs/outputs, models, timeouts, machine/advisory/human gates, artifacts, evidence, retros, and rollback/pause/stop controls. |
| **Operate and audit** | `aruhan doctor --bundle` for redacted diagnostics, `aruhan audit verify` for the hash chain, CycloneDX SBOM and license inventory export. |
| **Reach people where they work** | Native Windows desktop, headless browser UI on Linux/macOS, VS Code extension, and IM bot gateways (WeCom, Feishu, DingTalk, Telegram, Slack) through the desktop daemon. |
| **Use your own models** | BYOK text, image, video, speech, and local-model endpoints; a shared catalog; credentials encrypted at rest; no Aruhan-hosted model proxy. |
| **Run anywhere** | Windows, Linux, and macOS on amd64 and arm64, with the same configuration directory across every surface. |

### What that changes for you

- **Less context switching.** Persisted sessions and the same settings are
  available from the terminal, the desktop app, and VS Code.
- **Faster feedback.** The agent runs tests and builds itself instead of handing
  you an untested patch.
- **Auditable delivery.** Factory records what ran, what passed, and who
  approved it, so a teammate or customer can verify the result.
- **Data stays yours.** Prompts, keys, sessions, and artifacts live on your
  machine; model requests go only to the endpoint you configure.
- **No model lock-in.** Switching providers or models does not change the
  workflow, and the catalog is configuration rather than a fork of the engine.

## Editions and models

This download is the **International edition** (`intl`).

| Item | Value |
|---|---|
| Edition | `intl` (International edition) |
| Release channel | https://github.com/xizhihao1/Aruhan/releases |
| Update checks | GitHub Releases only |
| Language / text models | OpenAI, Anthropic, Google Gemini, xAI, Groq, OpenRouter, Ollama (local) |
| Image / video | OpenAI, Stability, Runway, Luma, HeyGen, D-ID |
| Speech | OpenAI TTS/STT; whisper.cpp local STT |

This build ships foreign model and media providers only.
Mainland-China providers are filtered from the catalog, settings, and every
outbound request; there is no cross-edition fallback.
The terms of service of each provider still apply where you deploy it; you
remain responsible for local compliance.

The two editions share the same engine, tools, permissions, Factory, and UI.
The difference is the model/media provider catalog and the update channel: CN
serves mainland-China providers and checks Gitee; the International edition
serves international providers and checks GitHub. There is no cross-edition
provider fallback.

## Quick start

### 1. Install

| You want | Download |
|---|---|
| Windows, everything (recommended) | **Aruhan-Setup-2.1.30-intl.exe** |
| Windows CLI only | **Aruhan-CLI-Setup-2.1.30-intl.exe** |
| Windows desktop only | **Aruhan-Desktop-Setup-2.1.30-intl.exe** |
| Windows Factory | **Factory-Setup-2.1.30-intl.exe** |
| Windows portable | the matching **aruhan-windows-*-2.1.30-intl.zip** |
| Linux CLI / Factory | **aruhan-linux-*.tar.gz**, **factory-linux-*.tar.gz** |
| macOS CLI / Factory | **aruhan-darwin-*.tar.gz**, **factory-darwin-*.tar.gz** |
| Linux / macOS desktop | the matching **aruhan-desktop-*--intl** binary |
| VS Code | **aruhan-vscode-intl.vsix** |

See [INSTALL.md](INSTALL.md) for step-by-step instructions, including PATH,
Gatekeeper, and SmartScreen notes.

### 2. Configure a model

The simplest path is an environment variable:

~~~bash
export ARUHAN_PROVIDER="your-provider"
export ARUHAN_MODEL="your-model"
export ARUHAN_API_KEY="your-key"
~~~

Provider-specific variables such as `OPENAI_API_KEY` also
work. The desktop app, VS Code extension, CLI, and TUI all show the environment
configuration as active, and you can save the same settings in the app.

### 3. Run

~~~bash
aruhan                         # full-screen TUI (default on a terminal)
aruhan --plain                 # line-oriented REPL
aruhan query "fix the failing test in ./internal/foo"
aruhan goal "upgrade the test suite and keep it green"
aruhan-desktop                 # desktop (headless on Linux/macOS)
factory serve                  # Factory console at 127.0.0.1:8787/console/
~~~

### 4. Verify the download

~~~bash
sha256sum -c SHA256SUMS
~~~

~~~powershell
Get-FileHash -Algorithm SHA256 <file>
# compare with the matching line in SHA256SUMS
~~~

Every International edition build reports its edition:

~~~bash
aruhan --version
~~~

## Platform status

| Platform | Status |
|---|---|
| Windows | Tested, including the native desktop and installers. |
| Linux | CLI and Factory smoke-tested under WSL; desktop is headless and uses the browser UI. |
| macOS | Published for Intel and Apple Silicon; not yet exercised on every macOS version. Please report issues. |

Windows installers are **not code-signed**. If SmartScreen warns, choose
**More info → Run anyway** after verifying `SHA256SUMS`. The warning reflects
the missing signature, not malware.

## Licensing and commercial use

Aruhan is **free for personal, non-commercial use**. Commercial use — including
use by a company or team, production deployment, private or on-premises
deployment, embedding Aruhan in a product or service, custom development, or a
source-code / perpetual buyout — requires a separate written commercial license.

We keep those terms practical:

- **Team and commercial licenses** sized to the number of users and deployments.
- **Private / on-premises deployment**, including offline and internal-network
  installation, with a written scope and support terms.
- **Custom development and integration** for your delivery process, providers,
  or internal systems.
- **Source-code or perpetual buyout** under a signed agreement. Ownership
  transfer is effective only when the agreement says so explicitly.

Contact **37735973@qq.com** with your organization, deployment scale, and
intended use. For a faster reply, use the subject
`Aruhan commercial license` or `Aruhan private deployment`.

The release repository contains compiled artifacts only; source code is not
published here. See [LICENSE](LICENSE) and [LICENSE.zh-CN.md](LICENSE.zh-CN.md)
for the binding terms.

## Security, acceptable use, and disclaimer

Aruhan executes model-requested tools and can change files, run commands, and
call external services. Read this before enabling unattended modes.

- **No warranty.** The software is provided **AS IS**, without express or
  implied warranties, including merchantability, fitness for a particular
  purpose, and non-infringement.
- **No liability for outcomes.** To the maximum extent permitted by law, the
  authors and copyright holders are not liable for any direct, indirect,
  incidental, special, consequential, or punitive damages, including lost
  profits, data, or business, arising from the software, AI output, tool
  execution, provider behavior, configuration, or data loss.
- **You are responsible for review and compliance.** Review every diff and
  command, keep backups, use least-privilege permissions, test outside
  production, and comply with the law and every model provider's terms. AI
  output is not legal, medical, financial, or other professional advice.
- **High-risk use is prohibited** without independent validation and all
  required approvals: life support, medical diagnosis, aviation, rail,
  automotive safety, industrial control, nuclear, chemical, biological,
  weapons, emergency response, law enforcement, judicial decisions, and
  critical infrastructure.
- **Prohibited conduct** includes unlawful use, malware, unauthorized access,
  evading security or licensing controls, and the restrictions below.

**Reverse engineering and resale.** Except where applicable law expressly
permits, you may not decompile, disassemble, or reverse engineer the official
binaries; bypass licensing, authentication, tenant isolation, or credential
protection; remove proprietary or attribution notices; or resell, rent, lease,
sublicense, distribute, or redistribute the software or modified binaries
without written permission. Modified binaries must never be presented as
official builds.

The full notice is in [DISCLAIMER.md](DISCLAIMER.md). It is a project notice,
not legal advice; obtain local counsel for your jurisdiction.

## Support and contact

- **Installation and usage:** see [INSTALL.md](INSTALL.md) and the issue tracker
  at https://github.com/xizhihao1/Aruhan/issues.
- **Security reports:** email **37735973@qq.com** privately. Do not open a public
  issue containing credentials, customer data, or exploit details.
- **Commercial, private deployment, and buyout:** **37735973@qq.com**.

## Assets in this release

<details>
<summary><strong>Show all 27 assets</strong></summary>

- **darwin/aruhan-darwin-amd64-2.1.30-intl.tar.gz** — 43,149,295 bytes
- **darwin/aruhan-darwin-arm64-2.1.30-intl.tar.gz** — 40,841,116 bytes
- **darwin/factory-darwin-amd64-2.1.30-intl.tar.gz** — 44,966,320 bytes
- **darwin/factory-darwin-arm64-2.1.30-intl.tar.gz** — 42,394,846 bytes
- **linux/aruhan-linux-amd64-2.1.30-intl.tar.gz** — 42,486,935 bytes
- **linux/aruhan-linux-arm64-2.1.30-intl.tar.gz** — 39,368,429 bytes
- **linux/factory-linux-amd64-2.1.30-intl.tar.gz** — 44,288,512 bytes
- **linux/factory-linux-arm64-2.1.30-intl.tar.gz** — 40,913,749 bytes
- **vscode/aruhan-vscode-intl.vsix** — 88,049,821 bytes
- **windows/Aruhan-CLI-Setup-2.1.30-intl.exe** — 21,845,316 bytes
- **windows/aruhan-desktop-arm64-intl.exe** — 38,301,696 bytes
- **windows/aruhan-desktop-intl.exe** — 44,044,288 bytes
- **windows/Aruhan-Desktop-Setup-2.1.30-intl.exe** — 33,469,853 bytes
- **windows/aruhan-desktop-windows-amd64-2.1.30-intl.zip** — 15,545,682 bytes
- **windows/aruhan-desktop-windows-arm64-2.1.30-intl.zip** — 13,071,207 bytes
- **windows/aruhan-intl.exe** — 72,709,632 bytes
- **windows/Aruhan-Setup-2.1.30-intl.exe** — 33,470,443 bytes
- **windows/aruhan-tui-installer-windows-amd64-intl.exe** — 76,289,024 bytes
- **windows/aruhan-tui-installer-windows-arm64-intl.exe** — 71,347,712 bytes
- **windows/aruhan-windows-amd64-2.1.30-intl.zip** — 28,983,445 bytes
- **windows/aruhan-windows-amd64-intl.exe** — 72,709,632 bytes
- **windows/aruhan-windows-arm64-2.1.30-intl.zip** — 26,715,977 bytes
- **windows/aruhan-windows-arm64-intl.exe** — 67,966,464 bytes
- **windows/factory-intl.exe** — 40,834,560 bytes
- **windows/Factory-Setup-2.1.30-intl.exe** — 33,996,842 bytes
- **windows/factory-windows-amd64-intl.exe** — 40,834,560 bytes
- **windows/factory-windows-arm64-intl.exe** — 37,615,616 bytes

</details>

## Documentation

| Document | Purpose |
|---|---|
| [INSTALL.md](INSTALL.md) | Install, upgrade, PATH, Gatekeeper, and SmartScreen |
| [LICENSE](LICENSE) / [LICENSE.zh-CN.md](LICENSE.zh-CN.md) | Binding license terms |
| [DISCLAIMER.md](DISCLAIMER.md) | Warranty, liability, acceptable use, and high-risk limits |
| [RELEASE_NOTES.md](RELEASE_NOTES.md) | What changed in this version |
| **SHA256SUMS** | Checksums for every distributed file |

