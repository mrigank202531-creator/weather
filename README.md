# Climate — weather dashboard

Dark, editorial weather dashboard. Search a city or tap a chip, read the weather as a mood, and inspect the raw and formatted WeatherAPI response.

- Frontend: HTML, CSS, vanilla JS (ES modules) in `public/`
- Backend: Vercel serverless function `api/weather.js` (proxy; key stays server-side)
- Mood engine: `public/js/mood.js` (pure function, documented priority order)

## Run / deploy

```bash
npm i -g vercel
vercel env add WEATHERAPI_KEY production   # paste your key when prompted
vercel --prod
```

Local dev: `WEATHERAPI_KEY=... vercel dev`

## Mood priority (first match wins)

1. Scorching: temp >= 38
2. Freezing: temp <= 5
3. Muggy: humidity >= 80 and temp >= 28
4. Windy: wind >= 30 km/h
5. Pleasant: temp 20-30, humidity <= 60, wind <= 20
6. Comfortable: temp 15-30, humidity <= 75, wind <= 30
7. Mixed: everything else

## Tests

`npm test` checks mood thresholds and the proxy's validation, error forwarding and key handling (mocked upstream).

## Security notes

- Never commit the API key; `.env*` is git-ignored.
- The proxy validates input, applies a 8s upstream timeout, and a best-effort per-instance rate limit.
