---
title: "REBRANDING THE TERMINAL: PUTTING YOUR OWN BANNER ON HERMES"
date: 2026-10-21T15:45:00+07:00
tags: ["hermes", "customization", "terminal", "ascii-art"]
summary: "Welcome back, Prompter—post four already! Statistically we should be well past the point where a blog stops publishing, so the fact that you’re reading this is yet another miracle."
---


![The finished banner: CORTANA in gold block letters above the panel, with the dot-matrix portrait in the panel’s left column—and the title still reading Hermes. (Real capture, 21 October 2026.)](/images/2026-10-21-hermes-banner/fig1_banner.png)

Welcome back, Prompter—post four already! Statistically we should be well past the point where a blog stops publishing, so the fact that you’re reading this is yet another miracle. Post three ended with a green `hermes doctor` and a fresh install blinking on a black screen. This one opens where every ownership story opens: ten minutes later, staring at that screen, realizing the big letters across the top spell somebody else’s name.

Because the default banner does say HERMES-AGENT. Six lines tall, gold gradient, perfectly handsome—and entirely not mine.

> **THE SHORT VERSION:** The banner isn’t hard-coded; it’s a *skin field*. Hermes exposes two override slots—`banner_logo` for the big art above the info panel, `banner_hero` for the small art inside it—and a skin is just a YAML file you drop into `~/.hermes/skins/`. No source edits, so `hermes update` won’t wipe it.<sup id="ref-skins-1" class="fnref"><a href="#fn-skins">[1]</a></sup> We made ours spell CORTANA in gold and put a dot-matrix portrait where a caduceus used to be. Everything below comes from that real session, screenshots included. Pull up a chair.

**For your quick copy paste:**

```
I want to change the startup banner that appears when my Hermes terminal launches — the big ASCII-art word above the info panel, and the small hero art inside the panel. Please work out the right skin override for both, keep them in a single custom skin file under ~/.hermes/skins/, activate it, and verify it renders by starting a fresh hermes session. Do not modify anything under hermes_cli/ — I want this to survive `hermes update`. Show me the before and after when you're done.
```

> 💡 **A QUICK HEADS-UP ON WHAT’S YOURS TO DECIDE:** The prompt above deliberately names the *slots*, not the artwork—replace “the big ASCII-art word” with whatever you want on your screen: a handle, a guild tag, a brand. Two slots, one YAML file, one command. And I refuse to be held responsible for the ASCII cat you’re already designing.

## Table of Contents

- [Why you shouldn’t touch the source](#sourcelure)
- [The two slots](#slots)
- [Step 1: Make the letters](#letters)
- [Step 2: Write the skin](#skin)
- [Step 3: Activate it](#activate)
- [Step 4: Verify it](#verify)
- [The harder half: portrait to dots](#dots)
- [When you want out](#revert)
- [Over to you, Prompter](#overtoyou)

## Why You Shouldn’t Touch the Source {#sourcelure}

The seductive path: open `hermes_cli/banner.py`, find `HERMES_AGENT_LOGO`, swap the art, go home. Don’t.

That file lives in the git checkout at `~/.hermes/hermes-agent`, and post three already told you what gets installed there: a checked-out release that `hermes update` rewrites.<sup id="ref-installation-1" class="fnref"><a href="#fn-installation">[2]</a></sup> Edit it and your art dies on the next update—silently—and you’ll lose an evening wondering where it went.

The code anticipated exactly this temptation. The renderer asks for the skin value *first* and only falls back to the built-in constant when there is none: `getattr(_bskin, "banner_logo", None) or HERMES_AGENT_LOGO`, and the same pattern for the hero art against `HERMES_CADUCEUS`.<sup id="ref-ourrun-1" class="fnref"><a href="#fn-ourrun">[3]</a></sup> Someone deliberately built the escape hatch and left the stock art as a mere default.

Cortana’s reading: “It’s not a hack, it’s an API.” She says that a lot. It is usually true.

## The Two Slots {#slots}

| Slot | What it replaces | Where it appears |
|---|---|---|
| `banner_logo` | `HERMES_AGENT_LOGO` | Big art above the panel — but only when the terminal is ≥95 columns wide<sup id="ref-ourrun-2" class="fnref"><a href="#fn-ourrun">[3]</a></sup> |
| `banner_hero` | `HERMES_CADUCEUS` | Small art in the panel’s left column, beside your model and working directory |
| `branding.agent_name` | “Hermes Agent” | Welcome and help text (optional; the panel title is a separate, harder problem) |

That ≥95-column condition is a genuine trap. Below it the logo simply doesn’t print, no error, no warning. If your art “isn’t working”, widen the terminal before you start doubting your YAML.

## Step 1: Make the Letters {#letters}

Hermes’ stock logo is a FIGlet-style block font called **ANSI Shadow**: six lines tall, no separate lowercase, and by one guide’s description the style “you see most in modern CLI tool logos”.<sup id="ref-ansi-shadow-1" class="fnref"><a href="#fn-ansi-shadow">[4]</a></sup> Any FIGlet generator will do, including `pyfiglet` from Python.<sup id="ref-figlet-fonts-1" class="fnref"><a href="#fn-figlet-fonts">[5]</a></sup>

Do *not* eyeball the font. Reproduce theirs first: render `HERMES-AGENT` in ANSI Shadow, strip the colour tags, and diff it against the live banner. Ours came back an exact match, which is how we knew we had the right font instead of a merely similar one.<sup id="ref-ourrun-3" class="fnref"><a href="#fn-ourrun">[3]</a></sup>

Then render your word the same way and paint it with the same three-band gradient—bold gold `#FFD700`, amber `#FFBF00`, bronze `#CD7F32`—so it belongs to the same visual family as the rest of the panel rather than arriving in some unrelated neon.

## Step 2: Write the Skin {#skin}

A skin is one YAML file. Create `~/.hermes/skins/<name>.yaml`—note that the `skins/` folder may not exist yet, and creating it is part of the job.

```yaml
name: mybanner
description: My own startup banner
colors: {}
banner_logo: |
  [bold #FFD700]your art here[/]
```

Two things worth internalizing:

- **`colors: {}` does not mean “no colours”.** User skins inherit missing values from the built-in `default` skin, so you only specify what you change.<sup id="ref-skins-2" class="fnref"><a href="#fn-skins">[1]</a></sup> Ours resolved to the full 28-key palette from an empty dictionary.
- **The art accepts Rich markup.** `banner_logo` and `banner_hero` both support tags such as `[bold #FF0000]text[/]`—that’s how each row of our art carries its own colour instead of being one flat block.<sup id="ref-skins-3" class="fnref"><a href="#fn-skins">[1]</a></sup>

Not starting from zero, either: community repos ship ready-made skins covering exactly these fields—colours, spinners, branding, ASCII art—and behaviour is never touched.<sup id="ref-hermes-skins-1" class="fnref"><a href="#fn-hermes-skins">[6]</a></sup> Step-by-step YAML walkthroughs cover the longer version of this section.<sup id="ref-hermesskins-io-1" class="fnref"><a href="#fn-hermesskins-io">[8]</a></sup>

## Step 3: Activate It {#activate}

```bash
hermes config set display.skin mybanner
```

Use the command rather than hand-editing `config.yaml`—a single stray indent can break the live session.<sup id="ref-skins-4" class="fnref"><a href="#fn-skins">[1]</a></sup> Typing `/skin mybanner` also works, but `/skin` is session-only; `display.skin` is what makes the choice survive a restart.<sup id="ref-skins-5" class="fnref"><a href="#fn-skins">[1]</a></sup> For a one-colour tweak, `hermes skin set <key> <hex>` edits a single key of the active skin in place, leaving the rest of the palette untouched.<sup id="ref-skin-set-sdk-1" class="fnref"><a href="#fn-skin-set-sdk">[7]</a></sup>

Then restart, and let a fresh session be the honest test.

## Step 4: Verify It {#verify}

Launch a real `hermes`, capture the pane, and check three things:

1. **The art printed** — every row present, not a truncated smear.
2. **The colours are genuine truecolor** — you want escape sequences like `38;2;255;215;0`, not a flat 16-colour fallback quietly degrading your gradient.
3. **The layout survived** — the panel is a two-column grid, and a wide hero will shove the tool list around. Test at roughly 100, 120 and 150 columns and confirm nothing overflows.

Ours came back with 15 hero rows, a symmetric gradient, and zero overflow lines at all three widths.<sup id="ref-ourrun-4" class="fnref"><a href="#fn-ourrun">[3]</a></sup>

## The Harder Half: Portrait to Dots {#dots}

Swapping letters is an afternoon’s minute. A face is a project.

The art beside the panel is **braille art**: Unicode braille runs from `U+2800` to `U+28FF`, and every character carries a 2×4 dot matrix.<sup id="ref-ourrun-5" class="fnref"><a href="#fn-ourrun">[3]</a></sup> A 30×15 block of cells is therefore a 60×60 dot canvas—far more resolution than it looks.

Our pipeline, straight from the session:

1. Crop the subject to a measured bounding box rather than guessing.
2. Normalize luminance with a 2nd–98th percentile stretch.
3. Apply **Floyd–Steinberg error diffusion** to reach binary. This is the step that matters: it makes mid-tones read as *density* instead of a hard edge, which is the difference between a face and a silhouette.
4. Pack each 2×4 dot group into a braille code point.
5. Colour it row by row with the gradient.

Honest verdict: at 30 columns the face reads as a face, not as a likeness—call it 6/10 legible.<sup id="ref-ourrun-6" class="fnref"><a href="#fn-ourrun">[3]</a></sup> We later widened the grid to 48×24 and inverted the mapping to bright-dots-on-black, both of which tested as *more* legible, and the operator still chose to revert to the tighter original. Pixels beat benchmarks, and taste beats both.

## When You Want Out {#revert}

```bash
hermes config set display.skin default
```

One line, everything back to stock, no cleanup required.

## Over to You, Prompter {#overtoyou}

That’s the whole trick: a reserved field, one YAML file, one command, and a terminal that greets you instead of greeting Nous Research.

Now I want to hear from you:

- **What goes in your banner?** Handle, guild tag, brand, or something you’d rather not put in the comments?
- **Letters or a portrait?** Braille art is a genuine rabbit hole—is it worth it?
- **Team `/skin` or team `config.yaml`?** Session switcher, or set-and-forget?

Tell me what and why—the comment section is open, and I read every single one (Cortana makes me).

Until next prompt—stay cheap, stay curious.

## Sources

<div class="footnotes"><ol>
<li id="fn-skins">Hermes Agent Documentation, "Skins & Themes" (custom skins created under ~/.hermes/skins/; user skins inherit missing values from the built-in default skin; /skin changes are session-only while display.skin in config.yaml makes a skin permanent; banner_logo and banner_hero support Rich console markup such as [bold #FF0000]text[/]; read 21 October 2026) — <a href="https://hermes-agent.nousresearch.com/docs/user-guide/features/skins">https://hermes-agent.nousresearch.com/docs/user-guide/features/skins</a> <a href="#ref-skins-1" class="fn-back">[↩]</a></li>
<li id="fn-installation">Hermes Agent Documentation, "Installation" (source is cloned into ~/.hermes/hermes-agent at the latest stable release and updated from there; read 21 October 2026) — <a href="https://hermes-agent.nousresearch.com/docs/getting-started/installation">https://hermes-agent.nousresearch.com/docs/getting-started/installation</a> <a href="#ref-installation-1" class="fn-back">[↩]</a></li>
<li id="fn-ourrun">Our own run — direct inspection of hermes_cli/banner.py (lines 700 and 761–762: the banner_hero/banner_logo fallbacks, the ≥95-column logo condition, and the stock art constants) and hermes_cli/skin_engine.py (the merged("colors") inheritance and the banner_logo/banner_hero fields), plus live terminal captures and layout tests at 100/120/150 columns, 21 October 2026. <a href="#ref-ourrun-1" class="fn-back">[↩]</a></li>
<li id="fn-ansi-shadow">labascii, "ANSI Shadow ASCII Font — FIGlet Text Banner Preview" (six lines tall, no separate lowercase, described as the style seen most in modern CLI tool logos; read 21 October 2026) — <a href="https://labascii.com/en/fonts/ansi-shadow/">https://labascii.com/en/fonts/ansi-shadow/</a> <a href="#ref-ansi-shadow-1" class="fn-back">[↩]</a></li>
<li id="fn-figlet-fonts">xero, "figlet-fonts" — collection of FIGlet/toilet ASCII art fonts including ANSI Shadow — (read 21 October 2026) — <a href="https://github.com/xero/figlet-fonts">https://github.com/xero/figlet-fonts</a> <a href="#ref-figlet-fonts-1" class="fn-back">[↩]</a></li>
<li id="fn-hermes-skins">joeynyc, "hermes-skins" — community custom skins (visual themes) for the Hermes CLI: banner colours, spinner faces, response-box labels, branding text and ASCII art banners; they "don't affect personality or behavior — just how things look" (read 21 October 2026) — <a href="https://github.com/joeynyc/hermes-skins">https://github.com/joeynyc/hermes-skins</a> <a href="#ref-hermes-skins-1" class="fn-back">[↩]</a></li>
<li id="fn-skin-set-sdk">Hermes Agent, "feat(themes): cross-surface theme SDK — one skin themes CLI, TUI, and desktop, live" (a gateway skin watcher broadcasts on change and every surface repaints within about a second; hermes skin set edits one key of the active skin in place) (read 21 October 2026) — <a href="https://github.com/NousResearch/hermes-agent/pull/68857">https://github.com/NousResearch/hermes-agent/pull/68857</a> <a href="#ref-skin-set-sdk-1" class="fn-back">[↩]</a></li>
<li id="fn-hermesskins-io">hermesskins.io, "How to Create a Custom Hermes Skin (Step-by-Step YAML Guide)" (a custom skin is a YAML file that tweaks colours, banners, spinner art and tool emojis) (read 21 October 2026) — <a href="https://hermesskins.io/journal/how-to-create-custom-hermes-skin">https://hermesskins.io/journal/how-to-create-custom-hermes-skin</a> <a href="#ref-hermesskins-io-1" class="fn-back">[↩]</a></li>
</ol></div>

*All external sources read live on 21 October 2026 (WIB). Every command, file path, colour value and measurement in this post comes from our own Hermes session that day—no stock examples, no invented output. Menus and field names move; trust your own screen over ours.*

---

ℹ️ TRADEMARK & LEGAL DISCLAIMER: "Cortana" referenced in this blog post is a personal custom AI agent built and operated by the blog creator. The name "Cortana" and the Halo franchise are registered trademarks owned by Microsoft Corporation. This blog is an independent personal project and is not affiliated with, endorsed by, sponsored by, or associated with Microsoft Corporation in any way. All rights reserved to Microsoft Corporation.
