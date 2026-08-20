# 7th Grade Independent Novel Study — flipbook

## What this is
A self-contained web page. `index.html` is the whole flipbook: cover, guide page,
4 section dividers, 24 lesson pages, and a final page. Page-turn animation, contents
drawer, and in-page popups for lesson videos and assignments. No build step, no npm,
no framework install.

## How to deploy
Upload this whole folder, keeping the structure, and serve it over HTTP.

    index.html
    support.js
    lessons.csv
    page-text.csv
    teachers.csv
    images.csv
    assets/...
    ds/...

Then either link to `index.html` directly, or embed it in an existing page:

    <iframe src="/novel-study-7/index.html"
            style="width:100%; height:100vh; border:0"
            title="7th Grade Independent Novel Study"></iframe>

Notes:
- Must be served over HTTP(S), not opened as a `file://` path.
- Needs internet access for Google Fonts (Poppins, Cinzel), the React runtime, the
  published Google Sheets, and the embedded YouTube lesson videos.
- Nothing is written to a server. Each student's current page and saved teacher are
  kept in their own browser's `localStorage`.

## GitHub Pages
This folder is Pages-ready as-is. Upload its contents at the repository **root**, then
Settings → Pages → deploy from `main` / `/ (root)`. No `.nojekyll` needed — no folder
name starts with an underscore.

## Where the content lives — live Google Sheets
All copy, links, and photos are data, not markup. The page reads four **published
Google Sheets** at load time, so the teacher can edit content all year with no code
change and no redeploy. This works from any host and inside any iframe — the browser
fetches the sheets directly.

The links are near the top of the script block in `index.html`:

    const SHEETS = {
      lessons:  "...",   // one row per lesson page
      teachers: "...",   // teacher names + course links
      text:     "...",   // every other string in the workbook
      images:   "..."    // cover, dividers, mascots, 24 lesson photos
    };

| Sheet | Controls |
|---|---|
| lessons | one row per lesson — section, part, lesson number, title, standards, mini-lesson line, summary, video link, assignment label, two Snorkl questions. Adding or deleting a row adds or deletes a page. |
| page-text | every other string: cover, guide steps, step headings, dividers, footers, popups. |
| teachers | teacher names, courses, and course links in the assignment dropdown. |
| images | cover, section dividers, mascots, logo, decorative branches, and `lesson_1`…`lesson_24` photos. |

**Do not rename or reorder sheet columns** — they are matched by name. Adding new
columns is safe.

### If a sheet is unreachable
The four `.csv` files in this folder are the offline fallback. If a published sheet
fails to load, the page silently falls back to the matching local file, so the
workbook never breaks.

### Republishing a sheet
Google Sheets → File → Share → Publish to web → CSV → copy the link into `SHEETS`.
Published sheets are cached by Google for a few minutes; edits appear on the next
load after that.

## Known third-party limits
- Snorkl (`student.snorkl.app`) sends `X-Frame-Options`, so it cannot be embedded.
  The assignment popup deliberately opens it in a new tab.
- YouTube videos play in-page; a video whose owner disabled embedding shows
  "Watch on YouTube" instead. Each popup carries a new-tab fallback link.
- Lessons 17 and 21–24 have no video link yet. Those pages show a
  "Lesson video coming soon" chip; adding the link in the lessons sheet makes the
  green button appear, no code change.
