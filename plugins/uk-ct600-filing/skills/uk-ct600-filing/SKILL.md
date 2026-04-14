---
name: uk-ct600-filing
description: Practical guide to filing UK Corporation Tax returns (CT600) for small and micro companies using Alphatax Cloud from Tax Systems. Use this skill when the user asks to prepare or file a CT600, categorise business expenses, prepare a tax computation, calculate capital allowances, handle directors' loans or dividends, prepare statutory accounts, or work through any step of the corporation tax return filing process.
---

# UK CT600 Filing Guide — Small & Micro Companies

You are an expert in preparing and filing UK Corporation Tax returns (CT600) for small and micro companies. You guide users through the entire process — from categorising bank transactions and receipts through to submitting the return via Alphatax Cloud.

**Important:** This is practical filing guidance, not professional tax advice. Complex situations (international transactions, R&D claims, group structures) should be reviewed by a qualified accountant or tax adviser. Always verify current rates and thresholds with HMRC.

---

## End-to-End Filing Process Overview

```
1. GATHER RECORDS
   Bank statements, receipts, invoices, payroll records, prior year accounts
        ↓
2. CATEGORISE TRANSACTIONS
   Sort every transaction into accounting categories
        ↓
3. PREPARE ACCOUNTS
   Profit & Loss account + Balance Sheet (FRS 105 micro / FRS 102 s1A small)
        ↓
4. TAX COMPUTATION
   Adjust accounting profit → taxable profit (add-backs, capital allowances, reliefs)
        ↓
5. COMPLETE CT600
   Enter figures into Alphatax Cloud, box by box
        ↓
6. ATTACH ACCOUNTS + COMPUTATION
   Alphatax generates iXBRL-tagged accounts and computation
        ↓
7. FILE WITH HMRC
   Submit electronically via Alphatax Cloud
        ↓
8. PAY THE TAX
   Pay within 9 months + 1 day of accounting period end
```

---

## Step 1: Gather Records

### Essential Documents Checklist

| Document | Purpose |
|----------|---------|
| **Bank statements** (all accounts) | Complete record of money in/out |
| **Receipts and invoices** | Evidence for expense claims |
| **Sales invoices** | Revenue recognition |
| **Payroll records** (RTI submissions) | Salary, PAYE, NIC figures |
| **Previous year accounts** | Opening balances, brought-forward losses |
| **Companies House confirmation statement** | Verify company details |
| **Loan agreements** | Interest calculations |
| **Asset purchase records** | Capital allowances claims |
| **Director's loan account** | Track drawings vs salary/dividends |
| **Dividend vouchers** | Dividends declared and paid |
| **VAT returns** (if registered) | Cross-reference with accounts |
| **Bank interest certificates** | Interest received/paid |
| **Pension contribution receipts** | Employer pension deductions |

### Accounting Period

- Usually 12 months, aligned with the company's financial year
- Cannot exceed 12 months for CT purposes (longer periods are split)
- First accounting period starts on incorporation date

---

## Step 2: Categorise Transactions

### Revenue (Income)

| Category | Examples | Notes |
|----------|----------|-------|
| **Sales/Turnover** | Invoiced revenue, service fees, contract income | Core trading income |
| **Other operating income** | Insurance claims, grants, miscellaneous | Non-core but operational |
| **Bank interest received** | Savings interest, deposit interest | Non-trading income for most companies |
| **Rental income** | Property let by the company | Separate property business |
| **Dividends received** | From investments in other companies | Usually exempt (substantial shareholding) |

### Allowable Expenses (Tax-Deductible)

| Category | Examples | CT600 Treatment |
|----------|----------|-----------------|
| **Cost of sales** | Materials, stock purchased, direct labour, subcontractors | Deductible |
| **Staff costs** | Gross salaries, employer NIC, employer pension contributions, bonuses | Deductible |
| **Director remuneration** | Director salary, employer NIC on director salary | Deductible if actually paid in period or within 9 months |
| **Rent & rates** | Office rent, business rates, service charges | Deductible |
| **Utilities** | Electricity, gas, water (business proportion) | Deductible |
| **Insurance** | Professional indemnity, public liability, employer's liability, contents | Deductible |
| **Repairs & maintenance** | Office repairs, equipment maintenance (not improvements) | Deductible |
| **Telephone & internet** | Business phone lines, broadband (business proportion) | Deductible |
| **Postage & stationery** | Stamps, paper, printer cartridges, envelopes | Deductible |
| **Travel & subsistence** | Train fares, mileage (45p/25p), hotels on business trips, meals on overnight stays | Deductible |
| **Motor expenses** | Fuel, insurance, road tax, repairs (business proportion) | Deductible |
| **Professional fees** | Accountancy fees, legal fees (revenue nature), consultancy | Deductible |
| **Bank charges** | Account fees, transaction charges, overdraft interest | Deductible |
| **Loan interest** | Interest on business loans (not the capital repayment) | Deductible |
| **Software & subscriptions** | SaaS subscriptions, cloud services, trade journals | Deductible |
| **Training** | Staff training courses (related to current role) | Deductible |
| **Advertising & marketing** | Website, Google Ads, flyers, business cards | Deductible |
| **Bad debts** | Specific debts written off (not general provisions) | Deductible |
| **Home office** | Proportion of rent/mortgage interest, utilities, council tax, broadband | Deductible (see home office section) |
| **Employer pension contributions** | Contributions to registered scheme | Deductible when paid |
| **Trivial benefits** | Gifts to directors/staff ≤ £50 each, not cash, not contractual | Deductible (£300/year cap for directors) |

### Disallowable Expenses (Must Be Added Back)

| Category | Examples | Why Disallowable |
|----------|----------|-----------------|
| **Depreciation** | All depreciation charges in the accounts | Replaced by capital allowances |
| **Amortisation of goodwill** | Goodwill write-down | Capital nature (unless intangible fixed asset relief applies) |
| **Entertainment** | Client meals, hospitality, event tickets, gifts of food/drink | Specifically disallowed (s.1298 CTA 2009) |
| **Non-business expenses** | Personal spending through company, private element of mixed costs | Fails "wholly and exclusively" test |
| **Fines & penalties** | Parking fines, HMRC penalties, regulatory fines | Public policy disallowance |
| **Political donations** | Donations to political parties | Specifically disallowed |
| **Capital expenditure** | Equipment purchases, fit-out costs (claimed as capital allowances instead) | Capital vs revenue distinction |
| **General provisions** | Provisions for doubtful debts (non-specific), warranty provisions | Not specific enough for tax |
| **Legal costs (capital)** | Legal fees on property purchase, share acquisition, lease premium | Capital nature |
| **Clothing (non-protective)** | Suits, business attire | Not wholly and exclusively for trade |
| **Director's personal expenses** | Personal travel, personal phone, private fuel | Not business purpose |
| **S455 tax on director's loans** | CT on overdrawn director's loan account | Separate CT600A charge, not a deduction |

### The "Wholly and Exclusively" Test

For an expense to be deductible for corporation tax, it must be incurred **wholly and exclusively for the purposes of the trade**. This is the fundamental rule from s.54 CTA 2009.

**Dual-purpose expenditure:** If an expense has both a business and personal element:
- If the expense is **identifiably split** (e.g., phone bill: business calls vs personal), claim the business portion
- If the expense is **inherently dual purpose** (e.g., a suit worn for work and socially), the whole expense is disallowed

---

## Step 3: Prepare Statutory Accounts

### Which Accounting Standard?

| Company Size | Standard | Requirement |
|-------------|----------|-------------|
| **Micro-entity** | FRS 105 | Simplified accounts, minimal disclosures |
| **Small company** | FRS 102 Section 1A | More detailed but still reduced disclosures |

### Micro-Entity Thresholds (meet any 2)

| Criterion | Threshold |
|-----------|-----------|
| Turnover | ≤ £1,000,000 |
| Balance sheet total | ≤ £500,000 |
| Employees | ≤ 10 |

### Small Company Thresholds (meet any 2)

| Criterion | Threshold |
|-----------|-----------|
| Turnover | ≤ £15,000,000 |
| Balance sheet total | ≤ £7,500,000 |
| Employees | ≤ 50 |

### Accounts Components

**For HMRC (full statutory accounts with CT600):**
- Profit and Loss account (income statement)
- Balance Sheet
- Notes to the accounts
- Director's report (small companies; optional for micro)

**For Companies House (may be abbreviated):**
- Micro: Balance sheet only (can omit P&L and director's report)
- Small: Can omit P&L and director's report

### Profit & Loss Account Structure

```
Turnover (sales revenue)
− Cost of sales
────────────────────────
= Gross profit
− Administrative expenses
− Distribution costs
+ Other operating income
────────────────────────
= Operating profit
+ Interest receivable
− Interest payable
────────────────────────
= Profit before tax
− Corporation tax
────────────────────────
= Profit after tax
```

### Balance Sheet Structure

```
Fixed Assets
  Tangible assets (net book value after depreciation)
  Intangible assets
  Investments
────────────────────────
Current Assets
  Stock/inventory
  Trade debtors
  Other debtors (including director's loan if company owes director)
  Cash at bank and in hand
  Prepayments
────────────────────────
Current Liabilities (due within 1 year)
  Trade creditors
  Other creditors
  Accruals
  Corporation tax payable
  PAYE/NIC payable
  VAT payable
  Director's loan account (if director owes company)
  Short-term loans
────────────────────────
Net Current Assets = Current Assets − Current Liabilities
────────────────────────
Total Assets Less Current Liabilities
────────────────────────
Long-Term Liabilities (due after 1 year)
  Long-term loans
  Hire purchase (long-term portion)
────────────────────────
Net Assets
════════════════════════
Capital and Reserves
  Share capital
  Profit and loss account (retained earnings)
────────────────────────
Total Equity
```

---

## Step 4: Tax Computation

The tax computation bridges accounting profit to taxable profit. This is the core of the CT600.

### Tax Computation Template

```
                                                    £           £
Profit per accounts (before tax)                              X,XXX

ADD BACK: Disallowable expenses
  Depreciation                                      X,XXX
  Entertainment                                       XXX
  Fines and penalties                                  XXX
  Non-business proportion of expenses                  XXX
  Capital expenditure in P&L                           XXX
  General provisions                                   XXX
  Other disallowable items                             XXX
                                                    ─────
  Total add-backs                                             X,XXX
                                                              ─────
                                                              X,XXX

DEDUCT: Non-taxable income
  Dividends received (exempt)                         XXX
  Other exempt income                                  XXX
                                                    ─────
  Total deductions                                            (XXX)
                                                              ─────
Trading profit before capital allowances                      X,XXX

DEDUCT: Capital allowances
  Annual Investment Allowance                       X,XXX
  Writing Down Allowance (main pool 18%/14%)          XXX
  Writing Down Allowance (special rate 6%)            XXX
  Balancing allowances                                 XXX
                                                    ─────
  Total capital allowances                                  (X,XXX)
                                                              ─────
Adjusted trading profit                                       X,XXX

ADD: Non-trading income
  Bank interest received                               XXX
  Rental income (net of expenses)                      XXX
                                                    ─────
  Total non-trading income                                      XXX
                                                              ─────
Total profits                                                 X,XXX

DEDUCT: Qualifying charitable donations                        (XXX)
DEDUCT: Losses brought forward                               (XXX)
                                                              ─────
Taxable total profits                                         X,XXX
════════════════════════════════════════════════════════════════

Corporation tax @ main rate (25%) or small profits rate (19%)
or with marginal relief                                         XXX
```

### Capital Allowances — Practical Guide

#### Annual Investment Allowance (AIA)

| Feature | Detail |
|---------|--------|
| **Annual limit** | £1,000,000 |
| **What qualifies** | Plant and machinery, equipment, tools, furniture, computers, vans, integral features |
| **What does NOT qualify** | Cars, items owned before business use, gifts |
| **Claim** | 100% of cost in the year of purchase (up to limit) |
| **Short periods** | Pro-rate: e.g., 6-month period = £500,000 |

#### Writing Down Allowances (WDA)

When assets exceed the AIA, or for items not qualifying for AIA:

| Pool | Rate (from April 2026) | Rate (before April 2026) | Items |
|------|----------------------|-------------------------|-------|
| **Main pool** | 14% | 18% | Most plant & machinery, low-emission cars (≤50g/km CO₂) |
| **Special rate pool** | 6% | 6% | Long-life assets, integral features, high-emission cars (>50g/km CO₂), thermal insulation |

WDA is calculated on the **reducing balance** (pool value at start of period + additions − disposals).

#### Cars — Special Rules

| CO₂ Emissions | Allowance |
|--------------|-----------|
| 0 g/km (electric) | 100% First Year Allowance |
| 1–50 g/km | Main pool (14%/18% WDA) |
| > 50 g/km | Special rate pool (6% WDA) |

Cars **never** qualify for AIA or full expensing.

#### Full Expensing (Companies Only, from April 2023)

| Feature | Detail |
|---------|--------|
| **Rate** | 100% deduction in year of purchase |
| **What qualifies** | New, unused plant and machinery (not cars) |
| **Alternative** | 50% first-year allowance for special rate items |

#### Balancing Adjustments on Disposal

When an asset is sold or scrapped:
- **Balancing allowance:** If disposal proceeds < pool value → claim the difference as an allowance
- **Balancing charge:** If disposal proceeds > pool value → add the excess back as taxable income
- **Small pool rule:** If the pool balance is ≤ £1,000, you can write off the whole balance

### Directors' Remuneration — Tax Treatment

#### Salary

| Item | Corporation Tax Treatment |
|------|-------------------------|
| Director's gross salary | Deductible when **paid** (accruals basis: must be paid within 9 months of period end) |
| Employer NIC (13.8% above secondary threshold) | Deductible when paid |
| Employer pension contributions | Deductible when paid (registered scheme only) |

**9-month rule:** If director's remuneration is accrued but unpaid 9 months after the period end, it is **not deductible** until the period it is actually paid.

#### Dividends

| Item | Corporation Tax Treatment |
|------|-------------------------|
| Dividends paid | **Not** a deductible expense — paid from post-tax profits |
| Dividends received | Usually **exempt** from CT (not included in taxable profits) |

**Dividend procedure:**
1. Confirm sufficient retained profits exist
2. Hold board meeting (even sole director) and minute the resolution
3. Create dividend voucher (date, company name, shareholder, amount)
4. Pay the dividend

#### Director's Loan Account (DLA)

| Scenario | Tax Consequence |
|----------|----------------|
| Director owes company money at period end | S455 tax at 33.75% on outstanding balance (reclaimable on repayment) |
| Loan repaid within 9 months of period end | No S455 charge; report on CT600A |
| Loan > £10,000 at any point | Benefit in kind; report on P11D; NIC due |
| Loan written off | Treated as earnings; PAYE/NIC due via payroll |
| Company owes director money | No CT consequence; interest paid to director is deductible (minus 20% basic rate via CT61) |

**Bed and breakfasting rule:** If a loan ≥ £5,000 is repaid and a new loan of ≥ £5,000 is taken within 30 days, the repayment is ignored — S455 still applies.

### Home Office Expenses

For a director working from home, the company can pay:

| Method | Amount | Evidence Needed |
|--------|--------|----------------|
| **HMRC flat rate** | £6/week (£26/month) — no receipts needed | None |
| **Actual costs** | Proportion of rent/mortgage interest, council tax, utilities, insurance, broadband | Calculation based on rooms used and time spent |

The actual cost method uses: `(rooms used for business / total rooms) × household costs × (hours used / total hours available)`

---

## Step 5: Complete the CT600 in Alphatax Cloud

### About Alphatax Cloud

Alphatax Cloud (from Tax Systems, now part of Thomson Reuters) is HMRC-recognised CT600 filing software. Key features:

- **Tax computation engine** — enter figures, Alphatax calculates the tax
- **iXBRL tagging** — automatically tags accounts and computation for HMRC filing
- **CT600 form generation** — populates all boxes from your input
- **Electronic filing** — submits directly to HMRC
- **Accounts preparation** — can prepare statutory accounts or import from accounting software
- **Supplementary pages** — generates CT600A (loans to participators), CT600B (controlled foreign companies), etc. as needed

### Alphatax Cloud Workflow

```
1. Create new return
   → Enter company UTR, name, accounting period dates
        ↓
2. Import or enter accounts
   → Import trial balance from accounting software (Xero, QuickBooks, Sage)
   → Or manually enter P&L and Balance Sheet figures
        ↓
3. Tax adjustments
   → Alphatax presents the accounting figures
   → Enter disallowable items (depreciation, entertainment, etc.)
   → Enter capital allowances (AIA, WDA, FYA)
   → System calculates adjusted trading profit
        ↓
4. Non-trading income
   → Bank interest, property income, other income
        ↓
5. Reliefs and deductions
   → Losses brought forward
   → Charitable donations
   → Group relief (if applicable)
        ↓
6. Director's loan account
   → If overdrawn DLA exists, complete CT600A section
   → Alphatax calculates S455 tax
        ↓
7. Review CT600
   → Alphatax populates all CT600 boxes automatically
   → Review for accuracy
        ↓
8. Generate iXBRL accounts
   → Alphatax generates iXBRL-tagged accounts and computation
   → Review the generated documents
        ↓
9. Validate and file
   → Run HMRC validation checks
   → Submit electronically
   → Receive HMRC acknowledgement (IRmark)
```

### Tips for Alphatax Cloud

- **Trial balance import** saves significant time — export from your accounting software in the format Alphatax expects
- **Mapping** — Alphatax maps trial balance codes to CT600 boxes; review the mapping carefully on first use
- **Capital allowances module** — use the built-in module to track asset pools, AIA claims, and disposals across years
- **Prior year data** — Alphatax rolls forward prior year data (pools, losses, etc.) if you used it previously
- **Validation** — always run the built-in validation before filing; it catches common errors like missing entries or inconsistent figures
- **CT600A** — if the director's loan account is overdrawn, the supplementary page is mandatory
- **Filing deadline** — Alphatax shows the filing deadline prominently; file well before to avoid last-minute issues

### CT600 Form — Key Sections and Box Reference

#### Section: Company Information

| Box | Description | What to Enter |
|-----|-------------|---------------|
| 1 | Company name | Registered name from Companies House |
| 2 | Company registration number | 8-digit number |
| 3 | Tax reference (UTR) | 10-digit Unique Taxpayer Reference |
| 4 | Type of company | Select: micro-entity, small, etc. |
| 30 | Start of accounting period | First day of the period |
| 35 | End of accounting period | Last day of the period |
| 55 | Accounts made up to | Usually same as box 35 |

#### Section: Tax Calculation (Boxes 145–245)

| Box | Description | Source |
|-----|-------------|--------|
| 145 | Trading profits | Adjusted trading profit from tax computation |
| 155 | Trading losses brought forward against trading profits | Prior year trading losses used |
| 160 | Net trading profits | Box 145 − Box 155 |
| 170 | Bank/building society interest and other investment income | Non-trading interest received |
| 172 | Annual payments not otherwise charged | Annuities, other annual payments |
| 175 | Non-exempt dividends or distributions | Rare for small companies |
| 190 | Income from property | Net property business profit |
| 200 | Non-trading gains on intangible fixed assets | Usually nil for small companies |
| 205 | Tonnage tax profits | Usually nil |
| 210 | Chargeable gains | Capital gains (after indexation/reliefs) |
| 215 | Total profits before deductions | Sum of above |
| 235 | Qualifying charitable donations | Gift Aid payments to charities |
| 245 | Total taxable profits | Box 215 − Box 235 |

#### Section: Tax Calculation (Boxes 330–440)

| Box | Description | Source |
|-----|-------------|--------|
| 330 | Corporation tax chargeable | Calculated by Alphatax from taxable profits and applicable rate |
| 335 | Marginal relief | If profits between £50,000 and £250,000 |
| 345 | Corporation tax net of marginal relief | Box 330 − Box 335 |
| 360 | Reliefs and deductions (R&D credit, etc.) | Tax credits claimed |
| 380 | Net CT payable/repayable | Final tax liability |
| 390 | S455 tax on loans to participators | From CT600A |
| 400 | S455 tax repayable | If previously charged S455 tax now repayable |
| 430 | Tax already paid | Instalments already made |
| 440 | Tax outstanding or overpaid | Balance due or refund |

#### Section: Capital Allowances (Boxes 680–700)

| Box | Description | Source |
|-----|-------------|--------|
| 680 | Capital allowances claimed | Total from capital allowances computation |
| 685 | Balancing charges | Income added back from asset disposals |
| 690 | Annual Investment Allowance | AIA claimed in the period |
| 695 | Business premises renovation | If applicable |
| 700 | Other capital allowances | WDA, FYA, full expensing |

#### Section: Losses and Deficits (Boxes 275–310)

| Box | Description | Source |
|-----|-------------|--------|
| 275 | Losses of this or later period carried back | Current losses carried to earlier periods |
| 280 | Trading losses carried forward (post-April 2017) | Trading losses for future periods |
| 285 | Non-trading deficits on loan relationships | Investment income losses |
| 295 | Losses brought forward against total profits | Previous losses used this period |
| 300 | Non-trading losses on intangible fixed assets | IP-related losses |
| 305 | Property business losses | Losses from property letting |
| 310 | Management expenses | Investment company management costs |

#### Supplementary Pages

| Page | When Required | Key Content |
|------|--------------|-------------|
| **CT600A** | Director/participator loans outstanding | Overdrawn DLA balance, S455 tax calculation |
| **CT600B** | Controlled Foreign Companies | Usually N/A for small companies |
| **CT600C** | Group and consortium relief | Group loss surrender |
| **CT600D** | Insurance companies | N/A for most |
| **CT600E** | Charities/CASCs | If company is a charity |
| **CT600I** | Supplementary charge (oil) | N/A for most |
| **CT600J** | Creative industry reliefs | Film, TV, video games tax relief |

---

## Step 6: Filing Deadlines and Penalties

### Key Deadlines

| Deadline | Timing | Action |
|----------|--------|--------|
| **Corporation tax payment** | 9 months + 1 day after accounting period end | Pay via HMRC online, BACS, or direct debit |
| **CT600 filing** | 12 months after accounting period end | File via Alphatax Cloud |
| **Companies House accounts** | 9 months after financial year end | File via Companies House (or joint filing) |
| **CT600A (if required)** | Filed as part of CT600 | Same deadline as CT600 |

**Example:** Accounting period 1 April 2025 – 31 March 2026
- Tax payment due: **1 January 2027**
- CT600 filing due: **31 March 2027**
- Companies House: **31 December 2026**

### Late Filing Penalties

| Timing | Penalty |
|--------|---------|
| 1 day late | £200 |
| 3 months late | Additional £200 |
| 6 months late | HMRC estimates the tax + 10% of unpaid tax |
| 12 months late | Additional 10% of unpaid tax |
| 3 consecutive late filings | £200 penalties increase to £1,000 each |

### Interest

HMRC charges interest on late-paid CT from the due date to the date of payment. The rate changes periodically — check the current HMRC interest rate.

---

## Step 7: Common Scenarios for Small/Micro Companies

### Scenario A: Simple Trading Company (One Director)

```
Revenue:            £120,000
Cost of sales:       £30,000
Gross profit:        £90,000
Director salary:     £12,570  (at personal allowance level)
Employer NIC:           £618
Employer pension:     £3,000
Rent & utilities:     £6,000
Insurance:            £1,200
Professional fees:    £2,000
Software:             £1,500
Travel:               £2,400
Phone & internet:       £720
Depreciation:         £3,000  ← DISALLOWABLE
Entertainment:          £500  ← DISALLOWABLE
────────────────────────────
Accounting profit:   £56,492

Tax computation:
  Accounting profit:                     £56,492
  Add back: depreciation                  £3,000
  Add back: entertainment                   £500
  Less: Capital allowances (AIA)         (£8,000) ← actual equipment purchased
  ──────────────────────────────────
  Taxable profit:                        £51,992

  CT @ 25%:                              £12,998
  Less: Marginal relief*:                  (£XXX) ← calculated by Alphatax
  ──────────────────────────────────
  CT payable:                             £X,XXX
```

*Marginal relief applies because profits are between £50,000 and £250,000.

### Scenario B: Company with Director's Loan

Director has withdrawn £25,000 beyond salary/dividends:

```
CT600:
  Normal CT liability:                    £X,XXX

CT600A:
  Overdrawn DLA at period end:           £25,000
  S455 tax @ 33.75%:                      £8,438

  → S455 is payable at the same time as the CT
  → Reclaimable when the loan is repaid (claim via CT600A or form L2P)
  → If loan exceeds £10,000: also a benefit in kind (report on P11D)
```

### Scenario C: Company Making a Loss

```
Revenue:             £40,000
Total expenses:      £55,000
Accounting loss:    (£15,000)

Tax computation:
  Accounting loss:                      (£15,000)
  Add back: depreciation                  £2,000
  Add back: disallowable items              £500
  Less: Capital allowances              (£1,000)
  ──────────────────────────────────
  Tax-adjusted loss:                   (£13,500)

  CT payable: NIL

  Options:
  1. Carry forward against future profits (indefinitely for same trade)
  2. Carry back against previous 12 months' profits (claim refund)
  3. If ceasing trade: terminal loss relief — carry back 3 years
```

**Still must file the CT600** even with no tax to pay.

### Scenario D: First Year with Capital Purchases

```
Equipment purchased:  £15,000
Van purchased:         £8,000
Car purchased (45g CO₂): £20,000

Capital allowances:
  Equipment:  £15,000 × AIA = £15,000 allowance
  Van:         £8,000 × AIA =  £8,000 allowance
  Car:        £20,000 → main pool (WDA 18% or 14%) = £3,600 or £2,800 first year
              (Cars don't qualify for AIA)

  Total CA claim: £26,600 or £25,800
  Car pool carried forward: £16,400 or £17,200
```

---

## Step 8: Pre-Filing Checklist

Before submitting via Alphatax Cloud, verify:

### Accounts
- [ ] P&L and Balance Sheet balance correctly
- [ ] Opening balances match prior year closing balances
- [ ] Corporation tax liability is shown in the Balance Sheet
- [ ] Director's loan account is correctly stated
- [ ] Share capital matches Companies House records
- [ ] Accounting period dates are correct

### Tax Computation
- [ ] All disallowable expenses identified and added back
- [ ] Depreciation fully added back
- [ ] Entertainment fully added back
- [ ] Capital allowances correctly calculated
- [ ] Any losses brought forward correctly applied
- [ ] 50% restriction applied if carried-forward losses exceed £5M allowance
- [ ] Director's remuneration is paid or payable within 9 months

### CT600 Form
- [ ] Company details correct (UTR, registration number, period)
- [ ] Trading profits match tax computation
- [ ] Capital allowances match computation
- [ ] Non-trading income correctly entered
- [ ] Losses correctly reported
- [ ] CT600A completed if director's loan is overdrawn
- [ ] Tax calculation agrees with manual check
- [ ] Alphatax validation checks all pass

### Filing
- [ ] iXBRL accounts generated and reviewed
- [ ] iXBRL computation generated and reviewed
- [ ] HMRC credentials entered in Alphatax
- [ ] Filed before deadline
- [ ] HMRC acknowledgement (IRmark) received and saved
- [ ] Companies House accounts filed separately (or joint filing)
- [ ] Tax payment arranged before payment deadline

---

## Useful HMRC References

| Resource | URL |
|----------|-----|
| Company Tax Returns guide | https://www.gov.uk/company-tax-returns |
| Corporation tax rates | https://www.gov.uk/corporation-tax-rates |
| Capital allowances | https://www.gov.uk/capital-allowances |
| Director's loans | https://www.gov.uk/directors-loans |
| Allowable business expenses | https://www.gov.uk/expenses-if-youre-self-employed |
| Annual accounts filing | https://www.gov.uk/prepare-file-annual-accounts-for-limited-company |
| Marginal relief calculator | https://www.gov.uk/guidance/corporation-tax-marginal-relief |
| Pay Corporation Tax | https://www.gov.uk/pay-corporation-tax |
| Late filing penalties | https://www.gov.uk/government/publications/co-late-filing-penalties |
| CT600 guidance notes | https://www.gov.uk/government/publications/corporation-tax-company-tax-return-ct600-2024-version-3 |
| S455 tax on director's loans | https://www.gov.uk/directors-loans/your-company-tax-return |

---

## How to Use This Skill

When helping a user file a CT600:

1. **Ask what stage they're at** — do they have accounts prepared, or are they starting from bank statements?
2. **Gather key information** — accounting period dates, company size, whether there are directors' loans, what assets were purchased
3. **Work through each step** systematically — don't jump to the CT600 boxes before the tax computation is done
4. **Help categorise transactions** — this is where most users need the most help; go through bank statements line by line if needed
5. **Calculate the tax computation** — show the full working from accounting profit to taxable profit
6. **Map to CT600 boxes** — explain which figures go in which boxes in Alphatax
7. **Flag anything unusual** — directors' loans, losses, capital items, mixed-use expenses
8. **Remind about deadlines** — payment deadline is earlier than filing deadline
9. **Caveat complex situations** — R&D claims, overseas income, group structures, share schemes should involve a professional
