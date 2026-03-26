# Github Explorer

A mobile-first app for discovering and exploring GitHub repositories — designed for people who use GitHub primarily to find interesting projects, not to develop on their phone.

## Concept

The native GitHub mobile app's Explore and Search features are limited — showing only 10–20 results with no pagination or infinite scroll. This app aims to solve that by providing:

### Core Features

1. **Enhanced Search & Explore** — Search GitHub repos by keyword (e.g., "MCP", "AI agents") with deep pagination (500+ results instead of 20), focused exclusively on repositories (no issues, PRs, or people clutter).

2. **Swipe-to-Star ("Tinder Mode")** — Browse search results one repo at a time with a swipe interface: swipe right to star, swipe left to skip. Optimized for casual mobile browsing.

3. **Star Management** — Review starred repos, de-star them inline (no need to click into each repo), and keep a history of de-starred repos in a local backend so nothing is permanently lost.

4. **Smart Recommendations (Stretch Goal)** — Learn from starring patterns to surface more relevant repos, filter out previously seen repos, and improve discovery over time.

## Motivation

> "It's usually in the long tail of these keywords that I end up finding good things. I'd happily go through a thousand MCP projects paginated to find good stuff to star."

This is a tool for the GitHub power-browser — someone who explores GitHub to stay current on AI, open source tools, and emerging projects, and wants to do it from their phone while relaxing.

## Technical Notes

- Uses the [GitHub REST API](https://docs.github.com/en/rest) for search, starring, and repo metadata
- Mobile-first (Android primary target)
- Requires a backend/database to persist de-starred repo history
- GitHub API rate limits apply (authenticated users get 5,000 requests/hour)

## Project Origin

This project was conceived via voice note on 26/03/2026. See [`transcript.md`](transcript.md) for the full ideation transcript.

## License

MIT
