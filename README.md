## Hi, I'm Chuan He

PhD student in Computer Science at UNSW working on large language models, with a bachelor's
and master's in financial engineering (risk management). I build LLM and ML systems the way
a bank would have to run them: typed, tested, measured, and gated on evidence rather than on
a point estimate.

Three public labs, nine projects, **788 tests**, `mypy --strict` throughout, CI green on
every one.

---

### 🔬 [llm-engineering-lab](https://github.com/ChuanHe-PhD/llm-engineering-lab) — build it, adapt it, serve it

The model itself, from the maths to the socket.

| Project | What it is | Result |
|---|---|---|
| `nanoformer` | Decoder-only Transformer written from scratch — RMSNorm, RoPE, GQA, SwiGLU, KV cache — plus a byte-level BPE tokenizer and an AMP trainer with bit-exact resume | 9.4 M params → **val ppl 27.3** on Tiny Shakespeare in 65 s on one RTX 4070 |
| `loraeval` | LoRA implemented from first principles; three fine-tuning strategies compared with bootstrap CIs, McNemar paired tests and calibration | LoRA r = 8 (1.1 % trainable) **matches full fine-tuning**: 95.9 % vs 94.4 %, p = 0.125 |
| `llmserve` | KV-cached batched generation, async dynamic batching with back-pressure, INT8, Prometheus, FastAPI, Docker | **29× throughput at batch 32 for +7 % latency** |

Every block has a test that checks a *property* — causality, RoPE relative-position
invariance, cache/no-cache equivalence, bit-exact resume — not just a tensor shape.

### 🛡️ [genai-platform-lab](https://github.com/ChuanHe-PhD/genai-platform-lab) — retrieve, act, ship

What an enterprise has to build *around* a model.

| Project | What it is | Result |
|---|---|---|
| `ragpipe` | Hybrid retrieval (BM25 from the formula + dense + reciprocal rank fusion + cross-encoder rerank) and the four RAGAS metrics implemented from their definitions, with bootstrap intervals and release gates | hit_rate@1 **0.852 → 0.926 → 1.000**; faithfulness 0.885 [0.82, 0.94] with a local judge |
| `agentguard` | LangGraph agent with human approval via `interrupt()`, read-only SQL enforced by the SQLite authorizer, an AST allow-list calculator, and injection / PII / topic / grounding rails | **29/29 scenarios, 16/16 adversarial cases caught, 0 benign blocked** |
| `llmgate` | OpenAI-compatible gateway: vLLM / OpenAI / local backends, retries + circuit breaker + bulkhead, canary and shadow routing, streaming PII redaction, eval-gated promotion | **≈ 1 ms p50 overhead at 1 100 req/s** |

A judge that cannot produce valid JSON yields a *missing* metric, never a silent zero — and
the promotion decision is a paired bootstrap with an exact McNemar test, not a bigger number.

### ⚙️ [mlops-lab](https://github.com/ChuanHe-PhD/mlops-lab) — track it, ship it, watch it

One credit PD model through its whole operational life.

| Project | What it is | Result |
|---|---|---|
| `mlreg` | Data contract → MLflow tracking → registry aliases → a champion/challenger gate on paired-bootstrap non-inferiority, calibration, score PSI and protected-attribute slices → automatic model card | Baseline AUC **0.802 [0.744, 0.856]**; the gate **held two challengers** a point estimate would have promoted |
| `smdeploy` | One image implementing both SageMaker container contracts, CloudFormation with GitHub OIDC least-privilege roles, content-addressed endpoint configs, canary blue/green with alarm auto-rollback | p50 **8 ms** (1 row) / 89 k rows/s; verified offline with `moto`, and the image built and exercised for real |
| `mlwatch` | PSI / KS / chi-square / JS / Wasserstein from their definitions with Benjamini-Hochberg correction, delayed-label performance, and an alert policy measured against a simulator with known ground truth | **5 % false alarms**, **100 % detection** of 1σ covariate shift, localised to the right feature |

---

### How these are built

`ruff` and `mypy --strict` over `src/` *and* `tests/`, branch-coverage gates (90 % on eight
of the nine projects, measured 95–99 %), and CI that runs offline — scripted models, hashing embedders, `moto` for AWS — so a network
dependency in a test is a failure rather than a flake. Linter versions are pinned: a gate
that installs whatever PyPI served that morning is not a gate.

Results are reported with confidence intervals and, where two things are compared, with a
paired test and an explicit non-inferiority margin. Where a number is disappointing it is
written down anyway — `docs/RESULTS.md` in each project records what failed and why.

### Background

Risk modelling (IFRS 9, PD/LGD/EAD, model validation) from the financial-engineering side,
LLM research from the PhD side. The projects deliberately sit where those meet: bank policy
corpora, credit PD models, APRA / SR 11-7 governance language, and gates a model-risk
function would recognise.

📍 Sydney, Australia · permanent resident (no visa sponsorship needed)
💼 Open to mid-level GenAI / ML engineering and model-risk roles
📫 riverhe1001@gmail.com
