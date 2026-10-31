# Self-hosted finance apps (and Indian adaptations) + LLM categorization and monthly insights

Research date: 2026-10-31. Some hosts (lists.gnucash.org, nehiljain-com-git.pages.dev) could not be fetched from this session, so those sources come from search snippets only. Most third-party release info comes from mirrors (newreleases.io), not the canonical GitHub pages.

## Q1: Self-hosted / open-source personal finance tools: features, import, INR support, maturity, Indian usage

### Takeaway
No self-hosted app connects directly to Indian banks. Indian users get data in by CSV/PDF statement exports or by parsing bank SMS, using importer scripts the community writes for each bank. Firefly III (rules plus a separate Data Importer), Actual Budget (envelope budgeting, SQLite) and Sure (the community fork of Maybe) are the main actively maintained GUI apps. Beancount/hledger/GnuCash have the most India-specific importer scripts. Maybe itself is dead (v0.6.0, July 2025).

### Cited Findings
**Maybe Finance / Sure**
- Maybe released a final v0.6.0 around 31 July 2025 and announced it was "pivoting to B2B financial forecasting and scenario planning and … will no longer be actively maintaining this repository." Commenters pointed to Sure, Firefly III and Actual as alternatives. — [Lemmy/selfhosted thread](https://deddit.petersanchez.com/g/selfhosted@lemmy.world/p/Y835h27836bM2q539T-Maybe-financial-tracker-shuts-down-releasing-a-f); [PieFed post](https://piefed.jeena.net/post/210491). Sources disagree on whether the code stays open source. The repo was AGPLv3, so the released code remains under that license. — [Lemmy comment](https://scribe.disroot.org/comment/6081275)
- An archive repository exists at github.com/maybe-finance/maybe-archive. — [GitHub](https://github.com/maybe-finance/maybe-archive)
- Sure (github.com/we-promise/sure) describes itself as a community-maintained fork of Maybe under AGPLv3. It runs in Docker and offers browser, macOS desktop, mobile app and API/LLM-agent access, plus an AI assistant. At fetch time it had about 10.7k stars, 718 forks, roughly 3,963 commits, 448 open issues and 353 open PRs. — [GitHub we-promise/sure](https://github.com/we-promise/sure). (The fetched summary said Maybe stopped development in "mid-2023". That conflicts with the July 2025 archival in the other sources and probably refers to Maybe's earlier 2023 shutdown of its hosted product.)
- Sure ships monthly releases. A 2026 history table runs from v0.6.7 (Jan) to v0.7.2 (Jun). v0.7.1 (May 2026) had 206 merged PRs, mostly UI/design-system work. v0.7.2 added Goals, mobile transaction metadata editing and Anthropic as a native LLM provider. v0.7.4-alpha.1 merged Budgets and Goals into a "Plan" tab. The newest version seen is v0.7.6-alpha.3. — [newreleases v0.7.1](https://newreleases.io/project/github/we-promise/sure/release/v0.7.1); [v0.7.4-alpha.1](https://newreleases.io/project/github/we-promise/sure/release/v0.7.4-alpha.1); [v0.7.6-alpha.3](https://newreleases.io/project/github/we-promise/sure/release/v0.7.6-alpha.3)
- Sure v0.6.5 added bank sync support. — [GitHub discussion #388](https://github.com/we-promise/sure/discussions/388). v0.6.9 added Pipelock security scanning of AI responses. — [newreleases v0.6.9](https://newreleases.io/project/github/we-promise/sure/release/v0.6.9)

**Firefly III**
- Firefly III is under very active development. v6.4.16 came out 2026-01-18 and v6.4.17 on 2026-02-06; v6.4.17 added an API "batch_processing" setting for bulk transaction creation. A develop build dated 2026-07-01 lists v6.6.6 as the latest release. — [changelog.md](https://raw.githubusercontent.com/firefly-iii/firefly-iii/main/changelog.md); [newreleases develop-20260701.1](https://newreleases.io/project/github/firefly-iii/firefly-iii/release/develop-20260701.1)
- Data comes in through a separate Data Importer, which handles CSV files and bank connections such as SimpleFIN and GoCardless. — [Firefly docs: importing data](https://docs.firefly-iii.org/tutorials/firefly-iii/importing-data/); [SimpleFIN tutorial](https://docs.firefly-iii.org/tutorials/data-importer/simplefin/)
- Indian adaptations:
  - "Firefly III Import Helper" (anil-dewani) parses Axis, SBI and ICICI e-statements, including credit-card PDFs, and posts the transactions through a personal access token. — [GitHub mirror](https://github.imc.re/anil-dewani/firefly-import-helper)
  - A March 2026 blog post describes a home-server setup ("Project Jugnu") with Docker, Firefly III and a Python parser for Indian bank SMS. It uses the UPI reference number to avoid double-counting transfers between the author's own accounts. — [vickytechlove blog](https://vickytechlove.blogspot.com)

**Actual Budget**
- Actual Budget is a local-first envelope budgeting app (a YNAB alternative). It runs as a single server container and stores data in SQLite on a mounted volume. Bank sync goes through GoCardless (EU/UK) and similar services. No Indian bank sync was found, so Indian users rely on CSV import. — [selfhosttools](https://selfhosttools.pages.dev/tools/actual-budget); [selfhostyourself](https://selfhostyourself.com/services/actual-budget)
- India context: a 2024 r/ynab post says YNAB costs close to ₹10,000/yr in India and has no Indian bank sync. That gap is what pushes Indian users toward Actual. — [r/ynab via libreddit](https://lr.eu.psf.lt/r/ynab/comments/1e3v11g/bidding_goodbye_fiver_years_of_ynab/ldbiej5/?context=3). An April 2026 Reddit post from an Indian couple asks for an app that handles INR and understands SIPs, LIC and PPF. — [Reddit snapshot](https://reddit.sentinel-team.org/posts/1sdp8rv/snapshots/2026-04-06T17%3A27%3A41.302954Z)
- Hostinger India sells one-click Docker hosting for Actual Budget. — [Hostinger IN](https://www.hostinger.com/in/vps/docker/actual-budget)

**Beancount + Fava / beangulp / smart_importer**
- Beancount v3 moved importing out to beangulp. — [Beancount docs: importing external data](https://beancount.github.io/docs/importing_external_data/)
- smart_importer trains ML on your existing journal and suggests accounts for newly imported CSV rows. Beancount's docs suggest a hybrid workflow: an LLM assigns accounts to a first batch, you correct the mistakes, then smart_importer learns from the corrected ledger. — [beancount.io LLM bookkeeping doc](https://beancount.io/docs/Solutions/using-llms-to-automate-and-enhance-bookkeeping-with-beancount); [forum: Smart Defaults](https://beancount.io/forum/t/smart-defaults-training-your-importers-to-categorize-like-you-would/230)
- Indian importers:
  - dumbPy/beancount-importers-india covers SBI, BOI, Paytm, HSBC and ICICI and is listed under "India" in Awesome Beancount. — [Awesome Beancount](https://beancount.io/awesome-beancount); [ecosyste.ms](https://awesome.ecosyste.ms/projects/1010954)
  - A single-file HDFC importer gist uses smart_importer and pandas. — [Gist BharatKalluri](https://gist.github.com/BharatKalluri/17d14353a83a3e58ffb5639e7b00348d)
  - dyumnin/dyu_accounting has ICICI bank/card and IDFC importers; HDFC was not yet supported. — [r/plaintextaccounting thread](https://redlib.ssps.io/r/plaintextaccounting/comments/1erbf0m/searching_for_a_repo_of_bank_statement_importers)
  - The GitHub "icicibank" topic lists an India broker/bank CSV importer (SBI, ICICI, IOB, KVB) last updated in 2020, so it is stale. — [GitHub topic icicibank](https://github.com/topics/icicibank)

**GnuCash / hledger**
- An April 2026 GnuCash-user mailing-list thread announced Python pre-processing scripts for statements from two large Indian banks. In July 2026 the author pushed updates after SBI changed its export format. That illustrates how much maintenance Indian statement formats need. — [gnucash-user Apr 2026](https://lists.gnucash.org/pipermail/gnucash-user/2026-April/120369.html); [Jul 2026](https://lists.gnucash.org/pipermail/gnucash-user/2026-July/121136.html)
- GnuCash ships only one contributed India account template, so users add GST accounts by hand. — [gnucash-user 2019](https://lists.gnucash.org/pipermail/gnucash-user/2019-August/086582.html). GnuCash supports reconciliation against bank statements. — [gnucash.org features](https://www.gnucash.org/features.phtml)
- hledger: no India-specific importer or INR-focused blog post was found (see Gaps).

**ezBookkeeping**
- ezBookkeeping (mayswind/ezbookkeeping) is a lightweight self-hosted app. It supports multiple currencies with auto-updating exchange-rate sources, and imports CSV, OFX, QFX, QIF, IIF, Camt.052/053, MT940, GnuCash, Firefly III and Beancount files. It runs on SQLite, MySQL or PostgreSQL in Docker, with custom date/number/currency formats. — [GitHub mayswind/ezbookkeeping](https://github.com/mayswind/ezbookkeeping). Hostinger also claims Alipay/WeChat Pay import, which is a third-party claim. — [Hostinger IN](https://www.hostinger.com/in/applications/ezbookkeeping)

### Inferences
- **Common India pattern:** whichever app you pick, the ingestion layer is custom. That means per-bank CSV/PDF parsers (HDFC, ICICI, SBI, Axis) or SMS parsers, keyed on the UPI reference for deduplication. Bank export formats change (the SBI July 2026 case), so parsers need versioning and tests.
- **INR support:** every app here is currency-agnostic, so INR as base currency works. The real India gaps are bank connectivity and investment instruments (SIP, PPF, EPF, LIC), not currency.
- **Picking a tool:**
  - Firefly III has the most mature REST API and rules engine, which suits an external LLM categorizer.
  - Actual has the best budgeting UX and an AI ecosystem (actual-ai).
  - Sure is fast-moving with a built-in AI assistant, but it is alpha-heavy, so expect churn.
  - Beancount gives the solo developer the most control: plain text, Python importers and smart_importer.

### Gaps
- No Reddit threads from r/IndiaInvestments or r/personalfinanceindia specifically about Firefly III, Actual or Beancount with Indian banks turned up in search. The evidence is blogs, GitHub repos and mailing lists.
- Budibase-style low-code setups: nothing found in the time available.
- Exact latest stable versions as of Oct 2026 for Firefly III (v6.6.6 seen as of July 2026), Actual and Sure (only the v0.7.6 alpha seen) could not be verified on canonical pages.
- Fava and hledger maintenance status not checked this session.
- Account Aggregator (RBI AA framework) support in any open-source app: none found.

## Q2: LLM transaction categorization: approaches, accuracy, cost, local options, existing projects

### Takeaway
The pattern that works in practice is layered: deterministic rules and a merchant/VPA lookup first, then a cache or nearest-neighbour match over past user-confirmed labels, then an LLM fallback. Every LLM decision is tagged for review and fed back as a rule or example. Published benchmarks put zero-shot general LLMs at about 60–70% on hard, many-category business taxonomies. Systems that use retrieval or fine-tuning on the user's own history do better. Personal setups with small taxonomies self-report around 95%.

### Cited Findings
**Existing projects**
- **sakowicz/actual-ai** (MIT, about 538 stars, 251 commits)
  - Sends each uncategorized Actual transaction (notes, amount, type, payee, imported payee, date) plus the category list to the LLM through a customizable Handlebars prompt template.
  - Optional ValueSerp web search for unknown merchants. Optional new-category suggestion, with checks for similar existing categories.
  - Tags guessed transactions `#actual-ai` and failures `#actual-ai-miss` for review. dryRun is on by default.
  - Providers: OpenAI (default gpt-5-mini), Anthropic, Google, Ollama (default phi3.5), OpenRouter (default deepseek-v3.2), Groq, or any OpenAI-compatible endpoint.
  - The README gives no accuracy or cost numbers.
  - — [GitHub sakowicz/actual-ai](https://github.com/sakowicz/actual-ai)
- **annis-souames/actual-categorizer:** a local Ollama variant that talks to Actual through actualpy. Tested with Llama 3.1 8B. — [GitHub](https://github.com/annis-souames/actual-categorizer)
- **deseven/telegram-actual-llm-helper:** a Telegram bot plus LLM for entering transactions into Actual. — [mirror](https://gh.hhj520.top/deseven/telegram-actual-llm-helper)
- **constLiakos/Firefly-III-LLM-Categorizer:** assigns categories and destination accounts using OpenAI, and caches AI decisions as local Firefly rules to cut cost. — [mirror](https://1clonegithubbyxjn.1934672944.workers.dev/constLiakos/Firefly-III-LLM-Categorizer)
- **controlol/firefly-assistant:** imports CAMT.053, reconciles, and categorizes with a local embedding model that learns from user corrections, with no cloud dependency. — [GitHub](https://github.com/controlol/firefly-assistant)
- **bramhoven/Luciferin (ff-iii-luciferin):** a Firefly III AI categorizer. — [GitHub](https://github.com/bramhoven/Luciferin); [PyPI](https://pypi.org/project/ff-iii-luciferin/1.0.0b1/)
- **hello-args/TxnResolver:** Ollama/Mistral-based transaction categorization. — [GitHub](https://github.com/hello-args/TxnResolver)
- **RoXsaita/NumbyAI-Public:** FastAPI + React + Ollama categorizer. — [via HN reader](https://hn.jakechampion.name/reader/https://github.com/RoXsaita/NumbyAI-Public)
- **Beancount:** beanhand uses an LLM to match receipts to ledger transactions alongside beangulp importers. — [PyPI beanhand](https://pypi.org/project/beanhand/). Beancount forum users report about 95% automated categorization with LLM-plus-smart_importer hybrids (self-reported). — [beancount.io forum](https://beancount.io/forum/t/finally-got-95-automated-expense-categorization-working-with-beancount-llms/93); [r/plaintextaccounting LLM for categorisation](https://redlib.hackliberty.org/r/plaintextaccounting/comments/1isfruh/llm_for_categorisation)

**Accuracy evidence**
- **QuickBooks (arXiv 2506.09234), few-shot setting:**

  | Method | Top-1 accuracy |
  |---|---|
  | Rel-Cat (graph model) | ~68.7% |
  | Embedding top-k nearest neighbour | ~65.8% |
  | Zero-shot Txn-BERT | ~45.5% |

  Nearest-neighbour retrieval does well on recurring transactions, and the graph model generalizes better. — [arXiv 2506.09234](https://arxiv.org/pdf/2506.09234)
- **SME bank transactions (arXiv 2508.05425):** a fine-tuned model reached 73.4% against 60.4% for zero-shot GPT-4o. — [arXiv 2508.05425](https://arxiv.org/pdf/2508.05425)
- **Digits (vendor):** on 17,792 SMB transactions with charts of accounts of 15–281 categories, the best general LLM (GPT-4.5-preview) plateaued at about 66%. — [Digits PDF](https://digits.com/_assets/downloads/beyond-the-hype-evaluating-llms-vs-digits-agl.pdf)
- **Mercado Libre (ZenML case study):** moving from LLM API calls to custom embeddings raised accuracy from 80% to 90% and cut cost by another 30%. LLMs were used mainly to generate training labels. — [ZenML LLMOps DB](https://zenml.io/llmops-database/financial-transaction-categorization-at-scale-using-llms-and-custom-embeddings)
- **transaction-ai docs:** claim few-shot Llama 3.1 8B at about 92% and hybrid ensembles at about 99.7%, but the same docs give inconsistent figures. Treat as unreliable. — [transaction-ai benchmarks](https://transaction-ai.readthedocs.io/en/latest/BENCHMARKS/)

**UPI / Indian narration normalization**
- NPCI's 2018 circular standardized UPI narrations across banks: rail code, 12-digit UPI reference, payee name, bank code, remark. — [NPCI UPI Circular 43](https://www.npci.org.in/PDF/npci/upi/circular/2018/UPI%20-%20Circular%20No.43.pdf)
- Credit narrations follow a `UPI/P2M` or `UPI/P2P` + reference + VPA/name + description structure. The guide recommends first classifying each narration into a family by anchor tokens (UPI/, NEFT, IMPS), because per-bank regexes multiply into dozens of brittle rules. — [terra-insight narration patterns](https://www.terra-insight.com/insights/bank-statement-narration-patterns-india); [classification](https://www.terra-insight.com/insights/bank-statement-narration-pattern-classification-india/)
- A suggested hybrid: regex for strict tokens, ML for fuzzy counterparty names and categories, and rule-based validators. — [aiaccountant guide](https://aiaccountant.com/blog/smart-narration-parsing-indian-statements)
- **finee (PyPI):** tiered design of regex fields, then a VPA→merchant→category map, then an optional LLM for leftovers. Claims 50+ regexes and 200+ VPA mappings (self-reported). — [PyPI finee](https://pypi.org/project/finee/)
- Related parsers: SurajMeena/upi-transaction-parser, and the Dart packages upi_sms_parser and transaction_sms_parser. — [GitHub](https://github.com/SurajMeena/upi-transaction-parser); [pub.dev upi_sms_parser](https://pub.dev/documentation/upi_sms_parser/latest/)

### Inferences
- **Recommended pipeline for a solo developer (Indian data):**
  1. Parse the narration into rail, reference, VPA, name and remark.
  2. Normalize the merchant through a VPA lookup table (e.g. `swiggy@…` maps to Swiggy, Food delivery).
  3. Apply user rules.
  4. Do embedding or exact-match lookup against previously confirmed transactions.
  5. Send the residue to an LLM with few-shot examples drawn from the user's own corrections. Use structured JSON output with the category constrained to the user's list.
  6. Tag the result as AI-guessed for review, and turn confirmations into rules. This is the actual-ai tagging plus Firefly-LLM-Categorizer rule-caching pattern.
- P2P UPI transfers to individuals cannot be categorized reliably from the narration alone. The UI should ask the user, or use the UPI remark field.
- **Cost (estimate, no source found with INR/USD-per-month figures):**
  - A typical Indian individual has perhaps 100–300 transactions a month. After rules and cache, maybe a few dozen reach the LLM.
  - At roughly 300–800 tokens per call (category list plus few-shot examples), that is well under 1M tokens a month.
  - That costs cents per month on small cloud models and nothing on local Ollama models (Llama 3.1 8B, phi3.5).
  - Verify against current provider pricing before quoting numbers.
- **Privacy:** local Ollama with 7–8B models is a viable default (actual-categorizer, TxnResolver, NumbyAI). Accuracy is likely lower than frontier models, so the rules and cache layers matter more.

### Gaps
- No published accuracy benchmark exists specifically on Indian UPI/bank narrations.
- No authoritative per-month cost reports from users of actual-ai or the Firefly categorizers.
- Maintenance status (last commit dates) of the smaller categorizer repos was not verified.

## Q3: LLM monthly insight / "what went well / where to improve" generation

### Takeaway
Best practice is "compute deterministically, let the LLM narrate." Code does every aggregate: month-over-month deltas, budget variance, top merchants, anomalies, recurring subscriptions. The LLM receives only those figures as structured input and writes the prose. A post-check confirms that every number in the output matches a computed value. No mature open-source project has a well-documented monthly-review generator. Sure's AI assistant and the MCP servers for Actual are the closest examples.

### Cited Findings
- **CIFQA framework:** bars the LLM from arithmetic and limits it to understanding the question, planning, and writing the response. Deterministic engines do every calculation. — [arXiv 2608.26114](https://arxiv.org/pdf/2608.26114)
- **Practitioner pipeline:** deterministic calculations produce structured metrics, and the LLM summarizes only those, which makes every figure auditable. — [wangari substack](https://wangari.substack.com/p/how-to-build-zero-hallucination-ai)
- **Verification:** recompute the math outside the LLM and check numbers, dates and percentages in the generated text, since these are the highest-risk tokens. — [aifinhub article](https://aifinhub-monorepo.pages.dev/articles/hallucination-detection-at-scale-production/)
- **VeNRA paper:** in deterministic domains, 99% accuracy can still mean "no operational trust", which supports strict separation of computing from generating. — [arXiv 2603.04663](https://arxiv.org/html/2603.04663v2)
- **2026 benchmark:** many LLMs wrongly flagged clean financial statements as inconsistent, so LLMs should not be the verifier. — [arXiv 2605.29586](https://arxiv.org/html/2605.29586)
- **Existing hooks:**
  - Sure includes an AI chat assistant and, as of v0.7.2, Anthropic as a native provider. — [GitHub we-promise/sure](https://github.com/we-promise/sure); [newreleases v0.7.1](https://newreleases.io/project/github/we-promise/sure/release/v0.7.1)
  - MCP servers for Actual Budget let an LLM agent query budget data. — [glama actual-mcp](https://glama.ai/mcp/servers/@s-stefanov/actual-mcp/tree); [actual-budget-mcp](https://glama.ai/mcp/servers/henfrydls/actual-budget-mcp/schema)
  - A 2026 Reddit/Lemmy thread asks which self-hosted finance AI features people actually find valuable. — [peachpie thread](https://peachpie.theatl.social/post/55382)

### Inferences
- **Reusable monthly-review design:**
  1. A SQL/pandas job produces a JSON "facts sheet" containing:
     - totals by category with prior-month and 3-month-average deltas
     - budget vs actual
     - top N merchants
     - new or recurring subscriptions detected
     - anomalies (z-score or more than X% above average)
     - savings rate
     - largest single transactions
     - uncategorized share
  2. The LLM prompt asks for "what went well / where to improve / 1–3 concrete actions." It must cite only IDs or values from the facts sheet and must not invent numbers.
  3. A validator extracts every number or ₹ amount in the output and checks it against the facts sheet. On mismatch, regenerate or fall back to a template.
  4. Optionally render the numbers yourself in templated sections and let the LLM write only the narrative sentences.
- Indian context worth including in the facts sheet: SIP/EMI outflows as committed spend, UPI small-ticket frequency (e.g. number of food-delivery orders), and credit-card bill payments treated as transfers so spending is not double-counted.

### Gaps
- No open-source project was found that publishes its monthly insight prompt and evaluation. The patterns come from general finance-LLM literature and practitioner posts, not personal-finance-specific evaluations.
- No user studies were found on whether LLM advice changes spending behaviour.
