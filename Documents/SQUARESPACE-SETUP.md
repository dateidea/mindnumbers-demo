# MindNumbers Interactive Phone Demo — Squarespace Setup

This guide gets the interactive phone demo live on your Squarespace site and shows you how to swap out any page in the future without touching code.

---

## One-time setup (~15 minutes)

### 1. Upload all 23 images to Squarespace

The demo needs 23 image files. They live in the `MindNumbers Interactive Phone items/` folder in this repo. Upload them all to Squarespace using the Custom Files feature — those URLs are the most stable Squarespace offers.

**How to upload:**

1. Log into Squarespace.
2. In the left sidebar, go to **Design → Custom CSS**.
3. At the bottom of the Custom CSS panel, click **Manage Custom Files**.
4. Click **Add Images or Fonts**.
5. Drag in all 23 PNGs at once (or upload them one by one).

The full list of files to upload:

**Screens (13):**
- `dashboard-main.png` — first screen you see when you open the app
- `dashboard-my-input.png` — "My Input" tab of the dashboard, with the voice sample button
- `mood-and-speech-details.png` — detail page when someone taps the Mood & Speech card
- `sleep-details.png` — detail page when someone taps the Sleep card
- `activity-details.png` — detail page when someone taps the Activity card
- `routine-details.png` — detail page when someone taps the Routine card
- `episodes-list.png` — list of past episodes
- `trends-tab.png` — Trends bottom-tab page
- `more-tab.png` — More bottom-tab page
- `record-tab.png` — Record bottom-tab page
- `voice-recording.png` — live voice-recording screen
- `meds-tab-taken.png` — Meds tab, "Meds Taken" view
- `meds-tab-prescription.png` — Meds tab, "Your Prescription" view

**Bottom nav icons (10):**
- `nav-home-active.png` and `nav-home-inactive.png`
- `nav-trends-active.png` and `nav-trends-inactive.png`
- `nav-record-active.png` and `nav-record-inactive.png`
- `nav-meds-active.png` and `nav-meds-inactive.png`
- `nav-more-active.png` and `nav-more-inactive.png`

### 2. Get each image's URL

Once uploaded, click each file in the Custom Files list. Squarespace copies its URL to your clipboard automatically. The URLs look something like:

```
https://static1.squarespace.com/static/abc123.../t/def456.../1234567890/dashboard-main.png
```

You'll paste these URLs in step 4.

### 3. Add the Code Block to your page

1. Edit the page where you want the demo to appear.
2. Add a new section (or pick an existing one).
3. Click the `+` to add a block, and choose **Code**.
4. In the Code Block editor, **delete the default "<p>Hello, World!</p>"** placeholder.
5. Open `squarespace-embed.html` from this repo, copy the **entire** contents of the file (from the first comment all the way down to the closing `</script>` tag), and paste it into the Code Block.
6. Make sure **Display Source** is OFF (toggle at the bottom of the Code Block editor) — you want Squarespace to render the HTML, not show it as text.
7. Click **Apply**.

At this point the section renders, but all the phone screens are blank because we haven't filled in the URLs yet. That's the next step.

### 4. Paste the 23 URLs into the code

Still in the Code Block editor, scroll down to the `<script>` block near the bottom. You'll see a clearly marked section:

```javascript
// =====================================================================
// EDIT HERE TO UPDATE IMAGES
// ...
// =====================================================================
var MN_ASSETS = {
  // --- SCREENS ---
  dashboardMain:       '',  // First screen you see when you open the app
  dashboardMyInput:    '',  // "My Input" tab of the dashboard...
  ...
```

For each line, paste the matching URL between the empty quotes. For example:

```javascript
dashboardMain:       'https://static1.squarespace.com/static/.../dashboard-main.png',
```

The comment on each line tells you which image goes there. Work through all 23 lines.

**Tip:** do one or two first and click **Apply** to verify those screens render. If they do, you know the pattern works — finish the rest.

### 5. Save and preview

Click **Apply**, save the page, and preview. The demo should look and behave exactly like the version at <https://dateidea.github.io/mindnumbers-demo/Documents/try-it-embed.html>.

---

## Updating an image later (30 seconds)

Whenever the MindNumbers app design changes and you want the demo on your website to reflect the new design, here's the full workflow:

1. **In Squarespace:** Design → Custom CSS → Manage Custom Files → Add Images or Fonts → upload the new image.
2. **Click the uploaded image** in the Custom Files list. The URL is now in your clipboard.
3. **Open the page** with the demo. Click the phone demo section, then click the Code Block to edit it.
4. **Scroll to the `MN_ASSETS` object** near the bottom.
5. **Find the line** for the screen you're updating (the comment after each line tells you which page it is).
6. **Paste the new URL** between the quotes, replacing the old one.
7. **Apply → Save.**

That's it. The page updates immediately.

> **Heads up:** Squarespace generates a brand-new URL every time you upload a file, even if you give it the same name. That's why we paste the new URL in — it's the only way to tell the demo about the new image. This is a Squarespace platform limitation, not something we can design around.

---

## Troubleshooting

### An image isn't showing

1. Open the page in a browser (published or preview).
2. Right-click → **Inspect** → **Console** tab.
3. Look for a warning like:
   ```
   [MindNumbers demo] Missing URL in MN_ASSETS for key: dashboardMain
   ```
   That tells you exactly which line of `MN_ASSETS` is empty — fill it in.
4. If the key has a URL but the image still isn't loading, check the URL in a new browser tab. If Squarespace returns a 404, the URL is wrong — go back to Manage Custom Files and copy it again.

### The demo looks misaligned or uses the wrong font

- The demo loads the Inter font from Google Fonts. If Squarespace is overriding it with a different font, go to **Design → Site Styles → Fonts** and make sure no global font rule is blocking `.mn-demo-section`.
- The demo has its own padding and max-width — if it looks cramped, check whether the Squarespace section it lives in has unusually tight padding.

### Hotspots aren't clickable

- Make sure **Display Source** is OFF on the Code Block. If it's ON, Squarespace shows the code as text instead of rendering it.
- Check the browser Console for JavaScript errors. If Squarespace is stripping the `<script>` tag, your plan might be Personal (Business+ is required for JS in Code Blocks).

---

## What if I want to add a brand-new screen (not just replace one)?

Replacing an existing screen is a 30-second URL paste. Adding a brand-new screen (say, a new Settings page that didn't exist before) is a slightly bigger change because the demo needs to know the new screen exists, where it sits in the navigation, and which hotspots on other screens link to it.

That's a code edit, not an asset swap. If you need to add a screen, reach out to whoever manages the site code — they'll update:

1. `MN_ASSETS` — add a new key and URL for the new screen.
2. The HTML — add a new `<div class="mn-s-img" id="mn-newscreen">` block with the image and any hotspots.
3. Any existing screens that should link to the new one — add a hotspot `div` with `onclick="mnGo('newscreen')"`.

The structure is simple enough that anyone comfortable editing HTML can do it in 10 minutes.
