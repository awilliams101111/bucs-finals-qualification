# BUCS Finals Qualification

Python utility to scrape BUCS indoor archery qualification results from ianseo.net, combine categories across tournaments, calculate national qualification thresholds, and export a multi-sheet Excel report.

## What it does

- Reads parameters from `inputParams.txt`:
	- `TournamentIDs` - 5 digit number from ianseo URL
	- `finalsCapacity` - Total archers shooting at finals, usually ~250
	- `ClubCode` - Code of club you want to see a summary of all results for, can be found from ianseo results sheets. 2373 for University of Bristol.
- Scrapes all `IQ` category permutations (`R/B/C/L` × `E/N` × `O/W`) for each tournament.
- Cleans and combines all result rows into one dataset.
- Computes:
	- National rank per class (tie-break: `Tot.` then `Hits` then `Golds`)
	- Class-specific placelimits with minimum-8 allocation logic
	- Qualification score per class
	- Club-specific safety margin (%) 
        - Rank relative to qualification cut off rank
        - Positive = qualified
        - Negative = not qualified
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
- Number of archers shooting at finals each year varies slightly

## Disclaimer

All code in this repository was written by agentic AI as an exercise in learning agentic coding.
