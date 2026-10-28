# Product

## Platform

web

## Users

**Streamers (primary).** Anyone who streams League of Legends on Twitch, at any
channel size, in any League region. They play on Windows, run the Tabline
Companion app alongside the game, and mostly queue ranked solo/duo, ranked flex,
Summoner's Rift draft, quickplay, and ARAM or ARAM Mayhem. They are not
developers: setup has to work without a terminal.

**Viewers (secondary, served through the streamer).** Desktop Twitch viewers who
want to follow the game in progress without waiting for the streamer to open the
in-game scoreboard. They install nothing and sign in to nothing. Twitch does not
show video-overlay extensions on mobile, so mobile viewers are out of reach.

**Reviewers (gatekeepers).** Riot Games developer relations, who decide on the
production API key, and Twitch's extension review. They read the public website
and judge the product as it stands.

## Product Purpose

Tabline puts a live League of Legends scoreboard over a streamer's Twitch video.
Viewers hover the video to see both teams by role with champions, levels, K/D/A,
CS, items, trinkets, summoner spells and runes, plus objectives, timers,
explanatory hover cards, and player cards with rank, champion mastery and recent
games.

Success over the next few months, in the streamer's own words:

1. Riot approves the production API key, so player cards keep working without
   the developer key that expires every 24 hours.
2. Invited streamers keep running it every stream, without being reminded.
3. Viewers notice it: they open the scoreboard, use the player cards, and
   mention it in chat.

Public listing in Twitch's extension directory is wanted eventually but is not a
goal this phase is shaped around.

## Positioning

The scoreboard comes from the streamer's own game. The companion app reads
League's local Live Client Data API on the streamer's PC and sends the board out,
so viewers see the same state the streamer sees on TAB, updated within about a
second of anything changing. Not a delayed spectator feed, not a third-party
estimate, and nothing the streamer could not already see.

Player cards use Riot's official APIs only, for information that exists before
the game starts.

## Operating Context

- The streamer plays on Windows with League open and Tabline Companion running
  in the tray. The app signs in to Twitch once and then runs unattended.
- Viewers watch on desktop Twitch and hover the video player. A "Scoreboard on
  hover" switch at the top-left of the video turns the board off for viewers who
  hover for the player's own controls, and the choice is remembered.
- The extension is in Hosted Test on Twitch; the installer is shared privately
  with trusted streamers and is not code-signed.
- The backend runs on Cloudflare Workers (`api.tabline.workers.dev`) on the free
  plan, which shapes how often data is sent.
- The Riot developer key expires every 24 hours until the production key is
  granted.

## Capabilities and Constraints

**Works today**

- Live scoreboard, objectives and objective timers, item/rune/spell/objective
  hover cards, and player hover cards (rank, mastery, last 5 games).
- Streamers can turn player cards off in the companion app.
- An optional "Today" card shows the streamer's session: record, last games and
  averages, from the games they finish. It is off by default, needs no Riot key,
  and holds nothing about other players. It keeps a separate record per account, so a
  smurf's games don't count toward the main's. The win or loss relies on an
  undocumented end-of-game event in League's local data, so a game's result may
  be unknown; rank and LP wait for the production key.
- An optional post-game vote: for two minutes after a game ends, viewers can give
  a thumbs-up to any of the other nine players, on the streamer's team or the
  other, on cards that mirror League's own end-of-game screen. It is off by
  default and needs no Riot key. The round and its votes are deleted within an
  hour; nothing about a player is kept after that, and there is no score or
  ranking.
- A click on a player's name opens their OP.GG profile. That needs no Riot API
  key (the link is built from the Riot ID and region), so it keeps working while
  cards are off for want of a key. Players the game hides have no link.
- Extension in Hosted Test; companion app 0.3.0; backend deployed.

**Fixed limits from the game and the platforms**

- CS updates in steps of 10. Riot's live game data reports it that way; it
  cannot be made exact.
- No ability cooldowns or cast events: League's local API does not provide them.
- Twitch PubSub allows 5 KB per message and 100 messages per minute, and does
  not show video-overlay extensions on mobile.
- The Cloudflare free plan sets the update budget, so sends happen on change
  rather than on a fast fixed schedule.

**True today, but not declared permanent**

- Free for streamers and viewers: no ads, subscriptions, paid features, or data
  sold on. The extension has no donation, bug-report or website links: Twitch
  review (Oct 2026) rejected the Ko-fi and tabline.pages.dev links under
  guideline 4.5, since the site asks for donations and an install. Only OP.GG
  and the credits' twitch.tv links leave the overlay. Donations, if any, live
  on the website.
- No cookies, analytics or viewer tracking anywhere.
- Windows only.

**Undecided**

- Whether and when to submit for public Twitch release.
- Code signing, a public download page, and auto-update.
- Website address: a free `*.pages.dev` name or a custom domain.
- A known discrepancy in Void Grub counts, parked deliberately.

## Brand Commitments

- **Name and mark:** "Tabline" and "Tabline Companion". The mark is original art:
  a scoreboard with blue rows left, red rows right, split by a gold divider.
- **Riot's rules come first.** This is binding, above visual or product
  preference: nothing the streamer cannot already see, no hidden players, no
  custom games in match history, no invented ratings or MMR, and Riot's legal
  notice wherever Tabline appears, word for word.
- **No borrowed branding.** No Riot, League of Legends or Twitch logos, and no
  official League art used as decoration on public pages.
- **Voice:** plain and specific. Short sentences, concrete facts, no marketing
  inflation. Claims must match what the software actually does, because Riot and
  Twitch read them.

## Evidence on Hand

- Working product: extension in Hosted Test, backend deployed, companion
  installer built (`companion/dist/`).
- Screenshots of the real thing: `store-assets/screenshot-1024x768.png` and
  `store-assets/screenshot-player-card-1024x768.png`. They show a sample match
  with invented player names, which must stay that way.
- Hero video: `website/public/assets/tabline-showreel.mp4` (15 s, 1080p60, loops),
  a motion-graphics showreel rendered by `showreel/` from the real viewer with the
  website's sample match, over 1.7 s of gameplay from a live test stream (cropped to
  the game). It replaced the earlier screen recording (`tabline-demo.mp4`, which
  showed a real third party's Riot ID and OP.GG's ads) on 2026-10-15. The board
  panel's placeholder image, `tabline-demo-poster.jpg`, is still a frame of that
  recording.
- Brand and store assets: `store-assets/` (logo, icons, discovery image) with
  sources in `store-assets/src/`.
- Written product record: `README.md`, `docs/website-requirements.md`,
  `docs/riot-production-key-application.md`.
- **Absent, and not to be invented:** user numbers, testimonials, partner or
  streamer names, press coverage, benchmarks, ratings, or launch dates.

## Product Principles

1. **Riot's rules decide it.** When a design idea and Riot's developer policy
   disagree, the policy wins, even at a cost to the product.
2. **Only what the streamer already sees.** No hidden information, no advantage,
   nothing derived that the game itself withholds.
3. **The viewer decides.** The board appears on hover, closes on demand, and
   never takes the stream over.
4. **No terminals.** A streamer sets it up and forgets it; anything that needs
   technical skill is a defect.
5. **Say only what is true.** No borrowed credibility, invented proof, or claims
   the software cannot back up.

## Accessibility & Inclusion

No user-specific need has been established. The public website targets WCAG AA
contrast, real headings, alt text, and visible keyboard focus. The extension
overlay is mouse-driven by nature, and Twitch's own review rules bar keyboard
shortcuts in extensions.
