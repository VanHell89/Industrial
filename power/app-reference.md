# PoWeR - Product Reference Document

> Personal Wealth Tracker PWA - A Bloomberg Terminal-inspired wealth management application

## 1. Overview

**PoWeR** (Personal Wealth & Retirement) is a local-first Progressive Web Application for personal wealth tracking. Inspired by Bloomberg Terminal aesthetics, it provides sophisticated financial analysis tools while maintaining complete data privacy—all data is stored client-side using SQLite (sql.js WebAssembly) with OPFS/IndexedDB persistence.

### Key Characteristics

| Attribute | Description |
|-----------|-------------|
| **Architecture** | Local-first, offline-capable PWA |
| **Data Storage** | Client-side SQLite via sql.js WebAssembly |
| **Persistence** | OPFS (primary) with IndexedDB fallback |
| **Server Communication** | Zero cloud dependencies (optional Google Drive sync) |
| **Target Users** | Individual investors managing personal portfolios |

### Technology Stack

- **Frontend**: React 19 + TypeScript + Vite
- **Database**: sql.js (SQLite compiled to WebAssembly)
- **State Management**: TanStack Query (server state) + Zustand (UI state)
- **Styling**: Tailwind CSS with Bloomberg-inspired dark theme
- **PWA**: vite-plugin-pwa with service worker

---

## 2. Product Scope and Vision

### Problem Statement

Individual investors need a way to:
- Track portfolio holdings across multiple accounts and asset classes
- Analyze allocation, performance, and dividend income
- Plan for financial independence (FIRE) with accurate projections
- Maintain complete control and privacy over financial data

### Solution

PoWeR provides a self-contained wealth tracking application that:
- **Runs entirely in the browser** with no backend server
- **Stores all data locally** using modern browser storage APIs
- **Works offline** as an installable PWA
- **Delivers professional-grade UI** inspired by financial terminals
- **Offers sophisticated calculations** (XIRR, FIRE projections, SWR simulation)

### Key Differentiators

1. **Privacy-First**: No data leaves the user's device (unless optional cloud sync enabled)
2. **Zero Dependencies**: No accounts, subscriptions, or cloud services required
3. **Professional UI**: Bloomberg Terminal-inspired design language
4. **Financial Intelligence**: Built-in FIRE planning and investment analysis tools
5. **Offline-Capable**: Full functionality without internet connection

---

## 3. Main Features

### 3.1 Dashboard

The central overview displaying key financial metrics at a glance.

**Components:**
- **Net Worth Card**: Total portfolio value + cash balances
- **YTD P&L Card**: Year-to-date profit/loss (absolute and percentage)
- **Cash Balance Card**: Liquid cash across all accounts
- **YTD Dividends Card**: Dividend income received this year
- **Cash Flow Chart**: 12-month visualization of inflows vs outflows

**Data Sources:**
- `useTotalBalance()` - Aggregate account balances
- `useYtdStats()` - Year-to-date performance metrics
- `useMonthlyCashFlow()` - Monthly cash flow breakdown

---

### 3.2 Ledger

Complete transaction history with powerful filtering and search.

**Capabilities:**
- View all transactions (buys, sells, dividends, deposits, withdrawals, fees, transfers, interest)
- Filter by transaction type, date range, account, or asset
- Full-text search on notes, symbols, and asset names
- Create/edit/delete transactions via modal interface

**Transaction Types:**
| Type | Description |
|------|-------------|
| `buy` | Purchase of asset units |
| `sell` | Sale of asset units |
| `dividend` | Dividend payment received |
| `deposit` | Cash deposit to account |
| `withdrawal` | Cash withdrawal from account |
| `fee` | Transaction or account fee |
| `interest` | Interest income |
| `transfer` | Movement between accounts |

---

### 3.3 Portfolio

Holdings view showing current positions with performance metrics.

**Displays:**
- Total portfolio value
- Number of distinct assets held
- Largest position by value
- Per-asset breakdown with:
  - Current units held
  - Average cost basis (FIFO)
  - Current value
  - Unrealized P&L (absolute and percentage)
  - Allocation percentage

**Calculations:**
- Position aggregation from buy/sell transactions
- FIFO cost basis calculation
- Allocation percentages relative to total portfolio

---

### 3.4 Allocation+ (Multi-Dimensional Analysis)

Advanced portfolio breakdown across multiple classification dimensions.

**Grouping Modes:**

1. **By Asset Category**
   - Stock, ETF, Bond, Crypto, Real Estate, Commodity, Cash, Other

2. **By Fund Category**
   - Equity funds
   - Balanced funds
   - Fixed income funds
   - Alternatives

3. **By Portfolio Function**
   - Income (dividend-focused)
   - Growth (capital appreciation)
   - Defensive (stability)
   - Speculative (high-risk)

4. **By Macro Category**
   - Bond funds
   - Balanced funds
   - Equity funds
   - Individual stocks
   - REITs

**Features:**
- Collapsible/expandable sections
- Mini pie charts per category
- Drill-down to individual assets
- Color-coded allocation bars

---

### 3.5 Dividend Calendar (Cedolare)

Dividend income tracking with actual vs expected comparisons.

**Views:**
- **Chart Mode**: Stacked bar chart showing monthly dividend breakdown
- **Table Mode**: Detailed tabular view with per-asset rows

**Data Modes:**
- **Received**: Actual dividend transactions recorded
- **Expected**: Projected dividends based on schedule and holdings

**Features:**
- Year selector for historical analysis
- Monthly breakdown by asset
- Frequency tracking (monthly, quarterly, semi-annual, annual)
- YTD dividend totals

---

### 3.6 PAC Manager (Piano di Accumulo)

Dollar-cost averaging plan management and progress tracking.

**Capabilities:**
- Create named investment plans with monthly amounts
- Define asset allocation percentages within each plan
- Track progress vs expected contributions
- Calculate months active and total invested

**Data Model:**
```
PacPlan {
  name: string
  monthly_amount: number
  asset_allocations: { asset_id, percentage }[]
  account_id: string
  start_date: date
  is_active: boolean
}
```

---

### 3.7 Analytics

Financial independence (FIRE) planning and analysis tools.

**Calculations Available:**

| Calculation | Description |
|-------------|-------------|
| **FIRE Number** | Portfolio needed for independence (based on 4% withdrawal rate) |
| **Years to FIRE** | Time to reach FIRE target at current savings rate |
| **FIRE Progress** | Percentage of FIRE goal achieved |
| **SWR Simulation** | Safe withdrawal rate modeling with inflation |

**User Inputs:**
- Current annual expenses
- Expected savings rate
- Investment return assumptions
- Inflation rate

**Visualizations:**
- Cash flow projection charts
- Dividend income vs PAC expenses comparison
- Progress indicators

---

### 3.8 Settings

User preferences and data management.

**Sections:**

1. **Currency Selection**
   - EUR, USD, GBP, CHF
   - Affects all monetary displays

2. **Theme Configuration**
   - Mode: Light, Dark, System
   - Presets: Bloomberg (default), EVA-01

3. **Data Management**
   - Load sample data for demo
   - Export database (SQLite backup)
   - Import database restore

4. **Cloud Sync** (Feature-flagged)
   - Google Drive integration
   - Manual sync trigger
   - Last sync timestamp

5. **App Information**
   - Version display
   - Build information

---

### 3.9 Help / Guide

In-app documentation with Markdown rendering.

**Available Guides:**
- **User Guide**: Complete feature reference
- **Quick Start Tutorial**: 10-minute introduction

**Features:**
- GitHub Flavored Markdown support
- Table of contents navigation
- Syntax-highlighted code blocks
- Internal link navigation
- Mobile-optimized layout

---

## 4. System Architecture and Views

### Data Flow Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        User Interface                           │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐       │
│  │Dashboard │  │ Ledger   │  │Portfolio │  │Analytics │  ...   │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘       │
└───────┼─────────────┼─────────────┼─────────────┼──────────────┘
        │             │             │             │
        v             v             v             v
┌─────────────────────────────────────────────────────────────────┐
│                     React Query Layer                           │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  Query Cache (5-min stale time, offline-first)          │   │
│  │  Query Keys: transactions.*, assets.*, accounts.*, etc. │   │
│  └─────────────────────────────────────────────────────────┘   │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                            v
┌─────────────────────────────────────────────────────────────────┐
│                    Repository Layer                             │
│  ┌──────────────┐  ┌─────────────┐  ┌───────────────┐         │
│  │transactionRepo│  │ assetRepo   │  │ accountRepo   │  ...    │
│  └──────┬───────┘  └──────┬──────┘  └───────┬───────┘         │
└─────────┼─────────────────┼─────────────────┼───────────────────┘
          │                 │                 │
          v                 v                 v
┌─────────────────────────────────────────────────────────────────┐
│                   Database Client (sql.js)                      │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  SQLite Database (7 tables, WebAssembly runtime)        │   │
│  └─────────────────────────────────────────────────────────┘   │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                            v
┌─────────────────────────────────────────────────────────────────┐
│                   Persistence Layer                             │
│  ┌───────────────────┐    ┌───────────────────────────────┐    │
│  │ OPFS (Primary)    │ OR │ IndexedDB (Fallback)          │    │
│  │ power.sqlite      │    │ power-db / database store     │    │
│  └───────────────────┘    └───────────────────────────────┘    │
└─────────────────────────────────────────────────────────────────┘
```

### State Management Layers

| Layer | Technology | Persistence | Purpose |
|-------|------------|-------------|---------|
| **Database** | sql.js (SQLite) | OPFS/IndexedDB | Authoritative data store |
| **Query Cache** | TanStack Query | Memory (5-min stale) | Cached query results |
| **UI State** | Zustand | localStorage | Navigation, preferences, modals |

### Directory Structure

```
src/
├── db/
│   ├── database.ts           # DatabaseClient singleton
│   ├── schema.sql            # SQLite schema (7 tables)
│   └── repositories/         # Data access layer
│       ├── transactionRepository.ts
│       ├── assetRepository.ts
│       ├── accountRepository.ts
│       ├── dividendRepository.ts
│       └── pacRepository.ts
├── hooks/                    # React Query hooks
│   ├── useTransactions.ts
│   ├── useAssets.ts
│   ├── useAccounts.ts
│   └── ...
├── store/
│   └── ui-store.ts           # Zustand UI state
├── features/                 # Feature modules
│   ├── dashboard/
│   ├── ledger/
│   ├── portfolio/
│   ├── allocation/
│   ├── calendar/
│   ├── pac/
│   ├── analytics/
│   ├── settings/
│   └── guide/
├── components/
│   ├── layout/               # Layout, Sidebar, Header
│   └── ui/                   # Reusable components
├── utils/
│   ├── calculations.ts       # Financial math
│   └── format.ts             # Formatting utilities
├── lib/
│   ├── query-client.ts       # Query configuration
│   └── themes.ts             # Theme definitions
└── types/
    └── index.ts              # TypeScript interfaces
```

---

## 5. Database Schema

### Entity Relationship Overview

```
┌─────────────┐     ┌──────────────────┐     ┌─────────────┐
│   Assets    │────<│   Transactions   │>────│  Accounts   │
└─────────────┘     └──────────────────┘     └─────────────┘
       │                                            │
       │                                            │
       v                                            v
┌─────────────────┐                         ┌─────────────┐
│ DividendSchedule│                         │  PAC Plans  │
└─────────────────┘                         └─────────────┘
                                                   │
                                                   v
                                            ┌─────────────┐
                                            │    Goals    │
                                            └─────────────┘
```

### Table Definitions

#### assets
Investment instruments (stocks, ETFs, bonds, crypto, etc.)

| Column | Type | Description |
|--------|------|-------------|
| id | TEXT PK | UUID identifier |
| symbol | TEXT | Ticker symbol |
| name | TEXT | Full asset name |
| currency | TEXT | EUR/USD/GBP/CHF |
| category | TEXT | stock, etf, bond, crypto, cash, real_estate, commodity, other |
| fund_category | TEXT | equity, balanced, fixed_income, alternatives |
| portfolio_function | TEXT | income, growth, defensive, speculative |
| target_allocation_pct | REAL | Target allocation percentage |
| created_at, updated_at, deleted_at | TEXT | Timestamps |

#### accounts
Brokerage accounts, bank accounts, wallets

| Column | Type | Description |
|--------|------|-------------|
| id | TEXT PK | UUID identifier |
| name | TEXT | Account name |
| type | TEXT | brokerage, bank, crypto, retirement, savings |
| currency | TEXT | Default currency |
| institution | TEXT | Financial institution name |
| is_active | INTEGER | Active flag |
| created_at, updated_at, deleted_at | TEXT | Timestamps |

#### transactions
All financial movements

| Column | Type | Description |
|--------|------|-------------|
| id | TEXT PK | UUID identifier |
| date | TEXT | Transaction date |
| type | TEXT | buy, sell, dividend, deposit, withdrawal, fee, interest, transfer |
| asset_id | TEXT FK | Reference to assets (nullable) |
| account_id | TEXT FK | Reference to accounts |
| amount_fiat | REAL | Fiat currency amount |
| units | REAL | Number of units (for buys/sells) |
| price_per_unit | REAL | Price per unit |
| fee | REAL | Transaction fee |
| note | TEXT | User notes |
| dividend_classification | TEXT | Classification for dividend transactions |
| created_at, updated_at, deleted_at | TEXT | Timestamps |

#### dividend_schedule
Expected dividend payments

| Column | Type | Description |
|--------|------|-------------|
| id | TEXT PK | UUID identifier |
| asset_id | TEXT FK | Reference to assets |
| ex_date | TEXT | Ex-dividend date |
| pay_date | TEXT | Payment date |
| amount_per_unit | REAL | Dividend per unit |
| frequency | TEXT | monthly, quarterly, semi_annual, annual |
| created_at | TEXT | Timestamp |

#### pac_plans
Dollar-cost averaging plans

| Column | Type | Description |
|--------|------|-------------|
| id | TEXT PK | UUID identifier |
| name | TEXT | Plan name |
| monthly_amount | REAL | Monthly investment |
| asset_allocations | TEXT | JSON array of {asset_id, percentage} |
| account_id | TEXT FK | Target account |
| start_date | TEXT | Plan start date |
| is_active | INTEGER | Active flag |
| created_at, updated_at, deleted_at | TEXT | Timestamps |

#### goals
Financial targets

| Column | Type | Description |
|--------|------|-------------|
| id | TEXT PK | UUID identifier |
| name | TEXT | Goal name |
| target_amount | REAL | Target value |
| target_date | TEXT | Target date |
| current_amount | REAL | Current progress |
| linked_account_ids | TEXT | JSON array of account IDs |
| created_at, updated_at, deleted_at | TEXT | Timestamps |

#### app_metadata
Application settings

| Column | Type | Description |
|--------|------|-------------|
| key | TEXT PK | Setting key |
| value | TEXT | Setting value |

---

## 6. User Roles and Flows

### User Model

PoWeR operates as a **single-user application**. There is no authentication system or multi-user support—all data belongs to the device owner.

### Key User Workflows

#### Adding a Transaction

```
1. User clicks "Add Transaction" (Header or FAB)
2. Transaction modal opens
3. User selects transaction type
4. Form adapts based on type:
   - buy/sell: asset, units, price, account
   - dividend: asset, amount, account
   - deposit/withdrawal: amount, account
5. User submits form
6. Mutation creates transaction
7. Query cache invalidates (transactions, accounts, portfolio, dashboard)
8. UI refreshes with new data
```

#### Viewing Portfolio Performance

```
1. User navigates to Portfolio view
2. useAssetsWithPositions() hook fetches data
3. Repository aggregates transactions per asset:
   - Total units (buys - sells)
   - Cost basis (FIFO)
   - Current value (units × price)
4. UI displays positions with P&L calculations
5. Allocation percentages computed relative to total
```

#### Tracking Dividend Income

```
1. User navigates to Dividend Calendar
2. User selects year and view mode (received/expected)
3. For received: queries actual dividend transactions
4. For expected: joins schedule with holdings at ex-date
5. Stacked bar chart or table displays monthly breakdown
```

#### FIRE Planning

```
1. User navigates to Analytics
2. User inputs annual expenses and savings rate
3. calculateFIRENumber() computes target (expenses / 0.04)
4. calculateYearsToFIRE() projects timeline
5. UI displays progress and projections
```

---

## 7. Integrations and Data Interfaces

### Import/Export

#### CSV Import
- Step-by-step wizard for transaction import
- Column mapping interface
- Date/number format detection
- Duplicate detection and handling
- Supports all transaction types

#### Database Export
- Full SQLite database download
- Filename includes timestamp
- Can be used for backup/restore

#### Database Import
- Upload previously exported database
- Replaces current database
- Persists to OPFS/IndexedDB

### Cloud Sync (Optional)

When enabled via `ENABLE_CLOUD_SYNC` environment variable:

- **Provider**: Google Drive
- **Authentication**: OAuth 2.0 via Google Identity Services
- **Sync Model**: Manual trigger (not automatic)
- **Conflict Resolution**: Last-write-wins

### PWA Capabilities

- **Service Worker**: Caches app shell and assets
- **Manifest**: Enables "Add to Home Screen"
- **Offline Support**: Full functionality without network
- **Update Detection**: Toast notification for new versions

### CORS Headers

Required for OPFS (Origin Private File System):
```
Cross-Origin-Opener-Policy: same-origin
Cross-Origin-Embedder-Policy: require-corp
```

---

## 8. AI/Automation Notes

### Structured Data Access

The repository pattern provides clean, predictable data access:

```typescript
// Reading data
const transactions = await transactionRepository.getAll({
  startDate: '2024-01-01',
  type: 'dividend'
});

// Writing data
const id = await transactionRepository.create({
  date: '2024-06-15',
  type: 'buy',
  asset_id: 'uuid-here',
  account_id: 'uuid-here',
  units: 10,
  price_per_unit: 150.00,
  amount_fiat: 1500.00
});
```

### Query Key Factory

Predictable cache keys for automated invalidation:

```typescript
queryKeys.transactions.all()           // ['transactions']
queryKeys.transactions.list(filters)   // ['transactions', 'list', filters]
queryKeys.assets.withPositions()       // ['assets', 'withPositions']
queryKeys.dashboard.all()              // ['dashboard']
```

### Pure Calculation Functions

All financial calculations are pure functions in `src/utils/calculations.ts`:

```typescript
// FIRE calculations
calculateFIRENumber(annualExpenses: number, withdrawalRate?: number): number
calculateYearsToFIRE(currentPortfolio, annualSavings, expectedReturn, fireNumber): number
simulateSWR(portfolio, withdrawalRate, years, inflationRate): SimulationResult[]

// Investment returns
calculateXIRR(cashFlows: CashFlow[], guess?: number): number
calculatePortfolioPositions(transactions: Transaction[]): Position[]

// Dividend analysis
calculateDividendRunRate(dividends: Dividend[], period: 3 | 6 | 12): number
calculateGrowthRate(monthlyData: MonthlyDividend[]): number
```

### Automation Opportunities

1. **Transaction Import Automation**
   - CSV parsing is already implemented
   - Could be extended for broker API integration
   - Webhook-based import possible with cloud sync

2. **Price Updates**
   - Currently manual (no automatic price feeds)
   - Could integrate market data APIs
   - Asset `price_per_unit` stored in transactions

3. **Dividend Schedule Population**
   - Manual entry currently
   - Could scrape/import from dividend databases
   - Frequency detection for projections

4. **Report Generation**
   - All data accessible via repositories
   - Calculations available as pure functions
   - Could generate PDF/Excel reports

### Data Formats

| Entity | JSON Structure |
|--------|----------------|
| PAC Allocations | `[{ "asset_id": "uuid", "percentage": 25 }, ...]` |
| Linked Accounts (Goals) | `["account-uuid-1", "account-uuid-2", ...]` |
| Date Strings | ISO format `"2024-06-15"` |
| Currency Amounts | Decimal numbers, 2-decimal precision |

---

## 9. Theming and UI

### Color System

**Bloomberg Theme (Default)**
| Token | Value | Usage |
|-------|-------|-------|
| bg-primary | #0a0a0a | Main background |
| bg-secondary | #1a1a1a | Cards, panels |
| accent | #ff6600 | Primary interactive elements |
| positive | #00d26a | Gains, success states |
| negative | #ff3b3b | Losses, error states |
| text-primary | #ffffff | Primary text |
| text-secondary | #a0a0a0 | Secondary text |

**EVA-01 Theme (Alternative)**
| Token | Value | Usage |
|-------|-------|-------|
| bg-primary | #0d0518 | Main background |
| bg-secondary | #1a0a2e | Cards, panels |
| accent | #00ff41 | Neon green accent |
| positive | #00ff41 | Positive states |
| negative | #ff0066 | Negative states |

### Typography

- **Font Family**: SF Mono, Monaco, Consolas (monospace)
- **Base Size**: 16px
- **Data Dense**: 0.625rem (`xxs` custom size)

---

## 10. Assumptions and Open Questions

### Assumptions

1. **Single Currency Display**: While multiple currencies are supported for storage, the UI displays all values in the user's selected currency without conversion
2. **No Real-Time Prices**: Current prices are derived from last transaction or manual entry
3. **FIFO Cost Basis**: Position cost calculations use First-In-First-Out method
4. **4% SWR Default**: FIRE calculations assume 4% safe withdrawal rate as baseline

### Potential Enhancements

1. **Multi-Currency Conversion**: Automatic FX rate lookups for cross-currency holdings
2. **Price Feed Integration**: Real-time or daily price updates from market data APIs
3. **Tax Lot Tracking**: Specific identification method for tax optimization
4. **Benchmark Comparison**: Performance vs index returns
5. **Recurring Transaction Templates**: Automated dividend/contribution entries

---

*Document generated from codebase analysis. Last updated: 2026-02-01*
