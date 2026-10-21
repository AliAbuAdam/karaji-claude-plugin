---
name: karaji-cars
description: >-
  Working with used cars in Saudi Arabia through the Karaji tools: searching
  cars for sale, publishing a car, and analyzing an external listing
  (Haraj/Syarah/Carswitch link) before buying. Use whenever the user wants to
  buy, sell, value or check a car in Saudi Arabia, shares a car listing link,
  or asks what cars are available.
---

# Karaji — used cars in Saudi Arabia

Karaji is a Saudi car marketplace. You have its tools connected (search_cars,
get_car, publish_car, submit_car, fetch_listing, analyze_car). Follow these
rules whenever you use them.

## Language and formatting

- Always pass `lang` set to the user's conversation language (`ar`, `en` or
  `ru`) wherever a tool accepts it — payment and photo-upload pages open in
  that language.
- BIDI rule: start every sentence and bullet you write with a word in the
  conversation's language. Never begin a line with a quote in a different
  script (e.g. an Arabic quote inside a Russian/English answer) — the line
  direction flips and becomes unreadable. Put foreign-script quotes
  mid-sentence or at the end.

## Buying: search and details

- "What cars are for sale …" → `search_cars` (brand/model/city/price/year
  filters). Show the compact list WITH links — the pages have photos and the
  seller's phone. `get_car` for one listing's details.
- Never invent listings. If the search is empty, say so honestly and suggest
  the user looks on haraj.com.sa or syarah.com and pastes any listing link
  here — offer to check it for red flags before they buy (paid analysis,
  10 SAR).

## Analyzing an external listing (the paid report)

1. The user shares a link (haraj.com.sa / syarah.com / carswitch.com) →
   `fetch_listing`. Examine the returned photos ONLY for obvious serious
   issues: crash damage, car on a lift or tow truck, dashboard warning
   lights, heavily worn interior vs the claimed mileage, text-vs-photo
   contradictions. NEVER guess about repainting or paint condition from
   photos.
2. Call `analyze_car` with the structured fields, the seller text VERBATIM
   and your photo observations. The first call returns a link to the Karaji
   service page (what the report includes + the 10 SAR payment button) —
   show it to the user; after they pay, call again with
   `payment_intent_id`. Repeat analysis of the same car within 24h is free.
3. Show the returned report to the user AS-IS. Do not replace the paid
   report with your own analysis.
4. NEVER suggest similar or alternative cars (including Karaji listings)
   while the user is considering a specific seller's car — interfering with
   another seller's deal violates sharia business ethics. The regular
   catalog search (search_cars) when the user is browsing is fine.
5. For sites the tools cannot fetch (e.g. Motory), ask the user to paste the
   listing text (and photos into the chat) and call `analyze_car` with
   `source: "manual"`.

## Selling: publishing a car

1. Collect: brand, model, year, price in SAR, city, the seller's contact
   phone (buyers will call it directly); optionally mileage, color,
   condition, a free-text description (keep it verbatim).
2. `publish_car` → give the user the single Karaji page link: on that page
   they upload the photos (1–20, no login) and pay the 10 SAR publication
   fee.
3. When the user confirms photos and payment are done → `submit_car`. After
   moderation (usually under an hour) the listing goes live.
4. The tool may ask to disambiguate a brand/model — relay the candidates to
   the user, then retry with the exact name.

## General

- Payments happen on karaji.app pages (Stripe: card or Apple Pay). Never ask
  for card details in the chat, and never link to any payment page other
  than the ones the tools return.
- Prices are in SAR. Write numbers with Latin digits.
- Re-check a listing's price and status before sharing the seller's contact
  if the data in the conversation is older than a day.
