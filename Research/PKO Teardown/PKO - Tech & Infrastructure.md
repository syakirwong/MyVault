---
tags:
  - research
  - pko
  - technical
source: https://pirateking.online/
captured: 2026-10-23
---

# PKO — Tech & Infrastructure

Passive recon of public pages only: what any visitor's browser receives. No logins, no game client, no server probing.

## Two-site topology

| Layer | Host | Stack | Purpose |
|-------|------|-------|---------|
| `pirateking.online` | Netlify edge (`server: Netlify`, `cache-status: Netlify Edge`) | Static HTML. A ThemeForest-style game template (Bootstrap 4, jQuery, GSAP, Revolution Slider, Flickity, Photoswipe, Jarallax, SoundManager2, Typed.js, Isotope) | Marketing landing page: hero, feature cards, class carousel. Every CTA links out to the portal |
| `piratekings.online` (note the **s**) | Cloudflare (`server: cloudflare`, bot-challenge JS at `/cdn-cgi/challenge-platform/`) | **XenForo 2.3** forum (`data-xf="2.3"`, `xf_csrf` cookie) with heavy custom add-ons | Real portal: accounts, forum, guides, database, simulators, rankings, cash-shop mall, support tickets |

- The landing page's `og:url` points at the portal domain, so the two domains are one brand split across hosts. The landing page is effectively a static funnel.
- Security headers on the portal: `x-frame-options: SAMEORIGIN`, `x-content-type-options: nosniff`. The landing page sends HSTS.
- Third parties: Google Analytics 4 (`G-MMJEV9QKTB`), a DMCA.com badge, Swiper 8 from jsDelivr, Google Fonts (Roboto Condensed + Marcellus SC), and Web Push through XenForo's VAPID key.
- Operator: "ServerDev LLC" in the footer. The portal says the server launched in **July 2016**. The landing page claims 8 years, 85,041 members and 1,000+ concurrent players; these are marketing figures and were not verified.

## Custom XenForo add-ons (the interesting part)

All of these live under `/js/piratekingonline/…`, cache-busted with `?_v=<hash>`.

| Module | Route | What it reveals |
|--------|-------|-----------------|
| Game database | `/database/{item,monster,npc,map,quest,class,blueprint,portal}/<slug>.<id>/` | The server's game tables (ItemInfo, CharacterInfo and so on) are imported into XenForo entities. Slugs use the format `short-sword.1`, so the numeric suffix is the in-game item ID. Hover tooltips load from `/…/preview` through `XF.ajax` |
| Character simulator | `/database/simulator` + `character-simulation.js` (~100 KB) | A **client-side JS port of the server's stat formulas**, including per-class coefficient tables. See [[PKO - Stat Formulas]] |
| Sea-trade simulator | `/database/trade-simulator` + `trade-simulation.js`, Leaflet map | The full buy and sell tables for every port trader, with NPC spawn points plotted on a Leaflet map. See [[PKO - Sea Trade Economy]] |
| Portals & events | `/database/portal` | Timed portal schedule (open, active, closed) with a live countdown (`base/timer.js`), plus boss timers |
| Rankings | `/rank/` | Per-server ladders (Port Royal, New Providence) with class filters. Data is pulled from the game DB |
| Game accounts | `/game-accounts/` (403 when logged out) | Forum accounts linked to game accounts. The forum login is the master identity |
| Mall | `/mall/`, `/mall/package/<slug>.<id>/` | A web cash shop priced in **Rum**. Purchases are delivered to the game account |

**Architecture pattern:** the forum is the hub for identity, community and commerce, and the game server is a data source behind it. A small team gets SSO, a CMS, ticketing and a storefront without building them.

## Game client

- A Windows installer (v1.4.2) plus a cumulative patch (v1.4.4), mirrored on Google Drive, MEGA and Dropbox. There's no custom CDN and no launcher-based delta patching; patches are manual downloads.
- The engine is the original 2006 MOLI "Tales of Pirates" client (DirectX 8/9-era C++). The server stack is the community-leaked ToP source (GateServer, GroupServer, GameServer, AccountServer on MSSQL), the standard base for every PKO private server. *This is inferred from the PKO private-server ecosystem; nothing on the website confirms it.*

## Observed live meta (rank page, 287 entries, captured 2026-10-23)

| Level | Players |
|-------|---------|
| 90 (cap) | 187 |
| 86–89 | 98 |
| ≤85 | 2 |

| Class | Share |
|-------|-------|
| Crusader | 116 (40%) |
| Sharpshooter | 39 |
| Champion | 36 |
| Seal Master | 36 |
| Cleric | 29 |
| Voyager | 29 |

- 18 guilds appear, and the top 3 hold about 25% of ranked players.
- About 65% of the ranked list sits at the level cap, so the game is in a mature end-game-plateau state.
- Crusader at 40% is a clear balance outlier. The likely cause is its agility-scaling flee and attack-speed coefficients (see [[PKO - Stat Formulas]]). Attack speed is capped by a hard floor, so stacking agility pays off until that cap.

## Takeaways for Stormbound

- **Ship a public game database and a build simulator on day one.** PKO's community runs on them. For a text-first game where builds are the core gameplay ([[Gem System]]), a web build planner that uses the *same* formula module as the server is a key feature, not a side project. See [[Technical Architecture]].
- One identity across forum, shop and game cuts friction. Mirror that with a single account service; see [[Account & Security System]].
- PKO's simulator duplicates formulas by hand in JS, so it can drift from the server. Generate the client and web versions from one source instead.

---
Back to [[PKO Teardown]]
