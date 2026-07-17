# 词径 · Wordpath

A vocabulary trainer built from your two Gaokao word lists.

- **887** words — 高中英语核心词汇887必背 (with exam-frequency counts and 精准出处 sources)
- **1441** words — 英语高考高频核心词 (1278 high-frequency + 187 connectives + 160 adjectives, all with Chinese example translations)
- **2328** total, studiable separately or combined

**Password:** `gaokao2026`

---

## Run it locally

The page fetches its word list, so it needs to be served over HTTP — opening
`index.html` by double-clicking will show a "can't load the word list" message.

```bash
cd vocab-site
python3 -m http.server 8000
```

Then open http://localhost:8000

---

## Put it online

Any static host works. No build step, no server code.

**GitHub Pages**
1. Create a repo, upload `index.html` and the `data/` folder.
2. Settings → Pages → Source: `main` branch, `/ (root)`.
3. Live at `https://yourname.github.io/reponame/` in a minute or two.

**Netlify / Vercel / Cloudflare Pages**
Drag the `vocab-site` folder onto their dashboard. Done.

---

## Change the password

`index.html`, near the bottom in the `CONFIG` block:

```js
const CONFIG = {
  password: 'gaokao2026',    // ← change this
  dataUrl:  'data/vocab.json',
  dailyGoal: 20,             // words per day
  ...
};
```

Note this is a front-door lock, not real security — the password sits in the
page source, so treat it as "keeps the casual visitor out," not "protects
private data."

---

## Swap in different words

Replace `data/vocab.json`. No code changes needed. Format:

```json
[
  {
    "i": "a1",                          // unique id (required)
    "d": 1,                             // deck: 1 or 2 (required)
    "w": "change",                      // word (required)
    "m": "v.改变 n.变化",                 // meaning (required)
    "p": "/tʃeɪndʒ/",                   // phonetic (optional)
    "s": ["v.", "n."],                  // parts of speech (optional)
    "f": 157,                           // exam frequency (optional)
    "e": [["The plan changed.", "计划变了。"]],  // examples: [en, zh] (optional)
    "r": "【2019年全国卷I完形】"           // source (optional)
  }
]
```

Deck names come from `CONFIG.decks` in `index.html`.

---

## What's in it

**Study modes**
| Mode | What it does |
|---|---|
| Flashcards | Flip and self-grade (Again / Hard / Good / Easy) |
| Review | Only words the scheduler says are due today |
| EN → 中文 | Pick the meaning |
| 中文 → EN | Recall the word |
| Listening | Hear it, pick it |
| Spelling | Type what you hear |
| Fill the gap | Complete the real exam sentence |
| Time attack | 60-second sprint |

**Spaced repetition** — 8 levels, intervals 1→2→4→8→16→32→64 days. "Again"
drops a word to level 0 and shows it again the same day; "Easy" jumps two
levels. The Review badge shows how many are due.

**Also** — progress ring, accuracy, day streak, 90-day heatmap, daily goal,
10 achievements, starred words, search in English or Chinese, filters
(starred / not started / struggling / mastered), light and dark mode.

**Text-to-speech** uses the browser's built-in voices. Quality varies by
device — Chrome and Safari on Mac/iOS sound best, and voices load after the
first user interaction on some browsers.

**Keyboard** — `Space` flips a card, `1`–`4` grades it, `Esc` exits a session.

---

## Notes

- Progress saves to `localStorage` — per device, per browser. Clearing site
  data resets it. There's a reset button at the bottom of Progress.
- The two lists share 556 words. In the combined deck they're kept as separate
  cards, since each source gives different examples and different information
  (one has exam frequencies, the other has Chinese translations).
- 2,197 of 2,328 words work in Fill-the-gap — the rest appear in their example
  in a form the blanker can't match, so they're skipped in that mode only.
