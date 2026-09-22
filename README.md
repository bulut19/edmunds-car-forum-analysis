# Edmunds Car Forum Competitive Analysis

A competitive intelligence study of the entry-level luxury car market, built from 6,000 unstructured Edmunds.com forum posts using a hybrid NLP + LLM pipeline.

## Overview

The raw dataset had no headers and several corrupted text patterns (inserted phrases, duplicated tokens, missing spaces between brand names), diagnosed and fixed before any analysis. From there, the pipeline identifies the top 10 discussed brands and top 5 attributes, measures brand-pair competitive overlap two ways (lexical string-matching vs. LLM-based semantic classification), maps the competitive landscape, links brands to attributes, and measures purchase/ownership aspiration by brand.

The core design choice throughout: use deterministic NLP (regex, frequency counts) for objective counting tasks, and reserve the LLM (`gpt-4o-mini`) for judgment calls that need context, always validated against a manual read of real posts rather than trusted blindly.

## Key Findings

- **Top 10 brands:** BMW, Toyota, Acura, Lexus, Audi, Cadillac, Honda, Pontiac, Infiniti, Mercedes-Benz, ranked by share of posts mentioning each.
- **Lexical vs. semantic lift agree overall** (Pearson r = 0.897, Spearman rho = 0.841), but diverge on specific pairs. The Honda-Mercedes-Benz gap (lexical 1.96 vs. semantic 1.01) traces to Honda being used as a mainstream reference point in comparison posts rather than a brand actually under discussion, a distinction string-matching can't make but the LLM can.
- **Toyota and Honda consistently separate from the main luxury cluster** across both measurement approaches, while BMW's exact competitive position shifts depending on which method is used.
- **Attribute associations differ by brand:** BMW leads on Driving Experience and Performance, Audi on Interior/Comfort, Acura on Price/Value and Interior/Comfort, Lexus on Luxury/Prestige.
- **Audi has the highest strict aspiration rate** (2.70% of its mentioning posts show clear purchase/ownership intent), consistent across the LLM classification and a keyword-only baseline. Under a broader definition that includes general positive preference, Mercedes-Benz leads instead, so the two definitions capture genuinely different things.
- **Total LLM cost across the full 6,000-post pipeline: well under $1** (gpt-4o-mini), judged worthwhile since it caught corrupted-text errors and entity resolution gaps (Toyota, Cadillac/Lexus) that a lexical-only approach missed.

## Limitations

1. The forum sample isn't representative of the broader vehicle-buying population, results describe this forum's conversation, not the market.
2. LLM classification has real error, it sometimes dropped genuine brand mentions or added false ones; every step was validated against a manual sample rather than trusted outright.
3. The 2D MDS competitive map has meaningful stress, individual distances shouldn't be read as precise.
4. "Aspiration" is an operational definition (textual evidence of purchase/ownership intent), not a directly observed quantity, and the ranking changes under a broader definition.
5. Lift measures association, not causation.

## Tech Stack

Python (pandas, NumPy, scikit-learn, scipy) · OpenAI API (gpt-4o-mini) · Google Colab

## Repository Contents

- `edmunds_car_forum_analysis.ipynb`: full pipeline (data cleaning, brand/attribute discovery, lexical and semantic lift analysis, competitive mapping, aspiration analysis, client recommendations)
- `README.md`: this file

## Team

Group project with Samantha Feinberg, Jonathan Choi, Shehryar Syed, and Yash Shrivastava.
