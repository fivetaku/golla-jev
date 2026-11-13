# Changelog

## 0.1.0 — 2026-11-13
- First public release. Rebuilt from the author's personal Jev workflow skill and renamed 골라제브 (golla-jev).
- Idea mode: when the scope is broad, list 3–5 candidate "pick one" moments before designing one in full.
- Bundled the Korean office benchmark (`skills/golla-jev/bench/`, 108 items, 5 workflows) as a reusable test template, re-measured on jev-1.14.0 (ko 96.3%, en 93.5%, dev threshold 0.70 → holdout 57/59 auto-handled with 0 errors). The API key is read only from the `TYPESAFE_API_KEY` environment variable.
- Added three example designs (`examples/`) produced by the plugin itself and reviewed against a checklist.
- Evidence now cites only the bundled benchmark, public repositories and docs.typesafe.ai. Image input is documented as Dreamer-ready (not production) and suggested only as a test condition.
- README in English, Korean, Japanese, Chinese and Spanish.
