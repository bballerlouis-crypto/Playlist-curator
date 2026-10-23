# Playlist Curator

A swipe-style app for building a Spotify playlist for an event (wedding, party, dinner…).

1. Name the playlist
2. Set the event length in hours (this sets how much music you need to pick)
3. Choose the event type (auto-suggested from the title) or type a custom vibe
4. Swipe right to add, left to pass — each card plays a 15-second bite near the chorus
5. When you hit the target length, review the list, then **Save to Spotify** (private playlist) and/or **Download Excel**

**You need Spotify Premium.** Spotify requires it for developer apps and for clip playback.

## Sharing it with others

The live site has a Spotify app built in (Client ID `ac26b229ef2e41ea9850edb521635dab`), so people just open the link and tap **Log in with Spotify**. No setup needed.

- Spotify allows **up to 5 people** on a personal app. Add each one in the Spotify developer dashboard → your app → **Settings → User Management**, using the name and email on their Spotify account. Anyone not added sees a message asking them to get added.
- Each person needs **Spotify Premium** to hear clips.
- **Use a different Spotify app** on the login screen lets someone use their own Client ID instead (the steps below).

The setup steps below are only needed to host your own copy.

---

## Setup on iPhone (no computer needed)

### 1. Put the page online
The app is one file, `index.html`. Upload it to any free static host that gives you an **https://** link (for example Static.app or EdgeOne Pages — both accept an HTML upload from Safari via the Files app). Save `index.html` to Files first, then upload it there.

Open the link you get in Safari. The Connect screen shows the exact address under **"Add this exact Redirect URI"** — you'll need it next.

### 2. Create the Spotify app (in Safari)
1. Go to https://developer.spotify.com/dashboard, sign in, and tap **Create app**.
2. Name/description: anything (e.g. "Playlist Curator").
3. **Redirect URI:** paste the address shown on the app's Connect screen, character for character (including `https://` and any trailing `/` or `index.html`).
4. APIs used: check **Web API**. (Also check **Web Playback SDK** if you'll ever use it on a computer.)
5. Save, open the app's **Settings**, and copy the **Client ID**.
6. Optional: to let someone else use it with their own Spotify account, add their name and Spotify email under **User Management** (up to 5 people).

### 3. Connect and go
Back in the app, paste the Client ID and tap **Connect with Spotify**.

**Clips play through the Spotify app on your phone.** Before you start swiping, open Spotify, play any song for a second, then come back to Safari. If Spotify falls asleep later, the card shows an **Open Spotify** button; when you come back, the clip starts on its own. If you'd rather not use the Spotify app, tap **Use Spotify's previews instead**.

**Home-screen icon (optional):** in Safari tap Share → **Add to Home Screen**. If the sheet shows an **Open as Web App** switch, turn it **off**, because Spotify's sign-in doesn't hand back correctly to a web-app window.

**Excel on iPhone:** "Download Excel" opens the share sheet, so you can save it to Files, AirDrop it, or email it.

---

## Setup on a computer

1. Create the Spotify app as above, but use the Redirect URI `http://127.0.0.1:8888/` and check **Web Playback SDK**.
2. From the folder containing `index.html`, run:
   ```bash
   python3 -m http.server 8888 --bind 127.0.0.1
   ```
   (Windows: `py -m http.server 8888 --bind 127.0.0.1`)
3. Open **http://127.0.0.1:8888/** in the same computer's browser. Clips play right in the browser tab.

Each Spotify app can list several Redirect URIs, so one Client ID can cover both your hosted link and your computer.

---

## Controls

| Action | Touch | Keyboard (computer) |
|---|---|---|
| Add song | Swipe right or ♥ | → |
| Pass | Swipe left or ✕ | ← |
| Replay / stop clip | Center play button | Space |
| Nudge clip start | −10 / +10 | — |
| Undo last swipe | ↶ | Z |
| Finish early | ✓ | — |

### Choosing what comes up (steps 3 and 4)

- **What should come up?** Pick one or more event vibes and any number of genres or eras (Motown, 80s pop, Latin…). They're mixed together.
- **Start from your Spotify playlist:** add one or more of your playlists. Their artists, genres and era steer suggestions from the first card, and their songs can also show up as cards (toggle). Spotify only lets this app read playlists you **created or collaborate on**. For one you just follow, add its songs to a new playlist of your own first.
- **What shouldn't come up?** Tap genres or eras to keep out (e.g. 70s soul & funk), and optionally turn on **No explicit songs**. Matching songs are skipped silently. A tag can't be in both lists; picking it in one removes it from the other.
- **Where the music is from:** pick one or more regions (USA, Canada, Latin America & Caribbean, Europe incl. UK, Africa, Middle East, Asia, Australia & NZ), or leave it on Anywhere. Each card shows the artist's country, and the Excel export has a "From" column. Spotify doesn't say where artists are from, so the app works it out from a built-in list for well-known artists, the free [MusicBrainz](https://musicbrainz.org) music database, country words in Spotify's genres (e.g. "uk pop"), and the country the recording was registered in. Songs it can't place are skipped unless you turn on **Allow songs if we can't tell where they're from**.
- **Tempo:** Any, Slow & mellow, Mid-tempo or Dance floor. It's set for you from the event (Wedding Reception and House Party → Dance floor, Dinner → Slow & mellow) and you can change it. Dance floor keeps songs at about 105+ BPM (85+ for hip-hop, reggaeton and afrobeats) and skips ballads. BPM comes from Deezer's free public catalog; when a song has no BPM there, the app goes by its genre and the curated lists. Cards show the BPM, and Excel has a BPM column.
- **Search or type any genre:** the box above the genre list suggests matches as you type, from the built-in list, about 300 real Spotify genres (yacht rock, city pop, jersey club, phonk…) and genres the app has seen on artists. Press Enter to add anything else. Your typed genres appear under "Your genres"; tap one to remove it. If Spotify has no artists tagged with what you typed, you'll get a heads-up.
- **Auto "leave out":** once you've picked genres, step 4 starts with every genre you didn't pick already left out, to keep the focus on what you chose. Tap any to allow it back, or **Clear all**. Once you change it yourself, it stops auto-filling. A song that matches a genre you picked is always kept, even if it also touches a left-out one (e.g. "indie R&B" when you picked Modern R&B). Search results also have to match your picked genres, not just the event's broad mix.
- **Genres** are grouped (Pop, Hip-hop & R&B, Dance & electronic, Soul & funk, Rock & indie, Country & folk, Latin & global, Jazz & standards), about 45 in all, from 50s oldies to 2025–26 releases, trap, drill, alt R&B, afro house, amapiano and regional Mexican. The same list is used for "What shouldn't come up?".
- Change any of this later from the **⚙︎** button while swiping.

**Keeping suggestions on-genre:** every search result is checked against the genres of your event, chosen genres, picks and imported playlists, and dropped if its artist doesn't share any of them. Kids' music, karaoke, sleep/meditation, workout and holiday artists are filtered out, along with live versions, remixes and compilation-album copies. Searches stay near the top of Spotify's results, where songs are better known.

Genre filtering uses the genres Spotify lists for each artist, plus the song's release year, so it's accurate for most mainstream artists. An artist with no genres on Spotify can't be filtered by genre.

**Loading and speed:** after step 4, a short "Building your mix" screen gets the first songs ready and connects to Spotify on your phone. While you swipe, the app keeps about 6 songs fully checked in the background, so the next card appears right away. The ready songs, curated-song lookups, genres, BPMs and artist countries are saved in your browser, so reopening a playlist is instant.

While swiping, the top bar has **← Save & exit**, **✨ Match picks** and **Export →**.

- **My playlists:** every playlist saves automatically in this browser, and several can be in progress at once. Open one from **My playlists** to keep going, or tap ✕ to delete it (that doesn't touch Spotify).
- **Export early:** **Export →** opens the review screen at any point. Save to Spotify or download Excel, then keep swiping. Saving again updates the same Spotify playlist instead of making a copy.
- **Match picks:** on by default. After you add 3 songs, about half of the suggestions come from the artists, genres and era of what you've added. Cards show why they were suggested (e.g. "✨ Because you added Beyoncé"). Artists you pass on twice stop appearing.

Saved playlists live in the browser you used, so Safari on your phone and a computer each have their own list.

## How it works (and Spotify limits to know about)

Spotify has removed several API features for new developer apps (30-second preview links, song-section/chorus data, and its recommendations engine), so the app works around them:

- **Clip start** is an *estimate* of the first chorus (~30% into the track, never before 0:30). It's right for most pop songs; use **+10 / −10** when it lands on a verse.
- **Song suggestions** come from curated lists per event type (in `index.html`, the `EVENTS` object near the top of the script), matched on Spotify via search, then topped up with genre searches. Edit the `'Title|Artist'` lines to tune a list.
- **The song target** uses real track lengths: the playlist completes when your picks add up to the hours you set.
- **Saving** creates a private playlist in your Spotify account. **Excel** includes title, artist, album, duration and a clickable Spotify link, plus a Details sheet.

## Troubleshooting

- **"INVALID_CLIENT: Invalid redirect URI"** — the Redirect URI in the dashboard must match the Connect screen's address exactly.
- **"Spotify only allows secure addresses" warning** — open your hosted link with `https://`.
- **Clips don't play on iPhone** — open the Spotify app, play any song briefly, return and tap **I'm back ▶**. Confirm the account has Premium.
- **"User not registered in the Developer Dashboard"** — add that Spotify account under User Management.
- **Rate limit message** — Spotify throttles development apps; wait a minute and continue.
