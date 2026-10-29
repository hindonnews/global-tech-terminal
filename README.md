# TECH//WIRE — Global Tech News Terminal

Next.js App Router RSS aggregation dashboard. English, global technology, Bloomberg-inspired dark UI.

## Quick start

```bash
npm install
npm run dev
```

Open http://localhost:3000. Deploy by importing this repository in Vercel (Framework Preset: Next.js).

## Architecture

- `lib/news.ts` fetches six RSS feeds concurrently, validates article URLs, classifies headlines, removes duplicate titles and sorts by publish time.
- `unstable_cache` caches the aggregated result for 300 seconds. Browser polls `/api/news` every 60 seconds.
- `app/api/news/route.ts` is a server-side RSS proxy; publishers are not fetched by each visitor's browser.
- `app/page.tsx` displays headlines, categories, search, status and publisher attribution.
- No database, API key, or Vercel cron is needed.

## Notes

- This is near-live, not push-based real-time: updates depend on publishers' RSS schedules and the 5-minute cache.
- Feeds can block requests or change URLs. Failed feeds are shown in the dashboard.
- Only headlines and short RSS excerpts are displayed, with original article links. Check each publisher's terms before public/commercial deployment. TechCrunch in particular restricts modification and advertising in its RSS feed.
- Feed requests are parallel with an 8-second timeout; production plan/function limits may affect reliability.
- For historical archive, alerts, and reliable ingestion independent of traffic, add a database and a separate scheduled worker later.
