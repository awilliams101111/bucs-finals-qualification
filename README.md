# BUCS Finals Qualification

Python utility to scrape BUCS indoor archery qualification results from ianseo.net, combine categories across tournaments, calculate national qualification thresholds, and export a multi-sheet Excel report.

## What it does

- Reads parameters from `inputParams.txt`:
	- `TournamentIDs`
	- `finalsCapacity`
	- `ClubCode`
- Scrapes all `IQ` category permutations (`R/B/C/L` × `E/N` × `O/W`) for each tournament.
- Cleans and combines all result rows into one dataset.
- Computes:
	- `class` from `class (with Exp)`
	- national rank per class (tie-break: `Tot.` then `Hits` then `Golds`)
	- class-specific placelimits with minimum-8 allocation logic
	- qualification score per class
	- club-specific safety margin (%)
- Exports `bucs_finals_qualification.xlsx` with:
	- class result sheets (`RO`, `RW`, `BO`, `BW`, `CO`, `CW`, `LO`, `LW`)
	- `Qualification_Entry_Numbers`
	- `Rank_to_Qualify`
	- `Score_to_Qualify`
	- `Club_Results`

## Run

From the repo root:

```powershell
./.venv/Scripts/python.exe main.py
```

## Notes

- Missing/empty category pages are silently skipped.
- `20y-1` and `20y-2` values are reduced to the score before `/`.
- Column headings in output sheets are title-cased.

## Disclaimer

All code in this repository was written by agentic AI as an exercise in learning agentic coding.
