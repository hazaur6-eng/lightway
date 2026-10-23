# Lightway

## Current State
All audio playback in the app uses browser `speechSynthesis` (TTS) — robotic, unnatural voice. Only the Quran page uses real human recitation from `verses.quran.com/Alafasy/mp3/` CDN.

Affected pages:
- `RandomAyahPage.tsx` — TTS for Arabic ayah
- `DailyPlanPage.tsx` — TTS for ayahs and duas
- `MoodGuidancePage.tsx` — TTS for ayahs and duas

## Requested Changes (Diff)

### Add
- Alafasy CDN audio playback utility: `https://verses.quran.com/Alafasy/mp3/{surah3digits}{ayah3digits}.mp3`
- `surahNum` and `ayahNum` fields to all Quran ayah entries across all 3 pages
- `surahNum` and `ayahNum` to duas that are Quran verses (DailyPlanPage: all 3 duas; MoodGuidancePage: confusion Taha 20:25, loneliness Anbiya 21:89, hope Baqarah 2:286, regret Araf 7:23, despair Anbiya 21:87)

### Modify
- `RandomAyahPage`: replace `handleSpeak` (speechSynthesis) with `HTMLAudioElement` using Alafasy CDN with `surahNumber`/`ayahNumber` already in state
- `DailyPlanPage`: replace `speakArabic` with `HTMLAudioElement` CDN player; add `surahNum`/`ayahNum` to AYAHS and DUAS
- `MoodGuidancePage`: replace `handleSpeak` with CDN audio for entries that have surah/ayah; remove speak button for hadith-based duas (no Quran CDN available)

### Remove
- All `window.speechSynthesis` usage from the 3 pages
- `ttsUnsupported` state from RandomAyahPage (no longer needed)

## Implementation Plan
1. Add helper `alafasyUrl(surah, ayah)` inline in each file
2. Add `surahNum`/`ayahNum` to AYAHS, DUAS data in DailyPlanPage
3. Add `surahNum`/`ayahNum` to GUIDANCE ayahs and qualifying DUAS in MoodGuidancePage
4. Rewrite audio playback using `new Audio(url)` with proper play/stop state management
5. Validate build
