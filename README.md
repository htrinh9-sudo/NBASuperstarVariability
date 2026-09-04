# How Does Scoring Variability Among NBA Superstars Affect Winning?

An exploratory analysis of whether superstars who score consistently produce better team results than superstars who swing between huge games and quiet ones.

This was my first data science project. A follow-up that revisits the same question with cross-validated machine learning lives in [NBASuperstarImpact](https://github.com/htrinh9-sudo/NBASuperstarImpact).

## Question

> Does game-to-game scoring variability among NBA superstars relate to team winning percentage and playoff advancement?

## Data and definitions

Built on a box-score dataset covering NBA games from 1947 to the present, filtered to 2010 onward and to appearances of at least 10 minutes.

**Superstar season:** a player averaging ≥30 PPG across ≥50 games in a season.

**Variability:** the standard deviation of a player's points across that season's games.

**Outcomes:** team regular-season win percentage, and playoff games played as a proxy for postseason advancement.

## Method

Aggregate to player-season level, compute per-player scoring mean and standard deviation, join team win percentage and playoff appearances, then examine the relationships through a scatter plot of variability against win percentage, a box plot comparing groups split at the median standard deviation, and an interactive Plotly scatter of variability against playoff games.

## What I'd change

Two genuine bugs that I found on review, both worth documenting:

**1. The team win percentage join ignores season.** Win percentage is computed with `groupby("playerteamName")` only, producing a franchise's average across the entire filtered period, which is then merged onto per-season player rows. A player's 2015 season gets joined to his team's 2010-present average rather than that team's 2015 record. Every win percentage figure in this analysis is affected. The fix is grouping by team *and* season.

**2. Season year is derived from calendar year.** `season_year` comes from the calendar year of the game date, but NBA seasons run October through June. Every season is therefore split across two `season_year` values, with October–December landing in one and January–June in the next. Season boundaries need to be defined explicitly rather than read off the date.

Beyond the bugs, the 30 PPG threshold is arbitrary and yields a small group, playoff games played is a weak proxy for advancement, and the analysis stops at visual inspection without any statistical testing — nothing here establishes whether an apparent relationship is distinguishable from noise.

The follow-up project addresses most of this: proper per-season joins, a possession-normalized impact metric instead of raw points, significance testing, and cross-validated models.

## Files

- `Project1_Code.ipynb` — full analysis notebook

## Running it

```bash
pip install pandas numpy matplotlib seaborn plotly
jupyter notebook Project1_Code.ipynb
```

Requires `playerstatistics.csv` in the same directory.

## Stack

Python · pandas · NumPy · matplotlib · seaborn · Plotly
