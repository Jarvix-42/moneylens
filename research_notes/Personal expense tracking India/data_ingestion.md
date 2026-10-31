# Data ingestion routes for a personal expense tracker in India (late 2026)

Note on method: the web search tool worked, but WebFetch could not reach most primary-source hosts in this session. These hosts were blocked: support.google.com, developers.google.com, docs.setu.co, sahamati.org.in, github.io and mojekmoney.help.center. So many findings below come from search-result snippets of the cited pages, not full-page reads. Where only secondary or SEO sources were found, that is flagged.

## Email/Gmail: statements, password patterns, programmatic access, PDF decryption and extraction

### Takeaway
Emailed password-protected PDF statements are the broadest single route: they cover both bank accounts and credit cards. But the password formats vary by bank and even by product, and the sources documenting them conflict. For one person, IMAP with an app password is the lowest-friction way into Gmail. The Gmail API in "Testing" mode is possible, but the refresh token dies every 7 days, and publishing an app that uses gmail.readonly needs restricted-scope verification plus a CASA security assessment.

### Cited Findings
**Statement password patterns (all from secondary/SEO sources; none from official bank pages; they conflict):**
- HDFC credit card: the most common claim is the first 4 letters of the name (CAPS, as printed on the card) plus the last 4 digits of the card. If the card was replaced, use the new card's digits — [ClearTax](https://cleartax.in/s/hdfc-bank-statement-password), [EaseMoney](https://easemoney.in/hdfc-credit-card-statement/). An alternative claim is the last 4 card digits plus DOB as DDMM — [BuddyLoan](https://www.buddyloan.com/hdfc-bank-statement-password). One source says HDFC offers both options — [Adobe India blog](https://www.adobe.com/in/acrobat/roc/blog/credit-card-statement-password-guide.html).
- ICICI: DOB as DDMMYYYY — [EaseMoney ICICI CC](https://easemoney.in/icici-credit-card-statement-password/). Conflicting claim: first 4 letters of the name plus DDMM (e.g., "raje0501"), from a 2018 post — [TaxHeal](https://www.taxheal.com/?p=68831).
- SBI: first 4 letters of the name plus DDMM (e.g., RAHU1508) — [ClearTax SBI](https://cleartax.in/s/sbi-statement-password). Conflicting claim for SBI Card: DOB DDMMYYYY plus the last 4 card digits — [Bajaj Finserv Markets](https://www.bajajfinservmarkets.in/credit-card/what-is-my-sbi-credit-card-statement-password), [Airtel blog](https://www.airtel.in/blog/credit-card/what-is-my-sbi-credit-card-statement-password/).
- Axis: first 4 letters of the name (CAPS) plus DDMM; some sources give name plus last 4 card digits as a fallback — [ClearTax Axis](https://cleartax.in/s/axis-bank-statement-password).
- Kotak CC: first 4 letters of the name on the card (lowercase) plus DDMM — [EaseMoney Kotak](https://easemoney.in/kotak-credit-card-statement-password/). Another source says DOB DDMMYYYY — [EaseMoney list](https://easemoney.in/bank-statement-password-list/).
- IDFC First: one general guide lists DOB DDMMYYYY, but it does not say whether that applies to cards — [EaseMoney list](https://easemoney.in/bank-statement-password-list/).
- Guides note that the account statement and the card statement from the same bank can use different passwords — [Univik](https://univik.com/blog/credit-card-statement-pdf-password.html).

**Gmail API (OAuth):**
- For External user type apps in "Testing" publishing status, refresh tokens expire after 7 days unless only basic scopes (openid/email/profile) are requested. Most Gmail scopes are classed "sensitive" or "restricted" — [Nango Gmail docs](https://www.nango.dev/docs/integrations/all/google-mail), [Unified.to](https://unified.to/blog/why_google_oauth_returns).
- The fix is switching to "In production". Blog reports say new tokens then do not expire after 7 days — [tech.sailjada.com](https://tech.sailjada.com/posts/2026-05-06-1856.html). One blog describes the limit as 7 days of inactivity, which conflicts with the absolute-expiry description above.
- There is a limit of 100 live refresh tokens per Google account per OAuth client ID. A password change can revoke tokens that carry Gmail scopes — [Nango](https://www.nango.dev/docs/integrations/all/google-mail).
- gmail.readonly, gmail.modify and mail.google.com are restricted scopes. They need full verification plus an annual CASA security assessment. gmail.send is only "sensitive" — [Unified.to skill doc](https://docs.unified.to/skills/gmail-oauth-app/SKILL.md) (secondary).
- Google's page says an annual security assessment (CASA framework) is required "if your app accesses restricted scope Google users' data from or through a third-party server". It also mentions apps that "do not qualify for an exception", without the snippet naming the exception — [Google restricted scope verification](https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification?hl=es).

**IMAP with app passwords:**
- Google removed "Less Secure Apps" (plain-password IMAP) for personal accounts around 30 Sep 2024 — [Nylas CLI guide](https://cli.nylas.com/guides/gmail-app-password-setup).
- App passwords still exist (as of a March 2026 tutorial) for accounts with 2-Step Verification. The App passwords page does not appear without 2SV, and Advanced Protection disables app passwords — [Nylas CLI guide](https://cli.nylas.com/guides/gmail-app-password-setup), [Mailwarm](https://www.mailwarm.com/blog/generate-app-password-gmail-tutorial).
- One source says IMAP is always on and the toggle was removed from January 2025. Others still describe an enable step — [Cadzow KB](https://cadzow.com.au/cadzow/details.aspx?ID=2853).
- K9 Mail users report Gmail still working with app passwords (2024) — [K9 forum](https://forum.k9mail.app/t/using-gmail-with-k9-mail-5-600-in-2024-using-app-passwords-not-oauth/8536).

**PDF decryption and table extraction:**
- pikepdf is built on qpdf, and the two share limitations. qpdf can be called via subprocess to decrypt before parsing — [Stack Overflow archive](https://kiwix.lema.org/stackoverflow.com_en_all_2023-11/questions/58226546/python-data-extraction-from-an-encrypted-pdf).
- How the three extractors compare — [invoicedataextraction.com](https://invoicedataextraction.com/blog/python-pdf-table-extraction-invoices):
  - pdfplumber gives fine control over character positions and layout.
  - Camelot has lattice and stream modes. It is good with bordered tables and weaker with borderless ones.
  - tabula-py needs a Java runtime.
  - All three need a text layer, so scanned PDFs need OCR first.
- Open-source examples: a DEV post on parsing a 165-page HDFC PDF statement to CSV — [DEV Community](https://dev.to/vishwaraja_pathivishwa/building-a-pdf-parser-for-hdfc-bank-statements-from-165-pages-to-csv-in-minutes-34c6). The `expense-statement-parser` package (v0.1.x) supports HDFC fixed-width and CSV — [PyPI](https://pypi.org/project/expense-statement-parser/).

### Inferences
- For a single user who owns the Google Cloud project, the realistic options are: (a) IMAP plus an app password, which has no expiry but grants full mailbox access; or (b) the Gmail API in Testing mode with a weekly re-consent. Publishing to production to avoid the 7-day expiry, while staying unverified, may trigger "unverified app" warnings and user caps. That was not confirmed here.
- Password patterns should be stored per card/account as config, not inferred. A "try candidate passwords" loop (name4+last4, name4+DDMM, DDMMYYYY, lowercase variants) is a pragmatic fallback.
- Bank statement tables are usually borderless, so start with pdfplumber or Camelot stream mode. Expect per-bank layouts.

### Gaps
- Official bank-issued password formats could not be retrieved. All password data is from SEO/aggregator pages and conflicts. AmEx India's statement format and password were not found. My unverified belief is that AmEx India statements are not password-protected and are downloaded from the portal; check this.
- I could not read Google's OAuth docs directly (host blocked). Two points are unconfirmed: whether a self-owned "internal"/personal project is exempt from CASA, and what happens to unverified production apps with restricted scopes (the 100-user cap, the warning screen).
- I found no source confirming which banks email monthly savings-account statements by default as opposed to on request.

## Transaction alert emails vs SMS: coverage and reliability

### Takeaway
Alerts are near-real-time, but coverage differs by channel and bank. HDFC is the clearest example: from 25 Jun 2024 it stopped SMS alerts for small UPI transactions but kept email for all UPI transactions. Users also complain that some banks don't email every transaction. Neither channel alone is complete.

### Cited Findings
- HDFC stopped SMS for UPI payments up to ₹100 sent and up to ₹500 received, effective 25 June 2024. Email alerts continue for all UPI transactions regardless of amount — [Business Today](https://www.businesstoday.in/industry/banks/story/hdfc-bank-to-stop-sms-alerts-for-upi-payments-up-to-rs-100-email-notifications-to-continue-431258-2024-05-29), [Business Standard](https://www.business-standard.com/amp/finance/personal-finance/hdfc-bank-to-stop-sms-alerts-for-upi-payments-up-to-rs-100-check-details-124052901252_1.html).
- HDFC's stated rationale is that UPI apps already notify users — [Business Today](https://www.businesstoday.in/industry/banks/story/hdfc-bank-to-stop-sms-alerts-for-upi-payments-up-to-rs-100-email-notifications-to-continue-431258-2024-05-29).
- HDFC InstaAlerts (phone and email) are managed via NetBanking or the app — [HDFC InstaAlerts](https://www.hdfc.bank.in/ways-to-bank/digital-banking/phone-banking/instaalerts).
- A 2022 page says RBI asked banks to send an SMS for every transaction (possibly outdated) — [StudyCafe](https://studycafe.in/?p=113409).
- In a discussion of an open-source email-based tracker, a commenter objected that their bank does not email every transaction — [Reddit snapshot](https://reddit.sentinel-team.org/posts/1pje5tz/snapshots/2025-12-11T02%3A21%3A17.901903Z).
- UPI Ingestor reads bank emails from a Gmail label to extract UPI debits. Only HDFC is supported, with an open parser registry — [UPI Ingestor](https://vinkamath.github.io/upi-ingestor).

### Inferences
- Alerts are good for real-time categorisation. The monthly statement should be the source of truth for reconciliation and deduplication.
- For HDFC, email is more complete than SMS for small UPI payments. For other banks the reverse may hold. This needs a per-bank check.

### Gaps
- I found no systematic per-bank matrix of which transaction types (UPI, card, NEFT, auto-debit) trigger email vs SMS for ICICI, SBI, Axis, Kotak or IDFC.
- I found no reliability data such as delivery delay or missed alerts.

## Android SMS parsing, forwarders, iOS limitations and Shortcuts workarounds

### Takeaway
On Android, Play Store apps can only get READ_SMS/RECEIVE_SMS as the default SMS handler or through a case-by-case Play exception. That is why sideloaded open-source forwarders (F-Droid/GitHub) are the practical personal route. On iOS no app can read SMS. The only workaround is a user-built Shortcuts personal automation triggered by "Message contains <keyword>".

### Cited Findings
- Play policy: SMS and Call Log permission groups are restricted to apps registered as the default SMS, Phone or Assistant handler, which must stop using the permission when no longer default. Temporary exceptions for listed core functionality are granted case by case — [Play Console Help 10208820](https://support.google.com/googleplay/android-developer/answer/10208820?hl=en), [Play Console Help 9047303](https://support.google.com/googleplay/android-developer/answer/9047303).
- Android developer guidance: request the default handler role before requesting READ_SMS, and the app must actually perform the handler function — [Android default handlers](https://DEVELOPER.ANDROID.COM/guide/topics/permissions/default-handlers).
- The exception lists differ between the two Play help articles, so check the current one. A policy preview removes call-based account verification as a READ_CALL_LOG use case, effective 27 Jan 2027 — [Play Console Help 17225965](https://support.google.com/googleplay/android-developer/answer/17225965).
- bogkonstantin/android_income_sms_gateway_webhook ([listing](https://awesome.ecosyste.ms/projects/github.com%2Fbogkonstantin%2Fandroid_income_sms_gateway_webhook), [v2.4.0 release](https://newreleases.io/project/github/bogkonstantin/android_income_sms_gateway_webhook/release/v2.4.0)):
  - MIT-licensed, distributed via GitHub and F-Droid.
  - POSTs incoming SMS as JSON to a URL, with sender filtering and retry with exponential backoff.
  - v2.4.0 added optional HMAC-SHA256 payload signing.
  - The README says it is "not actively developing" and points to a community fork (scottmconway).
- Example of piping it to Telegram via a webhook — [DEV Community](https://dev.to/alikhil/forward-sms-to-telegram-with-android-198f).
- Self-hosted projects:
  - Hisabi, a self-hosted finance app with an SMS parser and Docker — [GitHub](https://github.com/hisabi-app/hisabi).
  - OwnSpend, FastAPI plus SQLite, which captures Android SMS and UPI notifications (Kotak, UCO, GPay) — [GitHub](https://github.com/Godatcode/OwnSpend).
- iOS:
  - Apple doesn't let apps read texts — [Pocket Clear](https://pocketclear.app/blog/auto-log-apple-pay-bank-sms-iphone-shortcuts.html).
  - Community setups use a Shortcuts Message trigger with "Message Contains" keywords such as "spent" or "debited", set to Run Immediately — [r/shortcuts](https://redlib.ssps.io/r/shortcuts/comments/1ko40wz/sharing_an_automatic_expense_tracker_for_india), [Technofino](https://technofino.in/community/threads/expense-tracker-for-ios-mac-using-shortcuts-app.40920/latest).
  - Keywords must match each bank's wording. HDFC card SMS uses "Spent" — [Pocket Clear](https://pocketclear.app/blog/auto-log-apple-pay-bank-sms-iphone-shortcuts.html).
  - Commercial iOS apps built on this approach: iLekka ([App Store](https://apps.apple.com/app/id6766097186)) and Spender ([FAQ](https://mubaraknumann.github.io/Spender/faq.html)).

### Inferences
- A personal tracker should accept a generic webhook (JSON: sender, body, timestamp) so the same endpoint serves both Android forwarders and iOS Shortcuts "Get Contents of URL".
- Sideloading avoids Play policy entirely for personal use. A Play-distributed app would need to be the default SMS app or win an exception.
- Keyword-triggered iOS automations can miss messages when banks change wording. An LLM- or regex-based parser at the server is more robust than keyword logic on the device.

### Gaps
- I could not confirm whether the current Play exception list still includes "SMS-based money management". Older policy versions are believed to have listed it, but this could not be verified (host blocked).
- I could not confirm the reliability of iOS Message automations, e.g., whether they still run while the phone is locked. Community sources are anecdotal.

## Account Aggregator (AA)

### Takeaway
An individual cannot become an FIU. FIUs are entities regulated by a financial-sector regulator, and onboarding needs company PAN/GSTIN, an AA agreement and Sahamati certification. AAs themselves must be NBFC companies with at least ₹2 crore NOF. Credit cards appear not to be supported as an FI type. Consumer AA apps (Finvu, Onemoney) handle consent but show no data-export feature. AA is effectively not a personal-use ingestion route except indirectly via a third-party FIU app.

### Cited Findings
- AA licence (2016 Master Direction): NOF of at least ₹2 crore; only a company may be an AA; an NBFC-AA cannot do other business; permanently in the Base Layer — [RBI Master Direction](https://rbi.org.in/scripts/BS_ViewMasDirections.aspx?id=10598).
- A consultancy says the rules were rewritten as the Account Aggregator Directions, 2025 (Nov 2025), with NOF at para 11(3) and a 7:1 leverage cap. This is secondary; verify on rbi.org.in — [Treelife](https://treelife.in/fintech/nbfc-account-aggregator-licence-eligibility/).
- An FIU is defined as an entity registered with and regulated by a financial-sector regulator (RBI, SEBI, IRDAI, PFRDA) — [RBI Master Direction](https://rbi.org.in/scripts/BS_ViewMasDirections.aspx?id=10598), [Cybersigma](https://cybersigmacs.com/knowledge-center/account-aggregator/).
- FIU onboarding: the FIU signs an agreement with an AA, then Sahamati registers it in the Central Registry — [Sahamati](https://sahamati.org.in/?p=3007).
- Setu sandbox: register on "Bridge" with company PAN and GSTIN, create an FIU and an AA data product, and activate sandbox credentials. For go-live, Setu as TSP handles Sahamati certification via Aujas — [Setu support](https://support.setu.co/support/solutions/articles/81000412878-how-does-one-generate-sandbox-credentials-), [Setu go-live](https://docs.setu.co/data/account-aggregator/licenses-and-go-live/go-live). KYC requirements for FIU onboarding are listed at [Setu KYC FAQ](https://support.setu.co/support/solutions/articles/81000410135-what-are-the-kyc-requirements-to-get-on-boarded-as-an-fiu-on-the-aa-ecosystem-).
- Finvu publishes a sandbox plus FIP/FIU modules — [Finvu sandbox](https://finvu.github.io/sandbox). Sahamati runs a SahamatiNet sandbox, which requires ReBIT-compliant APIs — [Sahamati developer](https://developer.sahamati.org.in/sahamatinet-poc/integration-steps/sandbox-onboarding).
- Setu docs say the AA ecosystem supports 23 FI types across bank, insurance, pension and investments — [Setu FI types](https://docs.setu.co/data/account-aggregator/v1/fi-data-types).
- Mojek says credit card data is not part of the AA framework, so it uses an email-based method — [Mojek help](https://mojekmoney.help.center/article/1079-how-do-you-fetch-my-credit-card-related-data.md).
- Sahamati's "account types activated" table (updated 14 Nov 2025) lists savings, current, term deposit and recurring deposit. "CC/OD" there means cash-credit or overdraft on current accounts, not credit cards — [Sahamati account types](https://sahamati.org.in/account-types-activated-on-aas/).
- Scale: about 989 FIUs live and 17 AA licences (undated, attributed to Sahamati) — [DFS](https://financialservices.gov.in/beta/en/account-aggregator-framework).
- Consumers create an AA handle with a mobile OTP, and registration is voluntary — [DFS](https://financialservices.gov.in/account-aggregator-framework).
- Finvu provides a VUA handle and consent revocation, and the AAs do not store data passing through them — [Finvu iOS](https://apps.apple.com/in/app/finvu/id6499302887). Onemoney has Android, iOS and web apps — [Onemoney iOS](https://apps.apple.com/in/app/onemoney-aa/id1662335699). I found no export feature in either.

### Inferences
- The only AA-based personal route is consenting to a third-party FIU app (a PFM or lender app) and using whatever export that app offers. AA data is also "data-blind" to the AA, so consumer AA apps do not hold a copy to export.
- Because credit cards appear unsupported, AA could at best cover bank accounts. Cards still need email statements or SMS.

### Gaps
- I could not open Sahamati's or ReBIT's FI-type list to check whether a CREDIT_CARD FI type has been defined and whether any FIP has gone live with it (hosts blocked). Treat "no credit card support" as likely but unverified for late 2026.
- I could not verify the paragraph numbers of the 2025 RBI AA Directions from RBI directly.
- I found no evidence of any "personal FIU" or individual developer route.

## UPI app exports (PhonePe, Paytm, Google Pay, BHIM)

### Takeaway
Google Pay India officially offers an in-app statement (PDF) of Google Pay transactions only. Evidence on PhonePe, Paytm and BHIM exports is weak and secondary. UPI app history covers only payments made via that app, so it is a supplement for merchant names and notes, not a primary source.

### Cited Findings
- Google Pay India: transaction history lives under "See transaction history", and you can get a statement. History covers only Google Pay transactions, not all bank activity — [Google Pay Help](https://support.google.com/pay/india/answer/7430307?hl=en-IN). "Download Statement" produces a PDF for a date range — [Zetapp](https://zetapp.in/blog/how-to-check-upi-transaction-history).
- Google Takeout includes Google Pay activity and transactions "depending on your usage". I could not confirm India UPI coverage or the format — [Wikipedia: Google Takeout](https://en.wikipedia.org/wiki/Google_Takeout), [ITPro](https://itpro.com/security/privacy/368243/using-google-takeout-to-reclaim-your-data).
- PhonePe and Paytm: "Download Statement" flows with the file emailed. This is a single low-quality source that also wrongly claims GPay has no export — [Cal Poly page](https://grandavehousing.calpoly.edu/news/how-to-export-your-upi).
- BHIM: download from the three-dot menu into a "bhimTransaction" folder; reliability is uncertain — [GeekInstructor](https://www.geekinstructor.com/?p=87).

### Inferences
- UPI app statements are useful for enriching bank lines with payee names, notes and categories. The bank account still records the debit, so deduplicate against bank data.

### Gaps
- I found no official PhonePe or Paytm help pages. Export formats (PDF vs Excel), date-range limits and passwords were not verified.

## Net banking CSV/Excel/XLS statement downloads

### Takeaway
Most banks offer downloadable statements from net banking in PDF, XLS and sometimes "Delimited" (CSV-like) formats. These are the most structured manual route. Column layouts are undocumented and need per-bank parsers built from real samples.

### Cited Findings
- HDFC NetBanking: Accounts → Enquire → "A/c statement upto 5 years" → choose format "Delimited". Save as "All Files" with a .csv name, because the browser may drop the extension (early-2024 steps) — [Expense_manager GitHub](https://github.com/Anirudh1905/Expense_manager).
- HDFC also provides XLS. One project requires converting it into a defined column layout before parsing — [hdfc-analytics](https://awesome.ecosyste.ms/projects/github.com%2Fvipul-sharma20%2Fhdfc-analytics).
- Trakk is a web app that imports Excel statements from Indian banks — [GitHub](https://github.com/kartikbhardwaj017/trakk).
- Reddit users favour CSV/PDF statement import over SMS scraping for privacy — [Reddit snapshot](https://reddit.sentinel-team.org/posts/1pfdy7v/snapshots/2025-12-06T05%3A31%3A42.296911Z).

### Inferences
- A per-bank "importer" abstraction (sniff header row, map columns, parse DD/MM/YY dates and Dr/Cr or debit/credit columns) is needed. Statement downloads suit monthly reconciliation, not real-time tracking.

### Gaps
- I found no authoritative documentation of column names, header/footer rows, encoding or date formats for HDFC, ICICI, SBI, Axis, Kotak or IDFC exports. Credit card portals' CSV availability was not verified.

## Personal APIs from Indian banks

### Takeaway
No evidence was found of any Indian bank offering transaction APIs to individual retail customers. ICICI's API Banking portal targets businesses, corporates and fintech partners.

### Cited Findings
- ICICI API Banking covers collections, payments, reconciliations, balance and statements "from your server". It is aimed at current-account and business users and partners — [ICICI blog](https://www.icici.bank.in/blogs/current-account/api-banking-icici), [Finextra](https://finextra.com/newsarticle/35065/icici-bank-opens-api-portal-to-developers).
- A 2019 trade report says the ICICI portal is for businesses, fintechs, corporates and e-commerce start-ups — [Retail Banker International](https://www.retailbankerinternational.com/news/icici-bank-launches-api-banking-portal/).

### Inferences
- With AA closed to individuals and bank APIs being B2B, the personal-use stack is: email statements plus email alerts, SMS forwarding, and manual CSV/XLS downloads, with UPI app exports as enrichment.

### Gaps
- I did not check whether HDFC, Axis, Kotak or other banks run developer portals that accept individuals. ICICI's is likely representative, but this is unconfirmed.
