# Github Explorer — Project Plan

**Created:** 26/03/2026
**Source:** Voice note ideation session ([`../26_03_2026_15_58.m4a`](../26_03_2026_15_58.m4a) · [transcript](../transcript.md))

---

## Problem Statement

The native GitHub mobile app (Android) is poorly suited for **discovery-oriented users** — people who browse GitHub to find interesting projects, tools, and emerging tech rather than to write code on their phone.

Key pain points identified in the voice note:

1. **Explore is shallow** — only shows 10–20 trending repos (today/week/month) with no way to load more or infinitely scroll.
2. **Search is cluttered** — results mix code, issues, PRs, people, and orgs when the user only wants repositories.
3. **Pagination is missing** — no way to go beyond the first page of results; the "long tail" where interesting projects live is inaccessible.
4. **Star management is clunky** — de-starring requires clicking into each repo individually; no batch review workflow.
5. **No star history** — once a repo is de-starred, it's gone forever; no way to revisit previously-reviewed repos.

## Target User

Someone who:
- Uses GitHub primarily to **explore and discover**, not to develop on mobile
- Searches keywords like "MCP", "AI agents", "whisper speech" to stay current
- Stars repos as bookmarks for later review on desktop
- Wants to review/triage their star list periodically (every few days)
- Is typically on their phone, relaxing, in a browsing/exploration mindset

## Feature Plan

### Phase 1: Core (MVP)

#### 1.1 — Enhanced Search
- Single search bar, repos-only (no issues/PRs/people/orgs noise)
- Deep pagination — at least 500 results per query (vs GitHub app's ~20)
- Sort by: stars, recently updated, recently created, best match
- Filter by: language, date range, minimum stars

#### 1.2 — Enhanced Explore
- Trending repos with significantly more results than the native app
- Category/topic browsing
- Infinite scroll or explicit pagination controls

#### 1.3 — Star Management
- View all starred repos in a scrollable list
- Inline de-star button (no need to navigate into the repo)
- Pull-to-refresh

#### 1.4 — De-Star Archive
- Local database table storing every de-starred repo (name, URL, description, date starred, date de-starred)
- Searchable/browsable archive so nothing is permanently lost
- "Re-star" option from archive

### Phase 2: Swipe Interface ("Tinder Mode")

#### 2.1 — Swipe Discovery
- Search results presented one repo at a time (card UI)
- Card shows: repo name, description, language, stars, last updated, top topics
- Swipe right → star the repo
- Swipe left → skip (optionally save to "seen" list)
- Swipe up → open in browser for deeper look

#### 2.2 — Session Tracking
- Track which repos have been shown so they aren't repeated
- Session stats: "You reviewed 47 repos, starred 8"

### Phase 3: Smart Recommendations (Stretch)

#### 3.1 — Learning from Behaviour
- Analyse starring patterns (languages, topics, keywords, repo characteristics)
- Build a simple preference model
- Surface repos matching the user's interests that they haven't seen

#### 3.2 — "For You" Feed
- Personalised repo recommendations based on starring history
- Mix of trending + personalised results
- Deduplicated against already-seen repos

## Technical Architecture

### Platform
- **Primary:** Android (mobile-first, the main use case from the voice note)
- **Secondary:** Web app (could serve as both desktop companion and PWA)

### GitHub API
- [GitHub REST API v3](https://docs.github.com/en/rest) — search, starring, repo metadata
- Authentication: GitHub OAuth / personal access token
- Rate limits: 5,000 requests/hour (authenticated)
- Search API: 30 requests/minute (authenticated)
- Pagination: up to 1,000 results per search query (GitHub API hard limit)

### Backend Requirements
- Database for de-star archive and seen-repo tracking
- Options: SQLite (local-only), or a lightweight cloud DB if cross-device sync is desired
- User preferences / recommendation model storage (Phase 3)

### Key API Endpoints
| Feature | Endpoint |
|---------|----------|
| Search repos | `GET /search/repositories?q={query}` |
| List starred repos | `GET /user/starred` |
| Star a repo | `PUT /user/starred/{owner}/{repo}` |
| Unstar a repo | `DELETE /user/starred/{owner}/{repo}` |
| Trending (unofficial) | GitHub Trending page scraping or third-party API |

### Data Model (De-Star Archive)

```
starred_repos
├── id (primary key)
├── github_repo_id (integer)
├── owner (string)
├── repo_name (string)
├── description (text)
├── url (string)
├── language (string)
├── stars_count (integer)
├── topics (json array)
├── date_starred (datetime)
├── date_destarred (datetime, nullable)
├── status (enum: starred | destarred | archived)
└── notes (text, optional user notes)
```

## Open Questions

1. **Android native vs cross-platform?** — React Native / Flutter would allow web + mobile from one codebase
2. **Cloud sync?** — Should de-star archive sync across devices, or is local-only sufficient?
3. **Trending API** — GitHub has no official trending API; need to scrape or use a third-party source
4. **Recommendation engine** — Simple collaborative filtering, or just keyword/topic matching?
5. **Offline support** — Cache search results and star list for offline browsing?

## Priority Order

1. Enhanced search with deep pagination (highest impact, lowest complexity)
2. Star management with inline de-star
3. De-star archive (backend/database)
4. Swipe interface
5. Smart recommendations

## Voice Note Context

The full ideation was captured in a voice note recorded on 26/03/2026. The audio file is at [`../26_03_2026_15_58.m4a`](../26_03_2026_15_58.m4a) and the transcript is at [`../transcript.md`](../transcript.md). Key quotes:

> "It's usually in the long tail of these keywords that I end up finding good things. I'd happily go through a thousand MCP projects paginated to find good stuff to star."

> "I'm in a mood where I'd love to really dig into recent browser projects on GitHub and I don't find with the native GitHub app that I can do that very well."

> "A GitHub integrated app designed primarily for those who want to explore what's out there, as opposed to for using it for the primary intended means of code version controlling."
