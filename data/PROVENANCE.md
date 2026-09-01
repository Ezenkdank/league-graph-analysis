# Data provenance and reconstruction notes

## Directly verifiable facts

The following can be verified directly from the files in this repository:

- The data covers the 2015–2019 seasons. Dates range from
  2014-11-15T00:00:00Z to 2019-09-16T00:00:00Z; the earliest date belongs to
  records preceding the 2015 season.
- The included leagues are CBLOL, LCK, LCL, LCS, LEC, LJL, LMS, LPL, OPL, and
  TCL.
- Each player-team record contains games, wins, duration, first and last dates,
  and active years, separated into domestic and international totals.
- Numeric totals from affiliations_clean.csv are preserved in affiliations.csv
  after alias and lineage consolidation: 149,937 player-game records, 75,104
  player-wins, and approximately 5,294,622.88 player-minutes.
- team_alias_map.csv contains method labels such as normalized_name,
  cargo_short_plus_roster, and manual_review. This shows that team names were
  consolidated into lineages using Cargo short names, roster overlap, and
  manual review in addition to basic text normalization.
- The notebook starts from the final processed affiliations.csv; neither the
  raw API responses nor the collection code is present in the repository.

## Verifiable source system details

Leaguepedia exposes Cargo through the MediaWiki API cargoquery action:

https://lol.fandom.com/api.php?action=cargoquery&format=json&...

Leaguepedia's published Cargo schemas are consistent with the available data
fields:

- ScoreboardPlayers provides Link, Team, PlayerWin, DateTime_UTC, OverviewPage,
  GameId, and GameTeamId.
- ScoreboardGames provides DateTime_UTC, Gamelength, Gamelength_Number,
  OverviewPage, and game/team fields.
- Tournaments provides OverviewPage, DateStart, League, and Region.
- Teams and Teamnames provide long and short team names and team page
  identifiers.

Schema documentation:

- https://lol.fandom.com/wiki/Module:CargoDeclare/ScoreboardPlayers
- https://lol.fandom.com/wiki/Module:CargoDeclare/ScoreboardGames
- https://lol.fandom.com/wiki/Module:CargoDeclare/Tournaments
- https://lol.fandom.com/wiki/Module:CargoDeclare/Teams
- https://lol.fandom.com/wiki/Module:CargoDeclare/Teamnames

## Strong inference: likely collection pipeline

This section reconstructs the likely process from the available schemas and
tables; it is not an exact record of the missing collection script.

1. ScoreboardPlayers was queried for player and team identities and win status
   for each game.
2. ScoreboardGames was joined for game duration and date, and Tournaments for
   league information, likely through GameId and OverviewPage.
3. Records were limited to the 2015–2019 seasons and the ten selected leagues.
   The domestic/international split was likely derived from tournament or
   league classification.
4. Game duration was converted to minutes; games, wins, duration, first and
   last dates, and active years were aggregated by player and team.
5. Player names were converted into stable player_node identifiers.
6. Team short names and name variants were mapped to lineage clusters using
   Teams/Teamnames data, roster overlap, and manual review.
7. The mapping was stored in team_alias_map.csv and applied to produce the
   final affiliations.csv.

## Unknowns required for full reproduction

The following cannot be established without the raw data or original script:

- Exact Cargo fields, where, join_on, limit, and pagination parameters.
- The complete definition of domestic and international tournaments.
- Rules for missing, cancelled, or remade games.
- The exact player-name normalization algorithm.
- Rationales for manual_review team lineage decisions.
- The API collection date and corresponding Leaguepedia snapshot.

A future collection should preserve raw JSON responses in an immutable
data/raw/<collection-date>/ directory and record query parameters, UTC
collection time, pagination, User-Agent, and SHA-256 file hashes in a manifest.
Responses should be cached and requests rate-limited rather than repeatedly
sending identical queries to the API.
