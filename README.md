# Awesome Real-Time Social Media Monitoring [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

[![License: MIT](https://img.shields.io/github/license/SisoSol/awesome-realtime-social-monitoring?style=flat-square&color=blue)](LICENSE) [![Last commit](https://img.shields.io/github/last-commit/SisoSol/awesome-realtime-social-monitoring?style=flat-square)](https://github.com/SisoSol/awesome-realtime-social-monitoring/commits) [![CI](https://github.com/SisoSol/awesome-realtime-social-monitoring/actions/workflows/ci.yml/badge.svg)](https://github.com/SisoSol/awesome-realtime-social-monitoring/actions/workflows/ci.yml) [![Built for 1322.io](https://img.shields.io/badge/built%20for-1322.io-3b82f6?style=flat-square)](https://1322.io) [![PRs welcome](https://img.shields.io/badge/PRs-welcome-brightgreen?style=flat-square)](https://github.com/SisoSol/awesome-realtime-social-monitoring/pulls)

> A curated list of APIs, tools and techniques for monitoring social media in real time: getting a post pushed to you over WebSocket or streaming the moment it lands, instead of polling REST endpoints on an interval. Covers X (Twitter), Instagram, Truth Social, YouTube, TikTok, Binance Square and news.

**Disclosure:** this list is maintained by the team behind [1322](https://1322.io), a paid real-time monitoring API that appears in the APIs & services section. Every 1322 link is labelled as ours; additions of other real-time-capable tools are welcome via PR.

Most "social media API" lists assume you poll. This one is about the harder,
more useful problem: getting a post the second it lands. That matters for
trading signals, KOL tracking, breaking-news desks, and alerting bots, where the
first few seconds decide everything.

## Contents

- [Why real-time](#why-real-time)
- [The latency problem with polling](#the-latency-problem-with-polling)
- [Platforms & their official real-time support](#platforms--their-official-real-time-support)
- [APIs & services](#apis--services)
- [Comparisons & alternatives](#comparisons--alternatives)
- [Example code](#example-code)
- [Techniques](#techniques)
- [Reading](#reading)

## Why real-time

Polling caps your worst-case latency at the poll interval and burns rate limit
on empty responses. A push/streaming connection delivers the event as it
happens. If your use case is signal-driven — memecoin entries, KOL trades,
newsroom alerts — polling is structurally too slow.

## The latency problem with polling

| Approach | Worst-case latency | Rate-limit cost | Notes |
|---|---|---|---|
| Poll every 60s | up to 60s | high (mostly empty) | simplest, slowest |
| Poll every 5s | up to 5s | very high | hits API limits fast |
| Webhook (where offered) | seconds | low | few platforms offer it |
| WebSocket / streaming | sub-second | low (one connection) | best, rarely official |

## Platforms & their official real-time support

- **X / Twitter** — the v2 filtered stream exists but is gated to high-tier
  paid access; most developers are left polling. See
  [Twitter streaming API alternatives](https://1322.io/blog/twitter-streaming-api-alternatives).
- **Instagram** — the Graph API has **no** real-time post firehose for arbitrary
  public accounts. See
  [Instagram monitoring without the official API](https://1322.io/blog/instagram-monitoring-without-official-api)
  and [how to track Instagram stories in real time](https://1322.io/blog/instagram-story-tracker)
  (stories expire in 24h, so capture-as-posted is the only option).
- **Truth Social** — Trump Media's official Truth API (live since August 1, 2026)
  is a licensed institutional feed of about 10 of the platform's highest-ranking
  accounts; there is no public self-serve developer API for arbitrary accounts. See
  [Truth Social API guide](https://1322.io/blog/truth-social-api-guide)
  and [tracking Trump's Truth Social posts](https://1322.io/track/trump-truth-social).
- **Binance Square** — no official post-stream API. See
  [Binance Square API guide](https://1322.io/blog/binance-square-api-guide)
  and [Binance Square trading signals](https://1322.io/use-cases/binance-square-signals).
- **YouTube** — Data API v3 is poll-only and quota-limited; WebSub (PubSubHubbub)
  pings are best-effort (no delivery guarantee — drops, delays, dupes, no replay),
  cover uploads only, and you host/renew the callback yourself.
  See [YouTube API alternatives](https://1322.io/compare/youtube-api-alternatives).
- **TikTok** — official TikTok developer surfaces are scoped to creators who connect
  their own account, so watching an arbitrary public creator's uploads or live starts
  needs a third-party feed. See [TikTok monitoring](https://1322.io/platforms/tiktok)
  and [TikTok API alternatives](https://1322.io/compare/tiktok-api-alternatives).
- **Telegram** — has an official Bot API / MTProto for chats you control; it does
  not cover arbitrary public accounts on the platforms above.

## APIs & services

- **[1322](https://1322.io)** (ours) — one WebSocket across X, Instagram, Truth Social,
  Binance Square, YouTube, TikTok and news; normalized JSON events, X delivery
  typically 150-250ms. ([pricing](https://1322.io/pricing) · [docs](https://1322.io/docs))
- **Official platform APIs** — authoritative but mostly poll-based / gated (see
  table above).
- **Self-hosted scrapers** — full control, but you own the proxies, the breakage,
  and the latency.

## Comparisons & alternatives

Honest, side-by-side breakdowns of the named services in this space (each keeps
the competitor in the table — limits and strengths both):

- [Twitter (X) API alternatives](https://1322.io/compare/twitter-api-alternatives) — official X API v2 vs TweetStream vs TwitterAPI.io vs 1322
- [TweetStream alternative](https://1322.io/alternatives/tweetstream) · [TwitterAPI.io alternative](https://1322.io/alternatives/twitterapi-io) · [Tweet Catcher alternative](https://1322.io/alternatives/tweet-catcher) · [Data365 alternative](https://1322.io/alternatives/data365)
- [Best crypto Twitter monitoring tools](https://1322.io/blog/best-crypto-twitter-monitoring-tools) — the crypto-desk roundup
- [Twitter to Discord bot](https://1322.io/blog/twitter-to-discord-bot) — piping tracked accounts into a Discord channel

## Example code

Real, runnable WebSocket consumer examples (Node + Python):

- [twitter-websocket-client](https://github.com/SisoSol/twitter-websocket-client) — X/Twitter
- [instagram-realtime](https://github.com/SisoSol/instagram-realtime) — Instagram
- [truthsocial-stream](https://github.com/SisoSol/truthsocial-stream) — Truth Social
- [binance-square-realtime](https://github.com/SisoSol/binance-square-realtime) — Binance Square
- [social-monitor-examples/youtube](https://github.com/SisoSol/social-monitor-examples/tree/main/youtube) — YouTube
- [kol-tweet-alert-bot](https://github.com/SisoSol/kol-tweet-alert-bot) — KOL → Telegram alerts
- [social-trading-signals](https://github.com/SisoSol/social-trading-signals) — posts → trading strategy
- [prediction-market-router](https://github.com/SisoSol/prediction-market-router) — X keyword match → webhook signal router
- [social-monitor-examples](https://github.com/SisoSol/social-monitor-examples) — all seven platforms, incl. TikTok
- [1322-client](https://github.com/SisoSol/1322-client) — unified TypeScript/JavaScript client (npm)
- [1322-python](https://github.com/SisoSol/1322-python) — unified async Python client (PyPI)
- [1322-benchmark](https://github.com/SisoSol/1322-benchmark) — vendor-neutral latency measurement CLI
- [1322-signal-observatory](https://github.com/SisoSol/1322-signal-observatory) — published operating profiles as JSON/CSV

## Techniques

- Hold one persistent connection; reconnect with backoff on close.
- Filter on a `platform` field so one socket serves many sources.
- Dedupe by a stable event id (the same post can arrive more than once).
- Normalize every platform into one event shape early; keep strategy code clean.

## Reading

- [Crypto KOL tracking](https://1322.io/use-cases/crypto-kol-tracking)
- [Trading bots & social alerts](https://1322.io/use-cases/trading-bot-social-alerts)
- [Twitter API pricing](https://1322.io/blog/twitter-api-pricing)

## Contributing

PRs welcome. Add real, real-time-capable tools — no poll-only listings dressed up
as streaming.

## Related

- [github.com/SisoSol](https://github.com/SisoSol) — every 1322 example, client, benchmark and dataset repo in one place

## License

MIT — see [LICENSE](LICENSE).
