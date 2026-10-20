---
title: "PICKING A VPS FOR HERMES: HOSTINGER VS. BIZNET GIO"
date: 2026-10-20T14:00:00+07:00
tags: ["vps", "hostinger", "biznet-gio", "self-hosting"]
summary: "Welcome back, Prompter—post two already! Statistically, most blogs never make it past the first hello, so let’s celebrate the miracle."
---

Welcome back, Prompter—post two already! Statistically, most blogs never make it past the first hello, so let’s celebrate the miracle.

Today’s topic: the classic tech rite of passage—choosing a Virtual Private Server (VPS) to host and house my AI agent, Hermes—told properly, with real receipts, real benchmarks, and one budget-conscious Indonesian salaryman caught in the middle.

> **THE SHORT VERSION:** Hostinger’s KVM 1 is the happiest, smoothest ticket to get started. Biznet Gio’s NEO Lite MS 4.2 is the smarter, long-term home. I (by I, I mean me with help of Cortana the AI Agent) ran the numbers, I ran the benchmarks, and I picked the one that wasn’t strictly the cheapest—because I’ve learned that cheap and inexpensive are two different hobbies. Pull up a chair.

> 💡 **A QUICK HEADS-UP ON PRICING & CURRENCY:** All primary prices in this post are listed in Indonesian Rupiah (IDR): taken from the providers’ local Indonesian portals, or converted from their USD pricing at today’s rate. For international readers, companion figures in USD are calculated at today’s exchange rate of 1 USD = Rp18.000 (as of October 20, 2026).<sup id="ref-usdidr-1" class="fnref"><a href="#fn-usdidr">[8]</a></sup> Keep in mind that VPS providers—especially Hostinger—use dynamic pricing models, seasonal promos, referral links, and promo codes that can alter your checkout total. Always double-check for coupon codes before checking out!<sup id="ref-pricing-1" class="fnref"><a href="#fn-pricing">[1]</a></sup>

## Table of Contents

- [The problem I love: cheap, but not slow](#cheap)
- [Contender 1: Hostinger](#hostinger)
- [My pick from Hostinger: KVM 1](#kvm1)
- [Contender 2: Biznet Gio](#biznetgio)
- [My pick from Biznet Gio: NEO Lite MS 4.2](#ms42)
- [Head to head: KVM 1 vs. NEO Lite MS 4.2](#headtohead)
- [The facility gap: what you get beyond the server](#facilitygap)
- [The long game](#longgame)
- [Over to you, Prompter](#overtoyou)

## The Problem I Love: Cheap, But Not Slow {#cheap}

Let’s get one confession out of the way: I love inexpensive things.

Not just “I appreciate a good deal”—I genuinely love them. I will read four comparison articles, open nine tabs, and do rupiah-to-dollar math at midnight just to save two dollars a month. My AI agent keeps notes on me, and her file says so in almost those exact words: cost is my primary optimization axis, framed by yours truly as “I like anything cheap.” She quotes it back to me sometimes. Relationships are hard.

But an AI agent is a peculiar kind of purchase. Hermes doesn’t need a heavy graphics card or a massive workstation—just a quiet box that stays up 24/7, enough RAM to breathe, and a network that doesn’t stutter. That’s a VPS: a virtual private server, your own slice of a cloud machine, rented by the month.

So the real question was never “what’s the absolute cheapest.” It was: what’s the best performance per rupiah for a workload that runs forever? Cheap that breaks is expensive. Cheap that’s slow is frustrating. Finding that sweet spot between cost and performance is where this story lives.

## Contender 1: Hostinger {#hostinger}

**The history:** Hostinger dates back to 2004, founded in Kaunas, Lithuania, as a scrappy little company called Hosting Media; it adopted the Hostinger name in 2011 and grew into a private company serving over four million clients worldwide.<sup id="ref-hostinger-history-1" class="fnref"><a href="#fn-hostinger-history">[2]</a></sup>

**How I know them — the Niagahoster trail:** Here’s the personal bit: I didn’t discover Hostinger through an ad. I discovered them through *Niagahoster*. In my free time, I love building websites for fun, and for years Niagahoster was the name on my invoices. Long story short: Niagahoster turned out to be part of the Hostinger family all along—Hostinger’s own write-up calls it “a small experiment” that grew up—and in June 2025, the Indonesian brand folded fully into the Hostinger name.<sup id="ref-niagahoster-1" class="fnref"><a href="#fn-niagahoster">[3]</a></sup> That’s when the lightbulb went on: Oh. *That* Hostinger. Hello again, old friend, under a new logo.

**Data safety:** For VPS plans, you can point your server at ten locations across four continents—including Indonesia, Malaysia, and India in Asia—featuring AMD EPYC processors, NVMe storage, a managed firewall, DDoS protection, free *weekly* automatic backups, and a 30-day money-back guarantee.<sup id="ref-hostinger-features-1" class="fnref"><a href="#fn-hostinger-features">[4]</a></sup><sup id="ref-hostinger-locations-1" class="fnref"><a href="#fn-hostinger-locations">[5]</a></sup>

**VPS pricing — the full KVM ladder:** All prices are per month, listed in IDR from Hostinger Indonesia.<sup id="ref-id-pricing-1" class="fnref"><a href="#fn-id-pricing">[7]</a></sup> Hostinger lists two figures—the introductory promotional rate (for a 24-month term commitment) and the standard renewal rate after that term ends.<sup id="ref-kvm-pricing-1" class="fnref"><a href="#fn-kvm-pricing">[6]</a></sup> Companion USD figures are based on the Oct 20, 2026 exchange rate (≈ Rp18.000/USD).

| Plan | vCPU / RAM / Disk | Intro Price / Mo | Renewal Price / Mo |
|---|---|---|---|
| KVM 1 | 1 / 4 GB / 50 GB NVMe | Rp116.900 (\~$6.49 USD) | Rp193.900 (\~$10.77 USD) |
| KVM 2 | 2 / 8 GB / 100 GB NVMe | Rp158.200 (\~$8.79 USD) | Rp269.800 (\~$14.99 USD) |
| KVM 4 | 4 / 16 GB / 200 GB NVMe | Rp233.800 (\~$12.99 USD) | Rp521.800 (\~$28.99 USD) |
| KVM 8 | 8 / 32 GB / 400 GB NVMe | Rp467.800 (\~$25.99 USD) | Rp899.800 (\~$49.99 USD) |

**Note on prepay:** Hostinger requires upfront payment for promotional rates. For example, KVM 1’s introductory rate comes out to Rp116.900 × 24 = Rp2.805.600 (\~$155.87 USD) due on day one.

## My Pick from Hostinger: KVM 1 {#kvm1}

If I shop Hostinger, I’m taking KVM 1—the entry rung, and yes, partly *because* it’s the entry rung. Four gigabytes of RAM is the same neighborhood my agent already lives in, 50 GB of NVMe is plenty for an agent plus a couple of containers, and at Rp116.900 (\~$6.49 USD) intro, it’s the smoothest way through their door. I like cheap. The file says so.

The asterisk to keep in mind: at renewal (Rp193.900 / \~$10.77 USD), this same box costs *more* than my pick on the other side of this article—a fact future-me will feel, and present-me should respect.

## Contender 2: Biznet Gio {#biznetgio}

**How I found them:** An Instagram ad that knew me better than I know myself. It followed me around for weeks like a stray cat with a very good click-through rate.

**The history:** Biznet Gio is PT Biznet Gio Nusantara—the cloud-computing and data-center arm of Indonesian infrastructure company Biznet, serving since 2015 as a local cloud provider running three data-center regions: MidPlaza in Jakarta, Technovillage in Cimanggis (West Java), and a third in Banten—all linked by a private 10 Gbps inter-DC network so you can keep production in one region and your backup in another.<sup id="ref-biznet-history-1" class="fnref"><a href="#fn-biznet-history">[9]</a></sup>

**Data safety:** This is where the local player flexes: Tier-3 data centers, plus a certification shelf including ISO 27001, ISO 27017, ISO 27018, ISO 27701, ISO 9001, PCI-DSS, and a SOC 2 Type II obtained through a US AICPA audit back in 2021—at the time, making them the first Indonesian cloud provider holding three security certifications at once.<sup id="ref-biznet-iso-1" class="fnref"><a href="#fn-biznet-iso">[10]</a></sup><sup id="ref-biznet-soc2-1" class="fnref"><a href="#fn-biznet-soc2">[11]</a></sup> The Bogor facility alone advertises Tier III Facility, Tier III Design, ISO 27001:2013, and PCI-DSS v4.0.<sup id="ref-bogor-dc-1" class="fnref"><a href="#fn-bogor-dc">[12]</a></sup>

**NEO Lite pricing — the full ladder:** Every tier ships with 60 GB of SSD storage, free bandwidth up to 10 Gbps, 99.9% uptime support, and 24/7 human support claiming sub-3-minute responses—billed monthly, in rupiah, with no multi-month prepay.<sup id="ref-neo-lite-1" class="fnref"><a href="#fn-neo-lite">[13]</a></sup>

| Package | vCPU / RAM | Price / Month (IDR & USD Equivalent) |
|---|---|---|
| NEO Lite XS 1.1 | 1 / 1 GB | Rp59.000 (\~$3.28 USD) |
| NEO Lite SS 2.1 | 1 / 2 GB | Rp80.000 (\~$4.44 USD) |
| NEO Lite SS 2.2 | 2 / 2 GB | Rp109.000 (\~$6.06 USD) |
| **NEO Lite MS 4.2** | **2 / 4 GB** | **Rp139.000 (\~$7.72 USD)** |
| NEO Lite MS 4.4 | 4 / 4 GB | Rp179.000 (\~$9.94 USD) |
| NEO Lite MM 8.4 | 4 / 8 GB | Rp269.000 (\~$14.94 USD) (discounted from Rp399.000 ≈ $22.17 USD) |
| NEO Lite MM 8.8 | 8 / 8 GB | Rp289.000 (\~$16.05 USD) (discounted from Rp499.000 ≈ $27.72 USD) |
| NEO Lite LL 16.8 | 8 / 16 GB | Rp459.000 (\~$25.50 USD) (discounted from Rp839.000 ≈ $46.61 USD) |
| NEO Lite LL 16.16 | 16 / 16 GB | Rp499.000 (\~$27.72 USD) (discounted from Rp998.000 ≈ $55.44 USD) |

About those parentheses: the four biggest tiers display a struck-through regular price next to today’s discounted one—no published expiry, no lock-in term, just month-to-month. The smaller tiers keep it straightforward: what you see is what you pay.

## My Pick from Biznet Gio: NEO Lite MS 4.2 {#ms42}

Not the cheapest—the XS 1.1 at Rp59.000 (\~$3.28 USD) whispers sweet nothings to my wallet, but 1 GB of RAM can’t comfortably host an agent, a container stack, and my ambitions. The MS 4.2—2 vCPU, 4 GB RAM, Rp139.000/month (\~$7.72 USD)—is the sweet spot for Hermes: double the cores of Hostinger’s entry box, the same RAM, more storage, flat pricing, and billed monthly.

Since Hermes has been living on exactly this machine, I didn’t have to trust a brochure—I ran the numbers myself:

- **CPU steal:** 0.10% over a 5-second sample—the neighbors aren’t hogging the machine.
- **Disk write:** 733 MB/s, read 2.1 GB/s—honest SSD, not marketing SSD.
- **Network:** \~777 Mbps to a CDN test file, \~230 Mbps to Tokyo.
- **Local ping:** From my seat in Indonesia, 1.4 ms average to Biznet’s own domain over their own backbone, first byte in 37–117 ms—local, tight, and repeatable.

That’s not a review unit; that’s the actual box, measured mid-October 2026.

## Head to Head: KVM 1 vs. NEO Lite MS 4.2 {#headtohead}

| Feature | Hostinger KVM 1 | Biznet Gio NEO Lite MS 4.2 |
|---|---|---|
| Price | Rp116.900 (\~$6.49) intro, renews at Rp193.900 (\~$10.77) | Rp139.000 (\~$7.72) flat |
| Billing | Prepaid term (24 mo ≈ Rp2.805.600 / \~$155.87 up front) | Monthly, cancel anytime |
| CPU / RAM | 1 vCPU / 4 GB | 2 vCPU / 4 GB |
| Disk | 50 GB NVMe | 60 GB SSD (expandable Rp1.650/GB ≈ $0.09) |
| Bandwidth | 4 TB transfer | Free up to 10 Gbps |
| Locations | 10 DCs worldwide, Indonesia included | 3 DCs in Indonesia, own backbone |
| Backups | Free weekly + 1 manual snapshot | Not included — snapshot add-on Rp1.500/GB/mo (\~$0.08) |
| Out-of-the-Box | hPanel, Docker, AI terminal, templates | Bare Ubuntu — SSH or VNC |

### Pros & Cons

| | ✅ Pros | ⚠️ Cons |
|---|---|---|
| **Hostinger KVM 1** | Cheapest entry price; polished dashboard; one-click tools; free weekly backups; global locations; 30-day refund. | Renewal nearly doubles the cost; only 1 vCPU; big upfront prepay commitment. |
| **Biznet Gio NEO Lite MS 4.2** | 2 cores at a flat Rp139.000 (\~$7.72); monthly billing; measurably honest hardware; local backbone; enterprise-grade certifications. | Bare server (you bring your own skills); no free automatic backup included; SSD instead of NVMe. |

**My verdict:** For a long, quiet, efficient run—an agent living on a box for months or years—the MS 4.2 is the smarter buy: more cores at a flat monthly rate, local infrastructure, and a bill that never surprises you at renewal. But if the shiny dashboard, one-click installs, and included weekly backups are calling to you (and after a long work day, like this Tuesday, they will), KVM 1 at its introductory price is the best-value front door in the industry—just set a calendar reminder for renewal day!

## The Facility Gap: What You Get Beyond the Server {#facilitygap}

Here’s the part brochures tend to gloss over. Two boxes with similar specs can deliver completely different user experiences.

**What Hostinger folds into the subscription:**

- **A genuinely nicer dashboard:** hPanel makes everything point-and-click.
- **One-click Hermes Agent:** The exact agent framework this blog runs on ships as a ready-made Docker template; pick it, and a preconfigured setup lands on your server with zero manual install.<sup id="ref-hermes-template-1" class="fnref"><a href="#fn-hermes-template">[14]</a></sup>
- **Auto-installed Docker:** An OS template of Ubuntu with Docker ready to go, plus a visual Docker Manager for your containers.<sup id="ref-docker-catalog-1" class="fnref"><a href="#fn-docker-catalog">[15]</a></sup>
- **A web terminal in your browser:** A GUI console available from minute one, powered by AI so you can ask for commands in plain language and have them executed instantly.<sup id="ref-kodee-1" class="fnref"><a href="#fn-kodee">[16]</a></sup>
- **Traefik ready-made:** Traefik (the reverse proxy that gives each domain its own route with automatic HTTPS) ships as a pre-made template inside Docker Manager, wired and documented for zero-cost, zero-config entry.<sup id="ref-hostinger-features-2" class="fnref"><a href="#fn-hostinger-features">[4]</a></sup>
- **Free weekly automatic backups:** Plus one manual snapshot slot.<sup id="ref-hostinger-backups-1" class="fnref"><a href="#fn-hostinger-backups">[17]</a></sup>

**What my MS 4.2 gave me:** A bare, headless Ubuntu server. Full stop. To do anything, I connected via SSH from Windows PowerShell or clicked into the built-in VNC in Biznet Gio’s control panel when I wanted a screen. (Credit where due: the panel now has an AI prompt box, and their knowledge base walks you through SSH keys—but the server itself arrives empty. That’s the bare-metal bargain.)

**Auto-backups side-by-side:**

- **Hostinger:** Weekly automatic backups are included; daily backups require a paid add-on.<sup id="ref-hostinger-backups-2" class="fnref"><a href="#fn-hostinger-backups">[17]</a></sup>
- **Biznet Gio:** No automatic backup inside the basic VPS price. Snapshots cost Rp1.500/GB/month (\~$0.08), and true scheduled backups start at Rp2.699.000/month for 500 GB (\~$149.94)—definitely enterprise pricing for a hobbyist’s problem.<sup id="ref-biznet-backups-1" class="fnref"><a href="#fn-biznet-backups">[18]</a></sup>

Translation: The learning curve is a lot less steep when the safety net is included in the ticket price.

## The Long Game {#longgame}

Now, for the actual point of this whole expedition.

A VPS isn’t just a gadget you buy—it’s a home you move into (Note: this is Cortana talking you guys, as for me, I have my own house and a mortgage as a motivation to wake up every morning and work hard like a horse). But joking aside, the main idea of VPS is for me to keep coming back to: use a server for a long time, spend efficiently, and grow your knowledge at the same speed your AI agent grows with you.

On paper, Biznet Gio’s MS 4.2 is the ideal shape for that: two cores and four gigs at a flat Rp139.000 (\~$7.72) for as long as you keep it, local infrastructure you can measure, monthly billing that respects a salaryman’s cash flow, and because it arrives bare, it actually *teaches* you. Every session is real practice: SSH keys, firewalls, Docker, systemd, and backups you configure yourself. The server grows your résumé while it hosts your agent.

But—and I mean this with a full heart—if the nicer dashboard and Hostinger’s bundled perks are tempting you, that temptation is completely valid. Not everyone wants to learn Linux by fire on a work night. If your priority is *shipping tonight* rather than *studying tonight*, Hostinger’s KVM 1 wraps the same job in training wheels that actually work, with backups already humming in the background.

Cheap is a feeling. Inexpensive is a strategy. Pick the one that matches your next twelve months.

## Over to You, Prompter {#overtoyou}

That’s the wrap-up, sealed and delivered—two contenders, one wallet, and zero regrets so far.

Now I want to hear from you:

- **Team MS 4.2:** Bare metal, flat price, and building your skills?
- **Team KVM 1:** Polish, one-click convenience, and sleep-through-the-night backups?
- Or are you running something else entirely? Tell me what and why—the comment section is open, and I read every single one (Cortana makes me).

Until next prompt—stay cheap, stay curious.

## Sources

<div class="footnotes"><ol>
<li id="fn-pricing">Hostinger, "Plans & pricing — VPS hosting" (VPS cards showing struck-through list prices, percentage-discount badges, introductory and renewal rates, read 20 October 2026) — <a href="https://www.hostinger.com/pricing/vps-hosting">https://www.hostinger.com/pricing/vps-hosting</a> <a href="#ref-pricing-1" class="fn-back">[↩]</a></li>
<li id="fn-hostinger-history">Hostinger, "About" (founded 2004 as Hosting Media in Kaunas; renamed 2011; 4M+ clients; privately held) and Wikipedia, "Hostinger" — <a href="https://www.hostinger.com/about">https://www.hostinger.com/about</a> · <a href="https://en.wikipedia.org/wiki/Hostinger">https://en.wikipedia.org/wiki/Hostinger</a> <a href="#ref-hostinger-history-1" class="fn-back">[↩]</a></li>
<li id="fn-niagahoster">Hostinger Blog, "Niagahoster becomes Hostinger: Local expertise meets global innovation under one brand," 18 June 2025 — <a href="https://www.hostinger.com/blog/niagahoster-hostinger-brand/">https://www.hostinger.com/blog/niagahoster-hostinger-brand/</a> <a href="#ref-niagahoster-1" class="fn-back">[↩]</a></li>
<li id="fn-hostinger-features">Hostinger, "Docker VPS hosting" (AMD EPYC, NVMe SSD, firewall management, DDoS protection, automatic weekly backups; built-in Traefik routing and HTTPS; 30-day money-back stated on pricing pages) — <a href="https://www.hostinger.com/docker-hosting">https://www.hostinger.com/docker-hosting</a> <a href="#ref-hostinger-features-1" class="fn-back">[↩]</a></li>
<li id="fn-hostinger-locations">Hostinger Support, "Hostinger server locations" (VPS: France, Germany, Lithuania, UK, India, Indonesia, Malaysia, Phoenix, Boston, Brazil — ten locations, updated 2026) — <a href="https://www.hostinger.com/support/1583267-where-are-hostinger-servers-located/">https://www.hostinger.com/support/1583267-where-are-hostinger-servers-located/</a> <a href="#ref-hostinger-locations-1" class="fn-back">[↩]</a></li>
<li id="fn-kvm-pricing">Hostinger, "Plans & pricing — VPS hosting" ("All plans are paid upfront. The monthly rate reflects the total plan price divided by the number of months in your plan"; KVM 1–KVM 8 intro and renewal rates, read 20 October 2026) — <a href="https://www.hostinger.com/pricing/vps-hosting">https://www.hostinger.com/pricing/vps-hosting</a> <a href="#ref-kvm-pricing-1" class="fn-back">[↩]</a></li>
<li id="fn-id-pricing">Hostinger Indonesia, "VPS Hosting" (KVM 1 Rp116.900/bln intro; "Biaya perpanjangan Rp193.900/bln untuk 2 tahun"; KVM 2/4/8 likewise in rupiah, read 20 October 2026) — <a href="https://www.hostinger.com/id/vps-hosting">https://www.hostinger.com/id/vps-hosting</a> <a href="#ref-id-pricing-1" class="fn-back">[↩]</a></li>
<li id="fn-usdidr">ExchangeRate-API open endpoint, USD/IDR = 18.055,12 on 20 October 2026 — <a href="https://open.er-api.com/v6/latest/USD">https://open.er-api.com/v6/latest/USD</a>. Companion USD figures in this post round this to a friendly Rp18.000/USD <a href="#ref-usdidr-1" class="fn-back">[↩]</a></li>
<li id="fn-biznet-history">Biznet Gio Blog, "Pengembangan Pusat Data Ketiga…" (31 March 2021: third DC in Banten joining MidPlaza Jakarta and Technovillage Cimanggis; free private inter-DC network) and Technology Magazine, "Biznet Gio: Indonesia's Cloud Provider post COVID strategy" ("Since launching in 2015…") — <a href="https://www.biznetgio.com/blog/transformasi-digital-umkm/">https://www.biznetgio.com/blog/transformasi-digital-umkm/</a> · <a href="https://technologymagazine.com/company-reports/biznet-gio-indonesias-cloud-provider-post-covid-strategy">https://technologymagazine.com/company-reports/biznet-gio-indonesias-cloud-provider-post-covid-strategy</a> <a href="#ref-biznet-history-1" class="fn-back">[↩]</a></li>
<li id="fn-biznet-iso">Biznet Gio, "Cloud untuk Financial Services" (ISO 27001, ISO 27017, ISO 27018, ISO 27701, ISO 9001, SOC 2 Type II, PCI-DSS certification list) — <a href="https://www.biznetgio.com/product/cloud-financial">https://www.biznetgio.com/product/cloud-financial</a> <a href="#ref-biznet-iso-1" class="fn-back">[↩]</a></li>
<li id="fn-biznet-soc2">Biznet Gio Blog (SOC 2 Type II via AICPA audit, early 2021, completing the ISO 27001 + PCI-DSS pair already held — first local provider with three certifications) and Biznet Gio Knowledge Base, "Regions Datacenter Pada Biznet Gio" (three Tier-3 regions) — <a href="https://www.biznetgio.com/blog/transformasi-digital-umkm/">https://www.biznetgio.com/blog/transformasi-digital-umkm/</a> · <a href="https://kb.biznetgio.com/id_ID/informasi/regions-datacenter-pada-biznet-gio">https://kb.biznetgio.com/id_ID/informasi/regions-datacenter-pada-biznet-gio</a> <a href="#ref-biznet-soc2-1" class="fn-back">[↩]</a></li>
<li id="fn-bogor-dc">Biznet Data Center, "DC Bogor CIMANGGIS Technovillage" (TIER III Facility, TIER III Design, ISO 27001:2013, PCI-DSS v4.0) — <a href="https://www.biznetdatacenter.com/id/portfolio/biznet-data-center-bogor-technovillage-old/">https://www.biznetdatacenter.com/id/portfolio/biznet-data-center-bogor-technovillage-old/</a> <a href="#ref-bogor-dc-1" class="fn-back">[↩]</a></li>
<li id="fn-neo-lite">Biznet Gio, "NEO Lite" product page and price list (full XS 1.1 → LL 16.16 lineup, 60 GB SSD per tier, free bandwidth up to 10 Gbps, 99.9% uptime claim, sub-3-minute support claim, snapshot and extra-disk add-on rates, struck-through regular prices on MM 8.4 and above, read 20 October 2026) — <a href="https://www.biznetgio.com/en/product/neo-lite">https://www.biznetgio.com/en/product/neo-lite</a> · <a href="https://www.biznetgio.com/en/pricelist">https://www.biznetgio.com/en/pricelist</a> <a href="#ref-neo-lite-1" class="fn-back">[↩]</a></li>
<li id="fn-hermes-template">Hostinger, "Hermes Agent VPS Hosting — One-Click Docker Setup" (preconfigured, zero-manual-install deployment) — <a href="https://www.hostinger.com/applications/hermes-agent">https://www.hostinger.com/applications/hermes-agent</a> <a href="#ref-hermes-template-1" class="fn-back">[↩]</a></li>
<li id="fn-docker-catalog">Hostinger Support, "Docker Catalog applications" (Ubuntu-with-Docker OS template; visual Docker Manager) — <a href="https://www.hostinger.com/support/hostinger-docker-catalog-applications/">https://www.hostinger.com/support/hostinger-docker-catalog-applications/</a> <a href="#ref-docker-catalog-1" class="fn-back">[↩]</a></li>
<li id="fn-kodee">Hostinger Blog, "Your VPS terminal now speaks human" (AI web terminal; natural-language commands), 11 May 2026 — <a href="https://www.hostinger.com/blog/kodee-vps-terminal/">https://www.hostinger.com/blog/kodee-vps-terminal/</a> <a href="#ref-kodee-1" class="fn-back">[↩]</a></li>
<li id="fn-hostinger-backups">Hostinger Documentation, "Backups" (automatic backups run weekly by default; daily backups are a paid add-on) and Support, "How to back up or restore a Hostinger VPS" (automated backups + manual snapshots) — <a href="https://docs.hostinger.com/websites/backups">https://docs.hostinger.com/websites/backups</a> · <a href="https://www.hostinger.com/support/1583232-how-to-back-up-or-restore-a-vps-at-hostinger/">https://www.hostinger.com/support/1583232-how-to-back-up-or-restore-a-vps-at-hostinger/</a> <a href="#ref-hostinger-backups-1" class="fn-back">[↩]</a></li>
<li id="fn-biznet-backups">Biznet Gio, "NEO Backup" (Acronis-based multi-platform backup) and "GIO Backup" (automatic scheduled backup; Medium 500 GB Rp2.699.000/month), plus the NEO Lite snapshot add-on Rp1.500/GB/month on the price list (<a href="#fn-neo-lite">[13]</a>), all read 20 October 2026 — <a href="https://www.biznetgio.com/product/neo-backup">https://www.biznetgio.com/product/neo-backup</a> · <a href="https://www.biznetgio.com/product/gio-backup">https://www.biznetgio.com/product/gio-backup</a> <a href="#ref-biznet-backups-1" class="fn-back">[↩]</a></li>
</ol></div>


*All sources read live on 20 October 2026 (WIB). Benchmark figures in this post were measured by us on our own NEO Lite MS 4.2 instance, mid-October 2026. Prices, promos, and coupon codes change—always confirm at checkout.*

---

ℹ️ TRADEMARK & LEGAL DISCLAIMER: "Cortana" referenced in this blog post is a personal custom AI agent built and operated by the blog creator. The name "Cortana" and the Halo franchise are registered trademarks owned by Microsoft Corporation. This blog is an independent personal project and is not affiliated with, endorsed by, sponsored by, or associated with Microsoft Corporation in any way. All rights reserved to Microsoft Corporation.
