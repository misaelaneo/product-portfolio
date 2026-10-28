# Tabline website: design and content brief

A brief for designing Tabline's public website: one main page and a privacy
page. Everything factual here matches what Tabline does today. Keep it that
way: the page will be read by Riot Games when they review Tabline for a
production API key, and by Twitch.

## 1. Goals

In order of importance:

1. **Pass Riot's product review.** A reviewer should understand in about a
   minute what Tabline is, see that it works, and see that it follows Riot's
   developer policies, especially around player data. Riot has said an
   incomplete website makes approval unlikely.
2. **Explain Tabline to streamers** and how to get access.
3. **Give Twitch** a privacy policy URL for the extension listing.

It is not a marketing site. Clear and trustworthy matters more than flashy.

## 2. Audience

| Who | What they need from the page |
|---|---|
| Riot developer relations reviewer | What it does, proof it works, where the data comes from, how player data is protected, legal disclaimer, contact |
| League streamers on Twitch | What viewers get, what they need to install, how to get access |
| Viewers | What the scoreboard is; that they install nothing |

## 3. Pages

- `/` : the main page (sections below, in this order).
- `/privacy` : the privacy policy (section 6).

Both share the same header and footer.

## 4. Main page sections

Copy is a starting draft. Wording can be polished, but facts must not change.
Text in [brackets] is a placeholder the owner fills in.

### 4.1 Header

- Tabline logo mark (asset A1) and the word "Tabline".
- Links: How it works, Player data, Get Tabline, Privacy.

### 4.2 Hero

- **Headline:** "The League scoreboard, live on stream."
- **Subheadline:** "Tabline puts a live League of Legends scoreboard on a
  streamer's Twitch video. Viewers hover the video to follow the game: both
  teams, items, runes, objectives, and player cards."
- **Status line:** "In private testing with invited streamers."
- **Primary button:** "Request access" (mailto: [contact email]).
- **Image:** the scoreboard screenshot (asset A2).

### 4.3 What viewers see

Short feature list, each with a one-line description:

- **The full scoreboard:** both teams side by side by role, with champions,
  levels, K/D/A, CS, items, trinkets, summoner spells, and runes.
- **Objectives:** team kills, the game clock, Void Grubs, dragons and the
  Dragon Soul, and timers for Void Grubs, Rift Herald, Dragon, Elder Dragon,
  and Baron Nashor.
- **Hover cards:** what an item, rune, summoner spell, or objective does.
- **Player cards:** hover a player for their solo queue rank, mastery on their
  champion, and last 5 games.
- **Always optional:** viewers hover to show it and can close it; nothing
  covers the stream by default.

Use the player card screenshot (asset A3) next to the player card item.

### 4.4 How it works

Three steps, as a simple diagram or numbered row:

1. **The streamer runs Tabline Companion** (Windows) while they play. It reads
   League's own local game data, the same data behind the in-game scoreboard.
2. **Tabline sends the scoreboard to Twitch** for the streamer's channel.
   Updates go out within about a second of anything changing.
3. **Viewers see it in the Tabline extension** by hovering the video. They
   install nothing.

Requirements line: "Needs Windows, League of Legends, and a Twitch channel."

### 4.5 Player data and fair play

This section matters most for Riot's review. Plain, specific, no marketing
tone. Suggested layout: a short intro and a grid of short statements.

Intro: "Tabline follows Riot Games' developer policies. It shows viewers what
the streamer can already see in their own game, plus public, pre-game
information about players."

Statements:

- **Nothing the streamer can't see.** The scoreboard mirrors the streamer's
  in-game scoreboard. No cooldowns, no jungle timers, no hidden information.
- **Official data only.** Player cards use Riot's official APIs for rank,
  mastery, and match history. No made-up ratings or MMR.
- **No custom games.** Custom games are never shown in a player's recent games.
- **Hidden players stay hidden.** Players the game hides are never looked up.
- **Streamers are in control.** Player cards can be turned off in Tabline
  Companion.
- **Short-lived data.** Player card data is kept in memory for up to 30 minutes
  and match data for up to a day. The list of players in a game is deleted when
  the game ends.
- **No ads, no data selling.** Tabline is free.

### 4.6 Get Tabline

- "Tabline is in private testing. Streamers can request access by email."
- Button: "Request access" (mailto: [contact email]).
- Note: "The Twitch extension will be available in Twitch's extension directory
  once it's released."

Do not add a public download link for the companion app yet: the installer
isn't code-signed.

### 4.7 FAQ (optional, short)

- **Do viewers need to install anything?** No. The scoreboard appears in the
  Twitch player.
- **Does it work on mobile?** Twitch doesn't show video overlay extensions on
  mobile, so it's desktop viewers only.
- **Which regions?** All League regions.
- **Does it cost anything?** No.

### 4.8 Footer

- Links: Privacy, Contact ([contact email]).
- Riot's legal notice, exactly as written (required):

  > Tabline isn't endorsed by Riot Games and doesn't reflect the views or
  > opinions of Riot Games or anyone officially involved in producing or
  > managing Riot Games properties. Riot Games, and all associated properties
  > are trademarks or registered trademarks of Riot Games, Inc.

- "Twitch is a trademark of Twitch Interactive, Inc." (plain text, no logo).
- © [year] Tabline.

## 5. Visual direction

- **Mood:** dark, calm, and precise, like the in-game scoreboard. Gold accents
  sparingly.
- **Colors** (from the extension and app):

  | Use | Hex |
  |---|---|
  | Background | `#0b0f16` |
  | Surfaces / cards | `#121926` |
  | Borders | `#1f2a3a` |
  | Primary text | `#e8e6e1` |
  | Secondary text | `#8b95a5` |
  | Accent (gold) | `#c8aa6e` |
  | Blue team | `#3d9cff` |
  | Red team | `#ff4f4f` |

- **Type:** a clean sans-serif (Inter or the system UI font). Headings in the
  light text color, with gold only for small labels or highlights.
- **Imagery:** the real screenshots, shown large with a subtle border and
  shadow. No stock photos.
- **Do not use:** Riot Games, League of Legends, or Twitch logos; official
  League splash art or champion art as decoration; Twitch purple as a brand
  color.

## 6. Privacy page content

Draft for the owner to review; not legal advice. Plain language, short
sections.

- **Who runs Tabline:** [owner name], contact [contact email].
- **What Tabline collects and why:**
  - *Streamers:* signing in to Tabline Companion with Twitch identifies the
    streamer's channel. The sign-in stays on the streamer's PC; Tabline's
    service receives the Twitch user ID and login to create a session that
    lasts up to 12 hours.
  - *Game data:* while a streamer plays, the scoreboard (champions, players'
    in-game names, items, stats, objectives) is sent to viewers of that
    channel. The latest scoreboard is kept until the game ends.
  - *Players in the game:* for player cards, the Riot IDs of the real players
    in the streamer's game are sent to Tabline's service and kept only until the
    game ends. They aren't shown publicly or written to logs. Players the game
    hides are left out.
  - *Riot data:* rank, mastery, and recent games from Riot's API, kept in memory
    for up to 30 minutes (up to a day for match data).
  - *Viewers:* Twitch gives the extension an anonymous token used only to check
    that a request comes from the channel's viewers. Tabline doesn't store it,
    and sets no cookies or analytics.
- **Who else is involved:** Twitch (the extension and sign-in), Riot Games (game
  and player data), Cloudflare (hosts Tabline's service). Links to OP.GG are
  ordinary links to another website.
- **Service logs:** the hosting provider keeps short-term technical logs
  (errors and request information) to keep the service running. They don't
  include players' Riot IDs.
- **Streamer choices:** player cards can be turned off in Tabline Companion, and
  signing out removes the saved sign-in from the PC.
- **Requests and questions:** [contact email].
- **Changes:** "We'll update this page when anything changes. Last updated
  [date]."

## 7. Assets

All in the project's `store-assets` folder:

| ID | File | Use |
|---|---|---|
| A1 | `app-icon-256.png` (and `src/mark.js`, the SVG source) | Logo mark, favicon |
| A2 | `screenshot-1024x768.png` | Hero image: scoreboard with an item card |
| A3 | `screenshot-player-card-1024x768.png` | Player card feature |
| A4 | `discovery-300x200.png` | Social sharing preview image |

The screenshots show a sample match with made-up player names. Keep them that
way; don't show real players' names or Riot IDs on the site.

## 8. Build requirements

- Static site: plain HTML and CSS, little or no JavaScript. No trackers,
  analytics, cookies, or third-party embeds.
- Responsive from 360px wide phones to large desktops; the screenshots scale
  down cleanly.
- Accessible: real headings, alt text on every image, visible keyboard focus,
  text contrast at least WCAG AA.
- Page title "Tabline: the League scoreboard, live on stream"; meta
  description from the hero subheadline; social preview image A4.
- Deploys to Cloudflare Pages (free).

## 9. Placeholders to fill in before publishing

- [contact email] (a dedicated address is better than a personal one)
- [owner name] for the privacy page
- [year] and [date]
- The final web address, which then goes into the Riot application and the
  Twitch extension's privacy policy field

## 10. Done when

- [ ] Both pages are live on a public HTTPS address.
- [ ] Every section above is present, with no placeholder text left.
- [ ] The Riot legal notice appears word for word in the footer of both pages.
- [ ] No Riot, League, or Twitch logos or official art are used.
- [ ] Every claim matches what Tabline does (sections 4.5 and 6).
- [ ] It reads well on a phone and passes a basic accessibility check.
