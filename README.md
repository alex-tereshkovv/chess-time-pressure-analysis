# Critical Time Thresholds in Online Chess Decision-Making

This repository contains a data-science research project studying how remaining clock time affects move accuracy in online chess.

## Research Question

Does low remaining clock time increase the probability of inaccurate moves in online blitz and rapid chess games?

## Main Hypothesis

Blunder probability increases when remaining clock time becomes low, especially below critical time thresholds.

## Data

The project uses publicly available Lichess game data.

## Planned Methods

- PGN parsing
- Clock-time extraction
- Engine-based centipawn loss
- Blunder probability analysis
- Regression modeling
- Data visualization

## Status

Research design in progress. The current protocol is available at
[`manuscript/research_protocol.md`](manuscript/research_protocol.md).

The next milestone is an end-to-end pilot that parses Lichess clock comments,
recomputes engine evaluations, and produces the first move-level analysis table.
