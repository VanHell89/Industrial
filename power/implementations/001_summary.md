All tasks are complete. Here's a summary of the implemented features:

## Implementation Summary

### Completed Features

#### 1. Dividend Classification (HIGH Priority)
- Added `DividendClassification` type with 5 values: `coupon_bond_fund`, `coupon_balanced_fund`, `coupon_equity_fund`, `dividend_stock`, `dividend_reit`
- Added database migration to add `dividend_classification` column to transactions table
- Updated TransactionModal with conditional dropdown (shows only when type = "Dividend")
- Classification is required for dividend transactions

#### 2. Duplicate Detection in CSV Import (HIGH Priority - Bug Fix)
- Added `findDuplicate()` method to transactions repository
- Updated CsvImportWizard to check for duplicates during preview
- Duplicates are highlighted in orange in the preview table
- Added checkbox to skip duplicates during import
- Shows count of skipped duplicates after import

#### 3. Top 10 + Others Chart (LOW Priority)
- Changed `MAX_ASSETS` from 5 to 10 in DividendStackedChart.tsx

#### 4. FIRE Progress KPI Fix (MEDIUM Priority)
- Fixed `calculateDividendRunRate` to divide by period length instead of data length
- This ensures months with no dividends count as 0 in the average

#### 5. Allocation+ Subcategory Charts (MEDIUM Priority)
- Added 3 new repository methods: `getAllocationByMacroCategory`, `getAllocationBySuperMacro`, `getAssetBreakdown`
- Added corresponding hooks and query keys
- Updated AllocationView with 5 new collapsible chart sections:
  - Super Macro (Funds / Stocks / REITs)
  - Macro Categories (Bond/Balanced/Equity Funds, Stocks, REITs)
  - Funds Detail (individual fund breakdown)
  - Stocks Detail (individual stock breakdown)
  - REITs Detail (individual REIT breakdown)

### Files Modified
- `src/types/index.ts` - Added DividendClassification type
- `src/db/database.ts` - Added migration for new column
- `src/db/schema.sql` - Documentation (migration handles runtime)
- `src/features/ledger/TransactionModal.tsx` - Added classification dropdown
- `src/db/repositories/transactions.ts` - Added findDuplicate method
- `src/db/repositories/assets.ts` - Added 3 new allocation methods
- `src/features/importer/CsvImportWizard.tsx` - Added duplicate detection
- `src/features/importer/CsvImporter.tsx` - Added dividend_classification
- `src/features/calendar/DividendStackedChart.tsx` - MAX_ASSETS = 10
- `src/features/analytics/CashFlowFire.tsx` - (no change needed, fixed in utils)
- `src/utils/calculations.ts` - Fixed run rate calculation
- `src/features/allocation/AllocationView.tsx` - Added subcategory charts
- `src/hooks/useAllocation.ts` - Added new hooks
- `src/hooks/index.ts` - Exported new hooks
- `src/lib/query-client.ts` - Added new query keys
- `src/db/sample-data/transactions.ts` - Added dividend_classification