# FIFA World Cup 2026 Score Collector — Design Spec

**Date:** 2026-06-11
**Status:** Draft

## Overview

A command-line Java application that scrapes FIFA World Cup 2026 match results from a public website and outputs per-country score summaries to a local JSON file. The user runs it manually whenever they want updated scores.

## Goals

- Scrape final match scores (team names + goals) from a public football results page
- Aggregate results per country: wins, draws, losses, goals scored, goals conceded
- Output structured data to a JSON file
- Run as a simple CLI tool via Maven

## Non-Goals

- No scheduled/automated runs
- No detailed match stats (possession, shots, cards)
- No individual goal scorer tracking
- No database storage
- No web UI or REST API

## Architecture

```
┌─────────────┐     ┌──────────────┐     ┌────────────────┐     ┌──────────────┐
│  CLI Entry   │────▶│   Scraper    │────▶│  Aggregator    │────▶│  JsonWriter  │
│  (Main)      │     │  (Jsoup)     │     │  (by country)  │     │  (output)    │
└─────────────┘     └──────────────┘     └────────────────┘     └──────────────┘
```

### Components

#### 1. `MatchResult` (Data Model)

A plain Java class representing one match:

- `String teamA` — name of team A
- `String teamB` — name of team B
- `int scoreA` — goals scored by team A
- `int scoreB` — goals scored by team B
- `String stage` — group stage, round of 16, quarter-final, etc.
- `String date` — match date as a string (e.g., "2026-06-11")

#### 2. `WorldCupScraper` (Scraper)

Responsible for fetching and parsing HTML from a target website.

- Uses Jsoup to connect to a football results page (e.g., ESPN FIFA World Cup 2026 schedule/results)
- Parses the HTML structure to extract match results
- Returns a `List<MatchResult>`
- The target URL is configurable (passed as a constant or CLI argument)
- Handles HTTP errors and parsing failures gracefully with clear error messages

#### 3. `CountrySummary` (Aggregation Model)

Per-country aggregated stats:

- `String country`
- `int matchesPlayed`
- `int wins`
- `int draws`
- `int losses`
- `int goalsScored`
- `int goalsConceded`
- `int goalDifference` (computed)

#### 4. `ScoreAggregator` (Business Logic)

- Takes a `List<MatchResult>` and produces a `Map<String, CountrySummary>`
- Groups matches by country, computing W/D/L and goal tallies for each

#### 5. `JsonOutputWriter` (Output)

- Serializes the country summaries to a JSON file using Gson
- Default output path: `world-cup-2026-scores.json` in the project root
- Overwrites on each run

#### 6. `ScoreCollectorApp` (CLI Entry Point)

- `main()` method that orchestrates: scrape → aggregate → write
- Prints a summary to console (number of matches found, countries, etc.)
- Exit code 0 on success, 1 on failure

## Dependencies (Maven)

| Dependency | Purpose |
|---|---|
| `org.jsoup:jsoup:1.17.2` | HTML parsing and scraping |
| `com.google.code.gson:gson:2.11.0` | JSON serialization |
| `junit:junit:4.13.2` (existing) | Unit testing |

## Java Version Upgrade

The current project uses Java 8, which violates the repository's JDK 12+ requirement. As part of this work, the `pom.xml` will be updated to target **Java 17** (current LTS).

Changes:
- `maven.compiler.source` → 17
- `maven.compiler.target` → 17

## Package Structure

```
src/main/java/com/example/
├── ScoreCollectorApp.java       # CLI entry point
├── model/
│   ├── MatchResult.java         # Single match data
│   └── CountrySummary.java      # Per-country aggregation
├── scraper/
│   └── WorldCupScraper.java     # Jsoup-based HTML scraper
├── aggregator/
│   └── ScoreAggregator.java     # Groups results by country
└── output/
    └── JsonOutputWriter.java    # Writes JSON to file

src/test/java/com/example/
├── scraper/
│   └── WorldCupScraperTest.java
├── aggregator/
│   └── ScoreAggregatorTest.java
└── output/
    └── JsonOutputWriterTest.java
```

## Testing Strategy

- **Unit tests for `ScoreAggregator`** — feed known `MatchResult` lists, verify country summaries are correct
- **Unit tests for `JsonOutputWriter`** — verify JSON structure from known input
- **Unit tests for `WorldCupScraper`** — use saved HTML fixtures (local HTML files) to test parsing without hitting the network
- Existing `HelloWorldTest` remains untouched

## Output Format

`world-cup-2026-scores.json`:

```json
{
  "lastUpdated": "2026-06-11T12:00:00",
  "matchesCollected": 32,
  "countries": [
    {
      "country": "Brazil",
      "matchesPlayed": 3,
      "wins": 2,
      "draws": 1,
      "losses": 0,
      "goalsScored": 7,
      "goalsConceded": 2,
      "goalDifference": 5
    }
  ]
}
```

## Error Handling

- **Network failure**: Print error message with URL and HTTP status, exit with code 1
- **Parse failure**: If HTML structure doesn't match expectations, print warning and skip unparseable matches; continue with what was found
- **No matches found**: Print informational message, write empty JSON array, exit with code 0

## Scraping Target

The specific URL to scrape will be determined during implementation based on which site has the most stable, scrapeable HTML structure for World Cup 2026 results. ESPN's schedule/results pages are a strong candidate due to their relatively clean HTML tables. The scraper class isolates this concern so the target can be changed easily.
