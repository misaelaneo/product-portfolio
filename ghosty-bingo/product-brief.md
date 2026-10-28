# Product

## Platform

web

## Users

Ghosty's friends at his birthday party on Sat 26 Sep 2026. Most join from a
desktop browser; some join from a phone. They play League of Legends bingo live,
one card per match, and each person writes Ghosty a birthday card. Ghosty plays
too, but writes no card to himself and cannot see the cards until the recap.

## Product Purpose

One room, opened by a direct link, where friends mark bingo squares as a match
happens and leave birthday wishes. At the end the host publishes a shareable
recap: the moments from the night (the squares that landed, the notes and
reactions on them) and every birthday card on one wall. Success is a recap
that Ghosty wants to keep and send around.

## Positioning

A birthday card that is written during a night of games with the same friends,
in the jokes and emotes of those games. The squares and the card wall come from
this group's own night, not from a template.

## Operating Context

- The host opens a room, shares the link, and starts each match by hand.
- Players join with the room code and a display name. There are no accounts; a
  token in the browser is the identity.
- Players mark squares live during a match. They can add notes and reactions
  (five quick emoji plus any 7TV emote) to squares.
- Players write their birthday card any time before the recap is published. The
  card saves as they go and nobody else sees it until the recap.

## Capabilities and Constraints

- Card content: cover colour, font, text, GIFs from Giphy (rating G), and 7TV
  emotes stuck on as stickers, which may hang off the card edge.
- Card text tools the user asked for: font, text colour and size, alignment.
  Text stays in the flow of the card; it is not free-placed.
- The card editor must work with a thumb on a phone and must not show every
  control at once.
- Square text is capped at 40 characters so a 5x5 board fits a 375px phone.
- Ghosty's bingo cards use only squares that are not about him.
- Runs on Cloudflare Workers with one Durable Object per room.

## Brand Commitments

- Name: Ghosty Bingo. Themed on League of Legends and on Ghosty.
- Keep the existing cover colours and GIF support in the card editor.

## Evidence on Hand

- 102 seeded squares in `src/shared/squares.ts`, 47 of them written by the
  user about Ghosty.
- No real photos or testimonials. Do not invent any.

## Product Principles

1. The night's own material leads: squares, notes, emotes and cards written by
   the people in the room.
2. Nothing gets in the way of the match; marking a square is one tap.
3. A card should feel made by hand, not filled in.
4. Desktop and phone are both first-class.
