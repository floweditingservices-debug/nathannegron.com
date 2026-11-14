# nathannegron.com

One-page UGC portfolio. Plain HTML, no build step.

## Folder structure (drop your files in with these exact names)

```
index.html
CNAME                         -> nathannegron.com
assets/
  img/
    nathan.jpg                -> casual photo, square, ~600x600
    og-image.jpg              -> 1200x630 share image (link previews)
    case-auto-dealer.jpg      -> blurred Ads Manager screenshot
    case-organic.jpg          -> blurred IG insights across clients
    case-meta-totals.jpg      -> blurred 12-month Ads Manager summary
    case-my-channel.jpg       -> your IG profile / insights
  videos/
    01-talking-head-perspective.mp4   (best video goes first)
    02-casual-ugc-captions.mp4
    03-voiceover-demo.mp4
    04-green-screen.mp4
    05-organic-reel.mp4
    06-organic-reel.mp4
  posters/                    -> first frame of each video, same name, .jpg
    01-talking-head-perspective.jpg ... 06-organic-reel.jpg
  logos/                      -> SVG or PNG, any color (shown grey, color on hover)
    perspective.svg  captions.svg  neutonic.svg  saphire-media.svg  sound-campaigns.svg
```

Missing files are fine: the page shows a labeled placeholder until each file exists.
Using .png logos? Change the extension in index.html too.

Also search index.html for `TODO: fill` and replace every `[X]` placeholder and `hello@nathannegron.com`.

## 1. Compress videos (5–15MB, 9:16)

Install ffmpeg (`brew install ffmpeg`), then for each video:

```
ffmpeg -i input.mp4 -vf "scale=1080:1920:force_original_aspect_ratio=increase,crop=1080:1920" -c:v libx264 -preset veryfast -crf 26 -maxrate 4M -bufsize 8M -c:a aac -b:a 128k -movflags +faststart -t 45 assets/videos/01-talking-head-perspective.mp4
```

Make the poster (first frame):

```
ffmpeg -i assets/videos/01-talking-head-perspective.mp4 -frames:v 1 -q:v 4 assets/posters/01-talking-head-perspective.jpg
```

If a file is over 15MB, raise `-crf` to 28.

## 2. Push to GitHub Pages

1. Create a new public repo on github.com, e.g. `nathannegron.com`.
2. In this folder:
   ```
   git init
   git add .
   git commit -m "Portfolio"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/nathannegron.com.git
   git push -u origin main
   ```
3. Repo → Settings → Pages → Source: "Deploy from a branch", Branch: `main` / root → Save.
4. Same page → Custom domain: `nathannegron.com` → Save.

## 3. Point the domain (Squarespace)

1. **Disconnect from Full Frame Social first:** Squarespace → Domains → nathannegron.com → Menu → Website → Change connection → disconnect from the Full Frame Social site. Leave fullframesocial.com untouched.
2. Domains → nathannegron.com → DNS. Remove the Squarespace default A and `www` records. Do not touch MX or TXT records (email).
3. Add custom records:
   | Type | Host | Value |
   |---|---|---|
   | A | @ | 185.199.108.153 |
   | A | @ | 185.199.109.153 |
   | A | @ | 185.199.110.153 |
   | A | @ | 185.199.111.153 |
   | CNAME | www | YOUR-USERNAME.github.io |
4. Wait for DNS (minutes to a few hours). Back in GitHub → Settings → Pages, tick **Enforce HTTPS** once it's available.

## Updating later

Edit a file, then `git add . && git commit -m "update" && git push`. Live in about a minute.
