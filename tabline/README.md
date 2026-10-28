# Tabline — live League of Legends scoreboard for Twitch

**What it is:** a Twitch video-overlay extension. Viewers hover the stream to see both teams, items, runes, objectives and player cards, updated about a second after the game changes. It has four parts: the viewer extension, a Windows companion app for the streamer, a backend on Cloudflare Workers, and a public website.
**My role:** product owner and builder.
**Status:** in Twitch Hosted Test with invited streamers. The Riot production API key is in review.

## Product thinking

- **Three audiences, each with its own job:** streamers (setup must work without a terminal), viewers (install nothing, sign in to nothing), and gatekeepers. Riot and Twitch review the product as it stands, so the [website requirements](website-requirements.md) are written first to pass Riot's review.
- **Success measures defined up front:** Riot approves the key, invited streamers keep running it every stream without reminders, and viewers use the player cards and mention them in chat.
- **Policy work as product work:** before applying for the Riot key, I checked the product against Riot's developer policies and changed what didn't comply. Custom games are out of match history, players hidden by the game are never looked up, streamers can turn player cards off, and player IDs stay out of logs ([application draft](riot-production-key-application.md)).

## Documents

- [Product brief](product-brief.md): users, purpose, success measures, positioning, principles
- [Website requirements](website-requirements.md): goals ranked by priority, audience needs, section-by-section content brief
- [Riot production key application](riot-production-key-application.md): compliance checklist and draft answers
