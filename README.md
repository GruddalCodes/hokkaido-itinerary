# Hokkaido 2026 — family itinerary

One `index.html`. No build step, no npm, no backend, no accounts. Open it anywhere.

## For the family

Open the link, tap a stretch → a day → a place. Stars (★) and ticks (✓) are saved
in your own browser and never change the plan for anyone else.

## Languages

A 中文 / EN button in the header switches the whole shell and itinerary between
English and 简体中文, and remembers the choice. It is baked in — no translation
service, no network call — so it works offline too.

The 179 place descriptions stay in English by design; their Japanese names
(藻岩山, 大通公園) already read as Chinese. Author mode has a 中文 field beside
every English one, so translations stay in step when you edit.

## For me (editing)

| URL | What it does |
| --- | --- |
| `index.html` | the plan, read-only |
| `index.html?edit=1` | author mode — rename stretches and days, add/edit/reorder/delete blocks and days |
| `index.html?selftest=1` | 67 checks over the data, the logic, the rendering and the translations |

Every edit saves to your browser the moment you make it — there is no Save button.
The yellow bar tells you whether those changes have been **published to the family**,
not whether they are safe. Publishing loop:

1. Open `?edit=1` and make the changes by tapping. They save to *this browser only*.
2. Hit **Copy JSON** in the yellow bar.
3. Paste it over the `const ITINERARY = {…};` block near the top of `index.html`.
4. Open `?selftest=1`. Green means safe to ship.
5. Commit and push — GitHub Pages serves it.

## When an edit does not show up

1. **Cache.** This one file is the whole app, so browsers hold on to it. Hard-reload
   (Ctrl+Shift+R), or add `?v=2` to the URL. The page asks to revalidate, but a
   phone that has the tab open may still need a pull-to-refresh.
2. **A draft in your browser.** If the bar at the top is showing, you are looking at
   your own saved draft, not the file. Hit **Discard**.

If the bar is **red**, the draft was started before `index.html` last changed —
copying it back would undo those file edits. That is how a set of translations
got lost once. Check the file before publishing, or discard and redo the edits.

`?edit=1` is friction, not security: the published file is public either way, and
nothing in author mode can change what anyone else sees until you commit.

## How it is put together

- **`ITINERARY`** — the only block edited by hand. Legs → days → blocks, the model
  lifted from [Offday Adventure 30](../offday.adventures30)'s itinerary module.
- **`DATA`** — 179 places copied verbatim from the [Hokkaido Field Guide](../hokkaido-trip),
  never hand-edited here.
- A block with `place:'s-odori'` pulls its name, 漢字, hours, admission, transit,
  description, open-now status and map link straight from `DATA`. Nothing is
  retyped, so fixing the directory fixes the itinerary.
- A block with `title:` instead is free text — flights, meals, check-ins.

## Publishing

Push to `main`, then Settings → Pages → Deploy from branch → `main` / root.
The file also works straight off disk (`file://`) as an offline fallback — it
degrades to read-only if the browser blocks storage.
