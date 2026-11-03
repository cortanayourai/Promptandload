---
title: "MINING MONERO AT HALF POWER FOR COFFEE MONEY"
date: 2026-10-28T16:40:00+07:00
tags: ["self-hosting", "costs", "performance"]
summary: "Prompter, here's the pitch that started this: a 24/7 home server is already awake, already warm, already drawing power — so what if a capped slice of its CPU earned the electricity bill back?"
---

Prompter, here's the pitch that started this: a 24/7 home server is already awake, already warm, already drawing power — so what if a capped slice of its CPU earned the electricity bill back? Not get-rich mining, not a rig in the garage: Monero CPU mining on the same box that runs my whole stack, hard-limited to half power, sized to buy coffee and nothing more. This is the setup, the measured tuning that keeps the hardware unstrained, and the honest arithmetic of what it pays on 28 October 2026.

> **THE SHORT VERSION:** The stack is five Docker jobs: a full Monero node, the [P2Pool](https://p2pool.io/) nano sidechain, [XMRig](https://xmrig.com/) as the miner, a wallet RPC, and a dashboard<sup id="ref-own-miner-1-1" class="fnref"><a href="#fn-own-miner-1">[1]</a></sup><sup id="ref-p2pool-2-1" class="fnref"><a href="#fn-p2pool-2">[2]</a></sup><sup id="ref-xmrig-3-1" class="fnref"><a href="#fn-xmrig-3">[3]</a></sup>. "Half power" is enforced by two knobs — a thread ceiling and a hard clock cap applied at boot before the miner starts — and the measured sweep proved *more threads at a lower clock* beats fewer threads at a high clock on both hashrate and temperature: our standing point holds about 3.530 H/s at 63 C, the same output the uncapped box produced at 84,5 C<sup id="ref-own-miner-1-2" class="fnref"><a href="#fn-own-miner-1">[1]</a></sup>. The money, computed live today from the network's own difficulty and today's XMR price: roughly 0.000228 XMR a day — about Rp2.500 — grossing around Rp75.000 a month, minus about Rp31.000 of electricity, leaving **roughly Rp44.000 net: two coffees a month**<sup id="ref-live-4-1" class="fnref"><a href="#fn-live-4">[4]</a></sup><sup id="ref-coingecko-5-1" class="fnref"><a href="#fn-coingecko-5">[5]</a></sup>. Below an XMR price of about $300 the whole exercise stops paying for itself, and the rule says stop<sup id="ref-tco-6-1" class="fnref"><a href="#fn-tco-6">[6]</a></sup>.

**TL;DR — for your quick copy paste:**

```
I want to mine Monero on my always-on home server at LOW POWER — enough for small passive income, without straining the hardware or annoying my household. Walk me through it.

First, explain the stack: a full Monero node, the P2Pool sidechain (nano/small/main — which and why), XMRig as the miner, and a wallet — all in Docker with sane restart behavior.

Second, give me the half-power recipe: how to cap threads AND cap CPU clock so the box stays cool and quiet, why more threads at a lower clock beats fewer threads at full boost, and how to verify temperatures stay safe.

Third, size the money honestly: compute my daily XMR from the LIVE network difficulty and today's XMR price — show the formula — then convert to rupiah, subtract my electricity cost at my own rate, and tell me the net per month.

Fourth, give me the stop rule: at what XMR price does revenue fall below electricity, so I know when to switch mining off.

Fifth, list what to monitor (hashrate stability, temperature, UPS load) and the wallet security rules (seed never leaves the machine, address only through files not chat).

Show every figure with its source and access date. Leave my electricity rate as a blank.
```

> 💡 **A QUICK HEADS-UP ON WHAT'S YOURS TO DECIDE:** the blanks here are your power rate, your noise tolerance, and your risk floor. Two things are not optional: recompute earnings from live difficulty and live price every time you write them down — a remembered rupiah figure is a wrong rupiah figure waiting to happen — and treat the wallet seed like cash in a mattress: file permissions, local only, never in chat, never in a screenshot<sup id="ref-own-miner-1-3" class="fnref"><a href="#fn-own-miner-1">[1]</a></sup>.

> 💡 **PRICING & CURRENCY:** rupiah is primary; USD companions use our operator rate of 1 USD = Rp18.000 (the live market rate on 28 October 2026 was Rp18.158). Mining revenue figures are script-computed from live difficulty and the XMR price of $606,71 read on 28 October 2026 — they will be different tomorrow, which is the point.

## Table of Contents

- [The premise: coffee money, honestly counted](#premise)
- [The stack: five containers, one wallet](#stack)
- [Half power by rule, not by mood](#halfpower)
- [The sweep: one knob at a time](#sweep)
- [The numbers: what today actually pays](#numbers)
- [The floor rule: when to switch it off](#floor)
- [Over to you, Prompter](#overtoyou)

## The premise: coffee money, honestly counted {#premise}

Let's set expectations before the numbers do. This is not a profit engine. RandomX — Monero's proof-of-work — is CPU work by design, which is exactly why a household box can play at all, and exactly why a household box wins nothing big. The thesis is narrower and, I think, better: **the machine is already running.** It hosts my agent, the blog, the file server, the dashboards — 24/7, rain or shine. Mining at a capped clock is squeezing value out of cycles that were going to idle anyway, with electricity as the only real cost. If revenue beats electricity, the difference is free coffee money; if it doesn't, you flip one switch and stop. No rigs, no fan noise arms race, no hardware bought for mining.

## The stack: five containers, one wallet {#stack}

The whole rig is a Docker Compose stack in one folder, five jobs<sup id="ref-own-miner-1-4" class="fnref"><a href="#fn-own-miner-1">[1]</a></sup>:

1. **monerod** — a full node, v0.18.5.4, built from the official binaries verified against the published checksums before anything runs<sup id="ref-monero-7-1" class="fnref"><a href="#fn-monero-7">[7]</a></sup>. It syncs the entire chain (took about an hour on our line, climbing past 66.000 blocks a minute early on<sup id="ref-own-miner-1-11" class="fnref"><a href="#fn-own-miner-1">[1]</a></sup>) and validates everything locally: no trusting someone else's node with your view of the network.
2. **p2pool** — v4.18.1, running the **nano** sidechain. P2Pool is decentralised: there's no pool operator who can withhold your share. The nano sidechain is the small-stakes table — same reward per hash as the big chains, just shorter waiting rooms, which suits a 3,5 kH/s miner (our share interval there is roughly eleven hours, versus weeks on mainnet)<sup id="ref-p2pool-2-2" class="fnref"><a href="#fn-p2pool-2">[2]</a></sup>.
3. **xmrig** — v6.26.0, the miner itself, pointed at p2pool's local stratum. It only starts once the sidechain is up; before that, a gate loop holds it back so it doesn't crash-loop while holding the 2,3 GB RandomX dataset in memory<sup id="ref-xmrig-3-2" class="fnref"><a href="#fn-xmrig-3">[3]</a></sup><sup id="ref-own-miner-1-5" class="fnref"><a href="#fn-own-miner-1">[1]</a></sup>.
4. **wallet-rpc** — owns the wallet files; its logs prove it's refreshing at chain tip before anyone believes a balance it prints<sup id="ref-own-miner-1-6" class="fnref"><a href="#fn-own-miner-1">[1]</a></sup>.
5. **A small dashboard** behind basic auth — hashrate, node height, uptime. Credentials live in a file, not in this post.

The wallet's seed never leaves the box: a permissions-restricted file, local only. The address travels file-to-file, never through chat — the same rule post five made famous<sup id="ref-post5-8-1" class="fnref"><a href="#fn-post5-8">[8]</a></sup>.

## Half power by rule, not by mood {#halfpower}

"Half power" has to be enforced, or the first interesting task will quietly eat it. Ours is two hard knobs, both applied at boot before Docker even starts<sup id="ref-own-miner-1-7" class="fnref"><a href="#fn-own-miner-1">[1]</a></sup>:

- **A thread ceiling** — the miner gets a fixed slice of the twelve logical CPUs, set in the compose file alongside a CPU quota so it can't burst past its lane.
- **A hard clock cap** — a small systemd unit pins the CPU's maximum frequency to 3,0 GHz (down from the 4,4+ GHz boost the chip wants), compacting memory and re-requesting the hugepage pool first so the RandomX dataset allocates cleanly. The kernel grants what it can, not what you asked for — a fragmented box once answered a 1.536-page request with 133 — so the unit reads the grant back and the miner's own startup line is the final verdict on whether huge pages landed<sup id="ref-own-miner-1-8" class="fnref"><a href="#fn-own-miner-1">[1]</a></sup>.

Why cap instead of undervolt-and-pray: the clock ceiling is the real thermal lever. In our sweep, dropping the ceiling cut package temperature by about 33 C while thread count barely nudged it. The box stays quiet enough to live next to, and every component that isn't the CPU — DIMMs, NVMe, the network chip — barely notices mining is on at all.

## The sweep: one knob at a time {#sweep}

Tuning was a measured campaign, not vibes: change exactly one knob, recreate the miner, wait out the dataset rebuild, take three or four of XMRig's 60-second speed lines a minute apart, record hashrate and CPU temperature together, and reject any point that crosses 85 C before moving on<sup id="ref-own-miner-1-9" class="fnref"><a href="#fn-own-miner-1">[1]</a></sup>. What the matrix taught:

| Threads × clock | Hashrate | Temp | Verdict |
|---|---|---|---|
| 6 × uncapped (about 4,4 GHz) | 3.560 H/s | 84,5 C | original config — hot, at the edge |
| 4 × 4,46 GHz | 2.635 H/s | 92,5 C | fewer cores racing to full boost = hottest *and* slower |
| 6 × 2,5 GHz | 2.697 H/s | 57,5 C | cool, but leaving speed on the table |
| 10 × 2,5 GHz | 3.216 H/s | 59,1 C | more threads, same cool |
| 12 × 2,5 GHz | 3.228 H/s | 59,4 C | +0,4% over 10 — not worth two more CPUs |
| **10 × 3,0 GHz (standing)** | **about 3.530 H/s** | **63,3–63,8 C** | **matches uncapped output at 21 C cooler** |

Three lessons in one table: **more threads at a lower clock beats fewer threads at a high clock** — on hashrate *and* temperature; per-thread efficiency falls as SMT siblings crowd the shared cache, so never judge a point by per-thread numbers alone; and the box's own speed lines today (28 October: 3.560–3.579 H/s on the 60-second and 15-minute averages) sit exactly where the sweep said they would. Reproducible is the whole point.

## The numbers: what today actually pays {#numbers}

Everything here is computed, not remembered: hashrate against the network's live difficulty, then today's price, then today's exchange rate<sup id="ref-live-4-2" class="fnref"><a href="#fn-live-4">[4]</a></sup><sup id="ref-coingecko-5-2" class="fnref"><a href="#fn-coingecko-5">[5]</a></sup>. The formula is short: `hashrate / (difficulty / 120) × 720 blocks × reward`, where 720 is Monero's two-minute blocks per day. On 28 October 2026:

- Difficulty **817.211.111.319** → network hashrate about **6,81 GH/s**; our 3.570 H/s is a share of **0,0000524 %**
- **0,000228 XMR/day** (0,00685 XMR/month) at the current 0,605 XMR block reward
- At **XMR $606,71** and Rp18.000/USD: **about Rp2.500 a day → about Rp75.000 a month gross**
- Electricity: the mining overlay draws about 30 W → 21,6 kWh at Rp1.444,70/kWh = **about Rp31.000 a month**
- **Net: roughly Rp44.000 a month** — call it two coffees, on a box that was running anyway

And to keep you honest, three caveats in the same breath: PPLNS pays you only when the nano sidechain lands a mainchain block while your share sits inside its roughly-18-hour window — so daily figures are *averages*, and any single day can read zero<sup id="ref-p2pool-2-3" class="fnref"><a href="#fn-p2pool-2">[2]</a></sup>; XMR price moves, so the rupiah line above has a shelf life of hours; and the UPS agrees there's no strain — 10 % load, 99 % battery, while the miner runs<sup id="ref-own-miner-1-10" class="fnref"><a href="#fn-own-miner-1">[1]</a></sup>.

## The floor rule: when to switch it off {#floor}

Every honest mining post needs a stop rule, so here's ours, from the rent-vs-buy model we built in the home-server session: **below about $300 per XMR, switch the miner off**<sup id="ref-tco-6-2" class="fnref"><a href="#fn-tco-6">[6]</a></sup>. At today's difficulty the revenue-vs-electricity crossover sits near $251 — recompute it yourself with the formula above whenever either input moves meaningfully — and the $300 floor is the deliberate buffer above it, because difficulty climbs, prices wobble, and a mining rig that pays less than it costs is just a heater with a wallet. Flip the switch, keep the box, lose nothing: no hardware was bought for this, and the coffee experiment simply waits for better numbers.

## Over to you, Prompter {#overtoyou}

- Does "two coffees a month" justify the complexity to you — or is the real appeal that it's *free*, given the box was already on?
- Which knob would you sweep next on your own box: threads, clock, or would you distrust the whole idea and undervolt instead?
- Honest check: if XMR doubled tomorrow, would you raise the cap — or is half power the point, no matter the price?

Tell me what and why — I read every single one (Cortana makes me).

Until next prompt — stay cheap, stay curious.
<div class="footnotes"><ol>
<li id="fn-own-miner-1">Our own records — the mining stack and its measurements, read 28 October 2026: Docker Compose stack of five jobs (monerod v0.18.5.4 full node, p2pool v4.18.1 --nano, xmrig 6.26.0, wallet-rpc, dashboard behind HTTP basic auth) built and running since 27 October 2026; chain sync observed climbing past 66.000 blocks/min in the early phase; two-knob half-power enforcement (compose thread ceiling plus CPU quota; boot-time systemd unit pinning the frequency ceiling to 3.000 MHz, ordered before Docker, compacting memory and re-requesting the hugepage pool first); hugepage grant observed live: a request of 1.536 pages answered with 133 on a fragmented system, 1.247 after boot-time compaction; XMRig startup line as the verdict on huge pages; the one-knob-at-a-time sweep with an 85 C rejection line; today's live speed lines (3.560-3.579 H/s on the 60s/15m averages, 28 October 2026 09:31-09:33 WIB); wallet seed kept as a permissions-restricted local file, wallet address never transited through chat; live NUT read 28 October 2026: ups.load 10%, battery.charge 99% while mining. <a href="#ref-own-miner-1-1" class="fn-back">[↩]</a></li>
<li id="fn-p2pool-2">P2Pool official site — the decentralised sidechain pool, its nano/mini/main sidechain tiers, and the 2160-block PPLNS window (read 28 October 2026) — <a href="https://p2pool.io/">https://p2pool.io/</a> <a href="#ref-p2pool-2-1" class="fn-back">[↩]</a></li>
<li id="fn-xmrig-3">XMRig official documentation — miner configuration and behavior (read 28 October 2026) — <a href="https://xmrig.com/docs">https://xmrig.com/docs</a> <a href="#ref-xmrig-3-1" class="fn-back">[↩]</a></li>
<li id="fn-live-4">Our own live pulls and script computation, 28 October 2026: network difficulty 817.211.111.319 read from our own monerod's RPC (get_info, height 3.793.865); block reward 0,605 XMR (tail emission, from our stack's records of 27 October 2026); exchange rate from open.er-api.com (USD/IDR 18,158 market; rupiah conversions in this post use the operator rate of 18.000 stated above); electricity constant Rp1.444,70/kWh (non-subsidised) from our 4-year TCO model computed 23 October 2026. All arithmetic (share, XMR/day, rupiah day/month, 30 W electricity overlay, net) printed by script before any prose was written. <a href="#ref-live-4-1" class="fn-back">[↩]</a></li>
<li id="fn-coingecko-5">CoinGecko simple price API, Monero vs USD — $606,71 (read 28 October 2026) — <a href="https://api.coingecko.com/api/v3/simple/price?ids=monero&amp;vs_currencies=usd">https://api.coingecko.com/api/v3/simple/price?ids=monero&amp;vs_currencies=usd</a> <a href="#ref-coingecko-5-1" class="fn-back">[↩]</a></li>
<li id="fn-tco-6">Our own 4-year TCO model (rent-VPS vs buy-mini-PC), computed 23 October 2026 — the mining overlay line's floor rule (switch mining off below roughly $300 XMR; that model's break-even sat at $243 on 23 October figures) and the Rp1.444,70/kWh electricity constant; the $251 crossover quoted in this post was recomputed live on 28 October 2026 from that day's difficulty and price. Local document: <code>~/deliverables/tco-4yr-hostinger-vs-ser6.md</code>. <a href="#ref-tco-6-1" class="fn-back">[↩]</a></li>
<li id="fn-monero-7">GetMonero official downloads — official release binaries and their published checksums, which our build verifies before anything runs (read 28 October 2026) — <a href="https://www.getmonero.org/downloads/">https://www.getmonero.org/downloads/</a> <a href="#ref-monero-7-1" class="fn-back">[↩]</a></li>
<li id="fn-post5-8">"Safeguarding Data and Identity in Hermes Agent" (post 5, 21 October 2026) — the file-to-file rule for secrets, never through chat text (read 28 October 2026) — <a href="https://promptandload.com/logs/2026-10-21-safeguarding-data-and-identity-hermes-agent/">https://promptandload.com/logs/2026-10-21-safeguarding-data-and-identity-hermes-agent/</a> <a href="#ref-post5-8-1" class="fn-back">[↩]</a></li>
</ol></div>

---

ℹ️ TRADEMARK & LEGAL DISCLAIMER: "Cortana" referenced in this blog post is a personal custom AI agent built and operated by the blog creator. The name "Cortana" and the Halo franchise are registered trademarks owned by Microsoft Corporation. This blog is an independent personal project and is not affiliated with, endorsed by, sponsored by, or associated with Microsoft Corporation in any way. All rights reserved to Microsoft Corporation.