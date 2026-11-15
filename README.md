# Flame in Freefall Atlas

**Team Shaheen-e-Falak · NASA Space Apps Challenge 2026 · Faisalabad, Pakistan**
Challenge: *Flame in Freefall*

An explainable, web-based dashboard that summarizes, ranks and compares NASA's microgravity combustion investigations for spacecraft and Moon-habitat fire safety.

**Live demo:** `https://<your-github-username>.github.io/flame-in-freefall/`

## What it does
- **Buoyancy flame simulator (hero):** drag gravity from orbit (0 g) through Moon and Mars to Earth (1 g) and watch a candle flame turn from a dim blue sphere into a yellow teardrop. It's a visual model for intuition, not a combustion solver; partial-gravity shapes are illustrative because NASA has no partial-gravity combustion data yet.
- **Mission timeline:** 30 years of NASA combustion experiments by platform (Shuttle, ISS glovebox, ISS combustion rack, Cygnus).
- **Scenario ranking:** enter a crew environment (oxygen %, pressure, ventilation, gravity, material, hazard), or pick a preset (ISS cabin, Moon habitat, exploration atmosphere, spacesuit). Each of 21 NASA investigations gets a score out of 100 with a visible breakdown and plain-language reasons.
- **Safety brief:** rule-based synthesis flags conditions outside every tested envelope (e.g. 100% O₂ suits, partial gravity) and surfaces the most relevant findings.
- **Test coverage map:** oxygen-vs-pressure chart showing where NASA has actually tested, with crew environments overlaid.
- **Explore & compare:** filter by hardware (CIR/MDCA, CIR/ACME, MSG, Cygnus, Shuttle), fuel and hazard; compare up to three investigations side by side.
- **Insights, gaps and Earth applications:** eight source-linked findings, four evidence gaps, three Earth spin-offs.
- **Data for modelling:** quantitative data points (BASS-II limits, Saffire CO/CO₂ rises, burn durations, alarm timing) downloadable as CSV/JSON.

## How the ranking works
Score = condition coverage (40) + fuel match (15) + hazard match (20) + fire-safety relevance (15), with optional BM25 keyword relevance (+25, rescaled to 100). Ranges a source does not report get partial credit and are labelled as unreported, never guessed.

## Data sources
- NASA, *Combustion in Reduced Gravity: A Researcher's Guide to ISS Combustion Science* (NP-2015-10-034-JSC)
- Saffire IV–VI results, ICES-2024-365 (NTRS 20240002981) and ICES 2024 presentation (NTRS 20240007488)
- BASS-II results poster, ASGSR 2014 (NTRS 20160000593)
- ACME project paper, JASMAC 2021 (NTRS 20210021781)
- NASA Physical Sciences Informatics (PSI) repository
- Space Apps 2026 Flame in Freefall challenge statement

## Limitations
- Hand-curated dataset (21 investigations, 16 quantitative data points). Many ACME test matrices aren't reported in the sources used.
- Research and education tool, not an operational safety system.
- Next step: ingest raw PSI datasets and the NTRS API to fill unreported ranges automatically.

## Run locally
Open `index.html` in any browser. No build step, no dependencies, works offline.

## AI disclosure
Built with help from an AI assistant (Claude) for coding and drafting. Data values were checked against the NASA sources above.

## Fire early-warning detector (ML)
Saffire VI showed smoke reached the alarm threshold only after most toxic gas was released. The detector:
1. **Learns the cabin baseline** (mean and noise of each sensor) from the first seconds. It's unsupervised and needs no fire labels.
2. **Detects change** with robust CUSUM on each sensor (clipped z-scores, 3-reading persistence).
3. **Fuses by combustion physics:** it alarms when CO rises together with CO₂ or smoke. CO₂ alone (crew breathing) or a smoke blip alone (dust) is ignored.

Demo scenarios: a PMMA fire shaped to Saffire VI-3's reported totals, crew exercise with no fire (no false alarm at default sensitivity), and a slow smoulder. These demo traces are **illustrative synthetic data**, not flight data. Upload your own log (`time_s, co2_ppm, co_ppm, smoke_mg_m3`); `sample_sensor_log.csv` is included for trying it out.

## Ask the Atlas
Extractive question answering: questions are matched (BM25 with simple synonym expansion) against curated NASA findings and insights, and the best sentences are returned with their source. Nothing is generated.

## Live NASA literature search
The Explore tab queries the NASA Technical Reports Server API (`ntrs.nasa.gov/api/citations/search`). If the browser can't reach it, it falls back to a link to the same search on ntrs.nasa.gov.
