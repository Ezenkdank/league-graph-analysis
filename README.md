# League of Legends player-team graph analysis

This repository explores relationships between professional League of Legends
players and team lineages as an undirected, weighted bipartite graph. The main
analysis is available in [`code/league-graph.ipynb`](code/league-graph.ipynb).

## Scope

The dataset covers the 2015–2019 seasons across ten leagues: `CBLOL`, `LCK`,
`LCL`, `LCS`, `LEC`, `LJL`, `LMS`, `LPL`, `OPL`, and `TCL`.

The notebook builds and analyzes the player-team graph, examines centrality and
cross-league connections, compares five community detection methods, and
explores the observed relationship between network position and competitive
results.

## Run

Python 3.12 is recommended.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m jupyterlab code/league-graph.ipynb
```

Tables and figures are stored in the executed notebook; running it does not
create additional output files.

## Data

The processed tables were derived from Leaguepedia's MediaWiki Cargo API. The
original raw responses and collection script are unavailable, so the CSV files
should be treated as a fixed research snapshot rather than a current or fully
reproducible copy of Leaguepedia.

See [`data/README.md`](data/README.md) for the data dictionary and
[`data/PROVENANCE.md`](data/PROVENANCE.md) for the documented reconstruction of
the collection and transformation process.

## Data license and attribution

Leaguepedia-derived data and the processed CSV files are provided under CC
BY-SA 3.0 with source attribution. See
[`data/LICENSE.md`](data/LICENSE.md) for details.

This project is not endorsed by Riot Games. League of Legends and related
trademarks belong to their respective owners.
