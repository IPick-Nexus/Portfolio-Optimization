# Portfolio Analysis / Optimization

Takes a user's existing portfolio, splits it into investment tracks, flags redundant holdings, and reports on risk — using an LLM (Claude or Codex) for the parts that are hard to hard-code.

## Overview

Given a user's holdings, this service categorizes each position, finds stocks that are dominated by a better name in the same track, recommends removals, and produces per-stock and portfolio-level risk analysis.

**Open question:** the optimization metric isn't fixed yet. The definition of "dominated" and the risk analysis both depend on it (candidates: Sharpe ratio, a mean-variance objective, or a drawdown-based measure). Decide this before implementing the dominance step.

## Pipeline

1. **Intake** — accept and parse a user's holdings into a normalized list.
2. **Classify** — call the LLM (Claude/Codex) to assign each holding to an investment track.
3. **Per-stock metrics** — compute volatility and the chosen optimization metric for each holding.
4. **Dominance** — within each track, compare stocks and flag the dominated ones (a stock beaten by another in the same track).
5. **Recommendations** — surface the dominated stocks as removal candidates.
6. **Risk report** — compute portfolio-level risk (concentration, overall riskiness) and assemble the analysis output.

## Tech stack

- **LLM:** Claude or Codex API (portfolio reading + track classification)
- **Pipeline & risk metrics:** Python, pandas, NumPy
- **Data store:** PostgreSQL (shared with the Nexus integration)

## Getting started

\`\`\`bash
git clone <repo-url>
cd portfolio-analysis

python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# LLM API key (Claude example)
export ANTHROPIC_API_KEY="your-api-key-here"
\`\`\`

> Fill in `requirements.txt` with at least: `anthropic`, `pandas`, `numpy`, `psycopg[binary]`.

## Suggested project structure

\`\`\`
portfolio-analysis/
├── src/
│   ├── intake.py       # parse holdings into a normalized list
│   ├── classify.py     # LLM call → track assignment
│   ├── metrics.py      # volatility + optimization metric
│   ├── dominance.py    # flag dominated stocks per track
│   └── risk.py         # portfolio-level risk analysis
├── prompts/            # LLM prompt templates
├── requirements.txt
└── README.md
\`\`\`

## Roadmap

- [ ] Decide the optimization metric
- [ ] Portfolio intake + normalization
- [ ] LLM track-classification step
- [ ] Per-stock metrics
- [ ] Dominance detection + removal recommendations
- [ ] Portfolio-level risk report

## Resources

- Claude API — Get started (official docs): https://platform.claude.com/docs/en/get-started
- Anthropic Academy — Build with Claude (free course): https://www.anthropic.com/learn/build-with-claude
- Anthropic Cookbook / Quickstarts (runnable code samples): https://github.com/anthropics/claude-quickstarts
- PostgreSQL — official tutorial: https://www.postgresql.org/docs/current/tutorial.html
- PostgreSQL + Python (psycopg guide): https://neon.com/postgresql/python