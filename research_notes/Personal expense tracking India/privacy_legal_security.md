# Privacy, Security and Legal Considerations: Personal Expense Tracker (India, 2026)

Scope: an individual in India building a single-user, non-commercial tracker that reads their own Gmail, bank SMS and bank/credit-card statements and sends transaction text to an LLM API. This is not legal advice. Research note: direct fetches of primary sources (meity.gov.in, rbi.org.in, support.google.com, developers.google.com, privacy.claude.com) were blocked by this session's network allowlist, so most findings come from search-result snippets of law-firm, vendor and official pages. Treat pinpoint dates and numbers as needing confirmation against the official texts.

## Q1. Does the DPDP Act 2023 / DPDP Rules 2025 apply to purely personal processing of one's own data? What changes if offered to others?

### Takeaway
Section 3(c)(i) says the Act does not apply to personal data processed by an individual for a personal or domestic purpose, so a single-user tracker of your own data is very likely outside the Act. Once the tool processes other people's data as a service (even free), the operator becomes a Data Fiduciary with consent, notice, security (Rule 6), breach notification and retention duties. Most of these duties apply from about 13 May 2027.

### Cited Findings
- Section 3(c)(i) excludes from the Act personal data processed by an individual for any personal or domestic purpose. Commentators say two elements must both be met: the processor is an individual, and the purpose is personal or domestic. The exclusion removes the data from the Act entirely and is not a partial exemption — [DPDP Wiki, Section 3 guide](https://dpdp.myndsolution.com/wiki/guides/who-does-the-act-apply-to/); [Sansa Legal](https://www.sansalegal.com/post/how-to-determine-the-application-of-the-dpdp-act-under-section-3); [TaxClue](https://taxclue.in/blog/section-3-dpdp-act-2023-personal-domestic-use-and-publicly-available-data)
- A typical example given: an individual keeping family phone numbers on her phone is personal or domestic processing — [DPDP Wiki](https://dpdp.myndsolution.com/wiki/act/section-3/)
- No judicial or Data Protection Board decision interpreting "personal or domestic purpose" was found. The analysis rests on commentary — [Consent.in, DPDP exemptions](https://www.consent.in/blog/dpdp-exemptions); [Mondaq FAQs](https://www.mondaq.com/india/data-protection/1727550/faqs-on-the-digital-personal-data-protection-act-and-rules)
- The DPDP Rules 2025 were notified in November 2025 (reported as G.S.R. 846(E), 13 Nov 2025) with phased commencement. Rules 1, 2 and 17–21 took effect immediately. Rule 4 (consent managers) applies after 1 year. Rules 3, 5–16, 22 and 23 apply after 18 months — [Storyboard18](https://www.storyboard18.com/digital/breaking-govt-notifies-final-dpdp-rules-sets-staggered-rollout-with-key-obligations-for-data-fiduciaries-84201.htm); [YourStory](https://yourstory.com/2025/11/digital-personal-data-protection-dpdp-rules-2025-rollout-over-18-months); [The Week/PTI](https://www.theweek.in/wire-updates/business/2025/11/14/del148-biz-dpdp-rules-ld-govt.html)
- Sources give slightly different 18-month end dates: 12 May 2027 ([AZB via Mondaq](https://webiis08.mondaq.com/india/data-protection/1706878/indias-digital-personal-data-protection-act-phased-rollout-and-key-compliance-milestones)), 13 May 2027 ([TCSA](https://www.tcsa.in/resources/dpdp-rules-2025-implementation-roadmap)), 14 May 2027 ([AMS Shardul](https://www.amsshardul.com/insight/enforcement-of-the-dpdp-act-and-notification-of-the-dpdp-rules/)).
- Rule 6 (under Section 8(5)) sets minimum "reasonable security safeguards": encryption, obfuscation, masking or virtual tokens, access controls, log monitoring and backups. It also requires keeping logs and data for one year for specified purposes — [DPDP Wiki Rule 6](https://dpdp.myndsolution.com/wiki/rules/rule-6-reasonable-security-safeguards/); [King Stubb & Kasiva](https://ksandk.com/data-protection-and-data-privacy/dpdp-rule-6-and-indias-new-cybersecurity-compliance-standard/); [dpdpa.com Rule 6](https://dpdpa.com/dpdparules/rule6.html)
- Rule 7 requires breach notice to affected individuals and to the Data Protection Board "as soon as" the fiduciary becomes aware — [Lawrbit](https://lawrbit.com/article/digital-personal-data-protection-rules-2025); [Mondaq legal analysis](https://www.mondaq.com/india/data-protection/1708268/legal-analysis-of-digital-personal-data-protection-rules-2025). A 72-hour detailed-report deadline is widely reported but was not confirmed in the sources retrieved here.

### Inferences
- A single-user, self-hosted tool processing only your own data fits 3(c)(i) cleanly. One grey area: Gmail, SMS and statements contain third parties' personal data (payees' names, UPI IDs, phone numbers, merchant contacts). Processing these for your own household budgeting is still a "personal or domestic purpose", so the exclusion likely still applies.
- Sharing the code (open source) where each user runs their own copy on their own data probably keeps each user inside 3(c)(i), and the developer never processes anyone's data. Running a hosted service or a backend that touches others' data makes the operator a Data Fiduciary. Duties then include notice and consent, purpose limitation, Rule 6 safeguards, breach notification, erasure and grievance redress, plus possible Significant Data Fiduciary status at scale.
- Sending your own data to an LLM provider for personal use does not trigger DPDP duties for you. For a product, the LLM provider would be a Data Processor under contract, and cross-border transfer rules (Section 16, negative-list approach) would apply.

### Gaps
- Could not fetch the official Act or Rules text (meity.gov.in blocked). Confirm the exact wording of Section 3(c)(i) and the Rule 7 timelines in the Gazette.
- No guidance was found on whether a "family" deployment (for example, a spouse's accounts) stays within the "domestic" purpose.

## Q2. RBI rules if it became a product (AA/FIU, payment aggregators, data localisation, BBPS); how CRED operates

### Takeaway
Pulling bank data through the Account Aggregator rail requires being an FIU, and FIUs must be regulated by RBI, SEBI, IRDAI or PFRDA. A plain PFM app cannot join directly and would need a regulated partner. Payment-aggregator authorisation and payment-data localisation matter only if the app moves money or acts as a payment-system participant. Reading statements from email or SMS (CRED's original method) is not RBI-regulated in itself, but falls under DPDP and Google/Play policy.

### Cited Findings
- The AA framework is governed by RBI's Master Direction for NBFC-Account Aggregators (2016, updated as of 6 Sept 2024). AAs move data from FIPs (banks, NBFCs, depositories) to FIUs with consent — [RBI Master Directions](https://www.rbi.org.in/Scripts/BS_ViewMasDirections.aspx?id=12558); [DFS, Account Aggregator framework](https://financialservices.gov.in/beta/en/account-aggregator-framework)
- FIUs must be registered with and regulated by at least one financial-sector regulator (RBI, SEBI, IRDAI or PFRDA) — [Sahamati, FIU](https://sahamati.org.in/fiu/)
- RBI issued a notification on "Joining the Account Aggregator ecosystem as Financial Information User" (Oct 2023), later folded into the Master Direction. One source says it was withdrawn or consolidated; status unverified — [RBI notification](https://website.rbi.org.in/web/rbi/-/notifications/joining-the-account-aggregator-ecosystem-as-financial-information-user-12558)
- Sahamati states that entities generally participate as FIPs before or alongside being FIUs (reciprocity) — [Sahamati](https://sahamati.org.in/fiu/)
- The RBI 2018 "Storage of Payment System Data" circular requires payment system providers to store all payment-system data only in India. FAQs were issued in June 2019 — [Business Standard](https://www.business-standard.com/article/news-ani/rbi-issues-clarifications-on-data-localisation-circular-119062700235_1.html); [Tribune](https://www.tribuneindia.com/news/business/entire-payment-data-must-be-stored-only-in-india-rbi/793623.html); [Treelife](https://treelife.in/compliance/data-localisation-in-india/)
- RBI issued consolidated Payment Aggregator Master Directions on 15 Sept 2025. They require PAs to comply with the 2018 storage circular and the 2024 Cyber Resilience and Digital Payment Security Directions — [Mondaq](https://webiis08.mondaq.com/india/financial-services/1727528/rbi-master-direction-on-digital-payment-aggregators-understanding-compliance-requirements-and-industry-implications); [Lexplosion](https://lexplosion.in/rbi-issues-new-master-directions-to-consolidate-all-regulations-related-to-payment-aggregators-effective-immediately/)
- CRED: per policy text quoted in forums, if a user links email, CRED may access it to collect financial information. Reading is restricted to financial emails (card, loan and bank statements, insurance, biller details) — [TechnoFino forum quoting CRED policy](https://technofino.in/community/posts/852923/) (secondary, unverified against the live policy)
- CRED users report that since about July 2024 card dues are fetched from BBPS, through a separate "BBPS bill fetch" toggle independent of email access — [Reddit r/CreditCardsIndia, Apr 2025](https://pol1.lr.ggtyler.dev/r/CreditCardsIndia/comments/1k2qyva/how_does_cred_get_access_to_statement_amount) (anecdotal)
- CRED planned to apply for an AA licence in 2022. No confirmation of a grant was found — [MediaNama 2022](https://www.medianama.com/2022/03/223-cred-account-aggregator-license-application/)
- CRED says it does not sell or share personal info with unaffiliated third parties for their advertising without explicit consent — [CRED security page](https://cred.club/security); see also [CRED other disclosures](https://cred.club/legal/other-disclosures)

### Inferences
- For a personal tool, none of the RBI regimes apply: you are not an FIU, a PA, a payment-system operator or a BBPS participant.
- For a product, the realistic paths are: (a) email/SMS parsing under DPDP and Google policy (CRED's original path); (b) AA data through a regulated FIU partner or by becoming regulated (for example, an NBFC or SEBI RIA); (c) BBPS bill-fetch through a BBPS Operating Unit (a bank or authorised non-bank BBPOU). Payment-data localisation becomes relevant once you handle payment flows.

### Gaps
- BBPS (NPCI Bharat BillPay) participation rules for non-bank BBPOUs and Credit Card Bill fetch rules were not retrieved.
- The AA Master Direction text was not fetched (RBI site blocked). CRED's live privacy policy and its AA/FIU status are unverified.

## Q3. Bank terms on screen-scraping / automated net-banking access; legality and risk

### Takeaway
No Indian rule or court decision was found that specifically addresses scraping your own net-banking account. The risk is mainly contractual (bank terms and credential-sharing clauses, possible loss of fraud protection or account suspension) and, in theory, IT Act s.43 if access is "without permission". Parsing statements and alerts the bank already sends you avoids this risk.

### Cited Findings
- IT Act s.43 imposes civil liability for accessing, downloading or copying data from a computer resource without the owner's permission. s.66 makes it criminal only if done dishonestly or fraudulently — [law.asia, legality of scraping in India](https://law.asia/india-data-scraping-regulation/); [iPleaders](https://blog.ipleaders.in/data-scraping-legality/)
- Courts have not definitively applied s.43/66 to scraping. Whether ToS or technical restrictions define "permission" is unresolved. Evading blocks (for example, rotating IPs) could be read as knowledge that permission was absent — [law.asia](https://law.asia/india-data-scraping-regulation/); [S.S. Rana](https://ssrana.in/articles/data-scraping-issues-india/)
- No results addressed scraping one's own account specifically — (search gap noted above).
- Screen scraping typically involves sharing login credentials with a third party or automating the customer's session. Regulators abroad (the EBA under PSD2) rejected allowing it — [KuppingerCole](https://www.kuppingercole.com/blog/kuppinger/psd2-the-ebas-wise-decision-to-reject-commission-amendments-on-screen-scraping); [SARB consultation](https://www.resbank.co.za/content/dam/sarb/what-we-do/payments-and-settlements/regulation-oversight/Consultation%20Paper%20on%20open%20banking.pdf)
- Indian fintechs advertise direct bank APIs rather than scraping — [Open.money partners](https://open.money/company/partners) (vendor claim)

### Inferences
- Bank net-banking terms in India commonly forbid sharing credentials and hold the customer liable for transactions using them. Automating login (especially with stored OTPs or credentials) could weaken an unauthorised-transaction claim under RBI's customer-liability framework. This inference was not verified with a specific bank's terms.
- Using emailed statements, SMS alerts and downloadable CSV/PDF statements is the safest route. A product should never collect bank credentials.

### Gaps
- No specific Indian bank's net-banking T&C text was retrieved. Check your own bank's terms, for example the HDFC, ICICI or SBI internet banking terms.

## Q4. Google: Gmail API restricted scopes, OAuth verification / CASA, personal "testing" apps

### Takeaway
Gmail read scopes are restricted. Apps used by the public need OAuth verification plus an annual CASA security assessment (Tier 2 is about $540–$1k+/yr via self-serve labs; Unipile estimates 2–8 weeks). A personal app can stay unverified: it is capped at 100 users, users see the unverified-app warning, and in "Testing" status refresh tokens expire after 7 days.

### Cited Findings
- Unverified apps using sensitive or restricted scopes have a 100 new-user cap. Exhausting it without verification can disable sign-in — [Google Cloud Help](https://support.google.com/cloud/answer/13463817?hl=en) (via search snippet)
- Most Gmail scopes, including read access, are restricted. Production needs app verification plus a CASA assessment, renewed annually. The 100-user counter cannot be reset — [Nango, Google security review](https://nango.dev/docs/integrations/google-security-review)
- CASA Tier 2 costs about $540–$1k/yr through self-serve labs, versus $15k–$75k on a legacy track. Verification takes 2–8 weeks — [Unipile](https://www.unipile.com/integrating-google-oauth-2-0-user-authentication-into-your-app/) (vendor; note it labels gmail.readonly "sensitive", which conflicts with Nango's "restricted" label. Google lists gmail.readonly as restricted in its own docs, unverified here.)
- Testing-mode refresh tokens expire after 7 days. Moving to "In production" (with new credentials) stops this — [Google Ads API group](https://groups.google.com/g/adwords-api/c/QTdeilIh_QY/m/eOHg0Q9CAgAJ); [Zammad community](https://community.zammad.org/t/gmail-token-expires-every-week/6219)
- Publishing an unverified Gmail app does not lift the 100-user cap. A single-user app reportedly still needed publishing to avoid 7-day expiry — [tidemail wiki](https://github.com/allisonhere/tidemail/wiki/Google-OAuth-release); [Stack Overflow archive](https://kiwix.lema.org/stackoverflow.com_en_all_2023-11/questions/70653369/google-cloud-api-services-oauth-with-a-testing-app)

### Inferences
- Personal-use options: (1) keep the app in Testing and re-auth weekly; (2) set it "In production" unverified, click through the warning yourself, and stay under 100 users; (3) avoid OAuth entirely with Gmail IMAP plus an app password (needs 2FA), or Gmail filters that forward statement emails. Option 3 may be subject to Google's own changes to less-secure access and is unverified here.
- Use the narrowest scope (`gmail.readonly`, plus a server-side `q=` search filter such as `from:alerts@bank`). Never request `mail.google.com/` full scope.

### Gaps
- Google's official restricted-scope verification page (developers.google.com) could not be fetched. Exact exemptions ("personal use", "internal" Workspace apps) are unconfirmed.

## Q5. Google Play SMS / Call Log permission policy

### Takeaway
READ_SMS is a restricted permission on Play. "SMS-based money management (e.g., budgeting)" is listed as a permitted exception, but only when it is the app's core functionality and after a Permissions Declaration Form review. Side-loaded personal APKs are not subject to Play policy.

### Cited Findings
- Play restricts SMS and Call Log permission groups. Apps that don't qualify must remove them from the manifest. Access is allowed only for permitted uses that enable core functionality, subject to review — [Play Console Help 9047303](https://support.google.com/googleplay/android-developer/answer/9047303)
- The permitted-use exceptions include "SMS-based financial transactions (e.g., UPI, verifications for financial transactions)" (RECEIVE_SMS etc.) and "SMS-based money management (e.g., budgeting apps)" (READ_SMS, READ_CALL_LOG) — [Play Console Help 10208820](https://support.google.com/googleplay/android-developer/answer/10208820?hl=en) (via search snippet)
- A policy preview for July 2026 removes phone-call account verification as a READ_CALL_LOG use and disallows broad SMS/Call Log access for authentication — [Play Console Help 17225965](https://support.google.com/googleplay/android-developer/answer/17225965)

### Inferences
- For personal use, the simplest route is a side-loaded app, or exporting SMS (for example with an SMS backup tool) or forwarding alerts. Don't request Call Log at all. For a Play-distributed product, expect a declaration form, a demo video, a prominent disclosure and a privacy policy.

### Gaps
- Could not fetch the live Play pages to quote exact wording or confirm current status (support.google.com blocked).

## Q6. LLM API data handling, PII masking, local LLMs

### Takeaway
Both Anthropic and OpenAI say API (commercial) data is not used for training by default, and both retain it for a limited period (OpenAI: abuse logs up to 30 days; Anthropic: reported as 30 days, or 7 days after Sept 2025, sources conflict). Zero Data Retention needs approval. Masking card and account numbers before sending, or using a local model, reduces exposure further.

### Cited Findings
- OpenAI: abuse-monitoring logs are kept up to 30 days by default. ZDR/Modified Abuse Monitoring needs prior approval. Some endpoints (files, threads, batches, fine-tuning) are ZDR-ineligible. Responses API stored objects are kept at least 30 days when store=true — [OpenAI, Your data](https://developers.openai.com/api/docs/guides/your-data)
- OpenAI API data is not used for training by default (since 1 Mar 2023) — [VantagePoint](https://vantagepoint.io/docs/openai-api-data-controls) (secondary)
- Anthropic: commercial/API data is excluded from training. Retention is reported as 7 days for API inputs and outputs (cut from 30 days on 14 Sept 2025) by [Anarlog](https://anarlog.so/blog/anthropic-data-retention-policy) and [char.com](https://char.com/blog/anthropic-data-retention-policy), but as "within 30 days" by [DevelopersDigest (updated Jul 2026)](https://www.developersdigest.tech/models/data-retention/anthropic). These conflict; check Anthropic's Privacy Center. ZDR is available by approval for eligible customers — [getvoibe](https://www.getvoibe.com/resources/claude-api-data-retention/)
- ZDR may not carry over when accessed through Bedrock or Vertex, which run under those providers' terms — [Meetily](https://meetily.ai/llm-privacy/anthropic) (secondary)
- Rule 6 lists masking and virtual tokens as recognised safeguards — [DPDP Wiki Rule 6](https://dpdp.myndsolution.com/wiki/rules/rule-6-reasonable-security-safeguards/)

### Inferences
- Before sending text to an LLM: strip or tokenize PANs (keep the last 4), account numbers, IFSC+account combinations, UPI VPAs, phone numbers, emails, addresses, PAN/Aadhaar numbers, OTPs and balances (if not needed). Use regex plus a Luhn check for card numbers, and send only the transaction line rather than whole emails.
- Use deterministic parsers (regex or templates per bank) first, and the LLM only for leftovers or categorisation. This cuts both cost and exposure.
- Local alternatives (for example, Ollama or llama.cpp with small instruction models) keep all data on device. Categorisation and extraction are tasks small models handle reasonably well. No benchmark sources were retrieved for this.

### Gaps
- Anthropic's primary retention page (privacy.claude.com) was blocked, so the 7-vs-30-day conflict is unresolved.
- No sources were collected on local-model accuracy for Indian bank SMS parsing.

## Q7. Security best practices: secrets, tokens, local-first storage, backups

### Takeaway
Keep it local-first: an SQLCipher-encrypted SQLite database whose key lives in the OS keychain (not on disk or in code), OAuth refresh tokens and PDF passwords in the keychain, minimal scopes, and encrypted off-device backups.

### Cited Findings
- SQLCipher does not persist key material. The key should be held in a platform keystore: Windows DPAPI, Windows Hello or TPM; on Linux, Secret Service (gnome-keyring, KWallet), the kernel keyring or TPM; on servers, an HSM or KMS — [Zetetic, SQLCipher key material](https://www.zetetic.net/sqlcipher/database-key-material/)
- Python: use sqlcipher3, supplying the passphrase via an environment variable or python-keyring — [Charles Leifer](https://charlesleifer.com/blog/encrypted-sqlite-databases-with-python-and-sqlcipher/)
- Never hardcode the passphrase. Use a strong KDF if the key is derived from a user password — [OneUptime](https://oneuptime.com/blog/post/2026-02-02-sqlcipher-encryption/view)
- Set `PRAGMA cipher_compatibility` explicitly and zeroize key buffers — [Playbooks SQLCipher skill](https://playbooks.com/skills/martinholovsky/claude-skills-generator/sqlcipher) (low-authority)
- Rule 6 baseline (for products): encryption, masking, access control, logging, backups — [King Stubb & Kasiva](https://ksandk.com/data-protection-and-data-privacy/dpdp-rule-6-and-indias-new-cybersecurity-compliance-standard/)

### Inferences
- Indian card and bank statement PDF passwords are usually derived from name, DOB, PAN or card digits. Store them (or the components) in the keychain, decrypt PDFs in memory, and never write decrypted PDFs to disk or git.
- Store OAuth client secrets and refresh tokens in the keychain. Add `.gitignore` entries for `credentials.json`, `token.json`, `*.db` and `statements/`. Revoke tokens when no longer needed.
- Store LLM API keys in the keychain or an environment variable, never in the repo. Run a secret scanner before pushing.
- Back up the encrypted database file (it is already ciphertext), plus a separately stored key escrow (for example, a password manager). Test restores.
- Avoid logging raw email or SMS bodies. If logs are needed, log redacted text.

### Gaps
- No primary source was retrieved on OAuth refresh-token storage (for example, RFC 8252 or OAuth Security BCP). The keychain recommendation rests on general practice.
