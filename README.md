Dead Archive — Project Summary
I'm building Dead Archive (deadarchives.org), a single-file HTML app (dead-archive.html) + large JSON data file (dead_data.json) hosted on GitHub Pages (repo: iancarlson33/dead-archive). It's a Grateful Dead live-show archive and player styled as a vintage Fillmore/Avalon poster (Playfair Display + IBM Plex Mono, gold/red/cream/black palette). Vanilla JS, single HTML file, no framework, no required backend (one optional Firebase feature, see below).
Data sources in dead_data.json:

gdshowsdb — 2,076 shows, full setlists, venues, cities, debuts/finals, segues
jerrybase_songs — 423 songs: performance counts, debut/final dates, lead vocalist, songwriter credits, studio album, songwriting-team tags
official_releases — 163 releases with artwork
heady_songs/heady_lookup — 375 songs, 3,874 ranked community versions
setlists — tour names, venue type, debut/final flags
reddit_mentions — 494 dates, mention counts scraped from u/setlistbot's Reddit comment history (manual browser-console scroll+dedup, since Reddit locked down API self-service access in Nov 2025)
archive_views — ~1,940 dates with all_time/last_30day/last_7day play counts, top_identifier, recording_count — built via a Python script hitting archive.org's public views API
city_coords — 290 geocoded venue cities (OpenStreetMap Nominatim)
state_outlines — 51 US states+DC simplified boundary polygons (US Census Bureau via us-atlas TopoJSON, hand-decoded + Douglas-Peucker simplified)

Navigation — 5 tabs: Browse | Notable | Releases | Favorites/Attended/My Picks | Map

Browse is a faceted hub — segmented toggle: Year | Venue | Tour | Song, with a "Today in GD History" strip pinned above all facets.

Year: original year almanac
Venue: 526 venues, most-played first
Tour: 130 tours → drill-down now includes a Tour Map (SVG route map: real geocoded stops connected in order, US state-outline background that dynamically sizes/centers to each tour's actual shape, skips outlines entirely for international tours) and a Tour Summary panel (debuts, last-ever performances, archive listens, heady versions — all derived from existing data, no new crawling)
Song: 431 songs A-Z with an iOS-Contacts-style touch-drag index scrubber, lead-vocalist filter chips (Jerry/Bob/Pigpen/Brent/Phil/Donna/Vince)


Song detail page: performance count, debut/final dates, songwriter credits, studio album art (with graceful text fallback), tappable songwriting-tag badges, "Often segues into," Top Heady Versions, My Picks (drag-to-reorder personal ranking, 🏆 toggle on show-page setlist rows), a Performances-By-Year sparkline chart, and a full-performances browse button
Notable: Notable Shows + Guess the Year game + Iconic Segues chart (Drums/Space filtered out) + Most Played (archive.org views: hero card for #1 + ranked grid with relative-scale bars) + Trending (last-30-day plays) + Song Timeline (194 songs with 25+ performances, Gantt-style debut-to-final bars, ordered chronologically). Note: a "Most Discussed" (Reddit) chart existed briefly and was removed by request — the underlying reddit_mentions data is still intact if wanted later.
Favorites/Attended/My Picks: 3-way segmented toggle. Favorites sorts ascending (oldest first). My Picks = personal drag-to-reorder song rankings.
Sync: optional Firebase Firestore cross-device sync for Favorites/Attended/My Picks — code-based (8-char codes, not full accounts), additive merge with timestamp-based conflict resolution for reorders, manual "Sync Now" button. Reuses the same Firebase project as a separate SF Cache Quest app, in its own dead_archive_sync collection.

Key technical patterns/fixes built along the way:

normalizeSongTitle() handles gerund contractions (Playin'/Playing) for fuzzy song matching
getGDShow() resolves early/late double-show date-suffix keys (date-0/date-1)
fetchRecordingsForDates() — shared batched archive.org search helper with self-correcting truncation detection (splits overloaded batches recursively)
showDrillDownShows/renderFilterableShowList — shared list-rendering component; had a real bug where async-loaded show lists wiped out the Tour map/summary panel via innerHTML="" (fixed via extraContentFn threading)
Auto-advance playback fix for an iOS Safari audio.load() quirk

Known data quality issues: 22 heady entries with corrupted month:00 dates; venue indoor/outdoor data only covers 1978–1995; one heady_songs date-attribution disagreement for 1970-07-12 (left unresolved, flagged as a source conflict); a couple of gdshowsdb typos fixed along the way (Oakland, Rüsselsheim).
Working conventions: Files live at /home/claude/dead-archive-test/ in-session, shipped to /mnt/user-data/outputs/. Always run a Node syntax check (new Function() per script block) before shipping, and validate logic against real extracted data before calling something done. User deploys via GitHub Pages — needs both dead-archive.html and dead_data.json committed together whenever both changed. Has hit various deploy/cache gotchas before (Pages propagation delay, browser cache, Windows Python encoding/PATH issues) — worth recognizing those patterns quickly if they recur.
Where we left off: mid-implementation of an animated dot traveling along the Tour Map route (native SVG <animateMotion> along the existing path, duration scaled to stop count, respecting prefers-reduced-motion) — not yet built, a sandbox outage interrupted before I could implement it.
