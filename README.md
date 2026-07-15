# 🤿 Dive Logbook

A personal dive logbook — a self-contained HTML app, no build step required.

## Features
- Full dive records: profile, equipment (wetsuit/tank/weights), notes & marine life, bubble rating
- Import from dive computers (CSV, Subsurface XML, UDDF) with duplicate detection, preview and undo
- Photos of your paper logbook page and dive centre stamp, with AI digital stamp recreation*
- AI form pre-fill from a photo of your paper logbook*
- Map of your dive sites (Leaflet + OpenStreetMap, Nominatim geocoding)
- JSON backup
- Magic-link accounts (Supabase) with automatic cross-device sync
- Offline-first: log dives without internet, sync when you're back online

\* AI features require running inside the Claude preview.

## Getting started
- **Users:** see [HOW-TO-USE.md](HOW-TO-USE.md)
- **Admins / self-hosting:** see [SETUP.md](SETUP.md)

## Data
Without an account, dives live in each device's localStorage. With an account, they sync to Supabase — protected per user by Row Level Security.
