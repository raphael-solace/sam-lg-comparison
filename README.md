# SAM vs Naive Agent Comparison Experiment

This repository contains the setup to reproduce the Intelligent Data Management (IDM) experiment comparing Solace Agent Mesh (SAM) with LangGraph approaches.

## Experiment Overview

Run 10 complex analytical queries against a PostgreSQL database (5 tables, 1M rows) and compare:
- **LangGraph (Naive)**: Basic SQL tool with raw result dumps
- **LangGraph (Optimized)**: Improved with aggregations
- **SAM IDM**: Managed connectors with connection pooling and intelligent orchestration

## Files

- **`user_prompt.txt`**: The 10 analytical queries to run
- **`DATABASE_DESCRIPTION.md`**: Database schema and setup instructions

## Quick Start

1. Set up the PostgreSQL database following `DATABASE_DESCRIPTION.md`
2. Configure your LLM (Claude Haiku 4.5 recommended for comparison)
3. Run the queries from `user_prompt.txt` through each approach
4. Compare: DB time, agent time, token usage, and result accuracy

## Expected Results

SAM IDM should deliver:
- **150x faster** DB responses
- **8x faster** agent answers  
- **14.5x fewer tokens** (14.5x cheaper)
- **42% fewer hallucinations**

## Resources

- [Solace Agent Mesh](https://github.com/SolaceLabs/solace-agent-mesh)
