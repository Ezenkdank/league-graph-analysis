# Processed data dictionary

## `affiliations.csv`

The final table used directly by the notebook. A player-team lineage pair may
appear in multiple rows when it has been observed under different historical
team names; the notebook aggregates these rows into a single graph edge.

Main fields:

- `player_node`, `team_node`: stable graph node identifiers.
- `player_name`, `team_name`: display labels.
- `team_name_at_time`: team name observed in the corresponding matches.
- `team_name_history`: name history mapped to the lineage.
- `group`: `major` or `wildcard`.
- `leagues`: league code.
- `first_date`, `last_date`, `active_years`: observed time range.
- `games_*`, `wins_*`, `minutes_*`: domestic, international, and total
  aggregates.
- `domestic_win_rate`: `wins_domestic / games_domestic`.

Size: 3,561 rows, 2,029 unique players, and 244 team lineages.

## `affiliations_clean.csv`

The deduplicated player-team table before team lineage mapping. It contains
3,622 rows, 2,076 players, and 278 team identities. Each player-team pair is
unique within this file.

## `team_alias_map.csv`

Maps original team names and identifiers to canonical team lineages.

- `original_team_name`, `original_team_node`: identity in the source record.
- `canonical_team_name`, `canonical_team_node`: lineage used in the analysis.
- `changed`: whether the mapping changes the identity.
- `cluster_size`: number of names assigned to the same lineage.
- `method`: combined mapping evidence: `normalized_name`,
  `cargo_short_plus_roster`, `manual_review`, or a `|`-separated combination.

## Limitations

- These files are not raw API responses.
- Player names are public professional esports identities; some display labels
  may include real names in parentheses.
- The `international` fields indicate international participation but do not
  distinguish Worlds from MSI on their own.
- Recorded rosters may include short-term substitutes.
- Some team lineage mappings were manually reviewed, but a separate rationale
  log for those decisions was not retained.

See [`PROVENANCE.md`](PROVENANCE.md) for reconstruction notes and
[`LICENSE.md`](LICENSE.md) for the data license and attribution.
