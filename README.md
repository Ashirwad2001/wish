# For Sanu, With All My Love

A single-page, static birthday website. No build step, no dependencies, no backend.

```
index.html     the whole site: styles, markup, script and all editable text
photos/        the images (JPEG)
videos/        the video clips (MP4, H.264, plays on iPhone and Android)
vercel.json    caching + "do not index" header for Vercel
```

Videos: the moving hero uses `heroVideos`, the stories-style player uses `storyVideos` and `storyNotes` (the text beside each clip), and `videoRot` can stand a sideways clip upright. File names must match the files in `videos/` exactly. Keep clips short and under about 10 MB each; convert iPhone .MOV files to MP4 first.

## Open it on an iPhone

Opening `index.html` straight from the Files app will not run the page. Put the folder online (for example on Vercel, with the `vercel.json` already here) and open the link in Safari, or serve it from a computer on the same Wi-Fi: `python3 -m http.server 8000`, then open `http://<computer-ip>:8000` on the phone.

## Run it locally

Open `index.html` directly, or serve the folder so everything loads exactly as it will online:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Edit the content

Everything she reads is in one `CONFIG` object near the top of the `<script>` in `index.html`:

| What | Where in `CONFIG` |
| --- | --- |
| Opening screen text and button | `opening` |
| Welcome page text | `hero` |
| Relationship start date | `relationshipStart` |
| Photo captions, dates, private notes | `photos` |
| Timeline cards | `timeline` |
| The love letter | `letter` and `letterSign` |
| "Reasons I love you" cards | `reasons` |
| Final messages and the promise | `finale` |
| Scrolling ribbon, hidden heart and star notes | `marquee`, `psst`, `psstNote`, `starNote` |

`IMG` (just above `CONFIG`) maps each photo key to its file in `photos/`.

## Photos

To swap a photo, replace the file in `photos/` with a new JPEG of the same name (portrait photos around 825×1100 px, about 150–300 KB, keep the page fast on phones).

| File | Used for |
| --- | --- |
| `p2.jpg` | Featured polaroid, shop mirror (`photos[0]`) |
| `p5.jpg` | Collage and filmstrip, purple light |
| `p1.jpg` | Collage and filmstrip, antique mirror |
| `p3.jpg` | Collage and filmstrip, hands in the car |
| `p8.jpg` | Collage and filmstrip, roadside mountains |
| `p9.jpg` | Collage and filmstrip, stone hut |
| `p6.jpg` | Collage and filmstrip, video call (landscape) |
| `p7.jpg` | Timeline card "Your birthday" (bouquet) |
| `c1.jpg` – `c8.jpg` | Revealed when she flips reason card 1–8 |

To add a photo: save it in `photos/`, add a key to `IMG`, then add an entry to `photos` in `CONFIG`:

```js
{ file:"p10", ratio:"3/4", caption:"Your caption", date:"01 JAN 2026", heart:false, note:"" }
```

Useful flags on a photo entry: `heart:true` plus a `note` adds the glowing heart with a private note; `gallery:false` keeps it out of the collage and filmstrip (used for the timeline-only birthday photo); `collage:false` keeps it out of the collage only. The collage fits six photos in its layout, so extra photos should use `collage:false` and go to the filmstrip.

## Deploy

1. Create a **private** repository on GitHub and push this folder:

   ```bash
   git init
   git add .
   git commit -m "Birthday site for Sanu"
   git branch -M main
   git remote add origin https://github.com/<you>/<repo>.git
   git push -u origin main
   ```

2. Go to [vercel.com/new](https://vercel.com/new), import the repository, and deploy with the defaults:
   - Framework Preset: **Other**
   - Build Command: leave empty
   - Output Directory: leave empty (the site is served from the repo root)

3. Open the `*.vercel.app` URL Vercel gives you and test it on your phone. Every `git push` to `main` redeploys automatically.

Anyone with the Vercel link can open the site, and the letter and photos are on it. Share the link only with her. `vercel.json` adds a `noindex` header so search engines skip it. Vercel's password protection for deployments is a paid feature if you need more than that.

## Notes

- Music is off by default and only plays after she taps the Music button. The piano is generated in the browser, so there is no audio file.
- Fonts (Cormorant Garamond, Plus Jakarta Sans, Caveat) load from Google Fonts. The page still works without them using fallback fonts.
- The site respects the "reduce motion" setting of the device.
