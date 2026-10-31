# Open-source projects for Indian personal finance data (statement parsers, SMS parsers, Gmail extractors, India-focused trackers)

> Method note (as of 2026-10-31): The GitHub search and repo APIs were blocked in this research session (HTTP 403, "sessions are bound to their configured repositories"), and direct HTTP to github.com was refused by the egress proxy. Star, fork, and commit counts below come from WebFetch renders of the repo pages taken on 2026-10-31. Those renders usually did **not** show last-commit dates or release dates, so last-commit activity is mostly **unverified**. Where I had no repo fetch, the project's description comes from search-result snippets, and I say so. paisa.fyi could not be fetched (host not in the allowed network list).

## Which repos parse Indian bank/credit card/MF statements (HDFC, ICICI, SBI, Axis, Kotak, CAMS/KFintech) from PDF/CSV?

### Takeaway
The only mature, widely used statement parser in this space is **casparser**: MIT, 239 stars, 353 commits. It covers CAMS/KFintech/NSDL/CDSL CAS PDFs. Bank and credit-card statement parsing is spread across many small single-author Python repos, each with fewer than 20 stars. They are built on pdfplumber/PyMuPDF/pypdf with one parser per bank layout and a password-decrypt step. No dominant multi-bank library exists, and Axis coverage is especially thin.

### Cited Findings
**Mutual fund / demat CAS**
- **codereverser/casparser** (https://github.com/codereverser/casparser): Python parser for CAS PDFs. It supports CAMS Detailed/Summary, KFintech Detailed/Summary, and NSDL and CDSL demat consolidated statements. Stats: 239 stars, 94 forks, 353 commits, 13 open issues, 2 open PRs, MIT. — [GitHub](https://github.com/codereverser/casparser)
- casparser limitations: it rejects re-printed PDFs (printing flattens the watermark and strips generator metadata). MF Central statements are out of scope for v1.0. Broker-portal re-renders and Excel→PDF exports are unsupported. Unreconciled data is flagged through `parse_warnings` rather than blocked. ISIN/AMFI lookup can fail. Gift transfers are excluded from capital gains. — [GitHub](https://github.com/codereverser/casparser)
- casparser output is typed pydantic models (`CASData`, `NSDLCASData`) or JSON. Its CLI prints portfolio summary and capital gains. ISIN/AMFI lookup comes through the `casparser-isin` helper (since v0.4.3). v1.0 moved to pypdfium2. Up to v0.8, the optional mupdf extra pulled in PyMuPDF (GPL/AGPL implications). — [PyPI casparser](https://pypi.org/project/casparser/); [PyPI 1.0.0](https://pypi.org/project/casparser/1.0.0/)

**Bank account statements**
- **akhilnarang/bank-statement-parser** (https://github.com/akhilnarang/bank-statement-parser): Python (uv, pytest, ruff), MIT, 5 stars, 1 fork, 37 commits. Banks: HDFC, ICICI, IDFC FIRST, IndusInd, Kotak, Slice, SBI (monthly e-statement plus YONO "Statement of Account"), and Union Bank. It uses pdfplumber plus PyMuPDF block extraction. Output is a `ParsedBankStatement` with a reconciliation check (opening + credits − debits = closing, delta 0.00). Each transaction carries channel (UPI/NEFT), reference number, and derived counterparty. Exports are JSON and CSV. Encrypted PDFs prompt for a password. The `tests/` directory is empty. It is a companion to a `cc-parser` project for credit cards. — [GitHub](https://github.com/akhilnarang/bank-statement-parser)
- **himanshu15y/bank-statement-parser** (https://github.com/himanshu15y/bank-statement-parser): fork of the above, described in a search snippet as adding SBI support with the same reconciliation output (not fetched; stats unknown). — [search result](https://github.com/himanshu15y/bank-statement-parser)
- **tohundguide/bankstatementparser** (https://github.com/tohundguide/bankstatementparser): JK Bank, ICICI, HDFC, SBI, Equitas, PNB and others. Excel export, password-protected PDFs, OCR for scanned files (snippet only). — [GitHub](https://github.com/tohundguide/bankstatementparser)
- **MalayPalace/Bank-Statement-Utility** (https://github.com/MalayPalace/Bank-Statement-Utility): parses bank statement/transaction files, stores them, and supports expense analytics (snippet only). — [GitHub](https://github.com/MalayPalace/Bank-Statement-Utility)
- **Shivapande25/Banking-Statement-Analyzer** (https://github.com/Shivapande25/Banking-Statement-Analyzer): Python, ICICI and SBI. Categorization, end-of-day balances, basic fraud detection (snippet only). — [GitHub](https://github.com/Shivapande25/Banking-Statement-Analyzer)
- **punchagan/expense-tracker** (https://github.com/punchagan/expense-tracker): Python tool that scrapes Axis Bank statements into SQLite. The author found bank SMS sometimes lacked enough detail (snippet). — [GitHub](https://github.com/punchagan/expense-tracker)
- PyPI packages: **statementsparser** (SBI, HDFC, ICICI PDFs, auto-detects bank) — [PyPI](https://pypi.org/project/statementsparser/); **indian-finance-parsers** (ICICI, HDFC, SBI Card; optional in-memory decrypt so plaintext never touches disk; page dated Oct 2026) — [PyPI](https://pypi.org/project/indian-finance-parsers/); **xfina** — [PyPI](https://pypi.org/project/xfina/0.4.0/) (details not verified).

**Credit card statements**
- **xaneem/hdfc-credit-card-statement-parser** (https://github.com/xaneem/hdfc-credit-card-statement-parser): Python (`plumber.py`, i.e. pdfplumber), MIT, 17 stars, 6 forks, 8 commits. HDFC CC PDF → CSV, including forex amount/rate and a Cr/Dr column. The password is set in `run.sh`. The author says "built for personal use and does not guarantee accuracy". — [GitHub](https://github.com/xaneem/hdfc-credit-card-statement-parser)
- **rajatpandit/hdfc_creditcard_statement_parser** (https://github.com/rajatpandit/hdfc_creditcard_statement_parser): HDFC CC → CSV script (snippet). — [GitHub](https://github.com/rajatpandit/hdfc_creditcard_statement_parser)
- **tanumay-deb/credit-card-spend-intelligence** (https://github.com/tanumay-deb/credit-card-spend-intelligence): Gmail fetch of password-protected CC PDFs, HDFC/ICICI/SBI parsers (one per bank, pypdf + pdfplumber), reconciliation, Power BI dashboard, 369 tests (snippet). — [GitHub](https://github.com/tanumay-deb/credit-card-spend-intelligence)
- **madhav921/Card-Statement-Analyser** (https://github.com/madhav921/Card-Statement-Analyser): listed on the GitHub topics page as an Indian CC PDF tool for HDFC, ICICI, SBI, Axis and 5 more, using Ollama (snippet via topic listing). — [GitHub topic: credit-card](https://github.com/topics/credit-card?l=python&o=asc&s=forks&utf8=%E2%9C%93)
- **cshukla83/credit-card-tracker** (https://github.com/cshukla83/credit-card-tracker): PDF + password input with a preview before import. Passwords live in `.env`. Optional LLM fallback for unsupported banks (snippet). — [GitHub](https://github.com/cshukla83/credit-card-tracker)
- **BanerjeeArjun/awesome-statement-parser** (https://github.com/BanerjeeArjun/awesome-statement-parser): generic (not India-specific) PDF/CSV/XLS(X) parser with a `password` argument (snippet). — [GitHub](https://github.com/BanerjeeArjun/awesome-statement-parser)

### Inferences
- The common pattern is decrypt PDF (passwords often derived from DOB/name/card digits) → pdfplumber/PyMuPDF table or word extraction → one hand-written parser per bank layout → validate through balance reconciliation → CSV/JSON. LLM fallback for unknown layouts is a newer add-on (2025–26 repos).
- Most bank/CC parsers are personal-use scripts with single-digit to tens of stars, so any product would likely need its own parser suite. casparser is the one dependency mature enough to adopt directly (MIT).
- Layout drift is a structural risk. Templates change, reprinted PDFs break casparser's verification, and akhilnarang's repo ships no tests.

### Gaps
- Last-commit dates for all of these were not visible, because the GitHub API was blocked and page renders omitted them.
- No Axis-specific or Kotak-only CC parser of note was found. madhav921's Axis support is only claimed in a topic listing.
- The license and star counts of the snippet-only repos are unverified.

## Which repos parse Indian bank SMS transaction alerts?

### Takeaway
SMS parsing is the most active open-source area. **PennyWise AI** leads clearly (Kotlin/Compose, AGPL-3.0, 607 stars, 1,419 commits, on F-Droid, 57 Indian banks). **Cashiro** is a PennyWise-inspired AGPL app with 204 stars. **transaction-sms-parser** (TypeScript, MIT, 60 stars) is the main reusable library, though its npm release appears stale since mid-2024. Every one of these is regex/rule-based per bank, sometimes with an on-device ML/LLM add-on.

### Cited Findings
- **sarim2000/pennywiseai-tracker** (https://github.com/sarim2000/pennywiseai-tracker): 607 stars, 167 forks, 1,419 commits, 15 open issues, AGPL-3.0. Kotlin, Jetpack Compose, Room, Hilt, MVVM, Android 8.0+. The repo also contains `iosApp`, `pennywise-web`, `relay` and a separate `parser-core` SMS parsing module. On-device AI uses LiteRT-LM with Qwen 2.5 (~1.5 GB one-time model download). It lists 171 banks/services across 26 countries, 57 of them in India. A rules engine handles auto-categorization and blocking. — [GitHub](https://github.com/sarim2000/pennywiseai-tracker)
- PennyWise is on F-Droid (`com.pennywiseai.tracker`) and uses regex patterns for 20+ banks including HDFC, SBI, ICICI and Axis. F-Droid shows versions around 2.15.49–2.15.50, and 2.15.50 added auto-tracking from bank app notifications. One locale page says the AI was upgraded to Gemma 3 1B. — [F-Droid](https://f-droid.org/ko/packages/com.pennywiseai.tracker); [F-Droid zh](https://f-droid.org/zh_Hans/packages/com.pennywiseai.tracker)
- License conflict: GitHub says AGPL-3.0, but an IzzyOnDroid inclusion request lists MIT (probably an earlier license). An optional paid "Pro" tier adds unlimited rules, PDF statement import, CSV export and account merge. — [IzzyOnDroid issue](https://gitlab.com/IzzyOnDroid/repo/-/issues/821); [GitHub](https://github.com/sarim2000/pennywiseai-tracker)
- PennyWise open issues: transaction not detected (#1034), balance mismatches (#1022, #982), a category change from a notification also updating old transactions (#1029), loan bugs (#1032), and refund/reimbursement linking requests (#995, #983). Feature requests include a "generic extractor + suggested-transactions review" so unsupported banks work on day one (#994), custom user-defined parsing rules (#279), a review queue for ambiguous matches (#801), and email processing instead of SMS (#156). — [Issues](https://github.com/sarim2000/pennywiseai-tracker/issues)
- **ritesh-kanwar/Cashiro** (https://github.com/ritesh-kanwar/Cashiro): 204 stars, 45 forks, 828 commits, 94 open issues, AGPL-3.0. Kotlin/Compose, MediaPipe on-device AI, has a `parser-core` module. It credits PennyWise AI as "the base and inspiration", lists 40+ Indian banks (HDFC, SBI, ICICI, Axis, Kotak, PNB, Canara, BoB, IDFC, Yes, AU, IndusInd, IPPB, Post Office, etc.) plus Slice/LazyPay/Jio Pay, and is on F-Droid. Its README is inconsistent on coverage ("40+ banks across 5 countries" versus "80+ banks across 14 countries"). — [GitHub](https://github.com/ritesh-kanwar/Cashiro); [Wiki](https://github.com/ritesh-kanwar/Cashiro/wiki)
- **saurabhgupta050890/transaction-sms-parser** (https://github.com/saurabhgupta050890/transaction-sms-parser): TypeScript, MIT, 60 stars, 18 forks, 101 commits, 5 open PRs. API: `getTransactionInfo(message) → {account{type,number,name}, balance{available,outstanding}, transaction{type,amount,referenceNo,merchant}}`. Tested banks: Axis, ICICI, Kotak, HDFC, StanChart, IDFC, Niyo, SBM, Federal. Cards: HSBC, Citi, ICICI, Uni, IndusInd, Slice, OneCard, HDFC, IDFC. Wallets: Paytm, Amazon Pay, LazyPay, Simpl. The author calls it "a naive implementation using mostly regular expressions." — [GitHub](https://github.com/saurabhgupta050890/transaction-sms-parser)
- npm `transaction-sms-parser` latest v3.3.0, ~94 weekly downloads, last updated 13 July 2024. — [socket.dev](https://socket.dev/npm/package/transaction-sms-parser)
- Dart/Flutter packages: `transaction_sms_parser` v0.0.1 (claims 30+ Indian banks) — [pub.dev](https://pub.dev/packages/transaction_sms_parser); `upi_sms_parser` v0.1.0 (UPI SMS, OTP/promo filtering, multiple Indian date formats) — [pub.dev](https://pub.dev/documentation/upi_sms_parser/latest/)
- Smaller Android SMS trackers (snippet-level, stats unverified):
  - **allwin-antony/Expense_tracker** (Flutter, MIT, on-device FastText ML, blocks loan spam) — [GitHub](https://github.com/allwin-antony/Expense_tracker)
  - **DdarkSaber/Spend** (no INTERNET permission; generic rules checked against 278 real SMS from 22 banks) — [GitHub](https://github.com/DdarkSaber/Spend)
  - **bharathmay-boop/offspend** (one-tap categorization, on-device) — [GitHub](https://github.com/bharathmay-boop/offspend)
  - **nishanthdn96/expense-tracker** (Compose; SBI, HDFC, ICICI, Axis, Kotak, PNB, BoB, Canara) — [GitHub](https://github.com/nishanthdn96/expense-tracker)
  - **i-m-anurag/expense-manager** (React Native, GPLv3, salary-cycle budgets; HDFC/ICICI/SBI/Axis/BOI) — [GitHub](https://github.com/i-m-anurag/expense-manager)
  - **soham-raddi/expense-tracker** (offline, SMS + PDF import, ICICI; per-bank parser with synthetic fixtures) — [GitHub](https://github.com/soham-raddi/expense-tracker)
  - **Anandavnish/expense-tracker** (MIT, Gemini receipt scanner) — [GitHub](https://github.com/Anandavnish/expense-tracker)
  - **DeepakSilaych/umber** (offline, no INTERNET permission; the author says merchant extraction is the weakest part) — [GitHub release](https://github.com/DeepakSilaych/umber/releases/tag/v0.1.0)
  - **srineshr1/Paymenttracker** — [GitHub](https://github.com/srineshr1/Paymenttracker)
- **GreenIcePhoenix/TraceLedger** (https://github.com/GreenIcePhoenix/TraceLedger/): Kotlin, 30+ Indian banks/wallets plus PDF import. Its README says it is source-available for viewing/reference only, i.e. not open source. — [GitHub](https://github.com/GreenIcePhoenix/TraceLedger/)
- An F-Droid RFP for a related app exists. — [F-Droid RFP #3359](https://gitlab.com/fdroid/rfp/-/issues/3359)

### Inferences
- The PennyWise architecture has become a de-facto template: a separate `parser-core` module of per-bank regex parsers keyed by sender ID, Room storage, a rules engine, an optional on-device small LLM, and no cloud. Cashiro copies it, and its 94 open issues suggest it is under-maintained relative to usage.
- The dominant open-source licence for full apps is AGPL-3.0, which constrains reuse of their parser code in closed products. MIT options are transaction-sms-parser (stale) and small hobby apps.
- Recurring pain points are coverage gaps for unsupported banks, balance/accounting drift, refund/self-transfer handling, and merchant extraction. Requests are moving toward generic extractors with a user review queue and toward notification and email ingestion.

### Gaps
- Exact last-commit dates for PennyWise and Cashiro could not be confirmed. Their high commit counts and F-Droid version bumps suggest activity in 2025–26.
- Download counts from F-Droid and the Play Store were not retrieved.

## Which repos pull bank emails from Gmail and extract transactions (including LLM-based)?

### Takeaway
Gmail extractors are almost all small 2025–26 personal projects, typically 0 stars and one or a few commits. Two patterns dominate: Gmail API OAuth with Gemini extraction, or IMAP with an App Password, per-bank regex, and a local Ollama fallback. HDFC InstaAlerts is the most common target. No mature, widely adopted Gmail extractor exists.

### Cited Findings
- **medhruv7/Expense-Manager** (https://github.com/medhruv7/Expense-Manager): 0 stars, 1 commit, no license shown. Python/FastAPI/SQLite, Chart.js PWA, Tailscale. It ingests both SMS (Android forwarder app → token-protected webhook, plus SMS Backup & Restore XML import) and Gmail. Gmail access is IMAP with an App Password, polled every 15 minutes, read-only, filtered by sender, batches of 50, with a 90-day backfill. Banks: HDFC, Axis, ICICI, SBI Card, with generic fallbacks. The LLM (Ollama or an OpenAI-compatible API) is used only for regex failures and unmatched merchants. SMS and email copies of one transaction are deduped by UPI reference or by amount+account+time. Self-transfers are detected as matching debit/credit within 30 minutes. Reported pitfalls: SQLite corruption under OneDrive sync, and Android 13+ "restricted settings" blocking SMS permission. — [GitHub](https://github.com/medhruv7/Expense-Manager)
- **narendran-kannan/expense_tracker** (https://github.com/narendran-kannan/expense_tracker): Gmail API + Google Gemini extract and categorize debit alerts. Next.js/Prisma/Tailwind; covers IDFC FIRST among others (snippet). — [GitHub](https://github.com/narendran-kannan/expense_tracker)
- **prem05112002/my-expense-tacker** (https://github.com/prem05112002/my-expense-tacker): Python, Gmail IMAP with an App Password and a Gmail filter routing HDFC mail to a label. Extracts amount, merchant, date and payment mode. Multi-agent chatbot on Groq or Gemini (snippet). — [GitHub](https://github.com/prem05112002/my-expense-tacker)
- **balajiregt/myFinance** (https://github.com/balajiregt/myFinance): self-hosted single-file Indian portfolio tracker with AI insights and Gmail expense sync (Google OAuth read-only scope, HDFC InstaAlerts) (snippet). — [GitHub](https://github.com/balajiregt/myFinance)
- **androdev700/money-tracker** (https://github.com/androdev700/money-tracker): Gmail label over IMAP, per-bank regex, unknown formats sent to local Ollama, unparseable emails sent to a review queue. PWA over Tailscale on a Mac (snippet). — [GitHub](https://github.com/androdev700/money-tracker)
- **akkolluru/Expense-Tracker** (https://github.com/akkolluru/Expense-Tracker): runs on Termux or a Raspberry Pi with a local llama.cpp model (Qwen2.5-1.5B quantized) for classification (snippet). — [GitHub](https://github.com/akkolluru/Expense-Tracker)
- **okpalindrome/xpens** (https://github.com/okpalindrome/xpens): Gmail API, modular parsers, upsert keyed on Gmail message ID so re-syncs never duplicate (snippet). — [GitHub](https://github.com/okpalindrome/xpens)
- **akhilKinnu/money-manager** (https://github.com/akhilKinnu/money-manager): zero-database web app parsing HDFC UPI/card/netbanking emails without an LLM. **XashutoshX/moneymate-expense-tracker** (https://github.com/XashutoshX/moneymate-expense-tracker): local single-user HDFC Gmail tracker (snippets). — [GitHub](https://github.com/akhilKinnu/money-manager); [GitHub](https://github.com/XashutoshX/moneymate-expense-tracker)
- **tanumay-deb/credit-card-spend-intelligence** also fetches CC statement PDFs from Gmail before parsing. — [GitHub](https://github.com/tanumay-deb/credit-card-spend-intelligence)
- Reported pitfall: in Google OAuth "Testing" mode, refresh tokens expire after about 7 days. Duplicate alerts for the same payment also need dedupe (from search summary of these repos). — [search summary over repos above](https://github.com/narendran-kannan/expense_tracker)

### Inferences
- IMAP plus App Password is preferred by self-hosters to avoid Google's OAuth verification and restricted-scope (gmail.readonly) security assessment. A consumer product using the Gmail API would face CASA/restricted-scope review. That is a likely reason no polished open-source Gmail extractor exists.
- Best practices visible in this set: dedupe by message ID and UPI reference, regex first with LLM fallback, a review queue for failures, and local LLMs (Ollama/llama.cpp) for privacy.

### Gaps
- None of these Gmail repos showed meaningful stars or long-term maintenance. Most stats come from snippets and are unverified.
- I found no reliable accuracy benchmarks comparing LLM and regex extraction on Indian bank emails.

## Which India-specific open-source expense trackers exist (F-Droid Android apps, self-hosted web apps)?

### Takeaway
On Android, PennyWise AI (F-Droid) and Cashiro (F-Droid) are the notable India-first open-source trackers. For self-hosting, **ananthakumaran/paisa** is the most-starred project by an India-based author: 3.2k stars, AGPL-3.0, Go + Svelte, built on ledger plaintext accounting. Its India-specific features (Indian MF/NPS price fetching) could not be verified this session. Everything else is a hobby self-hosted app.

### Cited Findings
- **ananthakumaran/paisa** (https://github.com/ananthakumaran/paisa): 3.2k stars, 218 forks, AGPL-3.0. Go backend, Svelte/TypeScript/Vite/Tailwind frontend, built on ledger plaintext double-entry accounting. The fetched README excerpt did not mention CAMS/MF/NPS features. — [GitHub](https://github.com/ananthakumaran/paisa)
- PennyWise AI (F-Droid) and Cashiro (F-Droid). — [F-Droid PennyWise](https://f-droid.org/pl/packages/com.pennywiseai.tracker); [Cashiro](https://github.com/ritesh-kanwar/Cashiro)
- Self-hosted India-focused web apps: medhruv7/Expense-Manager, androdev700/money-tracker, balajiregt/myFinance, akkolluru/Expense-Tracker (see above). — [GitHub](https://github.com/medhruv7/Expense-Manager); [GitHub](https://github.com/balajiregt/myFinance)
- Other privacy-first Android apps found by search: Spentd (imports bank/UPI SMS plus PhonePe/GPay screenshots) and MFTracker (MIT, SMS and notifications, no internet permission). No repo URLs were returned for these. — [search summary](https://github.com/medhruv7/Expense-Manager)

### Inferences
- The India-specific open-source tracker landscape is dominated by SMS-first Android apps. Web and self-hosted options are fragmented one-person projects, and generic tools (paisa, Firefly III, Actual) need custom importers for Indian banks.

### Gaps
- paisa.fyi docs (import templates, Indian MF support) were unreachable because the host is not in the allowed network list. Confirm whether paisa ships HDFC/ICICI/SBI/Axis/Kotak CSV import templates and an mfapi.in integration. I believe it does, but this is unverified.
- Spentd and MFTracker repo URLs were not found.
- I found no Reddit or HN threads. Searches restricted to r/developersIndia and r/IndiaInvestments returned only GitHub results.

## Which projects are actively maintained vs abandoned? Common design patterns and pitfalls

### Takeaway
Actively maintained (by commit volume, F-Droid releases and open-issue churn): PennyWise AI, Cashiro (though with a large issue backlog), casparser, and paisa. Likely stale: transaction-sms-parser (npm last updated July 2024) and single-commit or personal-script repos (xaneem, rajatpandit, medhruv7). Common pitfalls are bank format drift, password-protected PDFs, deduping SMS and email copies, self-transfers and refunds, and merchant extraction.

### Cited Findings
- PennyWise: 1,419 commits, issue numbers past #1034, F-Droid versions 2.15.4x–2.15.50. — [GitHub](https://github.com/sarim2000/pennywiseai-tracker); [Issues](https://github.com/sarim2000/pennywiseai-tracker/issues); [F-Droid](https://f-droid.org/zh_Hans/packages/com.pennywiseai.tracker)
- Cashiro: 828 commits, 94 open issues. — [GitHub](https://github.com/ritesh-kanwar/Cashiro)
- casparser: 353 commits, v1.0 rewrite (pypdfium2), 13 open issues. — [GitHub](https://github.com/codereverser/casparser); [PyPI](https://pypi.org/project/casparser/1.0.1/)
- transaction-sms-parser: npm v3.3.0, last updated 2024-07-13, ~94 weekly downloads. — [socket.dev](https://socket.dev/npm/package/transaction-sms-parser)
- xaneem HDFC CC parser: 8 commits, "personal use, no accuracy guarantee". — [GitHub](https://github.com/xaneem/hdfc-credit-card-statement-parser)
- Pitfalls reported by projects themselves:
  - Reprinted PDFs fail verification — [casparser](https://github.com/codereverser/casparser)
  - Unsupported banks; users want generic extractors and custom rules — [PennyWise #994, #279](https://github.com/sarim2000/pennywiseai-tracker/issues)
  - Balance mismatches — [PennyWise #1022/#982](https://github.com/sarim2000/pennywiseai-tracker/issues)
  - Merchant extraction is the weakest part — [umber](https://github.com/DeepakSilaych/umber/releases/tag/v0.1.0)
  - SMS sometimes lacks enough detail — [punchagan](https://github.com/punchagan/expense-tracker)
  - Android 13+ restricted-settings hurdle for SMS permission — [medhruv7](https://github.com/medhruv7/Expense-Manager)
  - Loan-spam SMS filtering — [allwin-antony](https://github.com/allwin-antony/Expense_tracker)
  - Duplicate SMS and email alerts — [medhruv7](https://github.com/medhruv7/Expense-Manager)
  - Passwords stored in scripts or `.env` — [xaneem](https://github.com/xaneem/hdfc-credit-card-statement-parser); [cshukla83](https://github.com/cshukla83/credit-card-tracker)

### Inferences
- Design patterns across the ecosystem:
  1. A per-bank parser registry keyed by SMS sender ID or PDF fingerprint.
  2. Regex/rules first, with LLM/ML fallback (on-device Qwen/Gemma/FastText, or Ollama).
  3. Balance reconciliation as a correctness check for statements.
  4. Idempotent ingestion keyed on message ID or UPI ref.
  5. A review queue for low-confidence parses.
  6. Privacy positioning: no INTERNET permission, local DB, Tailscale instead of cloud.
- Maintenance correlates with having a distributable app on F-Droid and a community filing bank-format issues. Libraries without an app (transaction-sms-parser) and personal scripts stagnate.

### Gaps
- Precise last-commit dates for every repo (blocked API). Recommend re-verifying with `gh api repos/<owner>/<repo>` in a session with GitHub access.
- I did not mine issues for casparser, Cashiro or transaction-sms-parser in detail.
