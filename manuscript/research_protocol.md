# Research Protocol

## Working title

**When Seconds Matter: Critical Time Thresholds and Move Quality in Online Chess**

Alternative descriptive title: **Critical Time Thresholds in Online Chess Decision-Making: A Lichess-Based Study of Move Accuracy Under Time Pressure**

## Author information

**Alexandr Tereshkov**  
Independent Researcher, Pavlodar, Kazakhstan  
ORCID: [0009-0002-2255-7777](https://orcid.org/0009-0002-2255-7777)

## 1. Purpose and contribution

This study will estimate how move quality changes as a player's remaining clock time decreases in rated online chess. Its main contribution is not merely to show that low time and mistakes are correlated, but to test whether the relationship contains identifiable nonlinear thresholds and whether those thresholds differ between blitz and rapid chess.

The intended contribution has four parts:

1. A reproducible move-level dataset combining clock information, player characteristics, position features, and engine evaluations.
2. A continuous estimate of the time-pressure curve rather than a comparison based only on arbitrary time bins.
3. A formal change-point analysis of candidate critical thresholds.
4. Robustness checks designed around the endogeneity of time use: difficult positions can both consume more time and produce worse moves.

The final article is intended as an English-language SSRN preprint. SSRN is a dissemination platform rather than a single journal style, so the manuscript will follow a conventional empirical-paper structure and use the supplied article as a benchmark for completeness, not as a document to copy.

## 2. Research questions

### Primary question

How does remaining clock time relate to move quality in rated online blitz and rapid chess?

### Secondary questions

1. Is the relationship gradual, or does move quality deteriorate sharply below one or more time thresholds?
2. Does the shape or location of the threshold differ between blitz and rapid games?
3. Does player strength moderate the effect of time pressure?
4. Are results robust to position difficulty, game phase, increment, and alternative definitions of a blunder?

## 3. Hypotheses

- **H1:** Lower remaining clock time is associated with greater engine-evaluated move loss.
- **H2:** Lower remaining clock time is associated with a higher probability of a blunder.
- **H3:** The relationship is nonlinear: the marginal deterioration in move quality becomes steeper below a critical time threshold.
- **H4:** The time-pressure curve differs between blitz and rapid games after remaining time is normalized by the initial time allocation.
- **H5:** Higher-rated players experience a smaller deterioration in move quality at the same level of time pressure.

All candidate threshold rules and outcome definitions will be fixed before the confirmatory analysis. Exploratory plots will be labeled as exploratory.

## 4. Data source and scope

The primary source is the official Lichess open database: <https://database.lichess.org/>. Lichess releases the exports under CC0. Standard-game PGNs contain clock comments at one-second resolution from April 2017 onward. A separate 2013-2021 export contains centisecond clock comments.

The official documentation states that only about 6% of exported games include Stockfish evaluations. Because the decision to request analysis may be selective, the primary study will not rely solely on those pre-existing evaluations. Instead, Stockfish evaluations will be recomputed for a pre-specified sample. Existing Lichess evaluations may be used for pipeline validation and a secondary sensitivity analysis.

### Inclusion criteria

- rated standard chess games;
- blitz or rapid time control;
- both players human (no `BOT` title);
- usable clock comments for the analyzed moves;
- valid initial position and legal move sequence;
- both ratings observed;
- minimum game length chosen before analysis to exclude aborted games.

### Exclusion criteria

- variants and nonstandard starting positions;
- correspondence, bullet, classical, and unlimited games in the primary analysis;
- games with malformed or internally inconsistent clocks;
- moves after a detected clock anomaly;
- positions for which engine analysis fails.

The sampling period, rating range, number of games, Stockfish version, search limit, and random seed will be frozen after a computational pilot and before confirmatory estimation.

## 5. Unit of analysis and variable construction

The unit of analysis is one player move.

### Exposure variables

- remaining clock time before the move;
- remaining clock time after the move;
- estimated thinking time for the move;
- remaining-time share: remaining time divided by the initial base time;
- time-control category: blitz or rapid;
- increment in seconds;
- pre-specified time bins for descriptive tables: `>60`, `30-60`, `10-30`, `5-10`, and `<5` seconds.

For increment games, estimated thinking time will account for the increment. Clock values will be validated because network lag, premoves, and one-second rounding can create zero or implausible estimates.

### Primary outcomes

1. **Win-probability loss:** the decrease in engine-implied winning chances caused by the move. This is preferred as the main continuous outcome because raw centipawn loss behaves poorly in positions that are already clearly winning or losing.
2. **Blunder indicator:** a binary indicator based on a pre-specified loss in evaluation or win probability.

### Secondary outcomes

- centipawn loss, winsorized only according to a pre-specified rule;
- indicators for losses above 100 and 200 centipawns;
- Lichess-style accuracy or an explicitly documented equivalent transformation;
- move rank relative to the engine's candidate moves, if computationally feasible.

### Covariates and position features

- player and opponent rating;
- rating difference;
- move number and game phase;
- side to move;
- material balance;
- absolute pre-move engine evaluation;
- legal-move count;
- engine gap between the best and second-best move as a proxy for decision difficulty;
- tactical/forcing-position indicators where defensible;
- time control and increment;
- opening move indicator;
- game and player identifiers for clustered or fixed-effects analysis.

## 6. Statistical analysis plan

### Descriptive analysis

- distribution of remaining time by time control and rating band;
- median and mean outcome values across pre-specified time bins;
- raw blunder rates with cluster-aware confidence intervals;
- within-game plots showing clock trajectories and move quality.

### Primary model

The primary specification will model blunder probability as a flexible function of log remaining time using restricted cubic splines or a generalized additive model. It will include time-control interactions and the pre-specified covariates above. Standard errors will be clustered at least by game; a player-level clustering or multiway procedure will be used where computationally feasible.

The main estimand will be the adjusted difference in predicted blunder probability between substantively meaningful time points, reported separately for blitz and rapid.

### Threshold model

A segmented logistic model will estimate whether the slope changes at a critical remaining-time value. Candidate change points will be evaluated on a training subset or through nested cross-validation. Confirmatory uncertainty intervals must account for threshold selection; the final paper will not treat a visually selected break as known in advance.

### Within-player and within-game analysis

Where repeated observations permit, fixed effects or hierarchical random effects will separate time-pressure variation from stable player skill. Within-game specifications will reduce confounding from game-level characteristics, though they do not by themselves solve position-level endogeneity.

### Interpretation

This is an observational study. The primary language will be "associated with" rather than "causes." A causal interpretation would require stronger identification, such as an exogenous clock shock or a defensible quasi-experimental design.

## 7. Robustness checks

1. Separate analyses for increment and no-increment games.
2. Alternative continuous time transformations and knot placements.
3. Alternative blunder thresholds and win-probability transformations.
4. Exclusion of opening moves and positions already overwhelmingly won or lost.
5. Separate rating bands and titled-player subsamples.
6. Controls for position difficulty and pre-move evaluation.
7. Models using normalized remaining time rather than seconds.
8. Sensitivity to clock rounding, premoves, and implausible estimated thinking times.
9. Game-clustered and player-clustered uncertainty estimates.
10. Comparison between independently recomputed Stockfish evaluations and the subset of Lichess-provided evaluations.

## 8. Planned tables and figures

### Tables

1. Sample construction and exclusion flow.
2. Descriptive statistics by time control.
3. Move quality by remaining-time bin.
4. Primary regression results.
5. Threshold estimates and uncertainty intervals.
6. Robustness and subgroup results.

### Figures

1. Distribution of remaining time in blitz and rapid.
2. Unadjusted move-quality curve over remaining time.
3. Adjusted blunder-probability curve with confidence bands.
4. Estimated threshold or slope-change visualization.
5. Blitz-versus-rapid comparison using normalized remaining time.
6. Rating-group heterogeneity.

## 9. Manuscript structure

The final paper will target roughly 20-30 pages including tables and references:

1. Title page, author information, data statement, funding, conflicts, and contributions.
2. Abstract and keywords.
3. Introduction and contribution.
4. Related literature and conceptual framework.
5. Data and sample construction.
6. Measures and statistical methods.
7. Results.
8. Robustness and heterogeneity.
9. Discussion.
10. Limitations.
11. Conclusion.
12. References and optional appendix.

The final PDF should be clean before upload. SSRN-specific watermarks or the "preprint not peer reviewed" footer are platform-added features and should not be baked into the source manuscript.

## 10. Reproducibility and research integrity

- Raw archives will not be committed to GitHub.
- Download URLs, file hashes, sample filters, random seeds, and engine settings will be logged.
- Processed public data will be documented with a data dictionary.
- Statistical code will generate all final tables and figures from processed data.
- Results will not be fabricated or filled from expectations.
- Hypotheses, confirmatory outcomes, and threshold-selection rules will be timestamped in the repository before final estimation.
- Public usernames will be removed from the analytical dataset unless a scientifically necessary and ethically justified use is documented.

## 11. Execution sequence

1. Build a small end-to-end parser for PGN clocks and metadata.
2. Run a computational pilot to benchmark Stockfish and validate the outcome definitions.
3. Freeze the sample and analysis specification.
4. Construct the full move-level dataset.
5. Produce descriptive results and diagnostic checks.
6. Estimate primary, threshold, and robustness models.
7. Draft the full English manuscript.
8. Generate and visually verify the final PDF.
9. Prepare SSRN metadata: title, abstract, keywords, JEL codes if appropriate, author affiliations, and data/code links.

## 12. Remaining author decisions

- contact email for the manuscript;
- preferred scope: all eligible players or a high-rated/titled-player study;
- available compute budget for Stockfish analysis;
- whether the study should prioritize behavioral science, chess analytics, or econometric methodology in its framing.
