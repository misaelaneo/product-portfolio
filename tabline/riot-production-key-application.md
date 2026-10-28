# Tabline: Riot production key application (draft)

Draft answers for registering Tabline on the Riot Developer Portal
(https://developer.riotgames.com) and applying for a production API key.
Written against Riot's General Policies (last updated May 29, 2025) and the
League of Legends developer page, checked September 2026.

## Before you submit

Riot reviews the product as it is, so these need to be true first. Each one
is stated as done in the answers below.

- [x] **Leave custom games out of hover cards.** Riot: "Products may not
  publicly display a player's match history from the custom match queue."
  Today the last 5 games include custom games.
- [x] **Don't look up hidden players.** Riot: "Products cannot identify or
  analyze players who are deliberately hidden by the game." Hover cards must
  skip anyone the game doesn't show by name (for example streamer mode), and
  this needs checking in a real game.
- [x] **Let streamers turn hover cards off** in Tabline Companion, for
  streamers who don't want lookups on their stream.
- [x] **Keep Riot IDs out of the backend's logs.** `backend/src/app.js` logs the
  player's Riot ID when a lookup fails, and Cloudflare keeps those logs.
- [ ] **Have a public page** describing Tabline, even a simple one. Riot:
  "If your website isn't complete, we're unlikely to approve your product."
- [ ] Fill in the bracketed parts: your name, contact email, and the page URL.

## Product name

Tabline

## Product type and game

A Twitch extension for League of Legends, with a desktop companion app for
streamers (Windows).

## Short description

Tabline puts a live League of Legends scoreboard over a streamer's Twitch
video. Viewers hover the video to see both teams' champions, items, runes,
K/D/A, CS, and objective timers, the same information as the streamer's
in-game TAB scoreboard, and can hover a player for their rank, mastery on
their champion, and recent games.

## Detailed description

Tabline is for viewers who want to follow a League game on stream without
waiting for the streamer to open the scoreboard.

**How it works.** The streamer installs Tabline Companion. While they play,
it reads the game's own local Live Client Data API
(`https://127.0.0.1:2999/liveclientdata/allgamedata`) and sends a compact
scoreboard to Tabline's backend, which broadcasts it to the streamer's viewers
through Twitch. Viewers see it in the Twitch extension when they hover the
video. Viewers install nothing.

**What viewers see.**

- The scoreboard as the streamer sees it on the in-game TAB screen: both
  teams by role, champions, levels, K/D/A, CS, items, trinkets, summoner
  spells, and keystone runes.
- Team kills, the game clock, Void Grubs, dragons and the Dragon Soul, and
  objective timers, worked out from the game's own event feed.
- Hover cards explaining items, runes, summoner spells, and objectives, from
  Data Dragon.
- Player hover cards: a player's solo queue rank and win rate, their mastery
  on the champion they're playing, and their last 5 games (champion, win or
  loss, K/D/A). These use the Riot API and are the reason for this application.
- Links to a player's OP.GG profile and to the champion's page on OP.GG.

**What it doesn't do.** Tabline shows nothing the streamer can't already see
in their own game client. It has no cooldown timers, no jungle camp timers, no
fog-of-war information, and nothing that suggests decisions. It doesn't run
during champion select, doesn't calculate MMR or any ranking of its own, and
doesn't show win rates for Arena augments or items.

## Riot APIs used and why

All calls are made by Tabline's backend on Cloudflare Workers, never from a
viewer's browser or the streamer's PC. The API key is stored as a server
secret and never shipped in the extension or the companion app.

| API | Endpoint | Used for |
|---|---|---|
| ACCOUNT-V1 | `/riot/account/v1/accounts/by-riot-id/{gameName}/{tagLine}` | Finding a player's PUUID from the Riot ID shown in the game |
| LEAGUE-V4 | `/lol/league/v4/entries/by-puuid/{puuid}` | Solo queue rank, LP, wins and losses |
| CHAMPION-MASTERY-V4 | `/lol/champion-mastery/v4/champion-masteries/by-puuid/{puuid}/by-champion/{championId}` | Mastery on the champion being played |
| MATCH-V5 | `/lol/match/v5/matches/by-puuid/{puuid}/ids` and `/lol/match/v5/matches/{matchId}` | The last 5 games (champion, result, K/D/A), excluding custom games |

Static data (item, rune, spell, and champion names and icons) comes from Data
Dragon. The scoreboard itself uses only the local Live Client Data API, which
needs no key.

## Game integrity and policy compliance

- **No unfair advantage.** The scoreboard mirrors the streamer's own in-game
  TAB screen and event feed. It adds no information the streamer doesn't
  already have, and it is shown to viewers, not used to play.
- **Static, pre-game data only.** Hover cards show rank, mastery, and past
  games, all available before the game starts ("game overlays that provide
  static data that is available prior to the game"). Nothing about the
  current game session comes from the Riot API.
- **No hidden players.** Players are looked up only by the Riot ID the game
  client shows the streamer. Players the game hides, and bots, are never looked
  up.
- **No custom match history.** Games from the custom match queue are left out
  of hover cards.
- **No alternative rankings.** Tabline shows official ranked data only; it
  has no MMR or ELO estimates.
- **Streamer control.** Streamers can turn player hover cards off in Tabline
  Companion.
- **Legal notice.** The scoreboard and the companion app both show: "Tabline
  isn't endorsed by Riot Games and doesn't reflect the views or opinions of
  Riot Games or anyone officially involved in producing or managing Riot Games
  properties. Riot Games, and all associated properties are trademarks or
  registered trademarks of Riot Games, Inc."
- **Naming.** The product name uses no Riot trademarks.

## Player privacy and data handling

- The list of players in the streamer's game (Riot IDs and champions) goes
  from the companion to the backend and stays there. It isn't broadcast to
  viewers and isn't written to logs.
- Hover card data is cached in memory to limit Riot API calls: a player's card
  for 30 minutes (long enough for one game), and a finished match's data for up
  to 24 hours. Nothing from the Riot API is stored in a database or kept after
  the cache expires.
- The roster and the latest scoreboard for a channel are deleted when the
  streamer's game ends. Cached cards expire on their own.
- Viewers are anonymous to Tabline. The backend accepts hover card requests
  only with Twitch's signed extension token for the channel being watched.

## Rate limits and expected usage

- **Current scale:** a small group of trusted streamers in testing.
- **Per game, per channel:** at most about 9 calls per real player (account,
  rank, mastery, match list, and up to 5 match details), fewer when matches
  are already cached. Bots are skipped. A 10-player game is under 90 calls,
  made once at the start of the game and spread out in the background, not per
  viewer.
- **Protection:** every Riot call for all channels goes through one shared rate
  limiter on the backend, which waits for capacity rather than exceeding it,
  and Riot's 429 responses are respected.
- The default production limits (500 requests every 10 seconds and 30,000
  every 10 minutes, per region) are well above expected use.
- **Regions:** all League platforms (NA, EUW, EUNE, KR, BR, LAN, LAS, OCE, TR,
  RU, JP, ME, PH, SG, TH, TW, VN), set by each streamer.

## Monetization

Free for streamers and viewers. No ads, subscriptions, or paid features.

## Working prototype

- Twitch extension "Tabline", in Hosted Test.
- Tabline Companion 0.2.0 for Windows.
- Backend at https://api.tabline.workers.dev.
- Public page: [URL]

Screenshots of the scoreboard and a player hover card are in the project's
`store-assets` folder (`screenshot-1024x768.png` and
`screenshot-player-card-1024x768.png`).

## Contact

- Developer: [your name]
- Email: [contact email]
