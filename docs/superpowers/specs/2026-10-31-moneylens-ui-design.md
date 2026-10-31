# MoneyLens UI (v1, sample data) — Design

Date: 2026-10-31
Status: Draft for review

## 1. Purpose

MoneyLens is a personal, single-user monthly expense monitor for one person in India with
several bank accounts and credit cards. Its core value is a **monthly report with advice**:
where the money went, what went well, where to improve, and a plan for next month.

This spec covers **only the UI**, built against realistic **sample data**. The Gmail statement
pipeline (IMAP fetch → PDF unlock → Claude extraction → categorization) comes later and will
replace the sample data source without changing the screens.

### Success criteria

- All six screens below work end to end in the browser on sample data.
- Edits (recategorizing a transaction, adding a rule, changing a budget) take effect immediately
  across every screen and survive a page reload.
- Every number on the Monthly Report is computed by code from transactions; insight text only
  ever quotes those computed numbers.
- Usable on a laptop and on a phone-width screen; light and dark themes.

### Non-goals (v1)

No backend, no Gmail, no Claude calls, no login, no multi-user, no SMS, no Account Aggregator,
no mobile app build.

## 2. Inspiration

| Source | Borrowed idea |
|---|---|
| Sure (Maybe Finance fork) | Clean overview: accounts list, spending trend, net position |
| Actual Budget | Fast transactions table with inline recategorization; "₹X of ₹Y" budget rows |
| Firefly III | Rules engine ("description contains X → category Y"), category/month reports |
| PennyWise AI | Indian context: UPI merchant strings, ₹ lakh formatting, card due dates |

## 3. Tech stack

- **Vite + React 18 + TypeScript**, client-side only.
- **Tailwind CSS** for styling; CSS variables for light/dark theme tokens.
- **React Router** for screens.
- **Recharts** for charts.
- **Vitest** for unit tests of domain logic; **Playwright** smoke test that loads each screen.
- Persistence of user edits: `localStorage` (wrapped in try/catch; app works without it).

## 4. Architecture

```
src/
  domain/        pure TypeScript: types, money formatting, categorization, report math, insights
  data/          repository interface + sample-data implementation (seeded generator)
  state/         React context exposing repository data + mutations to screens
  components/    shared UI (cards, money, category pill, progress bar, charts, layout)
  screens/       Overview, Report, Transactions, Budgets, Accounts, Rules
```

Boundaries:

- **domain/** has no React and no I/O. Everything numeric lives here and is unit-tested.
- **data/** exposes a `Repository` interface (list accounts, transactions, categories, budgets,
  rules; update transaction category; add/edit/delete rule; set budget; reset sample data).
  v1 implements it with a deterministic seeded sample generator plus localStorage overrides.
  The future Gmail backend implements the same interface.
- **screens/** read through `state/` and never compute report figures themselves.

## 5. Data model

- **Account**: id, name, institution, type (`savings` | `credit_card`), masked number (`••4521`),
  for cards: credit limit, statement day, due day; last statement imported date.
- **Category**: id, name, group (`Essentials` | `Lifestyle` | `Financial` | `Income` | `Transfer`),
  color, icon.
- **Transaction**: id, accountId, date, rawDescription (bank text, e.g. `UPI/SWIGGY/412345/Paym`),
  merchant (cleaned name), amount (paise, integer), direction (`debit` | `credit`), categoryId,
  categorySource (`rule` | `auto` | `manual`), isTransfer flag (card bill payments, own-account moves).
- **Rule**: id, match text (case-insensitive "contains" on rawDescription), categoryId, createdFrom
  (`manual` | `correction`).
- **Budget**: month-independent monthly limit per categoryId (paise).

Money is stored as integer paise and formatted with Indian grouping (₹1,23,456).

## 6. Sample data

Seeded generator (same output every run) producing **July–October 2026**:

- 4 accounts modeled on common Indian banks: 2 savings accounts and 2 credit cards, with fictional
  masked numbers.
- ~120–180 transactions per month: salary credit, rent, UPI food delivery, groceries, fuel, cabs,
  shopping, subscriptions (streaming, music, cloud storage), utilities, mobile recharge, SIP,
  EMI, ATM cash, card bill payments (marked as transfers), occasional refunds.
- Deliberate patterns so insights have something to say: food delivery rising month over month,
  one unused-looking subscription, weekend spend higher than weekdays, a month where shopping
  dropped.
- 15–20 starter rules and budgets for the main categories.

## 7. Screens

1. **Overview** — month picker; tiles for total spent, income, net saved, % of budget used;
   spending-by-category donut; daily spend bar chart vs. last month; top 5 categories with budget
   progress; upcoming card bills (amount + due date); accounts summary.
2. **Monthly Report** (the headline feature) — for the selected month:
   - Summary: spent / income / saved, savings rate, change vs. previous month.
   - Where the money went: category table with amount, share, Δ vs. last month, budget status.
   - What went well: 2–4 insights (e.g. category spend down ≥15%, stayed under budget, savings
     rate up).
   - Where to improve: 2–4 insights (e.g. category over budget, rising 3-month trend, recurring
     subscriptions with amounts, weekend vs. weekday ratio, many small UPI payments).
   - Next month's plan: suggested budget per category (based on recent average, nudged toward
     targets) with an "apply to budgets" button.
   - Insight text is produced by deterministic templates in `domain/insights` from computed facts.
     (Later, Claude rewrites the narrative from the same facts sheet.)
   - Print-friendly layout.
3. **Transactions** — search, filters (month, account, category, debit/credit, uncategorized),
   sortable table on desktop and card list on mobile; click a category pill to change it; after a
   change, offer "Always categorize '<merchant>' as <category>?" which creates a rule and applies
   it to matching transactions.
4. **Budgets** — per-category rows: limit (editable inline), spent this month, progress bar
   (green / amber ≥80% / red >100%), 3-month average; total budget vs. total spent.
5. **Accounts** — cards per account: type, masked number, this month's debits/credits, card limit
   utilization and due date, last statement imported; a disabled "Connect Gmail" section
   describing the upcoming pipeline.
6. **Rules & Categories** — list, add, edit, delete rules; preview how many transactions a rule
   matches; categories list with colors and transaction counts; "Reset sample data" button.

Layout: left sidebar navigation on desktop, bottom tab bar on mobile; header with month picker
and theme toggle.

## 8. Error and edge handling

- Empty states for months/filters with no transactions.
- Previous-month comparisons show "—" when there is no prior month.
- Transfers (card bill payments, own-account moves) excluded from spend and income totals.
- Refunds (credits in a spending category) reduce that category's spend.
- localStorage unavailable or corrupt → fall back to pristine sample data.

## 9. Testing

- **Unit (Vitest)**: money formatting (Indian grouping, paise), rule matching and precedence
  (manual > rule > auto), monthly totals excluding transfers and netting refunds, month-over-month
  deltas, budget status thresholds, each insight generator (fires / doesn't fire), suggested
  budgets, sample generator determinism.
- **Smoke (Playwright)**: each route renders without console errors; recategorizing a transaction
  updates the Overview total for that category.
- `npm run build` and `npm run lint` (TypeScript strict) pass.
