# Ghosty Bingo — a side project with a fixed deadline

A real-time web app for a friend's birthday game night (26 Sep 2026). Friends join one room from a link and mark League of Legends bingo squares live during each match. Each of them also writes a birthday card. At the end, the host publishes a recap that collects the night's moments and the wall of cards.

**Why it's here:** a small product with a hard deadline and one clear success measure: a recap the birthday person wants to keep and share. I scoped it around that, for example a card editor that works with a thumb on a phone and square text short enough for a 5×5 board on a 375px screen.

**Built with:** React, Cloudflare Workers, and one Durable Object per room, so live hit counts stay correct without extra coordination.

- [Product brief](product-brief.md)
