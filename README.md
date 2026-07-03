Dead Archive — Project Summary
I'm building Dead Archive (deadarchives.org), a single-file HTML app (dead-archive.html) + large JSON data file (dead_data.json) hosted on GitHub Pages (repo: iancarlson33/dead-archive). It's a Grateful Dead live-show archive and player styled as a vintage Fillmore/Avalon poster (Playfair Display + IBM Plex Mono, gold/red/cream/black palette).
Tech stack: Vanilla JS, single HTML file, no framework, no backend. Audio streams live from archive.org. Data is a large pre-built JSON file committed to the repo. Deployed via GitHub Pages with custom domain via Porkbun DNS.
Data sources:

gdshowsdb — 2,076 shows with full setlists, venues, cities, set breaks, segues
jerrybase_songs — 423 GD songs with songwriter credits, performance counts, debut/final dates
official_releases — 163 releases (Dave's Picks 1–58, Dick's Picks, box sets) with 100% MusicBrainz artwork coverage
heady_songs / heady_lookup — 375 songs, 3,874 ranked community versions from headyversion.com
setlists — tour names (from jerrybase CSV exports), venue type (indoor/outdoor), debut/final flags
reddit_mentions — scraped mention counts per show

Current app features:

5 nav tabs: Browse (year grid) / Notable / Releases / Favorites+Attended / Map
Browse by year: Year grid → show list with filter bar (All/Soundboard/Audience/Released), star ratings, source tags, release tags, tour tags, indoor/outdoor tags, debut highlights (gold left border + ✨ badge), quick-play button, favorite (★) and attended (🎫) buttons
Show page: Banner with all tags → Show Info (collapsible, blue) → Setlist with song metadata, debut/final badges, heady-version badges (collapsible, red) → Tracks with set-break alignment (collapsible, amber, default open) → Reviews from archive.org (collapsible, teal)
Notable Shows: Curated landmark shows as a card grid, plus a 🎲 Guess the Year game (random clip, 60s playback, type the year, exact-match scoring)
Releases: Organized by series (Dave's Picks, Dick's Picks, etc.) with numeric-aware sort, cover artwork, multi-show box set picker
Favorites + Attended: Shared tab with segmented toggle (★ Favorites / 🎫 Attended), both backed by localStorage
Map by State: US tile-grid cartogram (color-coded by show count), A–Z state list, international section. Tapping a state fetches live archive.org data via exact-date batched OR-queries (50 dates/batch), renders with full filter bar and same row styling as Browse. Year-fetch results cached session-wide to avoid re-fetching.
Today in Grateful Dead History: Horizontal scroll strip at top of Browse showing all shows matching today's month+day across all years
Heady version search: Typing a song name in the search bar surfaces that song's ranked community versions instead of a generic archive.org text search
Media Session API: Lock screen Now Playing shows real SYF artwork, song title, venue, date
Guess the Year game: Random show → random clip → 60s playback → type the year → exact/close/miss verdict → reveal full date + venue

Known data quality issues:

22 heady version entries have corrupted month: 00 dates (rehearsal tapes/studio sessions not in gdshowsdb) — render as "date unconfirmed," non-clickable
reddit_mentions has some month: 00 date keys (not yet fixed)
Venue-type (indoor/outdoor) data only covers 1978–1995

Working files I maintain between sessions:

/home/claude/dead-archive-test/dead-archive.html — main app
/home/claude/dead-archive-test/dead_data.json — data file
Final outputs always go to /mnt/user-data/outputs/

Ongoing / not yet built:

YouTube integration (decided on "search link" approach rather than API)
Full heady version browse view (search integration done, dedicated browse not yet built)
Parallelizing state-show loads was addressed with batched exact-date queries instead


That should give a new session enough context to pick up right where we are. Let me know when you're ready to start fresh and I'll be there.
