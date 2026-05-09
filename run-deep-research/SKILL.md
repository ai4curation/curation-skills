---
name: run-deep-research
description: Run citation-bearing deep research with deep-research-client across OpenAI, Perplexity, Edison/Falcon, Cyberian, and site-specific providers such as OpenScientist. Use when a user asks for research reports, literature reviews, source-backed synthesis, or provider/model trade-off guidance.
---

# run-deep-research

Use `deep-research-client` for source-backed research tasks that should produce a report with citations and provider metadata.

Canonical project: https://github.com/monarch-initiative/deep-research-client

Do not add a skill `README.md`. The operative agent instructions belong in this `SKILL.md`; the main repository is the human-facing source of broader project context.

## Command Prefix

Select the command form from the current environment:

- In the `deep-research-client` repository, use `uv run deep-research-client ...`.
- If the package is already installed, use `deep-research-client ...`.
- If it is not installed and `uv` is available, use `uvx deep-research-client ...`.

Before hard-coding a provider or model, prefer checking local availability:

```bash
deep-research-client providers
deep-research-client models
```

Provider names and aliases can vary by installed version or local deployment. Trust the CLI output over stale examples.

## Provider Selection

Be explicit about speed, depth, cost, and source type. If the user has not already made that trade-off clear, ask before using expensive or long-running providers.

Use this default selection logic:

| Need | Provider guidance |
| --- | --- |
| Fast current/web answer | Perplexity `sonar` or `sonar-pro` |
| Deeper current/web synthesis | Perplexity `sonar-deep-research` |
| Comprehensive general report | OpenAI deep research |
| Scientific literature review | Edison/Falcon |
| Iterative agent-based deep research | Cyberian |
| Site-specific scientific agent workflow | OpenScientist, only when `providers` reports it |

### Edison/FutureHouse/Falcon Context

Edison Scientific is the successor/spinoff context for the old FutureHouse Falcon integration. The client and older examples may still use the provider name `falcon` for compatibility, while newer docs may say `edison`.

Use whichever provider name the installed CLI reports. Prefer `EDISON_API_KEY`; treat `FUTUREHOUSE_API_KEY` as a legacy fallback if the client supports it.

### Cyberian Context

Cyberian is not a simple hosted API provider. It runs a local agent workflow through `agentapi` using agents such as Claude, Codex, Aider, Cursor, or Goose. Use it when the user wants an exhaustive, iterative research pass and is comfortable with a slow, visible local-agent workflow.

For testing or cost control, consider limiting iterations if the installed Cyberian version supports it:

```bash
deep-research-client research "query" --provider cyberian \
  --param agent_type=codex \
  --param max_iterations=2
```

### OpenScientist Context

OpenScientist may be exposed by site-specific or newer `deep-research-client` deployments for scientific research workflows. Do not assume the provider name, model name, or setup from memory; use `providers`, `models`, and local docs to confirm availability before invoking it.

## Research Workflow

1. Clarify speed/depth/cost if ambiguous.
2. Check available providers and models when provider availability is uncertain.
3. Choose the provider/model according to the user's goal.
4. Run the research, saving to an output file when the user asks for an artifact or the result will be reused.
5. Review the result for citations, cache status, provider metadata, and obvious provider failure modes before presenting it.

Minimal command pattern:

```bash
deep-research-client research "research question" \
  --provider perplexity \
  --model sonar-pro \
  --output research.md
```

Use `--input-file` for long prompts. Use `--template` and `--var` only when the user supplies a template or when using a bundled template intentionally, such as `examples/gene_research.md`.

## Cache Safety

Treat `~/.deep_research_cache/` as valuable user data. It can contain expensive prior research results and provenance.

- Use the cache by default.
- Use `list-cache` or `search-cache` freely for non-destructive inspection.
- Use `--no-cache` only when the user asks for a fresh result or the task clearly requires bypassing stale results.
- Never run `clear-cache`, delete cache files, or remove `~/.deep_research_cache/` unless the user explicitly asks for cache deletion and confirms they understand it removes saved research.

## Output Expectations

Prefer reports with:

- a clear research question
- provider/model metadata
- citations or source links
- enough synthesis to be useful without rerunning the query

If the selected provider is unavailable, report the exact missing requirement, such as an API key, `agentapi`, Cyberian installation, or an unavailable local provider such as OpenScientist.
