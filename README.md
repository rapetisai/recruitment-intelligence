# Recruitment Intelligence — Football Scouting Analytics

A player recruitment analytics dashboard built to demonstrate the analytical
workflow behind modern football scouting: event-data metrics, tracking-derived
spatial analysis, player similarity modeling, and statistically-adjusted
percentiles, wrapped in a scouting tool a recruitment department could
plausibly use.

**[View the live dashboard](./scouting_dashboard.html)** — a single
self-contained HTML file, no build step or server required.

## What this is

This project was built as a portfolio piece to demonstrate the intersection of
data science and football recruitment analysis: turning raw event and tracking
data into the kind of shortlist, player dossier, and percentile profile a
recruitment analyst would actually hand to a scout.

It combines:
- **Event-data metrics** — per-90 stats engineered from StatsBomb-style event
  data (progressive passes, pressures, shot creation)
- **Tracking-derived spatial metrics** — pitch control, team compactness, and
  space-creation scores from player tracking coordinates
- **Statistical adjustments** — possession normalization, Bayesian shrinkage
  for small-sample reliability, and league-strength scaling, so raw counting
  stats aren't mistaken for adjusted ability estimates
- **Player similarity modeling** — PCA and cosine similarity across
  z-scored per-90 metrics to surface statistically comparable players
- **A real historical benchmark** — verified 2021–22 Indian Super League
  season stats (Hyderabad FC's title-winning campaign), used as a sanity
  check against the sample shortlist rather than presented as sample output

## What's real and what's illustrative

Being upfront about this matters more than it looks like it should:

- **The shortlist, player names, and per-90 stats are synthetic**, built to
  demonstrate the pipeline and dashboard, not real scouting output. Player
  names include a mix of invented names for anonymity.
- **The "Real season benchmark" section is genuine, sourced data** — actual
  2021–22 ISL statistics for Bartholomew Ogbeche, Javier Siverio, Joel
  Chianese, and Juanan, cited to Wikipedia and the Indian Super League's
  official site. It's deliberately kept to standard box-score stats
  (appearances, goals, assists) rather than the advanced metrics used
  elsewhere, since event-level data for that league and season isn't
  publicly available.
- **The pitch-control and Voronoi diagrams use illustrative single-frame
  positions**, not continuous tracking data, to demonstrate the technique.

See [`methodology.md`](./methodology.md) for a full breakdown of every
adjustment used, what each one corrects for, what it doesn't, and where a
human scout's judgment should override the model. That document is the more
important read if you're evaluating the analytical thinking behind this
project rather than just the interface.

## Tech stack

- **Frontend**: vanilla HTML/CSS/JS — no framework, no build step, opens
  directly in a browser
- **Charts**: [Chart.js](https://www.chartjs.org/) (loaded via CDN)
- **Syntax highlighting**: [highlight.js](https://highlightjs.org/) (loaded
  via CDN)
- **Fonts**: Oswald, Inter, and JetBrains Mono via Google Fonts
- **Data pipeline (reference code, in the dashboard's "Code" tab)**: Python
  with pandas, scikit-learn, statsmodels, and scipy — designed around
  [StatsBomb's open event data](https://github.com/statsbomb/open-data) and
  [Metrica Sports' open tracking data](https://github.com/metrica-sports/sample-data)
- **App layer (reference code)**: Streamlit, for the scouting shortlist tool
  the dashboard's UI is modeled on

## Repository structure

```
.
├── scouting_dashboard.html   # the dashboard — open this in a browser
├── methodology.md            # full write-up of every statistical adjustment
└── README.md                 # this file
```

The dashboard's "Code" tab contains the full reference implementation
(`metrics.py`, `adjustments.py`, `tracking.py`, `similarity.py`,
`scouting_app.py`, `dossier.py`) inline, with a copy button on each file, so
the pipeline code travels with the visual deliverable in one place.

## Running it

No installation needed for the dashboard itself:

```bash
# clone the repo, then just open the file
open scouting_dashboard.html        # macOS
xdg-open scouting_dashboard.html    # Linux
start scouting_dashboard.html       # Windows
```

Or double-click it in your file browser. It's fully self-contained — all
images are embedded as base64, and the only external dependencies are the
CDN-hosted fonts, Chart.js, and highlight.js, which need an internet
connection to load.

To actually run the Python pipeline behind it (rather than just read the
reference code in the "Code" tab):

```bash
pip install pandas numpy scikit-learn scipy statsmodels streamlit statsbombpy
```

Pull the code blocks from the dashboard's Code tab into a `src/` directory
matching the structure referenced in `methodology.md`, point `data_loader.py`
at [StatsBomb's open data](https://github.com/statsbomb/open-data), and run:

```bash
streamlit run app/scouting_app.py
```

## Known limitations

Documented in full in `methodology.md`, but the headline gaps:
- No shot- or pass-quality weighting (xG/xT) — raw counts only
- League-strength coefficients are illustrative placeholders, not calibrated
  against real cross-league outcomes
- No opponent-strength or role-change adjustment
- Similarity modeling compares aggregated rates, not action sequences

This is a portfolio demonstration of the analytical approach, not a
production recruitment tool — the methodology document is explicit about
where a real deployment would need more rigor and where a human scout's
judgment should always sit above the model's output.

## License

MIT — feel free to fork, adapt, or build on this for your own portfolio.
