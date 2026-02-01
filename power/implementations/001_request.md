# Power App - Suggestions

Consolidated feature requests, enhancements, and bug reports for the Power App.

---

## Summary Table

| Section | Type | Title | Priority |
|---------|------|-------|----------|
| Allocation+ | Enhancement | Subcategory Charts & Breakdown | Medium |
| Cedolare | Enhancement | Top 10 + Others Chart Aggregation | Low |
| Transaction | Enhancement | Mandatory Asset Category Field | High |
| Transaction | Enhancement | Dividend Classification Dropdown | High |
| Analytics/FIRE | Bug Fix | Progress KPI Calculation | Medium |
| Cedolare | Bug Fix | Duplicate Prevention on Copy-Paste | High |
| PGRF | New Module | Financial Resource Planning | High |
| Immobiliare | New Module | Real Estate Management | Medium |
| Protezione | New Module | Insurance & Protection | Medium |

---

## Feature Enhancements

### Allocation+

#### Subcategory Charts & Breakdown

**Source:** `power/App/Allocation/modifiche/allocation-plus-grafici.md`

**Context:** Each asset has a **Category** assigned at creation (enum: Fondi obbligazionari, Fondi bilanciati, Fondi azionari, Azioni, REIT). Allocation Plus currently shows KPIs, main "Allocation" chart, and "Breakdown" table.

**Objective:** Add a section with additional cards below the main chart, each with pie/donut and corresponding breakdown table.

**New Charts Required:**

1. **Asset Allocation - Macro Categories (5)**
   - Series: sum values for each of the 5 categories
   - Breakdown: Category | Value (EUR) | % of total portfolio | N. assets

2. **Super Macro - Fondi / Azioni / REIT (3)**
   - Series: Fondi = (obbligazionari + bilanciati + azionari), Azioni, REIT
   - Breakdown: Category | Value | % of total portfolio | N. assets

3. **Fondi - Detail by Individual Fund**
   - Filter: category in {Fondi obbligazionari, Fondi bilanciati, Fondi azionari}
   - Series: each slice = `asset_name`, value = `current_value`
   - Breakdown: Fund | Fund Category | Value | % of total "Fondi" | (optional) % of total portfolio

4. **Azioni - Detail**
   - Filter: category = Azioni
   - Breakdown: Stock | Value | % of total "Azioni" | (optional) % of total portfolio

5. **REIT - Detail**
   - Filter: category = REIT
   - Breakdown: REIT | Value | % of total "REIT" | (optional) % of total portfolio

**Calculation Rules:**
- `total_portfolio_value = Sum of current_value (assets in page filter)`
- `% of total portfolio = value / total_portfolio_value`
- `% of subset (Fondi/Azioni/REIT) = value / total_subset_value`
- Handle "Unclassified" assets if they exist

**Suggested UI Layout:**
- Row 1: Macro 5 + Super Macro 3
- Row 2: Fondi + Azioni
- Row 3: REIT (full width or single card)

---

### Cedolare

#### Top 10 + Others Chart Aggregation

**Source:** `power/App/Cedolare/modifiche/dividend-calendar-top10-others.md`

**Context:** In the Chart/Received view of Dividend Calendar, the stacked bar chart shows all underlying assets. Need to limit to Top 10 per month and group the rest in "Others".

**Objective:** For each month, show only the 10 assets with the highest dividend/coupon amount; aggregate all others in "Others". Top10 + Others = total month.

**Calculation Rules (per month):**
1. Select transactions with `Type = Dividend` filtered by view (e.g., Received = collected)
2. Filter by the bar's month
3. Group by Asset and calculate `amount_sum` per asset in the month
4. Sort by `amount_sum` desc, take top 10
5. Calculate `others_sum = total_month - sum_top10`
6. Add "Others" series only if `others_sum > 0`

**Chart Output:**
- Each monthly bar: max 11 segments (10 assets + Others)
- Legend: names of 10 assets + "Others"
- Total bar unchanged

**Dynamic Behavior:**
- Top 10 calculated per month (may change month to month)
- If assets in month are <= 10: show all, no Others
- Others is monthly and depends on excluded assets that month

---

### Transaction

#### Mandatory Asset Category Field

**Source:** `power/App/Transaction/modifiche/categoria-asset.md`

**Context:** The **New Transaction** screen remains unchanged. The change applies only to new asset/underlying creation flow.

**Objective:** Associate a unique category to each asset for correct aggregation and visualization in **Allocation Plus**. Category is an asset property, assigned once at creation, inherited by all future transactions on that asset.

**Point of Intervention:**
- Flow **Asset -> New** (new underlying creation)
- No modification to **New Transaction**

**New Field:**
- **Name:** Category
- **Type:** mandatory selection (radio or select)
- **Allowed Values (fixed):**
  - Fondi obbligazionari
  - Fondi bilanciati
  - Fondi azionari
  - Azioni
  - REIT

**Validation:**
- Mandatory field
- Cannot save asset without category
- Suggested message: "Select a category for the underlying"

**Application Behavior:**
- Category is **not editable from individual transaction**
- Category is editable **only** from asset edit screen
- All transactions linked to asset automatically inherit category

---

#### Dividend Classification Dropdown

**Source:** `power/App/Transaction/modifiche/classificazione-cedole.md`

**Context:** In **New Transaction**, the **Type** field includes "Dividend" but doesn't require specifying the nature of the flow. Need to distinguish the type immediately for correct use of the **Cedolare** page.

**Objective:** Classify each **Dividend** type transaction at insertion time to have pre-labeled flows, without manual reclassifications, enabling aggregation/analysis in Cedolare.

**Activation Condition:**
- Applies only if **Type = Dividend**
- For other types (Buy, Sell, Deposit, Withdrawal, Fee, Interest, Transfer) nothing changes

**New Field:**
- **Name:** Coupon/Dividend Classification
- **Type:** mandatory selection (dropdown or radio)
- **Fixed Options (5):**
  - Cedola - Fondo obbligazionario
  - Cedola - Fondo bilanciato
  - Cedola - Fondo azionario
  - Dividendo - Azione
  - Dividendo - REIT

**Validation:**
- If **Type = Dividend**, field is mandatory
- If missing selection: block save and show error "Select coupon/dividend classification"
- If **Type != Dividend**, field is not visible nor required

**Data Logic:**
- Save classification as transaction attribute (enum/fixed values)
- Possible technical names: `dividend_classification` or `cashflow_classification`

---

### Analytics/FIRE

#### Fix Progress KPI Calculation

**Source:** `power/App/Analytics/modifiche/fire-cashflow-progress.md`

**Context:** In FIRE Cash Flow component, KPIs are correct except **Progress**, which uses an inconsistent value (e.g., last month). Progress must be based on **Run Rate** average of the selected period.

**Objective:** Calculate Progress as advancement toward Target Income using the average of receipts (Run Rate) over the selected period, not the last month.

**Definitions:**
- `target_income`: set target (EUR/month)
- `run_rate`: monthly average of "Received" receipts (Type = Dividend) in selected period (e.g., last 12 months)

**Correct Formula:**
- `progress = run_rate / target_income`
- Display: `progress_pct = (run_rate / target_income) * 100`

**Constraints:**
- If `target_income = 0` -> progress = 0 (or n/a; avoid division by zero)
- If `progress_pct > 100%`: option A show >100 (e.g., 120%); option B clamp 100%. Suggested: show >100 because informative

**KPI Consistency:**
- Progress must use same variables as Gap and Time to Target (target_income, run_rate)

**Quick Test (example):**
- Target Income = 500 EUR/month
- Run Rate (12m avg) = 250 EUR/month
- Progress = 250 / 500 = 0.5 -> 50%
- Expected Gap: 250 EUR
- Changing Run Rate Period (e.g., 6 months), Progress updates consistently

---

## Bug Fixes

### Cedolare

#### Duplicate Prevention on Copy-Paste

**Source:** `power/App/Cedolare/problemi/duplicati-copia-incolla.md`

**Summary:** When importing/pasting data in Cedolare view, some rows don't align and duplicate entries appear. Need a mechanism to prevent identical names and ensure row-by-row alignment.

**Observed Problem:**
- Copy and paste from previous version generates misaligned rows
- Same entries appear multiple times (duplicates)
- No uniqueness check on coupon/dividend entry name

**Impact:**
- Data shown in wrong or repeated rows
- Totals and charts may be incorrect
- Confusing user experience (difficult to understand if data was correctly recorded)

**Probable Cause:**
- Pasted input parsing that doesn't respect column separation (e.g., unrecognized delimiters or inconsistent newlines)
- Lack of uniqueness constraint on coupon entry names

**Requested Fix:**
1. **Data alignment**: normalize pasted input (trim, consistent delimiters, column check) to ensure each row ends up in correct row
2. **Entry uniqueness**: introduce constraint (DB + UI) preventing insertion of two entries with same name
   - If name exists: block save and show error "An entry with this name already exists"
3. **Preventive validation**: show preview highlighting duplicates/misalignments before saving

**Notes:**
- Bug was noticed on data pasted from previous version; verify compatibility with legacy formats
- Solution must be backward compatible but reject new duplicates

---

## New Modules

### PGRF (Piano Gestione Risorse Finanziarie)

**Source:** `power/App/PAC/docs/piano-gestione-risorse-finanziarie.md`

**Purpose:** A single view comparing inflows and outflows, relating financial portfolio, real estate, and leverage (mortgages/loans). Evolution of PAC manager, focused on resource/obligation balance.

**Objectives:**
- Track periodic contributions, net coupons/dividends, rents, other cash inflows
- Link structural outflows: mortgage payments, loans, insurance, accumulation plans, recurring expenses
- Measure sustainability and leverage: ratio between assets (portfolio + real estate) and residual debt, rate coverage with recurring cashflow
- Simulate adjustments: increase contributions, pay off mortgages, change target leverage

**Inflows (E):**
- Recurring contributions (DCA/PAC on portfolio)
- Net coupons/dividends (linked to Ledger, filterable)
- Net rents from real estate (linked to Immobiliare page)
- Other inflows (bonuses, variable salaries, etc.)

**Outflows (U):**
- Mortgage payments (principal + interest + insurance)
- Personal loan payments
- Recurring real estate expenses (property taxes, basic maintenance)
- PAC/DCA contributions if considered outflow (toggle "treat contribution as outflow")
- Optional fixed/variable expenses (placeholder for future integrations)

**Key KPIs:**
- `Net Cashflow = Sum(E) - Sum(U)`
- `Coverage Rate = Sum(E) / Sum(U)` (threshold >1 = sustainable)
- `Debt to Assets = Residual Debt / (Portfolio + Real Estate)`
- `Cash-on-Cash after debt service = (Sum(E) - mortgage/loan payments) / invested equity`
- `Runway rate-friendly`: months covered by recurring income before depleting cash

**Proposed UI:**
- Header with KPI summary (Net Cashflow, Coverage, Debt/Assets, overall LTV)
- Two columns:
  - Inflows: table/stacked bar by source (Net Coupons, Rents, Contributions, Other)
  - Outflows: table/stacked bar by destination (Mortgages, Loans, Real Estate Expenses, PAC as outflow, Other)
- "Leverage and Assets" section: card with total assets (portfolio + real estate), residual debt, combined LTV and debt reduction scenarios
- CTA `+ PGRF` (ex `+ PAC`) to add contributions or link new flows (mortgages, rents, coupons) via existing selectors in Ledger/Immobiliare

**Required Integrations:**
- Ledger: tag flows with `income_type` / `outflow_type` and link to `property_id` or `asset_id` for consistent aggregations
- Immobiliare: import mortgage payments and net rents as outflows/inflows in PGRF view
- Cedolare: net coupons/dividends automatically flow into Inflows
- Portfolio: use current portfolio value for leverage ratios; add toggle "include cash"

**Roadmap:**
1. MVP: sum inflows/outflows from Ledger with manual category mapping; Net Cashflow & Coverage KPIs
2. Step 2: Immobiliare integration (mortgages/rents) and Cedolare (net coupons)
3. Step 3: leverage scenarios (partial payoffs, increase contributions) with what-if
4. Step 4: threshold alerts (coverage <1, debt/assets >60%)

---

### Immobiliare (Real Estate)

**Source:** `power/App/Immobiliare/docs/pagina-immobiliare.md`

**Objective:** Manage the entire lifecycle of a property (purchase -> management -> disposal) with dedicated flows and complete traceability within PoWeR.

**Scope and Boundaries:**
- Track "Property" assets with master data, historical value, and current value
- Cover recurring costs/revenues: mortgages, taxes, maintenance, insurance, rents/leases
- Enable quick entries via `+ Immobiliare` button in Transaction menu (dedicated shortcut)
- Keep real estate flows separate from other assets but integrate them in Net Worth and Cash Flow

**Key Property Data:**
- Identifiers: name, address (optional), type (residential, commercial, land), status (in use, income-producing, for sale)
- Values: purchase price, deed date, estimated current value, appraisal value, current LTV
- Cost/Revenue structure: expected/actual rent, condo fees, maintenance, insurance, IMU/TASI/TARI, registration taxes, notary fees
- Financing: associated mortgage (amount, duration, rate, payment, principal/interest split), down payment/deposit

**New Transaction Types (frontend scope):**
- `Purchase (Real Estate)` - records purchase: price, taxes/fees, down payment, associated mortgage
- `Mortgage Payment` - principal + interest + mortgage insurance
- `Property Tax` - IMU/TASI/TARI (fields: year, installment, amount)
- `Maintenance` - extraordinary/ordinary (selectable category)
- `Insurance` - home policy premium
- `Rent Income` - collected rent, accrual period, withholding/tax advance if applicable
- `Rent Vacancy` (opt.) - periods without income for yield analysis
- `Sale (Real Estate)` - sale value, disposal costs, gain/loss

**UI: Immobiliare Page Layout:**
- **Property Header**: photos/icons, status, current value, LTV, gross/net yield
- **"Financial Summary" Card**: purchase price, down payment/deposit, residual mortgage, equity, current yield (gross/net)
- **"Cash Flow" Section**: list of real estate transactions filterable by type/date; CTA `+ Immobiliare` opens pre-filtered form for above types
- **"Rents" Section**: accrual and collection calendar, vacancy rate, expected vs collected comparisons
- **"Mortgage" Section**: summary amortization schedule, next payment, principal/interest breakdown
- **"Taxes & Expenses" Section**: annual table (IMU/TASI/TARI), maintenance, insurance
- **"Valuation" Section**: current value, delta vs cost, IRR/simple ROI, optional link to external valuations (placeholder)

**Main Calculations and KPIs:**
- `Gross Yield` = annual rent / purchase price
- `Net Yield` = (annual rent - recurring expenses - property taxes) / purchase price
- `Cash-on-Cash` = (annual net cash flow) / (invested equity)
- Dynamic `LTV` = residual mortgage / current value
- `Vacancy Rate` = unrented months / 12 (or selected period)

**Integration with Existing Models:**
- Ledger: new real estate transactions save to Ledger with `immobiliare` tag + `property_id`
- Portfolio: property appears as "Real Estate" asset with current value; mortgage as linked liability
- Allocation+: suggested new category "Real Estate" (or subcategory of Alternatives) to reflect asset weight
- Cedolare: `Rent Income` can feed recurring cashflow views (not dividend), maintaining semantic separation

**Property Creation/Edit Form:**
- Required fields: name, type, purchase date, purchase price, currency
- Optional but recommended: address, square meters, initial capex, notary/taxes
- Mortgage link: select or create mortgage; enables immediate expected payment and LTV calculation

**Initial UX Tasks:**
- Add global CTA `+ Immobiliare` near `+ Transaction`
- In form, preset transaction type based on context (e.g., "Rent Income" if opened from Rents section)
- Quick filters for fiscal period (calendar year) and by property if multi-property

**Open Items / TODO:**
- Automatic revaluations (e.g., ISTAT index) - decided in phase 2
- Integration with attachments (deed, appraisal, lease contracts)
- Handling fiscal differences between markets (IT: flat tax vs IRPEF; US: property tax/local)
- Property data export for bank/insurance

---

### Protezione (Insurance & Protection)

**Source:** `power/App/Protezione/docs/pagina-protezione.md`

**Objective:** Provide a single view of financial/personal risks and active coverages, highlighting gaps and intervention priorities. Include insurance, emergency funds, and income protection.

**Scope:**
- Covered risks: life/disability, accidents, health, home, civil liability, cyber/identity, travel, professional
- Considered coverages: insurance policies, emergency funds, company benefits, card/bank guarantees
- Link with other pages: mortgages (payment protection), properties (home insurance), PGRF for cashflow stress test

**Base Data to Track:**
- Policy: type, company, number, start/end date, premium, coverage limit, deductible, beneficiaries
- Covered risk: category, insured event, known exclusions
- Premium: amount, frequency, payment method (link to Ledger)
- Status: active, renewing, scheduled cancellation
- Emergency funds: current balance, target (months of expenses), coverage percentage

**UI Sections:**
- **"Risk Coverage" Radar**: by category (life, health, home, RC, cyber, travel, professional) shows coverage level vs priority
- **Policies Card**: policy list with status badge, annual premium, coverage limit, next expiration; CTA `+ Policy`
- **Emergency Fund Card**: balance, target months, gap; link to funding transactions
- **Cashflow Stress Test**: simulates impact of event (e.g., accident, job loss) on PGRF: months of endurance with coverages + emergency fund
- **Expiration Alerts**: policies renewing within 60/30/7 days

**New Transaction Types/Labels:**
- `Insurance Premium` (outflow) linked to policy_id
- `Emergency Fund Deposit/Withdrawal` (in/out) with `emergency_fund` tag
- Option "associate policy" for mortgage/property to link premium to that asset

**KPIs:**
- Life/disability coverage: total coverage limit vs 10-15x annual income (parametric)
- Emergency fund: months covered = balance / monthly expense (from PGRF or Ledger)
- Premium/coverage limit ratio per policy
- % critical risks covered (weighted by user priority)

**Roadmap:**
1. MVP: policy list + premiums (tagged Ledger) + manual coverage radar + emergency fund
2. Integration with PGRF for monthly expense and stress test
3. Expiration alerts and gap coverage suggestions

---

*Generated from source documentation in `/power/App/` structure.*
