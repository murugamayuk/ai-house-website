# Development Playbook (AI Product Stack)

This playbook is a practical structure for building across:
- model fine-tuning,
- agentic architecture,
- multi-agent systems,
- web applications,
- mobile applications,
while maintaining reusable assets (skills markdown files, datasets, audio/video/text corpora).

## 1) Organize work as product streams, not random tasks

Use five parallel streams with clear ownership and review cadence:

1. **Model & Data Stream**
   - Fine-tuning, evals, data curation, labeling.
2. **Agent Systems Stream**
   - Tooling, orchestration, memory, safety policies.
3. **Platform Stream**
   - Backend APIs, observability, infra, cost controls.
4. **Experience Stream**
   - Web app + mobile app UX, release quality, analytics.
5. **Knowledge Ops Stream**
   - Skills, prompts, docs, governance, dataset catalogs.

Each stream should run in 2-week cycles with one integrated demo.

## 2) Recommended repository layout

```text
repo/
  apps/
    web/
    mobile/
  services/
    api/
    agent-gateway/
  agents/
    orchestrators/
    specialists/
    tool-adapters/
  models/
    finetunes/
    evals/
    prompts/
  data/
    raw/
    interim/
    curated/
    registry/
  corpora/
    text/
    audio/
    video/
    manifests/
  skills/
    AGENTS.md
    <skill-name>/
      SKILL.md
      templates/
      scripts/
  docs/
    architecture/
    runbooks/
    adr/
  ops/
    ci/
    monitoring/
    security/
```

## 3) Fine-tuning + evaluation workflow

1. Define target capability in a short **task card**:
   - Inputs, expected outputs, constraints, failure modes.
2. Build eval set first (minimum 200 examples per high-priority capability).
3. Track datasets with immutable versions:
   - `dataset_name@YYYYMMDD.vN`
4. Keep a training ledger for every run:
   - base model, hyperparams, tokenizer, data snapshot, compute cost, eval metrics.
5. Promote a model only if it beats baseline on:
   - quality metrics,
   - safety checks,
   - cost/latency budget.

## 4) Agentic and multi-agent architecture pattern

Use a **planner-router-worker-critic** pattern:

- **Planner**: decomposes tasks.
- **Router**: picks agent/tool based on capability map.
- **Workers**: focused specialists with narrow tool access.
- **Critic/Verifier**: validates outputs against policy + tests.

Design rules:
- Keep each agent specialized and stateless where possible.
- Use shared memory as explicit stores (session, long-term, vector).
- Require every tool call to be observable (trace id, latency, error code).
- Implement policy guardrails centrally (PII, security, legal constraints).

## 5) Web + mobile delivery model

Adopt backend-for-frontend (BFF) and shared contracts:

- Single API contract for web and mobile.
- Feature flags for controlled rollouts.
- Common analytics event taxonomy.
- Common prompt and policy endpoints to avoid drift across clients.

Release strategy:
- Web: weekly releases.
- Mobile: 2–4 week release trains + remote-config kill switches.

## 6) Skill markdown file governance

For each skill:

- `SKILL.md` should include:
  - purpose,
  - trigger conditions,
  - required inputs,
  - deterministic workflow steps,
  - failure/fallback behavior,
  - examples,
  - maintenance owner.

- Add semantic versioning to each skill:
  - `version: 1.2.0`
- Store changelog in same folder.
- Add automated lint/checks:
  - frontmatter completeness,
  - broken links,
  - stale examples.

## 7) Dataset/corpus asset management

Treat data as product assets with metadata-first management.

For every artifact (text, audio, video):
- Unique asset ID.
- Source and license.
- Consent/provenance status.
- Language + domain tags.
- Quality score.
- Safety classification.
- Retention policy.

Use manifests (CSV/JSONL/Parquet) that reference files rather than ad hoc folders.

Suggested minimal metadata schema:

```json
{
  "asset_id": "audio_call_000123",
  "type": "audio",
  "source": "customer_support",
  "license": "internal-use",
  "consent": "granted",
  "language": "en",
  "duration_sec": 93.2,
  "transcript_ref": "text_tx_000123",
  "quality": 0.87,
  "safety": "reviewed",
  "created_at": "2026-04-30"
}
```

## 8) Industry trend alignment (2026 practical bets)

Focus your roadmap on:
1. **Evals-first development** (fewer demos, more measurable reliability).
2. **Cost-aware inference** (routing, caching, smaller specialist models).
3. **Tool reliability + observability** (production traces and postmortems).
4. **Data governance** (licenses, consent, privacy controls).
5. **Human-in-the-loop review** for high-impact actions.

## 9) 90-day skill growth plan (for individuals/teams)

- **Month 1**: strengthen data + eval fundamentals.
- **Month 2**: build one production-grade multi-agent workflow.
- **Month 3**: harden deployment (monitoring, alerts, rollback, QA automation).

Weekly rhythm:
- 1 architecture review,
- 1 experiment review,
- 1 production incident/game-day drill,
- 1 learning note published to internal docs.

## 10) KPI dashboard to keep progress objective

Track weekly:
- Task success rate (offline + online).
- Hallucination/error escape rate.
- P95 latency and cost per successful task.
- Eval coverage (% capabilities with maintained eval sets).
- Data freshness and licensing compliance.
- Time-to-recover from agent/tool failures.

---

If you follow this structure, you will build durable capability instead of chasing short-term trends.
