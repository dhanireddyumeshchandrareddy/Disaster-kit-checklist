# Disaster Kit Checklist
DataSpark Challenge - Project #74 (Safety & Community). A zero-dependency Python 3.10+ terminal app to
build a disaster-preparedness checklist, track packed items, and report readiness.

## Run
```
python main.py            # Windows: py main.py
python -m unittest discover -s tests     # run the tests
```
Tip for first run: choose **9 - Load sample data** (44 realistic entries), then explore.

## Features
| Requirement | Where |
|---|---|
| Menu-driven terminal UI (ANSI colours, paginated tables) | `disasterkit/cli.py` |
| Defensive validation (loops until valid, never crashes) | `disasterkit/validation.py` |
| Core logic: checklist, completion, readiness score | `disasterkit/logic.py` |
| JSON persistence, atomic save, corrupt/missing file recovery | `disasterkit/storage.py` |
| Search by keyword, category, priority, status | `logic.search`, menu 5 |
| Executive summary (totals, averages, per-category, top gaps) | `logic.summary`, menu 6 |
| Warnings: expired / expiring (30 d), high-priority gaps, anomalies | `logic.warnings`, menu 7 |
| Export Markdown, text audit log, CSV | `disasterkit/reports.py`, menu 8 -> `exports/` |
| Bulk CSV import with row-by-row error log | `disasterkit/importer.py`, menu 10 -> `logs/` |

## Readiness score
Each item contributes `min(have/needed, 1)` weighted by priority (High=3, Medium=2, Low=1).
Ratings: >=90 Ready, >=70 Nearly ready, >=40 In progress, else At risk.

## Data
- `data/sample_dataset.csv` / `.json` - 44 entries incl. boundary values (qty 999, qty 0), expired and expiring items.
- `data/legacy_import_sample.csv` - deliberately messy file (blank name, bad numbers, bad date, unknown category, duplicate) to demo the import error log.
- `data/checklist.json` - your live data (created on first run). Regenerate samples: `python make_sample_data.py`.

## Suggested 5-minute demo
1. Launch -> branded welcome screen.  2. Menu 1: add an item, type bad values (blank name, `abc`, `-5`, `2027-13-45`) and show them being caught.
3. Menu 9 then 6: executive summary.  4. Menu 7: warnings.  5. Exit (0), relaunch: data persists.  6. Menu 8: export report; menu 10: import the messy CSV and show the error log.

## Project layout
```
main.py  make_sample_data.py  requirements.txt  README.md
disasterkit/  (models, validation, storage, logic, reports, importer, cli)
data/  tests/  exports/  logs/
```
