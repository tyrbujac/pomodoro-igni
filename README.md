# Pomodoro (Igni)

A small Pomodoro timer for the browser, written in [Igni](https://github.com/tyrbujac/Igni), a UI language that compiles to Flutter. 25-minute focus sessions, 5-minute breaks, Start / Pause / Resume / Reset, Focus / Break mode buttons, a clear countdown. When a phase ends the app says so and offers the next phase; nothing starts on its own. No backend, persistence, audio or notifications: reloading the page resets it. Dark mode follows the system; the layout fits phone and desktop widths.

Timekeeping is by timestamp: Start and Resume record the second the phase ends at, and a one-second timer re-reads the clock and compares, so a tab throttled in the background shows the right time on its next tick instead of drifting.

Source: `app.igni`. Tests: `app.test.igni` (thirteen, run against a frozen clock).

## Versions this was built with

| | |
|---|---|
| Igni | commit `681c590f3f7af0c4dc4ae60cf4e6a77adfa1f195` (spec v0.22.1, 2026-09-21) |
| Flutter | 3.44.2, stable channel, framework `c9a6c48423`, engine `77e2e94772` |
| Dart | 3.12.2 |
| Node | 22.x (for the Igni CLI) |

The Igni revision matters: two CLI fixes made that day are what keep the release bundle self-contained (no dev reload script, CanvasKit served from the bundle rather than Google's CDN).

## Rebuild

```bash
# 1. Igni CLI at the pinned revision
git clone https://github.com/tyrbujac/Igni.git
cd Igni && git checkout 681c590f3f7af0c4dc4ae60cf4e6a77adfa1f195
cd transpiler && npm install
# put `igni` on your PATH, e.g. a wrapper that runs: npx tsx <Igni>/transpiler/src/igni.ts "$@"

# 2. Flutter 3.44.2 on PATH (flutter --version)

# 3. This app
cd pomodoro-igni
igni test                 # thirteen tests
igni run localhost        # dev preview at the printed URL
igni build web            # release bundle in dist/web/
```

`dist/web/` and the Flutter scaffold in `.igni/` are generated and ignored by git.

## Publish an update

Deploys are manual from the local release build; nothing builds on Netlify.

```bash
igni test && igni build web
netlify deploy --prod     # publish dir comes from netlify.toml (dist/web)
```

Before deploying, `grep -c IGNI_RELOAD dist/web/index.html` should print `0` and `flutter_bootstrap.js` should contain `"useLocalCanvasKit":true`. Both hold at the pinned Igni revision.

## Record

Built as an experiment in the Igni repository (`tests/v0.22.1-pomodoro/README.md` there has the timings and the bugs found): first version to a verified release build in 13m20s, then a 4m23s revision that made the mode pills real buttons. Deployment was timed separately.
