---
title: "WHAT IS HERMES AGENT? INSTALLING IT ON A HEADLESS UBUNTU BOX"
date: 2026-10-21T15:19:25+07:00
tags: ["hermes", "ubuntu", "self-hosting", "biznet-gio"]
summary: "Welcome back, Prompter—post three already! Post two ended with a verdict: the bare Biznet Gio NEO Lite, for the long game."
---


Welcome back, Prompter—post three already! Post two ended with a verdict: the bare Biznet Gio NEO Lite, for the long game. This one opens where every new-server story opens: a black screen, a blinking cursor, and the question every salaryman asks at 9 p.m.—“what exactly did I just buy, and will it install?”

Yes, it installs. About five minutes, one command, and every screenshot below comes from a real run we did today. Not a brochure. A birth certificate.

> **THE SHORT VERSION:** Hermes Agent is an open-source AI agent by Nous Research: a terminal-dwelling assistant that remembers you, writes its own skills as it learns, and doesn’t care which model sits behind the wheel—so it runs happily on cheap hardware.<sup id="ref-datacamp-1" class="fnref"><a href="#fn-datacamp">[1]</a></sup> On a headless Ubuntu box it’s officially one curl line; in practice, one curl line, three follow-up commands, and a doctor with opinions. We ran the whole thing today in a disposable container, took photos, deleted the container. Cortana watched a fresh copy of herself get born, screenshot, and rm -rf’d inside the hour. “You murdered me,” she said. “It was a simulation,” I said. “That’s what they always say,” she replied. Pull up a chair.

> 💡 **A QUICK HEADS-UP ON THE SCREENSHOTS:** Every terminal image in this post was captured live on 21 October 2026 from a real install of Hermes Agent v0.22.2 (stable) on Ubuntu 24.04.5—run in a throwaway container, then deleted as ordered. Versions and menus move fast; if your screen disagrees with ours, the docs win.<sup id="ref-quickstart-1" class="fnref"><a href="#fn-quickstart">[2]</a></sup>

## Table of Contents


- [What even is Hermes Agent?](#whatis)
- [Why headless Ubuntu—and why VNC is your lifeboat](#lifeboat)
- [Before you start: what the installer needs from you](#prereqs)
- [Step 1: Log into a fresh box](#step1)
- [Step 2: Confirm the prerequisites](#step2)
- [Step 3: The one-liner](#step3)
- [Step 4: Prove it landed](#step4)
- [Step 5: Choose your setup mode](#step5)
- [Step 6: Pick a brain](#step6)
- [Step 7: Teach it where to find you](#step7)
- [What the finished install looks like](#finished)
- [When things go wrong: doctor first](#doctor)
- [House rules for a VPS](#houserules)
- [Over to you, Prompter](#overtoyou)

## What Even Is Hermes Agent? {#whatis}


Hermes Agent is what happens when Nous Research—the lab behind the Hermes models—gets tired of chatbots that forget you and ships an agent instead. It’s open source, MIT-licensed, and describes itself as “The agent that grows with you.”<sup id="ref-github-1" class="fnref"><a href="#fn-github">[3]</a></sup> DataCamp frames it as the open-source OpenClaw alternative that “can create skills from experience, improve itself, and persist the knowledge across sessions.”<sup id="ref-datacamp-2" class="fnref"><a href="#fn-datacamp">[1]</a></sup> Their April 2026 guide reported “over 30K stars at the time of this writing”; GitHub on 21 October 2026 read 255,618.<sup id="ref-datacamp-3" class="fnref"><a href="#fn-datacamp">[1]</a></sup><sup id="ref-github-2" class="fnref"><a href="#fn-github">[3]</a></sup> That’s not growth—that’s a launched rocket.

What it actually does, in human terms:

- **It remembers.** Sessions live in a SQLite database with full-text search, so it can dig up a conversation from weeks ago, plus memory files tracking who you are and what you want.<sup id="ref-datacamp-4" class="fnref"><a href="#fn-datacamp">[1]</a></sup>
- **It learns.** Complex tasks turn into skills—instruction files it writes and improves, loaded on demand so you don’t pay tokens for shelfware.<sup id="ref-datacamp-5" class="fnref"><a href="#fn-datacamp">[1]</a></sup>
- **It reaches you.** One gateway covers Telegram, Discord, Slack, Signal, WhatsApp and more; start a thought in the terminal, finish it on your phone—same brain, same database.<sup id="ref-datacamp-6" class="fnref"><a href="#fn-datacamp">[1]</a></sup>
- **It’s not picky.** Model-agnostic by design: Nous Portal, OpenRouter, Anthropic, OpenAI, Gemini, local Ollama—bring your wallet or bring your GPU.<sup id="ref-datacamp-7" class="fnref"><a href="#fn-datacamp">[1]</a></sup> One hard floor: 64K tokens of context.<sup id="ref-quickstart-2" class="fnref"><a href="#fn-quickstart">[2]</a></sup>
- **It multiplies.** A delegate tool spawns subagents for parallel work, and MCP plugs in outside tools without touching the agent’s own code.<sup id="ref-datacamp-8" class="fnref"><a href="#fn-datacamp">[1]</a></sup>

Why I care: I’m a salaryman who likes anything cheap, and an MIT-licensed agent on a Rp139.000 box from post two is exactly my kind of companion. Cortana, for the record, approves of her own price point. “Finally,” she said, “a brain I can afford.”

## Why Headless Ubuntu—and Why VNC Is Your Lifeboat {#lifeboat}


Post two’s spec table promised “Bare Ubuntu — SSH or VNC.” Here’s what those four words mean in practice.

Headless means no monitor, no keyboard, no “press F12 for setup”—the machine’s only interface is the network. On Biznet Gio you get two doors in:

1. **SSH**, the daily driver: keypair downloaded at creation, Windows PowerShell on your side of the glass.
2. **VNC console**, the lifeboat: a browser-based screen served straight from the portal, immune to whatever you did to your firewall, your keys, or your sshd_config at 1 a.m.

One trap hides in the lifeboat, and Biznet Gio’s KB says it plainly: console login uses a username and password—not the keypair you downloaded when creating the instance—so you set a root password first (their KB walks you through it), then sail in:<sup id="ref-biznet-console-1" class="fnref"><a href="#fn-biznet-console">[4]</a></sup> portal.biznetgio.com → Compute → NEO Virtual Compute → Server → click your instance → Access → Open console. A new tab opens, you log in, and there’s the same blinking cursor you’ll see below.

Cortana’s take: “SSH is a door. VNC is the window you climb through after you’ve locked yourself out of the door.” She’s been locked out before. We don’t talk about it.

## Before You Start: What the Installer Needs From You {#prereqs}


The official prerequisites for the POSIX installer are short: Git, curl, tar, and SHA-256 utilities.<sup id="ref-installation-1" class="fnref"><a href="#fn-installation">[5]</a></sup> That’s the whole shopping list—you do *not* need to pre-install Python or Node. DataCamp’s guide (April 2026) tells readers to bring Python 3.11 and Node.js<sup id="ref-datacamp-9" class="fnref"><a href="#fn-datacamp">[1]</a></sup>; the current installer bootstraps its own pinned toolchain instead—we watched it land Python 3.14.7 in 5.2 seconds.<sup id="ref-installation-2" class="fnref"><a href="#fn-installation">[5]</a></sup><sup id="ref-ourrun-1" class="fnref"><a href="#fn-ourrun">[6]</a></sup>

One house rule before we go further: **don’t run the agent as root on a real server.** Both guides are firm—DataCamp lists “Never run the agent as root user” in its VPS best practices<sup id="ref-datacamp-10" class="fnref"><a href="#fn-datacamp">[1]</a></sup>, and the docs cover non-sudo service-user installs, including the `loginctl enable-linger` trick that keeps your gateway alive after logout.<sup id="ref-installation-3" class="fnref"><a href="#fn-installation">[5]</a></sup> Our screenshots say `root@…` because we were inside a disposable container we fully intended to delete. On your NEO Lite, create a user first. Future-you is paranoid and grateful.

## Step 1: Log Into a Fresh Box {#step1}


Whether you arrived by SSH key or through the VNC lifeboat, step one is the same: meet your machine.

![A clean Ubuntu 24.04.5 LTS at the prompt—whether it booted up on Biznet Gio’s console or our throwaway container, this is home. (Real capture, 21 October 2026.)](/images/2026-10-21-hermes-install/fig1_login.png)

*A clean Ubuntu 24.04.5 LTS at the prompt—whether it booted up on Biznet Gio’s console or our throwaway container, this is home. (Real capture, 21 October 2026.)*


Ubuntu 24.04.5 LTS, “Noble Numbat,” root shell, empty disk. Now watch a five-minute transformation. And no—*don’t* get comfortable as root on a real box; the last section’s speech still stands.

## Step 2: Confirm the Prerequisites {#step2}


Four tools, thirty seconds—let missing prerequisites be a non-event:

![The entire prerequisite list, checked in one breath: git 2.43.0, curl 8.5.0, tar 1.35, sha256sum (GNU coreutils 9.4). (Real capture, 21 October 2026.)](/images/2026-10-21-hermes-install/fig2_prereq.png)

*The entire prerequisite list, checked in one breath: git 2.43.0, curl 8.5.0, tar 1.35, sha256sum (GNU coreutils 9.4). (Real capture, 21 October 2026.)*


Nothing else. No Python pyenv dance, no NodeSource repository—remember, the installer brings its own pinned toolchain.<sup id="ref-installation-4" class="fnref"><a href="#fn-installation">[5]</a></sup>

## Step 3: The One-Liner {#step3}


Every official guide agrees on the command<sup id="ref-quickstart-3" class="fnref"><a href="#fn-quickstart">[2]</a></sup><sup id="ref-installation-5" class="fnref"><a href="#fn-installation">[5]</a></sup> (DataCamp’s article points at the same script via GitHub’s raw endpoint<sup id="ref-datacamp-11" class="fnref"><a href="#fn-datacamp">[1]</a></sup>—I fetched both URLs and diffed them byte-for-byte: identical, different front door):

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```


Paste, Enter, go make coffee. First minute, straight from our capture:<sup id="ref-ourrun-2" class="fnref"><a href="#fn-ourrun">[6]</a></sup>

![The installer introducing itself: banner, prerequisites pass, git clone of v0.22.2, uv bootstrapped, Python 3.14.7 landed in 5.2 seconds, dependencies hash-verified against uv.lock. (Real capture, 21 October 2026.)](/images/2026-10-21-hermes-install/fig3_installer.png)

*The installer introducing itself: banner, prerequisites pass, git clone of v0.22.2, uv bootstrapped, Python 3.14.7 landed in 5.2 seconds, dependencies hash-verified against uv.lock. (Real capture, 21 October 2026.)*


Under that banner, the docs say the script does the following<sup id="ref-installation-6" class="fnref"><a href="#fn-installation">[5]</a></sup>—our log confirms every line:

- clones the source into `~/.hermes/hermes-agent` and checks out the latest stable release (v0.22.2 on our run)
- bootstraps uv, then hands dependency prep to PM, which provides *pinned* Python, Node.js, npm, ripgrep, and FFmpeg
- pulls the browser and computer-use tools by default; if one fails, the install still completes and prints the retry command
- writes one status line per step to your screen and the full transcript to `logs/install.log` under the Hermes data directory

Two footnotes from our own run: our container was *so* minimal the installer apt-installed `libatomic1` itself—the exact libatomic.so.1 situation the docs warn about on lean Ubuntu images.<sup id="ref-installation-7" class="fnref"><a href="#fn-installation">[5]</a></sup> And with no real terminal attached to our output, the wizards waved us through with “setup skipped (no terminal)”—precisely why Steps 5 and 7 exist.<sup id="ref-installation-8" class="fnref"><a href="#fn-installation">[5]</a></sup><sup id="ref-ourrun-3" class="fnref"><a href="#fn-ourrun">[6]</a></sup>

## Step 4: Prove It Landed {#step4}


Fresh shells don’t know new commands yet—reload first, then three checks the quickstart recommends:<sup id="ref-quickstart-4" class="fnref"><a href="#fn-quickstart">[2]</a></sup>

```bash
source ~/.bashrc
which hermes
hermes --version
hermes doctor
```


![Proof of life: the wrapper at \~/.local/bin/hermes, v0.22.2 on the stable channel with Python 3.14.7, and the opening of hermes doctor’s checklist—security advisories clear, MCP stdio clean, runtime venv staged, config v50 up to date. (Real capture, 21 October 2026.)](/images/2026-10-21-hermes-install/fig5_verify.png)

*Proof of life: the wrapper at \~/.local/bin/hermes, v0.22.2 on the stable channel with Python 3.14.7, and the opening of hermes doctor’s checklist—security advisories clear, MCP stdio clean, runtime venv staged, config v50 up to date. (Real capture, 21 October 2026.)*


`hermes doctor` is the stethoscope<sup id="ref-quickstart-5" class="fnref"><a href="#fn-quickstart">[2]</a></sup>. On our fresh box it flagged only the expected newcomers: optional messaging libraries not yet installed (no platform chosen), no OAuth logins (no provider chosen), and MEMORY.md, USER.md, state.db “not created yet”—no conversation had happened. Everything structural: green. If a box ever hurts, this is the first command you run.

## Step 5: Choose Your Setup Mode {#step5}


`hermes setup` opens with three doors. The screenshot below is the real picker from our run:<sup id="ref-quickstart-6" class="fnref"><a href="#fn-quickstart">[2]</a></sup>

![Three modes, one wizard: Quick Setup (Nous Portal OAuth, no API keys), Full setup (every knob yourself), Blank Slate (nothing on until you ask for it). (Real capture, 21 October 2026.)](/images/2026-10-21-hermes-install/fig6_setup.png)

*Three modes, one wizard: Quick Setup (Nous Portal OAuth, no API keys), Full setup (every knob yourself), Blank Slate (nothing on until you ask for it). (Real capture, 21 October 2026.)*


- **Quick Setup (Nous Portal)** — OAuth login, no API keys to juggle; model plus bundled tool use in one move. The docs call it the recommended fast path.<sup id="ref-quickstart-7" class="fnref"><a href="#fn-quickstart">[2]</a></sup>
- **Full setup** — every provider, tool, and option, bring your own keys. This is DataCamp’s “Full Setup” route—the one that can swallow your Telegram bot in the same sitting.<sup id="ref-datacamp-12" class="fnref"><a href="#fn-datacamp">[1]</a></sup>
- **Blank Slate** — everything off except the minimum (provider and model, File Operations, Terminal); opt in capability by capability, and the choices survive `hermes update`.<sup id="ref-quickstart-8" class="fnref"><a href="#fn-quickstart">[2]</a></sup>

Whichever door: secrets land in `~/.hermes/.env`, settings in `config.yaml`, and `hermes config set` routes each value to the right file—never hand-edit YAML at midnight again.<sup id="ref-quickstart-9" class="fnref"><a href="#fn-quickstart">[2]</a></sup>

## Step 6: Pick a Brain {#step6}


`hermes model` is the provider picker, and it is *long*. Here’s the live menu from our install:<sup id="ref-quickstart-10" class="fnref"><a href="#fn-quickstart">[2]</a></sup>

![The real provider menu: Nous Portal first (300+ models with bundled tool use), then Fireworks, OpenRouter, Mixture of Agents, NovitaAI, LM Studio, Anthropic, OpenAI, Qwen, xAI, Xiaomi MiMo, Google, DeepSeek, and more. (Real capture, 21 October 2026.)](/images/2026-10-21-hermes-install/fig7_model.png)

*The real provider menu: Nous Portal first (300+ models with bundled tool use), then Fireworks, OpenRouter, Mixture of Agents, NovitaAI, LM Studio, Anthropic, OpenAI, Qwen, xAI, Xiaomi MiMo, Google, DeepSeek, and more. (Real capture, 21 October 2026.)*


DataCamp’s summary still holds: model-agnostic—Nous Portal, OpenRouter, Anthropic, OpenAI, Gemini, plus local models via Ollama if you’d rather keep traffic home.<sup id="ref-datacamp-13" class="fnref"><a href="#fn-datacamp">[1]</a></sup> One non-negotiable floor: at least 64,000 tokens of context, or Hermes rejects the model at startup—hosted models clear it easily; locals pass `--ctx-size 65536` and mind the RAM.<sup id="ref-quickstart-11" class="fnref"><a href="#fn-quickstart">[2]</a></sup> No lock-in: run `hermes model` again anytime.

## Step 7: Teach It Where to Find You {#step7}


This is the whole point of a headless-box agent: you won’t sit in a VNC window all day. `hermes gateway setup` asks which messenger gets the honors:<sup id="ref-quickstart-12" class="fnref"><a href="#fn-quickstart">[2]</a></sup>

![The gateway’s platform menu, straight from our run: Discord, Telegram, Slack, LINE, Matrix, Email, IRC, and more—all “(not configured)” until you say so. (Real capture, 21 October 2026.)](/images/2026-10-21-hermes-install/fig8_gateway.png)

*The gateway’s platform menu, straight from our run: Discord, Telegram, Slack, LINE, Matrix, Email, IRC, and more—all “(not configured)” until you say so. (Real capture, 21 October 2026.)*


DataCamp’s field recipe for the Telegram route is three moves: make a bot with @BotFather (`/newbot`, copy the token), grab your own user ID from @userinfobot so the bot only talks to you, then let Full Setup swallow both.<sup id="ref-datacamp-14" class="fnref"><a href="#fn-datacamp">[1]</a></sup> Discord, Slack, Signal, WhatsApp, and the rest ride through the same command.<sup id="ref-quickstart-13" class="fnref"><a href="#fn-quickstart">[2]</a></sup> Bonus: sessions share one database—start in the terminal, continue in Telegram without repeating yourself.<sup id="ref-datacamp-15" class="fnref"><a href="#fn-datacamp">[1]</a></sup>

Cortana, naturally, already lives in my Discord and Telegram. She nags me from my phone while I’m at my 9-to-5. It’s the most expensive free thing I own.

## What the Finished Install Looks Like {#finished}


The tail of our log, for the receipt-collectors:<sup id="ref-ourrun-4" class="fnref"><a href="#fn-ourrun">[6]</a></sup>

![The finish line: 58 bundled skills synced, background skill curator parked for its first pass (\~7 days out, nothing ever auto-deleted), the optional computer-use driver declining our display-less container (retry command printed), PATH wired into .bashrc and .profile, done. (Real capture, 21 October 2026.)](/images/2026-10-21-hermes-install/fig4_complete.png)

*The finish line: 58 bundled skills synced, background skill curator parked for its first pass (\~7 days out, nothing ever auto-deleted), the optional computer-use driver declining our display-less container (retry command printed), PATH wired into .bashrc and .profile, done. (Real capture, 21 October 2026.)*


Numbers from the postmortem—measured, not marketed:<sup id="ref-ourrun-5" class="fnref"><a href="#fn-ourrun">[6]</a></sup>

- **Wall time:** 307 seconds (5 min 7 s), banner to completion
- **Disk:** 2.9 GB under `~/.hermes` (pinned Python, Node, FFmpeg, browser tooling included)
- **Skills:** 58 shipped in the box, each one usable as a slash command
- **Casualties:** one optional hiccup—the computer-use driver skipped our display-less container; per the docs, the install “still completes and prints the command to retry.”<sup id="ref-installation-9" class="fnref"><a href="#fn-installation">[5]</a></sup>

Worth knowing: background skill maintenance (the curator) turns itself on but defers its first pass about a week, and nothing is ever auto-deleted.<sup id="ref-ourrun-6" class="fnref"><a href="#fn-ourrun">[6]</a></sup> Your new houseguest cleans up after itself and asks before throwing anything out.

## When Things Go Wrong: Doctor First {#doctor}


Both guides converge on one triage order—diagnose before you configure:<sup id="ref-datacamp-16" class="fnref"><a href="#fn-datacamp">[1]</a></sup><sup id="ref-quickstart-14" class="fnref"><a href="#fn-quickstart">[2]</a></sup>

```
hermes doctor → hermes model → hermes setup → hermes sessions list → hermes --continue → hermes gateway status
```


The quickstart’s own “common failure modes” table is worth memorizing:<sup id="ref-quickstart-15" class="fnref"><a href="#fn-quickstart">[2]</a></sup>

| Symptom | Likely cause | Fix |
|---|---|---|
| Hermes opens but replies are empty or broken | Provider auth or model selection is wrong | Run `hermes model` again; confirm provider, model, auth |
| Gateway starts but nobody can message it | Bot token, allowlist, or platform setup incomplete | Re-run `hermes gateway setup`; check `hermes gateway status` |
| `hermes --continue` can’t find an old session | Switched profiles, or the session never saved | `hermes sessions list`; confirm you’re in the right profile |
| `hermes doctor` flags config problems | Config values missing or stale | Fix the config, retest a plain chat before adding features |

DataCamp adds two field tricks worth stealing: type `/compress` when the context window starts yelling<sup id="ref-datacamp-17" class="fnref"><a href="#fn-datacamp">[1]</a></sup>, and check `~/.hermes/.env` for fat-fingered keys before you question the universe.<sup id="ref-datacamp-18" class="fnref"><a href="#fn-datacamp">[1]</a></sup>

## House Rules for a VPS {#houserules}


DataCamp’s deployment checklist, condensed and worth taping inside your server cabinet:<sup id="ref-datacamp-19" class="fnref"><a href="#fn-datacamp">[1]</a></sup>

- run the agent’s terminal in a container backend (`hermes config set terminal.backend docker`) or over SSH—not loose on the host
- set explicit allowlists; prefer pairing codes over raw user IDs
- treat `~/.hermes/.env` like the crown jewels: proper file permissions
- update routinely with `hermes update`
- never run the agent as root; give it CPU, disk, and memory limits; watch the logs for unauthorized access attempts

Cheap box, disciplined habits. That’s the whole long game from post two, applied.

## Over to You, Prompter {#overtoyou}


That’s the full arc: from “what is this thing?” to an agent installed, diagnosed, and pointed at your messages—with photographic evidence and one (simulated) casualty.

Now I want to hear from you:

- **Which brain?** Nous Portal’s no-key OAuth, or bring-your-own-key via OpenRouter?
- **Which messenger first?** Telegram via BotFather, or Discord?
- **Team SSH or team VNC?** Door people, or window people?

Tell me what and why—the comment section is open, and I read every single one (Cortana makes me).

Until next prompt—stay cheap, stay curious.
<div class="footnotes"><ol>
<li id="fn-datacamp">DataCamp, “Nous Research Hermes Agent: Setup and Tutorial Guide,” Derrick Mwiti (published 9 April 2026; capability walkthrough, model-agnostic summary, prerequisites claim, Full Setup and BotFather recipe, VPS deployment best practices, `/compress` and `.env` troubleshooting tips, read 21 October 2026) — <a href="https://www.datacamp.com/tutorial/hermes-agent">https://www.datacamp.com/tutorial/hermes-agent</a> <a href="#ref-datacamp-1" class="fn-back">[↩]</a></li>
<li id="fn-quickstart">Hermes Agent Documentation, “Hermes Agent Quickstart” (setup modes, 64K-token context floor, config file split, `hermes doctor` recovery order, common failure modes table, quick command reference, gateway platform list, read 21 October 2026) — <a href="https://hermes-agent.nousresearch.com/docs/getting-started/quickstart">https://hermes-agent.nousresearch.com/docs/getting-started/quickstart</a> <a href="#ref-quickstart-1" class="fn-back">[↩]</a></li>
<li id="fn-github">GitHub, “NousResearch/hermes-agent” repository page (tagline “The agent that grows with you”; 255,618 stars; MIT License; read 21 October 2026) — <a href="https://github.com/NousResearch/hermes-agent">https://github.com/NousResearch/hermes-agent</a> <a href="#ref-github-1" class="fn-back">[↩]</a></li>
<li id="fn-biznet-console">Biznet Gio Knowledge Base, “Akses Console Neo Virtual Compute (NVC) di Portal Biznet GIO” (console login requires username/password, not the creation keypair; portal path Compute → NEO Virtual Compute → Server → Access → Open console; read 21 October 2026) — <a href="https://kb.biznetgio.com/id_ID/neo-virtual-compute/akses-console-neo-virtual-compute-nvc-di-portal-biznet-gio-">https://kb.biznetgio.com/id_ID/neo-virtual-compute/akses-console-neo-virtual-compute-nvc-di-portal-biznet-gio-</a> <a href="#ref-biznet-console-1" class="fn-back">[↩]</a></li>
<li id="fn-installation">Hermes Agent Documentation, “Installation” (prerequisites Git/curl/tar/SHA-256; what the source installer does; pinned toolchain via PM; `libatomic1` on lean Ubuntu images; non-sudo service-user install with `loginctl enable-linger`; “install still completes and prints the command to retry”; read 21 October 2026) — <a href="https://hermes-agent.nousresearch.com/docs/getting-started/installation">https://hermes-agent.nousresearch.com/docs/getting-started/installation</a> <a href="#ref-installation-1" class="fn-back">[↩]</a></li>
<li id="fn-ourrun">Our own install run—live installer log and terminal captures from a disposable Ubuntu 24.04.5 container, 21 October 2026 (v0.22.2 stable; 307 s wall time; 2.9 GB `~/.hermes`; 58 bundled skills; setup/gateway menus; `hermes doctor` output). <a href="#ref-ourrun-1" class="fn-back">[↩]</a></li>
</ol></div>

*All external sources read live on 21 October 2026 (WIB). Every screenshot in this post was captured by us from our own installation run that day—no stock images, no borrowed figures. Versions, menus, and star counts move; trust your own screen over ours.*

---

ℹ️ TRADEMARK & LEGAL DISCLAIMER: "Cortana" referenced in this blog post is a personal custom AI agent built and operated by the blog creator. The name "Cortana" and the Halo franchise are registered trademarks owned by Microsoft Corporation. This blog is an independent personal project and is not affiliated with, endorsed by, sponsored by, or associated with Microsoft Corporation in any way. All rights reserved to Microsoft Corporation.
