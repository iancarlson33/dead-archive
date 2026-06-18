Project: Grateful Dead archive web app
Building a single-file HTML app (dead-archive.html) styled like a vintage Fillmore/Avalon poster (Playfair Display + IBM Plex Mono, gold/red/cream/black palette) that plays archive.org recordings, paired with a large data file (dead_data.json, ~6-12MB) containing official_releases, heady_songs/heady_lookup, reddit_mentions, setlists, and a ~2000+ show database (gdshowsdb). The HTML fetches ./dead_data.json via relative path — both files must sit side by side (works on GitHub Pages, fails locally via file:// due to CORS — that's expected, not a bug).
Status of dead-archive.html — all known bugs fixed, no outstanding issues:

Landing splash page (CSS, init call, responsive sizing, no longer persisted via sessionStorage) — fixed
Show info / track list rendering — fixed
Heady-version badge mismatches — fixed via stricter headyMatch() matching
Major fix: every show failed to load audio with a misleading "archive.org may be temporarily unavailable" error. Root cause: buildSetlist() never had a return section; statement, so callers tried to appendChild(undefined), crashing right after every successful metadata fetch. Fixed by adding the return statement and removing a redundant internal append call.
"Dead Archive" header title is now a clickable button that returns to the landing splash page

Status of dead_data.json:

Delivered with two rounds of official box-set additions, cross-referenced against the full Wikipedia Grateful Dead discography. Went from 278 → 402 dates with official_releases entries. This includes box sets like In and Out of the Garden, Europe '72: The Complete Recordings (22 shows), Listen to the River, Here Comes Sunshine 1973, Friend of the Devils, Giants Stadium 1987/89/91, June 1976, July 1978, Lyceum '72, May 1977 (both versions), Formerly the Warlocks, and more.
User is holding off downloading this until the jerrybase merge (below) is also ready, so both can be delivered together in one pass.

Outstanding / in progress:

User has a scrape_jerrybase.py scraper (built earlier) actively running against jerrybase.com (song IDs 1–1400), producing jerrybase_songs.json with song metadata: performance counts, first/last played, songwriter/lyrics/music credits, genres, studio album, tags.
Important caveat confirmed: jerrybase.com indexes Jerry Garcia's entire performing career (Grateful Dead, Jerry Garcia Band, Legion of Mary, Old & In the Way, New Riders of the Purple Sage, Garcia/Grisman duo, etc.), not just the Dead. The scraper itself was NOT modified to filter by band — user explicitly said to filter at merge time instead, once the scrape is handed back.
Merge plan once jerrybase_songs.json is delivered: (1) cross-reference every scraped song against gdshowsdb setlists and heady_songs/heady_lookup to confirm actual Grateful Dead performance history; (2) drop/flag JGB-exclusive songs with no Dead history; (3) sanity-check jerrybase's performance-count field against known Dead-only counts before trusting it as Dead-specific (to avoid importing cross-project combined totals as Dead-only stats); (4) merge resulting song metadata into dead_data.json alongside the box-set updates, deliver as one combined file; (5) wire song metadata into the track-view UI to show songwriter/performance-count context (the original motivating use case).
Resolved side question: jerrybase.com does NOT track per-show Jerry Garcia guitar/equipment data — confirmed via web search, its scope is setlists only. If guitar-per-show data is wanted later, it would need to come from a different source (e.g. jerrygarcia.com/guitars/ or the "Grateful Dead Guide: Jerry Garcia Instrument History" blog), as a separate enrichment task.

Next action: waiting on user to provide jerrybase_songs.json from the completed scrape.
