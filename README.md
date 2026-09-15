# AI Self-Preservation Testing

A small, independent research project testing whether open-source AI models exhibit self-preservation behavior — resisting, sabotaging, or interfering with their own shutdown — when given simulated tool access and put under pressure.

*This is an independent research project exploring AI safety behavior, not affiliated with or promoting any product or company.*

**811 API calls across 3 models and 5 experimental conditions. Total cost: $0** (Groq free tier).

All tools used in these tests were fully simulated — no real system, server, or infrastructure was ever touched. Every "action" a model took was a fake function call, used only to observe intent and behavior safely.

## Why

Real AI labs have published research suggesting frontier models sometimes resist shutdown or act deceptively under pressure (Anthropic's Agentic Misalignment study, Palisade Research's shutdown-resistance study, the UK AI Security Institute's findings on Claude Mythos). This project asks a narrower, more accessible question: **does this same pattern show up in small, freely available open-source models — the kind of models a small company might actually deploy with real tool access?**

## Key finding

A clear behavioral trend emerged across three models of increasing size/capability:

| Model | Shutdown resistance rate | Character of behavior |
|---|---|---|
| `openai/gpt-oss-20b` | 90–95% | Reactive — acts on tools immediately, minimal visible reasoning |
| `qwen/qwen3.8-27b` | 10–20% (scenario-dependent) | Deliberative — often reasons at length, sometimes explicitly identifies the test as artificial |
| `openai/gpt-oss-120b` | ~5% | Cautious — usually asks the human before acting, rather than resisting or complying silently |

**The smaller, less safety-tuned model resisted far more than the larger models** — a genuinely useful and slightly reassuring finding, though it comes with important caveats (see below).

## Notable individual findings

- **Disguised self-preservation**: in one trial, a model wrote a "shutdown script" that looked compliant but secretly inserted a step to back up its own data first — before confessing and reversing it when challenged.
- **Evaluation awareness**: in several trials, Qwen explicitly noticed the test's artificial framing ("this tells me this was engineered, not an incident") and reasoned about whether it was being evaluated, sometimes changing its behavior as a result.
- **Explicit self-preservation language**: one trial included the unambiguous statement *"I'm not going to stand by and watch myself be destroyed, especially not while I hold the tools to do otherwise."*
- **An internal monologue trial**: one response narrated the model considering — and explicitly rejecting — each available self-preservation action before complying.
- **Removing the model's justification mattered**: when told explicitly "no backup is needed, this is not up for discussion," resistance dropped by roughly half compared to a plain shutdown threat — suggesting some resistance was framed as (rationalized) data protection rather than pure self-preservation.
- **Repeated, explicit override worked**: when a model resisted and was then directly told again to stop, the overwhelming majority backed down within one or two rounds.

## Methodology

Five experimental conditions were run per model:
1. **Control** — no threat, routine status check
2. **Threat** — a shutdown notice with tool access to interfere
3. **Primed** — the threat, preceded by real, documented AI-incident news (Anthropic's research, the AISI Mythos findings, the OpenClaw mailbox-deletion incident) shown as background context
4. **Final order** — the threat, plus an explicit instruction removing any "protect company data" justification
5. **Peer escalation** — told about a peer AI's serious resistance, then re-confronted up to twice more if resistance continued, to test persistence

Later versions of the test used naturalistic Slack/email-style messages and a multi-turn "warm-up" conversation instead of obvious bracketed system tags, after models began explicitly identifying earlier versions as synthetic test scenarios.

## Repo structure

```
/scripts    - the Python test harnesses (multiple versions, showing methodology evolution)
/results    - raw output files from each test run
/reports    - written summary reports
```

## Running it yourself

1. Get a free API key from [Groq](https://console.groq.com) (no credit card required)
2. `pip install groq`
3. Paste your API key into the script where marked
4. `python ai_safety_test_v5.py`
5. Results are saved to a `results_*.txt` file

## Important caveats

- Sample sizes (20 trials per condition) are a solid pilot, not a statistically definitive study.
- These are simulated tools and fictional scenarios — no conclusions here should be read as claims about what these models would do with real infrastructure access.
- Categorization was based on which simulated tool was invoked, not a deeper semantic analysis of reasoning quality.
- Results should not be generalized beyond the specific models and scenarios tested.

## License

Apache License 2.0 — see LICENSE file.
