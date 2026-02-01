# PoWeR App Feature Implementation Plan

## Overview

This plan addresses the feature requests, enhancements, and bug fixes from the consolidated requirements document. The implementation is organized by priority and dependency.

---

## Current State Analysis

### Database Schema (src/db/schema.sql)
- **7 tables**: assets, accounts, transactions, goals, dividend_schedule, pac_plans, app_metadata
- **Asset categories**: `stock | etf | bond | crypto | cash | real_estate | commodity | other`
- **Fund categories**: `equity | balanced | fixed_income | alternatives` (optional field)
- **Transaction types**: `buy | sell | dividend | deposit | withdrawal | fee | interest | transfer`

### Key Findings
1. **No dividend sub-classification** - Dividends are simple transaction type without classification
2. **Asset category is basic** - Uses generic types (stock, etf, bond), not the Italian-specific categories requested (Fondi obbligazionari, bilanciati, azionari, Azioni, REIT)
3. **Allocation charts** - 3 grouping modes (category, fund_category, portfolio_function), using Recharts donut charts
4. **Dividend chart** - Already has Top 5 + Others logic (`MAX_ASSETS = 5` in DividendStackedChart.tsx:29)
5. **No dedicated real estate module** - `real_estate` is just an asset category value

---

## Implementation Phases

### Phase 1: Foundation Changes (High Priority)

#### 1.1 Add Dividend Classification to Transactions
**Files to modify:**
- `src/types/index.ts` - Add `DividendClassification` type
- `src/db/schema.sql` - Add `dividend_classification` column to transactions
- `src/features/ledger/TransactionModal.tsx` - Add conditional dropdown
- `src/db/repositories/transactions.ts` - Include classification in queries

**New Type:**
```typescript
export type DividendClassification =
  | 'coupon_bond_fund'      // Cedola - Fondo obbligazionario
  | 'coupon_balanced_fund'  // Cedola - Fondo bilanciato
  | 'coupon_equity_fund'    // Cedola - Fondo azionario
  | 'dividend_stock'        // Dividendo - Azione
  | 'dividend_reit';        // Dividendo - REIT
```

**UI Labels (English):**
- Coupon - Bond Fund
- Coupon - Balanced Fund
- Coupon - Equity Fund
- Dividend - Stock
- Dividend - REIT

**UI Change:** When Type = "Dividend", show mandatory classification dropdown.

#### 1.2 Align Asset Categories with Requirements
**Files to modify:**
- `src/types/index.ts` - Update `AssetCategory` type
- `src/db/schema.sql` - Migration for category values
- `src/features/portfolio/AssetModal.tsx` - Update category selector
- `src/features/ledger/TransactionModal.tsx` - Update inline asset creation

**Keep Existing Categories** (no changes needed):
```typescript
// Current categories are already in English - no changes required
export type AssetCategory =
  'stock' | 'etf' | 'bond' | 'crypto' | 'cash' | 'real_estate' | 'commodity' | 'other';
```

**Use existing `fund_category` field** to distinguish fund types:
- `equity` → Equity Fund
- `balanced` → Balanced Fund
- `fixed_income` → Bond Fund

**No migration needed** - existing data structure supports the requirements.

---

### Phase 2: Allocation+ Enhancements (Medium Priority)

#### 2.1 Add Subcategory Charts
**Files to modify:**
- `src/features/allocation/AllocationView.tsx` - Add new chart sections
- `src/db/repositories/assets.ts` - Add new aggregation methods
- `src/hooks/useAllocation.ts` - Add new hooks
- `src/lib/query-client.ts` - Add new query keys

**New Charts:**
1. **Macro Categories (5)** - Bond Funds, Balanced Funds, Equity Funds, Stocks, REITs
2. **Super Macro (3)** - Funds (combined), Stocks, REITs
3. **Funds Detail** - Individual funds breakdown (ETFs + bonds by fund_category)
4. **Stocks Detail** - Individual stocks breakdown
5. **REITs Detail** - Individual REITs breakdown

**New Repository Methods:**
```typescript
getAllocationByMacroCategory()
getAllocationBySuperMacro()
getAssetsByCategory(category: string)
```

---

### Phase 3: Cedolare Enhancements

#### 3.1 Top 10 + Others Chart Aggregation (LOW PRIORITY - Simple Change)
**Files to modify:**
- `src/features/calendar/DividendStackedChart.tsx` - Change `MAX_ASSETS = 5` to `MAX_ASSETS = 10` (line 29)

**Note:** The logic already exists! Just update the constant from 5 to 10.

#### 3.2 Duplicate Prevention on Copy-Paste (HIGH PRIORITY - Bug Fix)
**Files to modify:**
- `src/features/ledger/CsvImportWizard.tsx` - Add duplicate detection
- `src/db/repositories/transactions.ts` - Add uniqueness check method

**Approach:**
- Add `findDuplicates()` method to check (date, type, asset_id, account_id, amount_fiat)
- Show preview with duplicate warnings before import
- Allow user to skip duplicates or abort import

---

### Phase 4: Analytics/FIRE Fix (Medium Priority)

#### 4.1 Fix Progress KPI Calculation
**Files to modify:**
- `src/features/analytics/CashFlowFire.tsx` - Fix Progress calculation

**Current Issue:** Uses inconsistent value (possibly last month)
**Fix:** `progress = run_rate / target_income` where run_rate is the selected period average

---

### Phase 5: New Modules (Future - Lower Priority)

These require significant new development and should be scoped separately:

#### 5.1 PGRF (Financial Resource Planning)
- New feature module
- Integrates inflows/outflows
- Leverage tracking
- Requires extensive new UI

#### 5.2 Immobiliare (Real Estate)
- New feature module
- Property lifecycle tracking
- Mortgage management
- New transaction types

#### 5.3 Protezione (Insurance)
- New feature module
- Policy tracking
- Risk coverage analysis
- Emergency fund tracking

---

## Detailed Implementation: Phase 1

### 1.1 Dividend Classification

**Step 1: Update Types** (`src/types/index.ts`)
```typescript
// Add after line 9 (after PortfolioFunction)
export type DividendClassification =
  | 'coupon_bond_fund'      // Coupon - Bond Fund
  | 'coupon_balanced_fund'  // Coupon - Balanced Fund
  | 'coupon_equity_fund'    // Coupon - Equity Fund
  | 'dividend_stock'        // Dividend - Stock
  | 'dividend_reit';        // Dividend - REIT

// Update Transaction interface (line 41-55) to add:
dividend_classification: DividendClassification | null;
```

**Step 2: Schema Migration** (`src/db/database.ts`)
Add to `runMigrations()` method:
```typescript
// Check if dividend_classification column exists
const txInfo = this.db.exec("PRAGMA table_info(transactions)");
const txColumns = txInfo[0]?.values?.map((r) => r[1] as string) || [];
if (!txColumns.includes('dividend_classification')) {
  this.db.run('ALTER TABLE transactions ADD COLUMN dividend_classification TEXT');
}
```

**Step 3: TransactionModal Update** (`src/features/ledger/TransactionModal.tsx`)
- Add `dividendClassifications` array with English labels:
  ```typescript
  const dividendClassifications = [
    { value: 'coupon_bond_fund', label: 'Coupon - Bond Fund' },
    { value: 'coupon_balanced_fund', label: 'Coupon - Balanced Fund' },
    { value: 'coupon_equity_fund', label: 'Coupon - Equity Fund' },
    { value: 'dividend_stock', label: 'Dividend - Stock' },
    { value: 'dividend_reit', label: 'Dividend - REIT' },
  ];
  ```
- Add conditional dropdown when `form.type === 'dividend'`
- Add validation: required field when type is dividend
- Optional: Auto-suggest classification based on selected asset's category/fund_category

**Step 4: Repository Update** (`src/db/repositories/transactions.ts`)
- Include `dividend_classification` in INSERT/UPDATE queries
- Update TypeScript return types

### 1.2 Asset Category Strategy

**Decision: Keep existing categories as-is**

The current categories (`stock`, `etf`, `bond`, etc.) are already in English and work well as system-level identifiers. No changes needed to the AssetCategory type.

**Use `fund_category` for fund type distinction** (already exists):
- `equity` → Equity Fund
- `balanced` → Balanced Fund
- `fixed_income` → Bond Fund
- `alternatives` → Alternatives

**For Allocation+ charts**, we'll aggregate using both `category` and `fund_category`:
- **Macro Categories**: Stock, ETF (by fund_category), Bond, REIT (real_estate), Other
- **Super Macro**: Funds (etf + bond), Stocks, REITs

**This approach requires no data migration.**

---

## Verification Plan

### Testing Approach
1. **Unit tests** for new repository methods
2. **Manual testing** of form validation
3. **Data migration** verification (check migration runs on app start)
4. **Chart rendering** with various data combinations

### Test Cases
- [ ] Create dividend transaction → classification dropdown appears and is required
- [ ] Edit existing dividend transaction → classification field shows
- [ ] View Allocation+ with subcategory charts (all 5 new charts render)
- [ ] Dividend calendar with >10 assets/month → shows Top 10 + Others
- [ ] FIRE progress = run_rate / target_income (verify formula)
- [ ] CSV import with duplicates → shows warning and allows skip

---

## Files to Modify Summary

| File | Changes | Priority |
|------|---------|----------|
| `src/types/index.ts` | Add DividendClassification type, update Transaction interface | HIGH |
| `src/db/database.ts` | Add migration for dividend_classification column | HIGH |
| `src/features/ledger/TransactionModal.tsx` | Add classification dropdown (conditional on type=dividend) | HIGH |
| `src/db/repositories/transactions.ts` | Include classification in CRUD operations | HIGH |
| `src/features/ledger/CsvImportWizard.tsx` | Add duplicate detection logic | HIGH |
| `src/features/allocation/AllocationView.tsx` | Add 5 new chart sections | MEDIUM |
| `src/db/repositories/assets.ts` | Add macro/super-macro allocation methods | MEDIUM |
| `src/hooks/useAllocation.ts` | Add new allocation hooks | MEDIUM |
| `src/lib/query-client.ts` | Add new query keys | MEDIUM |
| `src/features/analytics/CashFlowFire.tsx` | Fix Progress KPI calculation | MEDIUM |
| `src/features/calendar/DividendStackedChart.tsx` | Change MAX_ASSETS from 5 to 10 | LOW |

---

## Open Questions for User

1. **Dividend Classification Backfill:** Should existing dividend transactions get a default classification, or remain null (show as "Unclassified")?

2. **New Modules Scope:** Are PGRF, Immobiliare, and Protezione modules in scope for this iteration, or should we defer them to a future release?

3. **Allocation+ Layout:** Should the 5 new charts appear:
   - A) In tabs/sections (one visible at a time)
   - B) All visible on a scrollable page with collapsible sections
   - C) In a dashboard-style grid

4. **Asset Category Field:** The original request mentioned making category "mandatory at asset creation". Currently it defaults to 'other'. Should we:
   - A) Keep the default and just ensure UI prompts user to select
   - B) Remove the default and require explicit selection

---

## Implementation Estimate

| Phase | Items | Complexity |
|-------|-------|------------|
| Phase 1 | Dividend Classification, Duplicate Prevention | ~15 files |
| Phase 2 | Allocation+ Charts | ~8 files, new components |
| Phase 3 | Cedolare Top 10, FIRE Fix | ~3 files, minor changes |
| Phase 4 | Testing & Polish | Verification |
| Future | PGRF, Immobiliare, Protezione | Major new modules |

---

*Plan created: 2026-02-01*
