# /videos/ — drop your reels here

This folder holds every video file on the site. Three subfolders:

| Folder | What lives here | Used by |
|---|---|---|
| `tiles/` | Compressed previews (1–50 MB each, 720p, no audio, autoplay loops) | All bento grid tiles on `/` and `/work` |
| `full/` | High-quality finished cuts (8–140 MB each, full audio) | The hero reel on the homepage. Eventually: click-to-play modals (will move to Vimeo/Cloudflare Stream) |
| `posters/` | Still-frame JPGs auto-generated from tile videos via ffmpeg | Used as the `poster=""` attribute so tiles look polished while video loads |

---

## Current inventory (26 pieces)

All 26 files exist in **both** `tiles/` and `full/` with matching filenames. Tiles are compressed; full versions are master quality.

| Filename (in `tiles/` and `full/`) | Client / Project |
|---|---|
| `ArenaClub_02_Nug_2025_0418_02C_Tim_v01 9x16.mp4` | Arena Club — Nug (9:16) |
| `BaloneyBrains_YT_02_MOTO_2026_0418_v1.mp4` | Baloney Brains — MOTO YouTube spot |
| `DK_84_TT_2026_0107_Rocket_MissionControl_v01.1.mp4` | DraftKings TikTok — Rocket Mission Control v01 |
| `DK_84_TT_2026_0112_HuffnMorePuff_Pig_v2.mp4` | DraftKings TikTok — Huff n' More Puff Pig |
| `DK_84_TT_2026_0112_Rocket_MissionControl_v2.mp4` | DraftKings TikTok — Rocket Mission Control v2 |
| `DK_MISSIONCONTROL_FINAL.mp4` | **DraftKings — Mission Control Final cut** (used as homepage hero) |
| `DTF_A_HERO_04_2026_0410_v1.mp4` | DTF — Hero |
| `DTF_B_LIFESTYLE_S06_2026_0410_v1.mp4` | DTF — Lifestyle |
| `DTF_C_DYNAMIC_79_2026_0410_v1.mp4` | DTF — Dynamic |
| `DoorDash_AI_TT_2026_0205_SJT_Farmer_v1.mp4` | DoorDash — AI Farmer (SJT) |
| `FlamnHot_Doritos_SnapAd_FollowUp_2022_0211_v02.2.mp4` | Doritos — Flamin' Hot SnapAd |
| `IIC_FLORIDAMAN_01 FINAL.mp4` | BLOW UPS — Florida Man |
| `Malibu Rum Tough Nut.mp4` | Malibu Rum — Tough Nut |
| `MichelobUltra_01_TT_2026_0115_02_v01.mp4` | Michelob Ultra — TikTok |
| `SierraNevada_01_TT_2024_1125_06_Golden_v03.mp4` | Sierra Nevada — Golden TikTok |
| `TTCX-MN_CODM_A-Skit-Reactions-HandheldThrills-1_v_9x16_22s_EN_251108.mp4` | Call of Duty Mobile — Skit Reactions |
| `VRBO_Symphony Remix_TT_AD_2025_114_01_TofinoCabin_V02.mp4` | VRBO — Symphony Remix (Tofino Cabin) |
| `Zolloro_01_2026_0508_v1 9x16.mp4` | Zolloro — 01 (9:16) |
| `Zolloro_02_Dracula_2026_0512_v1.mp4` | Zolloro — Dracula |
| `instagram_v1 (1080p).mp4` | Instagram — Brand Film |
| `kit_kat_trees_v1 (1080p).mp4` | Kit Kat — Trees |
| `mission_control_v1 (1080p).mp4` | DraftKings — Mission Control (alt cut) |
| `scream_vi_-_day_in_my_life_v1 (1080p).mp4` | Scream VI — Day in My Life |
| `sleep_cocoa_v1 (1080p).mp4` | Sleep Cocoa |
| `smile_v1 (1080p).mp4` | Smile — Trailer Tease |
| `teenage_mutant_ninja_turtles_v1 (1080p).mp4` | TMNT — Social Cut |

---

## ⚠️ Two files exceed Vercel's 100 MB per-file limit

These cannot ship from Vercel as direct assets:

- `full/BaloneyBrains_YT_02_MOTO_2026_0418_v1.mp4` (111 MB)
- `full/Malibu Rum Tough Nut.mp4` (133 MB)

Both have compressed tile versions in `tiles/` (22 MB and 45 MB) that the site uses for autoplay. The full-quality versions should eventually live on **Vimeo** or **Cloudflare Stream** and embed via URL when we build click-to-play modals — keeping them out of the Vercel build entirely.

---

## Naming conventions for future drops

When you add more pieces:

1. **Drop the master file in `full/`** with whatever filename makes sense for your archive.
2. **Drop a compressed copy in `tiles/`** with the **exact same filename**. Target 1–5 MB, 720p, no audio, 6–15 sec, seamless loop.
3. **Regenerate the poster** by running this in Terminal from this folder:
   ```bash
   ffmpeg -y -ss 00:00:01.5 -i "tiles/YOUR-FILE.mp4" -vframes 1 -q:v 3 -vf "scale=720:-1" "posters/YOUR-FILE.jpg"
   ```
4. **Tell Claude** to add the new file to the bento grid — give the filename, the client/project label, the aspect ratio, and which categories (AI / Film / Social / Design / Personal) it fits.

---

## Tile encoding specs (recap)

| Setting | Tile (autoplay loop) | Hero (with sound) |
|---|---|---|
| Resolution | 1280×720 or 720×1280 | 1920×1080 |
| Codec | H.264 (x264) | H.264 (x264) |
| Quality | CRF 28–30 (aggressive) | CRF 22–24 |
| Audio | **None (strip the track)** | AAC 128 kbps stereo |
| Length | 6–15 sec, seamless loop | 30–60 sec |
| Target size | 1–5 MB | 5–15 MB |

HandBrake preset: "Web → Discord Tiny 240p30" as base, then bump resolution to 720p and uncheck the audio track. Optimize for web on.

---

## ⚠️ CRITICAL: enable "Web Optimized" / Faststart on every export

When exporting MP4s for this site, make sure your encoder writes the `moov` atom at the **start** of the file (not the end). Without this, browsers can't start autoplay until the entire file downloads — most will silently skip the video.

- **HandBrake:** the "Web Optimized" checkbox at the top of the Summary tab. Must be on.
- **Adobe Media Encoder / Premiere:** Format → MP4 → Video tab → "Fast Start" checkbox at the bottom.
- **CapCut:** Settings → check "Optimize for streaming" if available, otherwise re-encode through HandBrake.
- **ffmpeg CLI:** add `-movflags +faststart` to the output args.

If you ever add a file and it doesn't play on the live site, tell Claude — there's a one-line ffmpeg remux that fixes it without re-encoding.
