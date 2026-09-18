# Milestone 2 — User Acceptance Test Reports · Four Journeys

**Feature:** Rolling Loan ("Refinance via Dano") — **open**, **view**, **refinance** and **repay** a
loan from the Danogo front end with the user's own wallet.

**What is in this file:** four complete sessions walked end-to-end on **preprod**
(https://preprod.danogo.io) on **18 September 2026**, on **three different wallet accounts — all
Eternl**, two of them separate installations running different Eternl versions, against
**Cardano preprod** and the **Fluid preprod** smart contracts. Each session is captured screen by
screen — every screen from the first click to the settled transaction, a screen recording of the
whole session, the wallet signing dialog for each signature, and the public-explorer record of each
transaction — and each is then checked, line by line, against what the chain actually recorded.
**123 QC checks in total, 123 hold.**

> **What these sessions are, and what they are not.** Journeys 1, 2 and 4 were run by the delivery
> team; journey 3 was run by a second tester on their own wallet and their own wallet installation.
> None of them is offered as evidence that the interface is intuitive to someone who has never seen
> it — that is the claim the previous submission made and the review rejected
> ([`08_CORRECTIONS.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/08_CORRECTIONS.md) C-2).
> What every row below is checkable against is the **complete click path of each journey** and the
> agreement between **what the interface promised before the signature** and **what the chain
> recorded after it**. Nothing between the first click and the settled loan is omitted, and the two
> sessions that found defects are published with the defects in them.
>
> **Why preprod.** This is interface evidence. The milestone's settlement evidence is the fifteen
> **mainnet** transactions in
> [`00`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/00_POA_SUBMISSION_FORM.md) §A,
> which anyone can verify without us. On-chain figures below come from the public explorer and, for
> journeys 3 and 4, from the public **Koios** preprod API — never from our backend.

## At a glance

| | Journey 1 | Journey 2 | Journey 3 | Journey 4 |
|---|---|---|---|---|
| **Wallet** | Eternl v2.1.7.1 · *1kang (#0)* · `addr_test1qr4rll…5g494` | the same wallet | Eternl v2.1.5.0 · *Ngan Wallet (#0)* · `addr_test1qqv2pd7…027ghs` | Eternl · *Deploy Oracle (#0)* · `addr_test1qr20zc2v…sm9xmy` |
| **Run by** | delivery team | delivery team | **a second tester** (identity not recorded — §3) | not recorded — §4 |
| **Borrowed** | 25 ADA | 11 fUSDM | 11 fUSDM | **905.004 ADA → two loans, 20 + 885** |
| **Collateral** | 100 fUSDM | 100 ADA | 100 ADA | 98,327.69 fUSDM |
| **Journeys covered** | open · view · refinance · repay | open · view · refinance · repay | open · view · refinance · repay | open · view · refinance **×2** — **no repay** |
| **Refinance** | [`0bfa25db…ccfd`](https://preprod.cardanoscan.io/transaction/0bfa25db49a676c15645c25d1d8b35acf5630d1d9cb1d472d386430cb954ccfd) | [`78d434d5…95d6`](https://preprod.cardanoscan.io/transaction/78d434d5d3028e2f8025f9ad06ad65849cee4dcbd89d6abd206334baaa2495d6) | [`a04fe52e…68d8f4`](https://preprod.cardanoscan.io/transaction/a04fe52e0545f546b71a866ed48b1a83aac271dbece74d21fb27e835d268d8f4) | [`6ab3bde0…c7ad43`](https://preprod.cardanoscan.io/transaction/6ab3bde0739c1a53aacbbd6c43aacd848b3311a8958030be412c22ffe1c7ad43) · [`3cbb01a7…9694a8`](https://preprod.cardanoscan.io/transaction/3cbb01a7b9dce89f175a75174dff109d48c231f42b2059fc8950d2485f9694a8) |
| **Repay** | [`e134b85e…681416`](https://preprod.cardanoscan.io/transaction/e134b85eb5631391089598adefd8f06024307fc7d41ec4ac8d421276d8681416) | [`bae9a9c9…c75b7d`](https://preprod.cardanoscan.io/transaction/bae9a9c9e0e4f63657d43cc21b4ab071e82d57919733ddb8908a958937c75b7d) | [`a4bebdb7…3fbeed`](https://preprod.cardanoscan.io/transaction/a4bebdb7ca185ffc1ce4fc22873e9e3d9cefa80e0d1e0d8ad698e6b1813fbeed) | — |
| **QC checks** | **31 / 31** | **33 / 33** | **22 / 22** | **37 / 37** |
| **Defects found** | — | — | **D-1** submit failed, recovered by *Retry* | — |
| **Recording** | [`01.mp4`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/01-open-and-refinance-borrow-ADA-collateral-USDM.mp4) | [`02.mp4`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/02-open-and-refinance-borrow-USDM-collateral-ADA.mp4) | [`03.mp4`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/03-open-refinance-repay-borrow-USDM-collateral-ADA.mp4) (7 min 29 s) | [`04.mp4`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/04-open-and-two-refinances-borrow-ADA-collateral-USDM.mp4) (3 min 50 s) |
| **Screenshots** | [`journey-1/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/journey-1-borrow-ADA-collateral-USDM) | [`journey-2/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/journey-2-borrow-USDM-collateral-ADA) | [`journey-3/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/journey-3-borrow-USDM-collateral-ADA) | [`journey-4/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/journey-4-borrow-ADA-collateral-USDM) |

Two open items recur across the sessions and are stated once, at the end:
[**OI-1** (the *Deposit* / *Fee* preview line)](#open-items-common-to-more-than-one-session) and
[**OI-2** (which token the refinance burns)](#open-items-common-to-more-than-one-session).

---

## Journey 1 — borrow 25 ADA against 100 fUSDM

Borrow **25 ADA** from Fluid against **100 fUSDM** collateral, refinance it into a Dano Finance loan,
then **repay that Dano loan in full and unlock the collateral** — *open → view → refinance → view →
repay*, all four approved journeys in one thread on one loan.
**Timing:** open 04:51:28 and refinance 04:54:27 (wallet clock); the repay settled later the same
day, in block 5,190,592 at 2:29:37 PM (explorer clock).

### 1.1 The session, step by step

Thirteen screens, in the order the user sees them. *Open* is steps 1–3, *View* is step 4,
*Refinance* is steps 5–6, *View* again is steps 9–10 on the loan the refinance created, and *Repay*
is steps 11–13, on that same loan.

| # | Step | Expected | Actual result | Screenshot |
|---|---|---|---|---|
| 1 | **Borrow Market** — choose fUSDM as collateral, pick a pool, enter the amounts | pools listed with their rates; a *Loan Impact* preview before anything is signed | collateral **fUSDM**, balance 128.29; Fluid Flexible **4.00%** net cost (67% LTV) selected; **You Borrow 25 ADA** ($7.62), **Collateral 100 fUSDM** ($100.00); *Loan Impact* — Current Borrow APR **4.00%**, incremental monthly interest 2.04%/month, New Collateral $100.00, **New Health Factor 7.20 Healthy**, Deposit 5 ADA, Fee 5 ADA ($1.52) | [`01`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/01-borrow-market-create-loan.png) |
| 2 | **Create Loan** → sign in Eternl | one transaction, memo *"Dano Finance: Borrow from Fluid"*, collateral leaving, position token minted | Eternl: *"You have 1 transaction(s) to confirm"*, tags **Sent Tokens · Contract · Mint · Memo**, memo **Dano Finance: Borrow from Fluid**, **fUSDM −100,000,000** (100 fUSDM), token `649a4b44…eade68` **+1**, wallet net **+t₳20.71** | [`02`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/02-eternl-sign-open-from-fluid.png) |
| 3 | **Portfolio** after the open | the new Fluid position appears | Total value **$1,344.78**; **Fluid · Borrow −25 (−$7.62)**; **Fluid · Supply 100 fUSDM ($100.00)** | [`03`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/03-portfolio-after-open.png) |
| 4 | **My Account → Loans** *(the View journey)* | one row per open loan, with debt, collateral, APR and health factor | *ADA · Loans*: **25 ADA ($7.6)**, collateral $0.1K (fUSDM), **APR 4.00%**, badge **"Dano save 3.32% net cost"**, **Health Factor 7.27 · Healthy**, action **Manage** | [`04`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/04-my-account-loans.png) |
| 5 | **Loan Details → Refinance via Dano** | the quote, in full, before signing | Loan Details (Fluid): Total Debt **$7.62 / 25 ADA**, HF **7.27 Healthy**, APR **4.00%**, collateral **fUSDM 100 ($100.00)**. Card: **Save 3.32% net cost**; Net cost **4.00% → 0.68%**; Health Factor **7.27 Healthy → 1.21 Fair**; ✓ *Same loan, Same collateral*; *"You don't need extra funds to close your Fluid loan."*; **Fee 2 ADA** | [`05`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/05-loan-details-refinance-quote.png) |
| 6 | **Confirm Refinance via Dano** → sign in Eternl | **one** transaction, memo *"Dano Finance: Create Loan"* | Eternl: 1 transaction, tags **Sent TADA · Contract · Mint · Burn · Memo**, memo **Dano Finance: Create Loan**, **Transaction ID `0bfa25db…954ccfd` visible before signing**, network fee **t₳1.56**, wallet net **−t₳4.31**; 3 inputs, one of them the Dano pool `addr_test1wpuxsu23…5z3psh` **−t₳9,313.02**, *Float ADA Market Token* −1, dADA −12,693,629,080 | [`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/06-eternl-sign-refinance.png) |
| 7 | **Eternl → Transactions** — the wallet's own record | both transactions, with memos, block and confirmations | 09/18/2026 **04:51:28** *Dano Finance: Borrow from Fluid* (+t₳20.71) and **04:54:27** *Dano Finance: Create Loan* (−t₳4.31), **block 5,189,867**, **180 confirmations**, fee t₳1.56 | [`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/07-eternl-transaction-list.png) |
| 8 | **Cardanoscan** — the refinance as a third party sees it | the Fluid loan UTxO consumed, the Dano loan UTxO created, the collateral carried | **3 inputs → 6 outputs**, 7 contracts, 3 mints & burns, 1 metadata entry, 3 withdrawals, 14 reference inputs. Input **#0** `addr_test1zpyg4esu…n47ypv` carries **fUSDM 100,000,000**; output **#1 +25.000003 ₳** to `addr_test1qr9ew225…s2n6em`; output **#3 +5.0 ₳ + fUSDM 100,000,000** to `addr_test1zzx7xnch…56t24n`; output **#4 +2.0 ₳** to the fee address | [`08`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/08-cardanoscan-refinance-utxos.png) |
| 9 | **Loan Details, reopened** *(View)* | the loan is now a Dano loan, same collateral | **Dano Finance** · Total Debt **$8.23 / 27 ADA** · HF **1.21 Fair** · APR **3.21%** · Collateral Backing $100.00 · utilisation 8% · collateral **fUSDM 100** · actions *Repay Loan · Modify Collateral · Increase Loan* | [`09`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/09-loan-details-after-refinance.png) |
| 10 | **Portfolio** | the Fluid borrow row is replaced by a Dano borrow row | Total value **$1,338.87**; **Dano · Borrow −27 (−$8.23)**; **no Fluid · Borrow row** | [`10`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/10-portfolio-dano-borrow-27-ADA.png) |
| 11 | **Loan Details → Repay Loan** *(the Repay journey)* | the amount pre-fills to the full debt; the preview says what repaying will do | *Repay Loan · Dano Finance* — *"Fully repay your loan to unlock all collateral… Minimum amount to repay is 21 ADA."* **You Repay 27.000474 ADA** (\$8.23), available 1,041.83; collateral **fUSDM 100** (\$100.00); *Loan Impact* — Current Debt **27 ADA** → New Debt **↘ Repaid**, collateral **No change**, health factor **1.21 Fair → --** | [`11`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/11-repay-quote-dano-27-ADA.png) |
| 12 | **Confirm** → sign in Eternl | one transaction, memo *"Dano Finance: Repay Loan"*, the debt leaving and the collateral returning | Eternl: 1 transaction, tags **Sent TADA · Sent Tokens · Contract · Burn · Memo**, memo **Dano Finance: Repay Loan**, wallet net **−t₳23.25**, token `18a7d7a6…4ddd195e` **−1** — the same token this wallet received from the refinance at step 6 — and **fUSDM +100,000,000** returning | [`12`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/12-eternl-sign-repay.png) |
| 13 | **Cardanoscan** — the repayment as a third party sees it | the loan UTxO consumed, the collateral released, the debt paid | tx `e134b85e…681416`, **block 5,190,592**, epoch 314 / slot 26,977, **4 in → 3 out**, fee **1.252505 ₳**, confirmed within 8 secs, **2 mints & burns**, metadata **label 674 · CIP-20** `{"msg": ["Dano Finance: Repay Loan"]}`. The loan contract `addr_test1zzx7xn…56t24n` sends **−5.0 ₳**, **fUSDM −100,000,000** and its loan token `144020f…966` **−1**; the borrower receives **fUSDM +100,000,000**; the Dano pool receives **+25.581001 ₳** and the fee address **+1.419472 ₳** | [`13`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/13-cardanoscan-repay-metadata.png) |

A Fluid loan opened at 04:51:28 was refinanced into a Dano Finance loan at 04:54:27 in **one signed
transaction** — the Fluid debt settled, a new Dano loan opened, and the 100 fUSDM collateral moved
across without ever returning to the wallet. Later the same day that Dano loan was **repaid in
full**: the loan UTxO the refinance had created was consumed, its loan token burned, and the 100
fUSDM collateral released back to the borrower. **One loan, followed from origination to
settlement, through all four journeys.**

### 1.2 Verification (QC) — what was quoted against what settled

Checks UAT1-TC-01 … TC-25 cover the refinance `0bfa25db…954ccfd`; TC-26 … TC-31 cover the repayment
`e134b85e…681416` that closed the same loan. The on-chain column is Cardanoscan's own rendering of
the transaction, not our backend.

**Preview values (UI correctness)**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT1-TC-01 | Savings label = net-cost delta | 4.00% − 0.68% = **3.32%** → "Save 3.32% net cost" | "Dano save 3.32% net cost" on the row; "Save 3.32% net cost" on the card | `04`, `05` | ✅ |
| UAT1-TC-02 | Net cost before → after | source 4.00% → Dano 0.68% | 4.00% → 0.68% | `05` | ✅ |
| UAT1-TC-03 | Health factor before → after | 7.27 Healthy → 1.21 Fair | 7.27 Healthy → 1.21 Fair | `05` | ✅ |
| UAT1-TC-04 | Origination fee shown | 2 ADA | Fee 2 ADA | `05` | ✅ |
| UAT1-TC-05 | "Same loan, Same collateral" and the no-extra-funds note | both shown | both shown | `05` | ✅ |

**Signing & submission**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT1-TC-06 | Confirm builds **one** transaction and opens the wallet | 1 transaction, memo "Dano Finance: Create Loan" | Eternl shows 1 transaction, memo *Dano Finance: Create Loan*, tagged Mint **and** Burn | `06` | ✅ |
| UAT1-TC-07 | The transaction id is visible **before** signing and is the one that settled | id shown in the dialog = id in the wallet history = id on the explorer | `0bfa25db…954ccfd` in all three | `06`, `07`, `08` | ✅ |
| UAT1-TC-08 | The hash resolves on a public explorer | preprod Cardanoscan shows the transaction | present, 3 in → 6 out | `08` | ✅ |
| UAT1-TC-09 | Included in a block and confirmed | one block, confirmations accruing | **block 5,189,867**, 180 confirmations at capture | `07` | ✅ |

**Inputs consumed (source side)**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT1-TC-10 | The Fluid loan UTxO is consumed with its collateral | input spends fUSDM 100.000000 from the Fluid loan address | input **#0** `addr_test1zpyg4esu…n47ypv`, **fUSDM 100,000,000**, −2.25413 ₳ | `08` | ✅ |
| UAT1-TC-11 | The Dano pool UTxO is consumed | pool input spends its market token and dADA | `addr_test1wpuxsu23…5z3psh` −9,313.024336 ₳, *Float ADA Market Token* −1, dADA −12,693,629,080 | `06`, `08` | ✅ |
| UAT1-TC-12 | The Fluid debt is **not** paid from the borrower's own funds | the settlement output is funded by the pool | pool net **−27.000017 ₳** (9,313.024336 − 9,286.024319) covers the 25.000003 ₳ settlement and the 2.000000 ₳ fee; the borrower's own net outlay is **t₳4.31** = network fee 1.55990 + the 2.74587 ₳ that tops the loan UTxO from 2.25413 to 5.00000 | `06`, `08` | ✅ |

**Outputs created (target side)**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT1-TC-13 | A new Dano loan UTxO is created | output at the Dano loan contract holds ADA + the collateral + a loan token | output **#3** `addr_test1zzx7xnch…56t24n`: **+5.0 ₳**, **fUSDM 100,000,000**, token `144020f…966` +1 | `08` | ✅ |
| UAT1-TC-14 | The Fluid debt is settled in the same transaction | one output pays the lender the full debt | output **#1 +25.000003 ₳** to `addr_test1qr9ew225…s2n6em` | `08` | ✅ |
| UAT1-TC-15 | The origination fee is a separate leg | one output of exactly 2 ADA to the fee address | output **#4 +2.0 ₳** | `08` | ✅ |

**Conservation & "the number shown is the number that settled"**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT1-TC-16 | **Collateral continuity** | fUSDM in = fUSDM out to the Dano loan; not returned to the borrower | 100,000,000 in (#0) = 100,000,000 out (#3) | `08` | ✅ |
| UAT1-TC-17 | **Atomicity** | one transaction, one block, inputs balance outputs | Σ in 10,483.165924 ₳ = Σ out 10,481.60602 ₳ + fee; block 5,189,867 | `08` | ✅ |
| UAT1-TC-18 | Quoted fee = fee paid | 2 ADA quoted → 2.000000 ₳ on chain | 2 ADA (`05`) = +2.0 ₳ (`08` #4) | `05`, `08` | ✅ |
| UAT1-TC-19 | Quoted health factor = health factor after | 1.21 Fair → 1.21 Fair | 1.21 Fair quoted, 1.21 Fair on the settled loan | `05`, `09` | ✅ |
| UAT1-TC-20 | Resulting debt = debt + fee | 25 + 2 = **27 ADA** | 27 ADA in Loan Details and in Portfolio | `09`, `10` | ✅ |
| UAT1-TC-21 | The pool disbursed exactly what the loan is for | disbursement = settlement + fee | **27.000017 ₳** disbursed = 25.000003 + 2.000000 + 0.000014 dust to the borrower | `08` | ✅ |
| UAT1-TC-22 | Network fee is separate from the origination fee | ~1.56 ₳ network vs 2 ADA origination | t₳1.55990 network fee, 2.0 ₳ origination | `07`, `08` | ✅ |

**Post-state**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT1-TC-23 | The Fluid position is settled | no Fluid borrow row remains | *Fluid · Borrow* absent after the refinance | `03` → `10` | ✅ |
| UAT1-TC-24 | A Dano position stands in its place | one Dano borrow row, −27 ADA | **Dano · Borrow −27 (−$8.23)** | `10` | ✅ |
| UAT1-TC-25 | The APR moves to the rate the quote named | the ADA pool's 3.21% | Loan Details after: **APR 3.21%**, the same rate the Borrow Market listed for that pool | `01`, `09` | ✅ |

**The repayment that closed the loan**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT1-TC-26 | Quoted repay amount = amount actually paid | 27.000474 ADA quoted → the same, split between pool and fee address | pool **+25.581001 ₳** + fee address **+1.419472 ₳** = **27.000473 ₳**, one lovelace of rounding from the quote | `11`, `13` | ✅ |
| UAT1-TC-27 | Full repayment releases **all** the collateral | fUSDM 100.000000 returns to the borrower | loan contract sends **fUSDM −100,000,000**; borrower receives **fUSDM +100,000,000** | `12`, `13` | ✅ |
| UAT1-TC-28 | The UTxO consumed is the one the refinance created | same address, same loan token as output #3 of `0bfa25db…` | `addr_test1zzx7xn…56t24n` with token `144020f…966`, both created by the refinance | `08`, `13` | ✅ |
| UAT1-TC-29 | The borrower's title to the loan is burned | the token the refinance paid to the wallet is destroyed | Eternl shows `18a7d7a6…4ddd195e` **−1**, the same token it showed **+1** at step 6; Cardanoscan reports **2 mints & burns** | `06`, `12`, `13` | ✅ |
| UAT1-TC-30 | The borrower's net outlay reconciles | debt paid + network fee − min-UTxO returned | **−23.252978 ₳** = 27.000473 paid + 1.252505 fee − 5.000000 returned from the loan UTxO | `12`, `13` | ✅ |
| UAT1-TC-31 | Built by this application | metadata label 674 naming the action | label **674 · CIP-20** — `{"msg": ["Dano Finance: Repay Loan"]}` | `13` | ✅ |

**31 checks, 31 hold** — 25 on the refinance, 6 on the repayment.

### 1.3 Coverage, stability and findings

| Journey | Covered here | How |
|---|---|---|
| **Open** | ✅ end to end | steps 1–3: amounts entered, wallet dialog, resulting position |
| **View** | ✅ end to end | step 4 and steps 9–10: the loan list and the loan details, before and after |
| **Refinance** | ✅ end to end | steps 5–10, with the transaction on a public explorer |
| **Repay** | ✅ end to end, on the loan this session created | steps 11–13: the *Repay Loan* quote, the wallet dialog burning the loan token, and the settled transaction on a public explorer |

No crash, white screen or unrecoverable state; every value the interface displayed matched the
chain; all three signatures signed, submitted and confirmed. Whether an independent user finds the
interface intuitive is **not tested here** — see the note at the top of this file.

*Not captured:* no screenshot of the portfolio **after** the repayment, so this session does not show
the app's post-repay state; what it shows is the quote, the signature, and the settled transaction.
*Also in the wallet's list at step 7,* three earlier *"Dano Finance: Repay Loan"* transactions from
the same wallet — 04:47:17 (+t₳2.58), 04:48:21 (+t₳97.59) and 04:48:50 (+t₳2.58, **Borrower NFT −1**,
fUSDM −12,354,268) — which are **not** part of this walkthrough and are noted so the recording is not
misread.

| ID | What |
|---|---|
| **OI-1** | the *Deposit 5 ADA* / *Fee 5 ADA ($1.52)* line in the Borrow Market preview is not reconciled — see [Open items](#open-items-common-to-more-than-one-session) |
| **F-1** *(minor)* | The Borrow Market quoted a *New Health Factor* of **7.20**; the loan list showed **7.27** three minutes later. Consistent with price and interest movement between the two screens; not investigated further. |

---

## Journey 2 — borrow 11 fUSDM against 100 ADA

Journey 1's asset direction reversed, run on the same day from the same wallet: borrow **11 fUSDM**
from Fluid against **100 ADA** collateral, refinance into Dano, then **repay in full and unlock the
collateral**.
**Timing:** open 04:59:12 and refinance 05:01:04 (wallet clock; the explorer renders that settlement
at 10:01:04 AM in its own timezone); the repay settled later the same day, in block 5,190,611 at
2:39:18 PM (explorer clock).

### 2.1 The session, step by step

*Open* is steps 1–3, *View* is step 4, *Refinance* is steps 5–6, *View* again is steps 7 and 10, and
*Repay* is steps 11–13, on the loan the refinance created.

| # | Step | Expected | Actual result | Screenshot |
|---|---|---|---|---|
| 1 | **Borrow Market** — choose ADA as collateral, pick a pool, enter the amounts | pools listed with their rates; a *Loan Impact* preview before anything is signed | collateral **ADA**, balance 1,163.58; **fUSDM** market, Fluid Flexible **4.00%** (67% LTV) selected; **You Borrow 11 fUSDM** ($11.00), **Collateral 100 ADA** ($30.50); *Loan Impact* — Current Borrow APR **4.00%**, 2.04%/month, New Collateral $30.50, **New Health Factor 3.63 Healthy**, Deposit 5 ADA, Fee 5 fUSDM ($5.00) | [`01`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/01-borrow-market-create-loan.png) |
| 2 | **Create Loan** → sign in Eternl | one transaction, memo *"Dano Finance: Borrow from Fluid"*, collateral leaving, position token minted | Eternl: 1 transaction, tags **Sent TADA · Contract · Mint · Memo**, memo **Dano Finance: Borrow from Fluid**, wallet net **−t₳102.05**, token `2d3883b3…70b67d8a` **+1**, **fUSDM +11,000,000** (the full 11 fUSDM borrowed); the app behind the dialog shows *"Waiting for wallet signature…"* | [`02`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/02-eternl-sign-open-from-fluid.png) |
| 3 | **Portfolio** after the open | the new Fluid position appears | Total value **$1,342.30**; **Fluid · Supply 110 ADA ($33.55)** — the 100 just locked plus 10 already supplied; Journey 1's **Dano · Borrow −27** still listed | [`03`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/03-portfolio-after-open.png) |
| 4 | **My Account → Loans** *(the View journey)* | one row per open loan | *fUSDM · Loans*, two rows: **11 fUSDM ($11.0)**, collateral $30.5 (ADA), APR **4.00%**, badge **"Dano save 9.86% net cost"**, **HF 3.63 · Healthy** — and a second, unrelated **2 fUSDM** loan, HF 1.99, which this session does not touch | [`04`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/04-my-account-loans.png) |
| 5 | **Loan Details → Refinance via Dano** | the quote, in full, before signing | Loan Details (Fluid): Total Debt **$11.00 / 11 fUSDM**, HF **3.63 Healthy**, APR **4.00%**, collateral **ADA 100 ($30.50)**. Card: **Save 9.86% net cost**; Net cost **4.00% → −5.86%**; Health Factor **3.63 Healthy → 1.87 Healthy**; ✓ *Same loan, Same collateral*; *"You don't need extra funds to close your Fluid loan."*; **Fee 2 fUSDM** | [`05`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/05-loan-details-refinance-quote.png) |
| 6 | **Confirm Refinance via Dano** → sign in Eternl | **one** transaction, memo *"Dano Finance: Create Loan"* | Eternl: 1 transaction, tags **Sent TADA · Contract · Mint · Burn · Memo**, memo **Dano Finance: Create Loan**; inputs include the Fluid loan script `addr_test1zpyg4esucs…n47ypv` at **−t₳100** carrying token `2d3883b3…` −1; **6 outputs**, among them **+t₳1.96 and fUSDM +11,000,001** to `addr_test1qr9ew225…s2n6em` | [`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/06-eternl-sign-refinance.png) |
| 7 | **Loan Details, reopened** *(View)* | the loan is now a Dano loan, same collateral | **Dano Finance** · Total Debt **$13.00 / 13 fUSDM** · HF **1.87 Healthy** · APR **3.07%** · Collateral Backing $30.50 · utilisation 3% · collateral **ADA 100** · actions *Repay Loan · Modify Collateral · Increase Loan* | [`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/07-loan-details-after-refinance.png) |
| 8 | **Eternl → Transactions** — the wallet's own record | both transactions, with memos and amounts | 09/18/2026 **04:59:12** *Dano Finance: Borrow from Fluid* (−t₳102.05) and **05:01:04** *Dano Finance: Create Loan* (−t₳4.7, token `7b8a46d8…` +1, fUSDM +6 = 0.000006 fUSDM of change); Journey 1's two transactions are in the same list | [`08`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/08-eternl-transaction-list.png) |
| 9 | **Cardanoscan** — the refinance as a third party sees it | the collateral moved, the debt settled, the fee paid | **3 inputs → 6 outputs**, 7 contracts, **3 mints & burns**, 1 metadata entry, 3 withdrawals, 14 reference inputs. **ADA Sent −100.0** from the Fluid side, **ADA Received 100.0** on the Dano side; **Tokens Sent `8dcf151…cf6` −1**, **Tokens Received `904770e…4c5` +1**; the pool sends **fUSDM −13,000,007**; **fUSDM +11,000,001** to the lender and **fUSDM +2,000,000** to the fee address; borrower net **−4.70041 ₳**, of which fee **1.58428 ₳**. Block **5,189,885**, index 3 of 3, epoch 314 / slot 10,864, 5.5 KB (34.4% of the limit), ex-units 3.7M mem · 1.2B steps (21.35% / 12.37% of budget), 1 signatory | [`09`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/09-cardanoscan-refinance-overview.png) |
| 10 | **Portfolio** | the refinanced Fluid loan is replaced by a Dano loan | Total value **$1,338.87**; **fUSDM · Dano Borrow −13 (−$13.00)**; **fUSDM · Dano Supply 100** (Journey 1's collateral); the only remaining **Fluid · Borrow is −2**, the untouched second loan | [`10`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/10-portfolio-after-both-refinances.png) |
| 11 | **Loan Details → Repay Loan** *(the Repay journey)* | the amount pre-fills to the full debt; the preview says what repaying will do | *Repay Loan · Dano Finance* — *"Fully repay your loan to unlock all collateral… Minimum amount to repay is 10.001 fUSDM."* **You Repay 13.000218 fUSDM** ($13.00), available 139.29; collateral **ADA 100** ($30.50); *Loan Impact* — Current Debt **13 fUSDM** → New Debt **↘ Repaid**, collateral **No change**, health factor **1.87 Healthy → --** | [`11`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/11-repay-quote-dano-13-fUSDM.png) |
| 12 | **Confirm** → sign in Eternl | one transaction, memo *"Dano Finance: Repay Loan"*, the debt leaving and the collateral returning | Eternl: 1 transaction, tags **Sent Tokens · Contract · Burn · Memo**, memo **Dano Finance: Repay Loan**, wallet net **+t₳97.58**, **fUSDM −13,000,218** paid from the wallet, token `7b8a46d8…5d469b70` **−1** — the same token this wallet received from the refinance at step 8 | [`12`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/12-eternl-sign-repay.png) |
| 13 | **Cardanoscan** — the repayment as a third party sees it | the loan UTxO consumed, the collateral released, the debt paid | tx `bae9a9c9…5b7d`, **block 5,190,611**, epoch 314 / slot 27,558, **3 in → 3 out**, fee **1.271961 ₳**, confirmed within 54 secs, **2 mints & burns**, metadata **label 674 · CIP-20** `{"msg": ["Dano Finance: Repay Loan"]}`. The loan contract `addr_test1zzx7xn…56t24n` sends **−100.0 ₳** and its loan token `904770e…4c5` **−1**; the pool receives **fUSDM +12,999,665** and the fee address **fUSDM +553**; the borrower pays **fUSDM −13,000,218**, burns `3e8aa6c…74d` **−1** and receives **+97.577269 ₳** | [`13`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/13-cardanoscan-repay-metadata.png) |

One loan, followed from origination to settlement: opened on Fluid at 04:59:12, refinanced into Dano
at 05:01:04, and repaid in full later the same day — the loan UTxO the refinance created consumed,
its loan token burned, and all 100 ADA of collateral released.

### 2.2 Verification (QC) — what was quoted against what settled

Checks UAT2-TC-01 … TC-27 cover the refinance `78d434d5…95d6`; TC-28 … TC-33 cover the repayment
`bae9a9c9…5b7d` that closed the same loan.

**Preview values (UI correctness)**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT2-TC-01 | Savings label = net-cost delta | 4.00% − (−5.86%) = **9.86%** → "Save 9.86% net cost" | "Dano save 9.86% net cost" on the row; "Save 9.86% net cost" on the card | `04`, `05` | ✅ |
| UAT2-TC-02 | Net cost before → after | 4.00% → −5.86% (the Dano position earns more than it costs) | 4.00% → −5.86% | `05` | ✅ |
| UAT2-TC-03 | Health factor before → after | 3.63 Healthy → 1.87 Healthy | 3.63 Healthy → 1.87 Healthy | `05` | ✅ |
| UAT2-TC-04 | Origination fee shown, in the borrowed asset | 2 fUSDM | Fee 2 fUSDM | `05` | ✅ |
| UAT2-TC-05 | "Same loan, Same collateral" and the no-extra-funds note | both shown | both shown | `05` | ✅ |

**Signing & submission**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT2-TC-06 | Confirm builds **one** transaction and opens the wallet | 1 transaction, memo "Dano Finance: Create Loan" | Eternl shows 1 transaction, memo *Dano Finance: Create Loan*, tagged Mint **and** Burn | `06` | ✅ |
| UAT2-TC-07 | The transaction in the wallet history is the one on the explorer | same memo, same amounts, same moment | wallet: 05:01:04 *Create Loan* −t₳4.7 · explorer: `78d434d5…95d6`, borrower net −4.70041 ₳ | `08`, `09` | ✅ |
| UAT2-TC-08 | Included in a block, accepted by the validators | one block, script budget within limits | **block 5,189,885**, index 3 of 3, ex-units 21.35% / 12.37% of budget | `09` | ✅ |
| UAT2-TC-09 | Built by this application | Danogo metadata present | 1 metadata entry, memo *Dano Finance: Create Loan* | `06`, `09` | ✅ |

**Inputs consumed (source side)**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT2-TC-10 | The Fluid loan UTxO is consumed with its collateral | 100 ADA leaves the Fluid script | Plutus V3 input `addr_test1zpyg4esucs…n47ypv` **−t₳100**; **ADA Sent −100.0** | `06`, `09` | ✅ |
| UAT2-TC-11 | The Dano pool funds the transaction | the pool disburses the borrowed asset | `addr_test1wpuxsu…5z3psh` sends **fUSDM −13,000,007** | `09` | ✅ |
| UAT2-TC-12 | The Fluid debt is **not** paid from the borrower's own funds | the settlement is funded by the pool | the borrower's fUSDM balance **increases** by 0.000006 in this transaction; their only ADA outlay is **4.70041 ₳**, of which **1.58428 ₳** is the network fee, and no leg of the transaction pays Dano in ADA | `08`, `09` | ✅ |

**Outputs created (target side)**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT2-TC-13 | A new Dano loan UTxO is created holding the collateral | 100 ADA arrives at the Dano side with a new loan token | **ADA Received 100.0**, **Tokens Received `904770e…4c5` +1** | `09` | ✅ |
| UAT2-TC-14 | The Fluid debt is settled in the same transaction | the lender receives the full debt | **fUSDM +11,000,001** to `addr_test1qr9ew225…s2n6em` | `06`, `09` | ✅ |
| UAT2-TC-15 | The origination fee is a separate leg, in the borrowed asset | exactly 2.000000 fUSDM to the fee address | **fUSDM +2,000,000** | `09` | ✅ |
| UAT2-TC-16 | The source position token leaves the Fluid script and a new one is minted | one token out, one token in | **`8dcf151…cf6` −1** out, **`904770e…4c5` +1** in, **3 mints & burns** in the transaction | `09` | ✅ |

**Conservation & "the number shown is the number that settled"**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT2-TC-17 | **Collateral continuity** | ADA in = ADA out to the Dano loan; not returned to the borrower | 100.0 ₳ out of Fluid = 100.0 ₳ into Dano | `09` | ✅ |
| UAT2-TC-18 | **Atomicity** | one transaction, one block, 3 in → 6 out | 3 → 6, block 5,189,885, total output 1,164.948262 ₳ | `09` | ✅ |
| UAT2-TC-19 | The pool disbursed exactly what the loan is for | disbursement = settlement + fee (+ dust) | **13.000007 = 11.000001 + 2.000000 + 0.000006** fUSDM | `09` | ✅ |
| UAT2-TC-20 | Quoted fee = fee paid | 2 fUSDM quoted → 2.000000 fUSDM on chain | 2 fUSDM (`05`) = +2,000,000 (`09`) | `05`, `09` | ✅ |
| UAT2-TC-21 | Quoted health factor = health factor after | 1.87 Healthy → 1.87 Healthy | 1.87 Healthy quoted, 1.87 Healthy on the settled loan | `05`, `07` | ✅ |
| UAT2-TC-22 | Resulting debt = debt + fee | 11 + 2 = **13 fUSDM** | 13 fUSDM in Loan Details and in Portfolio | `07`, `10` | ✅ |
| UAT2-TC-23 | Network fee separate from the origination fee | ADA network fee vs fUSDM origination fee | 1.58428 ₳ network (281 lovelace/byte) vs 2.000000 fUSDM origination | `09` | ✅ |

**Post-state**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT2-TC-24 | The refinanced Fluid position is settled | it no longer appears as a Fluid borrow | after: **fUSDM · Dano Borrow −13**; no Fluid borrow of 11 remains | `10` | ✅ |
| UAT2-TC-25 | Only the selected loan is touched | the second, unrelated 2 fUSDM Fluid loan is untouched | it is still listed, still **Fluid · Borrow −2** | `04`, `10` | ✅ |
| UAT2-TC-26 | The APR moves to the rate the quote named | the fUSDM pool's 3.07% | Loan Details after: **APR 3.07%**, the rate the Borrow Market listed for that pool | `01`, `07` | ✅ |
| UAT2-TC-27 | The collateral is the same collateral | 100 ADA, unchanged, never returned in between | 100 ADA on the Dano loan; no ADA returned to the wallet in the transaction | `07`, `09` | ✅ |

**The repayment that closed the loan**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT2-TC-28 | Quoted repay amount = amount actually paid | 13.000218 fUSDM quoted → the same, split between pool and fee address | wallet pays **fUSDM −13,000,218**; pool **+12,999,665** + fee address **+553** = **13,000,218** exactly | `11`, `12`, `13` | ✅ |
| UAT2-TC-29 | Full repayment releases **all** the collateral | 100 ADA returns to the borrower, less the transaction's own costs | loan contract sends **−100.0 ₳**; borrower receives **+97.577269 ₳** = 100 − 1.271961 network fee − 1.150770 to the fee address | `13` | ✅ |
| UAT2-TC-30 | The UTxO consumed is the one the refinance created | same address, same loan token the refinance minted | `addr_test1zzx7xn…56t24n` with token **`904770e…4c5`**, the token that arrived there in `78d434d5…` | `09`, `13` | ✅ |
| UAT2-TC-31 | The borrower's title to the loan is burned | the token the refinance paid to the wallet is destroyed | Eternl shows **`7b8a46d8…5d469b70` −1**, the token it showed +1 at step 8; Cardanoscan shows the borrower sending **`3e8aa6c…74d` −1**, the token they received in the refinance; **2 mints & burns** | `08`, `09`, `12`, `13` | ✅ |
| UAT2-TC-32 | Built by this application | metadata label 674 naming the action | label **674 · CIP-20** — `{"msg": ["Dano Finance: Repay Loan"]}` | `13` | ✅ |
| UAT2-TC-33 | Accepted and settled | one block, inputs balance outputs | **block 5,190,611**, 3 in → 3 out, total output 1,137.307193 ₳, confirmed within 54 secs | `13` | ✅ |

**33 checks, 33 hold** — 27 on the refinance, 6 on the repayment.

### 2.3 Coverage, stability and findings

| Journey | Covered here | How |
|---|---|---|
| **Open** | ✅ end to end | steps 1–3 |
| **View** | ✅ end to end | step 4 (two loans listed) and steps 7, 10 |
| **Refinance** | ✅ end to end | steps 5–10, with the transaction on a public explorer |
| **Repay** | ✅ end to end, on the loan this session created | steps 11–13: the *Repay Loan* quote, the wallet dialog burning the loan token, and the settled transaction on a public explorer |

No crash, white screen or unrecoverable state; every value the interface displayed matched the
chain; all three signatures signed, submitted and confirmed.

*Not captured:* no screenshot of the portfolio **after** the repayment.

| ID | What |
|---|---|
| **OI-1** | the *Deposit 5 ADA* / *Fee 5 fUSDM ($5.00)* line in the Borrow Market preview is not reconciled — see [Open items](#open-items-common-to-more-than-one-session). Here the wallet received **fUSDM +11,000,000**, the full 11 fUSDM borrowed with no fee deducted, and the debt afterwards was **11 fUSDM**, not 16 |
| **OI-2** | which token the refinance burns is not established by this session's captures — see [Open items](#open-items-common-to-more-than-one-session) |

---

## Journey 3 — a second tester, on a second wallet

The same four journeys as journeys 1 and 2 — borrow **11 fUSDM** from Fluid against **100 ADA**
collateral, refinance into Dano, repay in full and unlock the collateral — **run by a second tester
on a different wallet and a different Eternl installation**.
**Wallet:** Eternl v2.1.5.0, account *Ngan Wallet (#0)*, address
`addr_test1qqv2pd75qaeye7w8fcddtwd8uk8wmxgr979wx4zcjknswfjpq4xedf86fkzd7ln9escmvdd4s5enl7ma50q4tk3dn8ys027ghs`.
**Session:** one continuous recording of **7 min 29 s**, 08:22:02 → 08:29:31 UTC.

**Tester:** a second person, who did not run journeys 1 and 2. **Their name, their relation to the
delivery team, the instructions they were given and the questions they asked during the session were
not recorded**, and this package does not reconstruct them after the fact.

> That omission is stated rather than worked around, because a usability claim depends on exactly
> those facts. Without them this session establishes that the four journeys can be completed by a
> second person on a second wallet and a second Eternl installation, and it establishes the defect in
> [§3.3](#33-coverage-stability-and-findings). **No claim about how intuitive the interface is is
> drawn from it**, here or anywhere else in the package.

### 3.1 The session, step by step

Timestamps are positions in the recording. *Open* is steps 1–3, *View* is steps 3–4, *Refinance* is
steps 5–8, and *Repay* is steps 9–13.

| # | Time | Step | Expected | Actual result | Screenshot |
|---|---|---|---|---|---|
| 1 | 0:24 | **Borrow Market** — ADA as collateral, Fluid pool, amounts entered | a *Loan Impact* preview before anything is signed | Fluid Flexible **4.00%** (67% LTV) selected; **You Borrow 11 fUSDM** ($11.00), **Collateral 100 ADA** ($30.50, of 1,119.47 available); *Loan Impact* — APR **4.00%**, 2.04%/month, New Collateral $30.50, **New Health Factor 4.00 Healthy**, Deposit 5 ADA, Fee 5 fUSDM ($5.00) | [`01`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/01-borrow-market-11-fUSDM-100-ADA.png) |
| 2 | 0:36 | **Create Loan** → sign in Eternl | one transaction, memo *"Dano Finance: Borrow from Fluid"* | Eternl: 1 transaction to confirm, memo **Dano Finance: Borrow from Fluid**; settled as `69e04600…3b84b8`, block **5,190,728**, 08:23:32 UTC | [`02`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/02-eternl-sign-open.png) |
| 3 | 1:40 | **Portfolio** *(View)* | the new Fluid position appears | wallet `addr_tes…7ghs`; **fUSDM · Fluid Borrow −11 (−$11.00)**; fUSDM · Dano Supply 100 | [`03`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/03-portfolio-after-open.png) |
| 4 | 2:00 | **Loan Details → Refinance via Dano** *(View → Refinance)* | the quote, in full, before signing | Fluid loan: Total Debt **$11.00 / 11 fUSDM**, HF **3.63 Healthy**, APR **4.00%**, collateral **ADA 100 ($30.50)**. Card: **Save 9.84% net cost**; Net cost **4.00% → −5.84%**; Health Factor **3.63 Healthy → 1.87 Healthy**; ✓ *Same loan, Same collateral*; **Fee 2 fUSDM** | [`04`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/04-loan-details-refinance-quote.png) |
| 5 | 2:28 | **Confirm** → sign in Eternl | one transaction, memo *"Dano Finance: Create Loan"* | Eternl: 1 transaction, tags **Contract · Mint · Burn · Memo**, memo **Dano Finance: Create Loan**, signing wallet *Ngan Wallet (#0)* | [`05`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/05-eternl-sign-refinance.png) |
| 6 | 2:46 | **The submit fails** | — | the app shows **"Submit failed: Your wallet may not have finished syncing. Please wait a moment or reload the page, then try again."** with **Retry** and **Close** — see [§3.3](#33-coverage-stability-and-findings), D-1 | [`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/06-submit-failed-retry-prompt.png) |
| 7 | 3:02 | **Retry** → sign again | the retry produces the same transaction and goes through | Eternl reopens with the same memo, wallet net **−t₳4.69 (−$1.01)**; settled as `a04fe52e…68d8f4`, block **5,190,735**, 08:25:22 UTC — **one refinance on chain, not two** | [`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/07-eternl-sign-refinance-retry.png) |
| 8 | 3:20 | **The app confirms the refinance** | a success state, not a silent return | *Refinance via Dano* — **✓ Transaction confirmed**, with a **Done** button; the tester then returns to *Portfolio*, where the fUSDM position is now a **Dano** row | [`08`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/08-refinance-transaction-confirmed.png) |
| 9 | 5:04 | **Loan Details** on the new loan *(View)* | Dano loan, same collateral | **Dano Finance** · Total Debt **$13.00 / 13 fUSDM** · Loan Health Factor **1.87 Healthy** · Current Interest (APR) **3.10%** · Collateral Backing **$30.50** · utilisation **4%** · collateral **ADA 100** · actions *Repay Loan · Modify Collateral · Increase Loan* | [`09`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/09-loan-details-dano-13-fUSDM.png) |
| 10 | 5:14 | **Repay Loan** *(the Repay journey)* | the amount pre-fills to the full debt | *"Minimum amount to repay is 10.001 fUSDM."* **You Repay 13.00001 fUSDM** ($13.00); collateral **ADA 100**; *Loan Impact* — Current Debt **13 fUSDM** → **↘ Repaid**, collateral **No change**, HF **1.87 Healthy → --** | [`10`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/10-repay-quote-13-fUSDM.png) |
| 11 | 5:35 | **Confirm** → sign in Eternl | one transaction, memo *"Dano Finance: Repay Loan"* | Eternl: 1 transaction, memo **Dano Finance: Repay Loan**; settled as `a4bebdb7…3fbeed`, block **5,190,743**, 08:29:15 UTC | [`11`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/11-eternl-sign-repay.png) |
| 12 | 7:08 | **Eternl → Transactions** — the tester checks their own wallet | both transactions present, with memos and confirmations | *Dano Finance: Create Loan* — tx `a04fe52e…`, block 5,190,735, **7 confirmations**, network fee −t₳1.57; below it the repayment, **+t₳97.59**, **fUSDM −13,000,009**, Borrower NFT `185a63e4…` **−1** | [`12`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/12-eternl-refinance-confirmed.png) |
| 13 | 7:27 | **Cardanoscan** — the tester checks a public explorer | the repayment as a third party sees it | `a4bebdb7…3fbeed`: **4 in → 3 out**, total output **1,131.534492 ₳**, fee **1.266142 ₳** (268 lovelace/B), block **5,190,743**, index 2 of 3, 4.6 KB, ex-units 2.4M mem · 764.9M steps, 1 signatory | [`13`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/13-cardanoscan-repay.png) |

**Transactions:**
[open `69e04600…`](https://preprod.cardanoscan.io/transaction/69e04600fa218dfd3e6826eace41b14d241987783a2942cc8243a8a9f93b84b8) ·
[refinance `a04fe52e…`](https://preprod.cardanoscan.io/transaction/a04fe52e0545f546b71a866ed48b1a83aac271dbece74d21fb27e835d268d8f4) ·
[repay `a4bebdb7…`](https://preprod.cardanoscan.io/transaction/a4bebdb7ca185ffc1ce4fc22873e9e3d9cefa80e0d1e0d8ad698e6b1813fbeed)

### 3.2 Verification (QC)

All on-chain figures re-derived from the public **Koios** preprod API
(`/tx_info`, `/tx_utxos`, `/tx_metadata`).

**Preview values (UI correctness)**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT3-TC-01 | Savings label = net-cost delta | 4.00% − (−5.84%) = **9.84%** | "Save 9.84% net cost" | `04` | ✅ |
| UAT3-TC-02 | Health factor before → after | 3.63 Healthy → 1.87 Healthy | as quoted | `04` | ✅ |
| UAT3-TC-03 | Origination fee shown in the borrowed asset | 2 fUSDM | Fee 2 fUSDM | `04` | ✅ |
| UAT3-TC-04 | Resulting debt = debt + fee | 11 + 2 = **13 fUSDM** | Loan Details after: 13 fUSDM | `04`, `09` | ✅ |

**Signing & submission**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT3-TC-05 | Each action builds **one** transaction with a naming memo | memo per action under metadata 674 | open *Borrow from Fluid*, refinance *Create Loan*, repay *Repay Loan* — all three confirmed by Koios `/tx_metadata` | `02`, `05`, `11` | ✅ |
| UAT3-TC-06 | A failed submit does not produce a partial or duplicate transaction | at most one refinance on chain | the wallet's whole history in this window is **three** transactions — one open, one refinance, one repay | Koios `/address_txs` | ✅ |
| UAT3-TC-07 | The failure is recoverable from inside the app | Retry completes the action | Retry → one more signature → settled in block 5,190,735, ~2½ minutes after the first attempt | `06`, `07` | ✅ *(with the defect in §3.3)* |
| UAT3-TC-08 | Accepted by the validators | scripts pass, within budget | refinance 5,338 B, 3 in → 6 out; repay ex-units 2.4M mem · 764.9M steps (well inside the limit) | `13` | ✅ |

**Open — what the chain recorded**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT3-TC-09 | The collateral quoted is the collateral locked | 100 ADA into the Fluid loan script | **100.000000 ₳** at `addr_test1zpyg4esucs…` with the loan's position token | Koios | ✅ |
| UAT3-TC-10 | The amount quoted is the amount borrowed | 11 fUSDM disbursed by the Fluid pool | pool fUSDM **834.000000 → 823.000000** = **11.000000 disbursed** | Koios | ✅ |
| UAT3-TC-11 | The borrower's own outlay is the collateral plus costs | ≈ 100 ADA + min-UTxO + fee | wallet ADA falls by **101.988262**, of which **100.000000** is the collateral now locked | Koios | ✅ |

**Refinance — what the chain recorded**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT3-TC-12 | The Fluid loan UTxO is consumed with its collateral | 100 ADA leaves the Fluid script | input `addr_test1zpyg4esucs…` **100.000000 ₳** + position token | Koios | ✅ |
| UAT3-TC-13 | **Collateral continuity** | the same 100 ADA arrives at the Dano loan contract | output `addr_test1zzx7xnch…` **100.000000 ₳** + new loan token `991d751a…` | Koios | ✅ |
| UAT3-TC-14 | The Fluid debt is settled in the same transaction | the lender is paid in full | **fUSDM 11.000002** to `addr_test1qr9ew225…` | Koios | ✅ |
| UAT3-TC-15 | Quoted fee = fee paid | 2 fUSDM → exactly 2.000000 on chain | **fUSDM 2.000000** to the fee address `addr_test1qrrrfm89…` | `04`, Koios | ✅ |
| UAT3-TC-16 | The pool disbursed exactly what the loan is for | disbursement = settlement + fee (+ dust) | pool fUSDM **5,464.211244 → 5,451.211236** = **13.000008** = 11.000002 + 2.000000 + 0.000006 | Koios | ✅ |
| UAT3-TC-17 | The borrower needs no capital of their own | only fee and min-UTxO leave the wallet | wallet net **−4.688518 ₳** = network fee 1.572388 + 3.116130 of min-UTxO ADA; **no fUSDM leaves the wallet** | Koios | ✅ |

**Repay — what the chain recorded**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT3-TC-18 | Quoted repay amount = amount paid | 13.00001 fUSDM quoted | pool **+12.999904** + fee address **+0.000105** = **13.000009 fUSDM**, the figure the wallet also shows | `10`, `12`, Koios | ✅ |
| UAT3-TC-19 | Full repayment releases all the collateral | 100 ADA returns, less the transaction's costs | wallet **+97.587398 ₳** = 100 − 1.266142 network fee − 1.146460 to the fee address | Koios | ✅ |
| UAT3-TC-20 | The UTxO consumed is the one the refinance created | same contract, same loan token | input `addr_test1zzx7xnch…` 100 ₳ + loan token `991d751a…`, both created by `a04fe52e…` | Koios | ✅ |
| UAT3-TC-21 | The borrower's title to the loan is burned | the Borrower NFT the refinance paid to the wallet is destroyed | `185a63e4…` is an **input** and appears in **no output** — the wallet's own history shows it as **−1** | `12`, Koios | ✅ |
| UAT3-TC-22 | Only the intended loan is touched | the tester's other positions are untouched | their second Dano loan (203.32759 ADA) is still open at the end of the session | `08`, Koios | ✅ |

**22 checks, 22 hold.**

### 3.3 Coverage, stability and findings

| Journey | Covered | How |
|---|---|---|
| **Open** | ✅ | steps 1–2, settled as `69e04600…` |
| **View** | ✅ | steps 3, 4, 8, 9 — portfolio, loan list and loan details, before and after |
| **Refinance** | ✅ | steps 5–8, settled as `a04fe52e…` **after one failed submit** |
| **Repay** | ✅ | steps 10–13, settled as `a4bebdb7…`, collateral released |
| **Self-verification** | ✅ | steps 12–13: the tester checked the result in their own wallet **and** on a public explorer |

**Not a transaction:** at 3:50 the tester opened *Repay Loan* on a **second, pre-existing Dano loan**
(203.32759 ADA against 100 fUSDM, HF 1.29 Fair), read the preview, and closed it without confirming.
No such transaction exists on chain — the wallet's whole history for this window is the three
transactions above. It is recorded here so the recording is not misread.

| ID | Severity | What happened |
|---|---|---|
| **D-1** | **major (recoverable)** | **The first refinance submit failed.** After the signature, the app showed *"Submit failed: Your wallet may not have finished syncing. Please wait a moment or reload the page, then try again."* The tester clicked **Retry**, signed a second time, and the refinance settled. Cost: one extra signature and about 2½ minutes. Nothing was double-submitted — the chain holds exactly one refinance. The message is honest but puts the cause on the user's wallet; the tester had no way to tell whether the first signature had cost them anything. ⬜ **Root cause not established** — reported as open, not as fixed. |
| **D-2** | minor | While the repay preview loads, the dialog reads *"Minimum amount to repay is **--** fUSDM"* and shows skeleton bars for several seconds before the real numbers appear (`10` is the loaded state; the skeleton is visible in the recording at 5:08–5:12). |
| **D-3** | observation | The repay of the Dano loan took **~1 min 40 s** between signing and the confirmation clearing (5:35 → 7:08), with the dialog showing *"Waiting for confirmation…"* throughout. The block itself settled at 08:29:15; the wait is the app polling, not the chain. |
| **OI-1** | carried over | the *Deposit 5 ADA* / *Fee 5 fUSDM ($5.00)* line again, against a loan of **11 fUSDM of debt for 11.000000 fUSDM disbursed** — see [Open items](#open-items-common-to-more-than-one-session) |

Stability: no crash, white screen or unrecoverable state — the one failure (D-1) recovered inside the
app; every displayed value matched the chain; all three actions signed and submitted (four
signatures, because of D-1); each action reached a confirmation the tester could verify
independently.

---

## Journey 4 — one borrow, two pools, two loans

Borrow **905.004 ADA** from Fluid against **98,327.69 fUSDM** collateral — an amount large enough
that it **fills two Fluid pools and opens two separate loans** — then refinance **both** of them into
Dano Finance loans. This is the *open → view → refinance* path **at a size that exercises the
multi-pool case**, on a **third wallet**. It does **not** cover repay.
**Wallet:** Eternl, account *Deploy Oracle (#0)*, address
`addr_test1qr20zc2v0fyxwjf42jy8vjctfpqrc9f3md3cwe6fccsza9g5q42ut28rqch23fj0j4hp479jdkeyn2mpv8tmxk3h5jeqsm9xmy`
(the interface shows it as `addr_tes…9xmy`).
**Session:** one continuous recording of **3 min 50 s**, ≈08:33 → 08:37 UTC. Each screenshot is
unaltered except that the browser's **bookmarks bar** has been cropped out; the address bar is kept,
and the recording is the unedited source for every frame.

**Tester: not recorded.** This session ran on a third wallet, but who operated it, their relation to
the delivery team and what they were told were not logged at the time — so, as in
[§3](#journey-3--a-second-tester-on-a-second-wallet), nothing about independence or usability is
claimed from it. What it establishes is the multi-pool behaviour below.

### 4.1 The session, step by step

Timestamps are positions in the recording. *Open* is steps 1–3, *View* is steps 4, 8, 9, *Refinance*
is steps 5–7 (first loan) and 10–11 (second loan).

| # | Time | Step | Expected | Actual result | Screenshot |
|---|---|---|---|---|---|
| 1 | 0:12 | **Borrow Market** — fUSDM as collateral, Fluid pool, **Max** borrow entered | a *Loan Impact* preview before anything is signed, and a warning if one borrow will not fit in one pool | Fluid Flexible **4.00%** (67% LTV) selected, labelled **"$0.3K · 2 pools"**; **You Borrow 905.004 ADA** ($275.99, of 905 available), **Collateral 98,327.69 fUSDM**; the card warns **"Fluid · This rate may fill several pools and open more than one loan"**; *Loan Impact* — APR **4.00%**, 2.04%/month, New Collateral $98,327.69, **New Health Factor 195 Healthy**, Deposit 5 ADA, Fee 9.05 ADA ($2.76) | [`01`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-4-borrow-ADA-collateral-USDM/01-borrow-market-905-ADA-98327-fUSDM.png) |
| 2 | 0:30 | **Create Loan** → sign in Eternl | one transaction, memo *"Dano Finance: Borrow from Fluid"* | Eternl: 1 transaction to confirm, tags **Meta · Contract · Mint · Burn**, **+ t₳ 896**, memo **Dano Finance: Borrow from Fluid**, signing wallet *Deploy Oracle (#0)*; settled as `5527f624…6d0c1b`, block **5,190,758**, 08:33:57 UTC | [`02`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-4-borrow-ADA-collateral-USDM/02-eternl-sign-open.png) |
| 3 | 0:46 | **The app confirms the borrow** | a success state, not a silent return | *Create Loan* — **✓ Transaction confirmed**, with a **Done** button | [`03`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-4-borrow-ADA-collateral-USDM/03-open-transaction-confirmed.png) |
| 4 | 1:04 | **Portfolio → ADA · Loans** *(View)* | **two** loans, because the borrow filled two pools | **two rows**: ADA **$6.1 / 20 ADA** with **$2.2K** fUSDM collateral, and ADA **$0.3K / 885 ADA** with **$96.2K** fUSDM collateral — both **4.00%** APR, Health Factor **197 Healthy**, both offering **"Dano save 3.32% net cost"** | [`04`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-4-borrow-ADA-collateral-USDM/04-ada-loans-two-loans-from-one-borrow.png) |
| 5 | 1:24 | **Loan Details** on the *first* loan **→ Refinance via Dano** *(View → Refinance)* | the quote, in full, before signing | Fluid loan: Total Debt **$6.10 / 20 ADA**, HF **197 Healthy**, APR **4.00%**, Collateral Backing **$2,172.98** (**2,172.97 fUSDM**). Card: **Save 3.32% net cost**; Net cost **4.00% → 0.68%**; Health Factor **197 Healthy → 32.3 Healthy**; ✓ *Same loan, Same collateral* — *"You don't need extra funds to close your Fluid loan."*; **Fee 2 ADA** | [`05`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-4-borrow-ADA-collateral-USDM/05-loan-details-20-ADA-refinance-quote.png) |
| 6 | 1:56 | **Confirm Refinance via Dano** → sign in Eternl | one transaction, memo *"Dano Finance: Create Loan"* | Eternl: 1 transaction, tags **Meta · Contract · Mint · Burn**, **− t₳ 4.36**, memo **Dano Finance: Create Loan**, asset **`04df3b105287bd665af89dd3c7192aa3be444cb047398ebb2…`**; settled as `6ab3bde0…c7ad43`, block **5,190,766**, 08:35:17 UTC | [`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-4-borrow-ADA-collateral-USDM/06-eternl-sign-refinance-loan-a.png) |
| 7 | 2:10 | **The app confirms the refinance** | a success state | *Refinance via Dano* — **✓ Transaction confirmed**, with a **Done** button | [`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-4-borrow-ADA-collateral-USDM/07-refinance-transaction-confirmed.png) |
| 8 | 2:35 | **Portfolio** *(View)* | the collateral now split between the two protocols | Total value **$13,732,526.44**; **fUSDM 9.878M** broken down as **Wallet 9.78M ($9,780,000.00)**, **Fluid · Supply 96.15K ($96,154.71)** and **Dano · Supply 2.172K ($2,172.98)** — the refinanced loan's collateral has moved to Dano, the other has not | [`08`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-4-borrow-ADA-collateral-USDM/08-portfolio-collateral-split-fluid-dano.png) |
| 9 | 2:56 | **ADA · Loans** again *(View)* | the refinanced loan has left the Fluid list | **one row left**: ADA **$0.3K / 885 ADA**, collateral **$96.2K**, 4.00%, HF **197 Healthy**, still offering **"Dano save 3.32% net cost"** | [`09`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-4-borrow-ADA-collateral-USDM/09-ada-loans-one-loan-left.png) |
| 10 | 3:12 | **Loan Details** on the *second* loan **→ Refinance via Dano** | the same quote shape, on the larger loan | Fluid loan: Total Debt **$269.89 / 885 ADA**, HF **197 Healthy**, APR **4.00%**, Collateral Backing **$96,154.71** (**96,154.71 fUSDM**). Card: **Save 3.32% net cost**; Net cost **4.00% → 0.68%**; Health Factor **197 Healthy → 35.5 Healthy**; ✓ *Same loan, Same collateral*; **Fee 2 ADA** | [`10`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-4-borrow-ADA-collateral-USDM/10-loan-details-885-ADA-refinance-quote.png) |
| 11 | 3:44 | **Confirm** → sign in Eternl | one transaction, memo *"Dano Finance: Create Loan"* | Eternl: 1 transaction, **− t₳ 4.36**, memo **Dano Finance: Create Loan**, asset **`589391707a6d5d06036d61c9f1d82ec851f1a3ef983ff308e…`**; **the recording ends here**, seconds before the block was minted — the chain shows it settled as `3cbb01a7…9694a8`, block **5,190,774**, 08:37:12 UTC | [`11`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-4-borrow-ADA-collateral-USDM/11-eternl-sign-refinance-loan-b.png) |

**Transactions:**
[open `5527f624…`](https://preprod.cardanoscan.io/transaction/5527f6248003c78ea442ba00b66013562d0a31aee05dbeadba87ab6f796d0c1b) ·
[refinance 1 `6ab3bde0…`](https://preprod.cardanoscan.io/transaction/6ab3bde0739c1a53aacbbd6c43aacd848b3311a8958030be412c22ffe1c7ad43) ·
[refinance 2 `3cbb01a7…`](https://preprod.cardanoscan.io/transaction/3cbb01a7b9dce89f175a75174dff109d48c231f42b2059fc8950d2485f9694a8)

### 4.2 Verification (QC)

All on-chain figures re-derived from the public **Koios** preprod API
(`/address_txs`, `/tx_info`, `/tx_utxos`, `/tx_metadata`).

**Preview values (UI correctness)**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT4-TC-01 | The interface warns when one borrow will not fit in one pool | the *"may fill several pools and open more than one loan"* notice, and two loans afterwards | notice shown on the Fluid card (*"$0.3K · 2 pools"*); the borrow consumed **two** Fluid pool UTxOs and produced **two** loans | `01`, `04`, Koios | ✅ |
| UAT4-TC-02 | *Max* offered = liquidity actually available | 905.004 ADA | the two pool UTxOs held **885.004000 ₳** and **20.000000 ₳** — exactly **905.004000 ₳**, and both were consumed in full | `01`, Koios | ✅ |
| UAT4-TC-03 | Amount quoted = amount disbursed | 905.004 ADA | **905.004000 ₳** left the two pools | `01`, Koios | ✅ |
| UAT4-TC-04 | Collateral quoted = collateral locked | 98,327.69 fUSDM | **96,154.711980 + 2,172.978020 = 98,327.690000 fUSDM** locked at the Fluid loan script | `01`, Koios | ✅ |
| UAT4-TC-05 | The two loans sum to the borrow | 20 + 885 = 905 ADA | list shows **20 ADA** and **885 ADA**; on chain the principals are **20.000000** and **885.004000** | `04`, Koios | ✅ |
| UAT4-TC-06 | Each loan's collateral matches its pool share | $2.2K and $96.2K | **2,172.978020** and **96,154.711980 fUSDM**, one per loan UTxO, each with its own position token | `04`, `05`, `10`, Koios | ✅ |
| UAT4-TC-07 | Savings label = net-cost delta, first loan | 4.00% − 0.68% = **3.32%** | "Save 3.32% net cost" | `05` | ✅ |
| UAT4-TC-08 | Savings label = net-cost delta, second loan | 4.00% − 0.68% = **3.32%** | "Save 3.32% net cost" | `10` | ✅ |
| UAT4-TC-09 | Health factor before → after quoted for each loan | one pair per loan, shown before signing | 197 → **32.3** (20 ADA loan), 197 → **35.5** (885 ADA loan) | `05`, `10` | ✅ * |
| UAT4-TC-10 | Origination fee quoted in the borrowed asset | 2 ADA on each | Fee **2 ADA** on both cards | `05`, `10` | ✅ |

\* UAT4-TC-09 checks only that the pair is quoted before signing. This session never re-opened the
two **Dano** loans afterwards, so the "after" figure is not confirmed against the resulting loan
here; that check is made in journeys 1, 2 and 3, where the loan is opened again after the refinance.

**Signing & submission**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT4-TC-11 | Each action builds **one** transaction with a naming memo | memo per action under metadata label 674 | open **"Dano Finance: Borrow from Fluid"**, both refinances **"Dano Finance: Create Loan"** — all three confirmed by Koios `/tx_metadata` | `02`, `06`, `11` | ✅ |
| UAT4-TC-12 | The net movement Eternl shows is the net movement the chain records | +t₳ 896 / −t₳ 4.36 / −t₳ 4.36 | **+896.997892 ₳**, **−4.364925 ₳**, **−4.355241 ₳** | `02`, `06`, `11`, Koios | ✅ |
| UAT4-TC-13 | The asset shown in the signing dialog is the token actually minted | the Borrower NFT of that refinance | `04df3b105287bd665af89dd3c7192aa3be444cb047398ebb221d7cc6` and `589391707a6d5d06036d61c9f1d82ec851f1a3ef983ff308ef555fdb`, both minted **+1** under policy `8de34f17…` | `06`, `11`, Koios | ✅ |
| UAT4-TC-14 | Nothing is double-submitted | one transaction per action | the wallet's whole history in this window is **exactly three** transactions — one open, two refinances | Koios `/address_txs` | ✅ |
| UAT4-TC-15 | Accepted by the validators, within budget | scripts pass | open 5,188 B / fee 0.894608 ₳ (11 in → 5 out); refinance 1 6,642 B / 1.619065 ₳ (3 in → 6 out); refinance 2 6,744 B / 1.627078 ₳ (4 in → 6 out) | Koios | ✅ |

**Open — what the chain recorded**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT4-TC-16 | One borrow, two pools, two loans | two pool UTxOs consumed, two loan UTxOs created | pool UTxOs of **885.004000 ₳** and **20.000000 ₳** consumed and their pool tokens burned; **two** loan UTxOs created at `addr_test1zpyg4esucs…`, each with its own position token under policy `8dfb447e…` | Koios | ✅ |
| UAT4-TC-17 | The wallet gives up exactly the quoted collateral | −98,327.69 fUSDM | wallet fUSDM **9,878,327.691214 → 9,780,000.001214** = **−98,327.690000** | Koios | ✅ |
| UAT4-TC-18 | The borrower receives the borrow less chain costs only | no origination fee to any Dano or Fluid address | **+896.997892 ₳** = 905.004000 − 0.894608 (network fee) − 2.586000 (two lender-NFT min-UTxOs) − 4.525500 (two collateral min-UTxOs). **Nothing else left the wallet** — see OI-1 | Koios | ✅ |
| UAT4-TC-19 | Each loan is titled to somebody | a lender NFT and a borrower NFT per loan | two lender NFTs (`bcd713bb…`) to `addr_test1qr9ew225…`, two borrower NFTs (`eadc69a5…`) to the tester's wallet | Koios | ✅ |

**Refinance — first loan (20 ADA), `6ab3bde0…`**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT4-TC-20 | The Fluid loan UTxO is consumed with its collateral | that loan's collateral leaves the Fluid script | input `addr_test1zpyg4esucs…` **2.254130 ₳ + 2,172.978020 fUSDM** + position token `8dfb447e….4a22b7f2…`, which is **burned** | Koios | ✅ |
| UAT4-TC-21 | **Collateral continuity** | the same fUSDM arrives at the Dano loan contract | output `addr_test1zzx7xnch…` **5.000000 ₳ + 2,172.978020 fUSDM** — the same amount, to the decimal | Koios | ✅ |
| UAT4-TC-22 | The Fluid debt is settled in the same transaction | the lender is paid in full | **20.000003 ₳** to `addr_test1qr9ew225…` | Koios | ✅ |
| UAT4-TC-23 | Quoted fee = fee paid | 2 ADA → exactly 2.000000 on chain | **2.000000 ₳** to the fee address `addr_test1qrrrfm89…` | `05`, Koios | ✅ |
| UAT4-TC-24 | The Dano pool disbursed exactly what the loan is for | disbursement = settlement + fee (+ dust) | pool **9,311.605320 → 9,289.605307 ₳** = **22.000013** = 20.000003 + 2.000000 + 0.000010 | Koios | ✅ |
| UAT4-TC-25 | The borrower needs no capital of their own | only fee and min-UTxO leave the wallet | wallet net **−4.364925 ₳** = 1.619065 network fee + 2.745870 min-UTxO top-up − 0.000010 dust returned; **no fUSDM leaves the wallet** | Koios | ✅ |
| UAT4-TC-26 | The borrower gets title to the new loan | a Borrower NFT is paid to the wallet | `8de34f17….04df3b10…` minted **+1** to the tester's address — the id Eternl displayed before signing | `06`, Koios | ✅ |

**Refinance — second loan (885 ADA), `3cbb01a7…`**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT4-TC-27 | The Fluid loan UTxO is consumed with its collateral | that loan's collateral leaves the Fluid script | input **2.271370 ₳ + 96,154.711980 fUSDM** + position token `8dfb447e….647d5a96…`, which is **burned** | Koios | ✅ |
| UAT4-TC-28 | **Collateral continuity** | the same fUSDM arrives at the Dano loan contract | output `addr_test1zzx7xnch…` **5.000000 ₳ + 96,154.711980 fUSDM** | Koios | ✅ |
| UAT4-TC-29 | The Fluid debt is settled in the same transaction | the lender is paid in full | **885.004206 ₳** to `addr_test1qr9ew225…` | Koios | ✅ |
| UAT4-TC-30 | Quoted fee = fee paid | 2 ADA → exactly 2.000000 on chain | **2.000000 ₳** to the fee address | `10`, Koios | ✅ |
| UAT4-TC-31 | The Dano pool disbursed exactly what the loan is for | disbursement = settlement + fee (+ dust) | pool **9,289.605307 → 8,402.600634 ₳** = **887.004673** = 885.004206 + 2.000000 + 0.000467 | Koios | ✅ |
| UAT4-TC-32 | The borrower needs no capital of their own | only fee and min-UTxO leave the wallet | wallet net **−4.355241 ₳** = 1.627078 network fee + 2.728630 min-UTxO top-up − 0.000467 dust returned | Koios | ✅ |
| UAT4-TC-33 | The borrower gets title to the new loan | a second, **distinct** Borrower NFT | `8de34f17….58939170…` minted **+1** — a different asset from the first refinance's, so the two Dano loans are separable | `11`, Koios | ✅ |
| UAT4-TC-34 | The action completes even though the recording stops at the signature | the transaction settles | block **5,190,774**, 08:37:12 UTC — the recording's last frame is the Eternl dialog seconds earlier | `11`, Koios | ✅ |

**The interface against the chain (View)**

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT4-TC-35 | The portfolio's split between protocols matches the chain | one loan's collateral at Dano, one still at Fluid | UI: *Fluid · Supply* **$96,154.71**, *Dano · Supply* **$2,172.98**; chain: **96,154.711980** fUSDM at the Fluid loan script, **2,172.978020** at the Dano loan contract — equal to the cent | `08`, Koios | ✅ |
| UAT4-TC-36 | A refinanced loan leaves the Fluid loan list | one row left, the 885 ADA loan | *ADA · Loans* shows a single row after the first refinance | `04`, `09` | ✅ |
| UAT4-TC-37 | Interest accrues between open and refinance | settlement slightly above principal, of the order of 4.00%/yr | 20.000000 → **20.000003** over 80 s and 885.004000 → **885.004206** over 195 s; 4.00% simple interest over those spans would be 0.000002 and 0.000219 | Koios | ✅ |

**37 checks, 37 hold.**

### 4.3 Coverage, stability and findings

| Journey | Covered | How |
|---|---|---|
| **Open** | ✅ | steps 1–3, settled as `5527f624…` — and specifically the **multi-pool** open: one borrow, two loans |
| **View** | ✅ | steps 4, 8, 9 — loan list before and after, loan details on both loans, portfolio split across two protocols |
| **Refinance** | ✅ | steps 5–7 (`6ab3bde0…`) and 10–11 (`3cbb01a7…`) — **two** refinances in one session |
| **Repay** | ❌ | **not exercised.** Both Dano loans were still open when the recording stopped. Repay evidence is in journeys 1, 2 and 3 above |
| **Self-verification** | ❌ | the tester did not open their wallet history or a block explorer during the session; the on-chain column above is our verification, done afterwards from Koios |

**What this journey adds that the other three do not:** the **multi-pool open** — a single borrow that
the interface warns may "fill several pools and open more than one loan", and that does exactly that
— and the **second refinance in the same session**, showing that refinancing one loan leaves the
tester's other positions untouched (UAT4-TC-35, TC-36).

| ID | Severity | What |
|---|---|---|
| **OI-1** | carried over | the *Deposit 5 ADA* / *Fee 9.05 ADA ($2.76)* line again — here the whole difference between the **905.004000 ₳** the pools paid out and the **896.997892 ₳** the wallet received is network fee plus min-UTxO, and the resulting debt is **905.004 ADA**, not 914.054. See [Open items](#open-items-common-to-more-than-one-session) |
| **OI-2** | carried over | the two **Fluid borrower NFTs** minted at open (`eadc69a5….4a22b7f2…` and `eadc69a5….647d5a96…`) are **still in the wallet** after both refinances — neither refinance burns them, although both Fluid loans are gone. See [Open items](#open-items-common-to-more-than-one-session) |
| **D-1** | minor | The loan list and the loan dialogs show the second loan as **885 ADA**; its actual principal is **885.004000 ADA**, and it settled at **885.004206**. Rounding in the display only — every figure the user confirms before signing is exact. |
| **D-2** | observation | Both Dano loan UTxOs carry a loan token with the **same** asset name (`8de34f17….ea5041c0ae6b0969…`); the two loans are told apart by their distinct **Borrower NFTs** (`04df3b10…`, `58939170…`). This looks like a pool identifier rather than a per-loan id, and the loans are separable either way — ⬜ *for the team to confirm as intended*. |
| **D-3** | observation | Loan Details quotes the health factor falling from **197** to **32.3** / **35.5** on refinance, with *both* labelled **Healthy**, next to the line *"Same loan, Same collateral"*. Nothing in the card explains why an unchanged position's health factor drops six-fold; a user cannot tell from the screen whether the two numbers are on the same scale. This session did not re-open the Dano loans, so the resulting health factor is not confirmed here either — ⬜ *for the team to explain in the UI, or correct*. |

Stability: no crash, white screen or unrecoverable state, and no failed submit (unlike journey 3
D-1); every displayed value matched the chain; all three actions signed and submitted; the interface
warned about the multi-pool case, opened two loans, listed them separately, and refinanced them one
at a time without touching the other.

---

## Open items common to more than one session

**OI-1 · The *Deposit* and *Fee* lines in the Borrow Market preview are not reconciled.** Before the
borrow is signed, the *Loan Impact* block quotes a *Deposit* and a *Fee*. In all four sessions the
loan that resulted does not carry them:

| Session | Quoted at open | What the chain shows |
|---|---|---|
| Journey 1 | Deposit 5 ADA · Fee 5 ADA ($1.52) | 25 ADA borrowed, debt afterwards **25 ADA** (not 30); the open transaction's UTxO breakdown was not captured in that session |
| Journey 2 | Deposit 5 ADA · Fee 5 fUSDM ($5.00) | wallet received **fUSDM +11,000,000** — the full amount, no fee deducted; debt afterwards **11 fUSDM** (not 16) |
| Journey 3 | Deposit 5 ADA · Fee 5 fUSDM ($5.00) | pool disbursed **11.000000 fUSDM**, none deducted, none capitalised |
| Journey 4 | Deposit 5 ADA · Fee 9.05 ADA ($2.76) | **905.004000 ₳** disbursed, **896.997892 ₳** received, and the whole **8.006108 ₳** difference is network fee (0.894608) plus min-UTxO (7.111500) — no payment to any Dano or Fluid fee address; debt afterwards **905.004 ADA** (not 914.054) |

So whatever those two lines describe, it is **not** an amount deducted at open or capitalised into
the loan. It is recorded as an open item rather than a pass. ⬜ **Root cause not established.** Note that the
**refinance** fee is a different figure and does reconcile exactly, in every session: 2 ADA or
2 fUSDM quoted, 2.000000 paid to the fee address, five times out of five.

**OI-2 · Which token the refinance burns, and what is left behind.** Two related observations:

- *Journey 2.* The token name `2d3883b3…70b67d8a` appears in the refinance **as a −1 from the Fluid
  script and again in an output to the borrower's wallet (+1)**, which reads as though the token
  minted at open were not the token burned. **Koios resolves it: the two are different tokens that
  share one asset name.** `tx_info` for `78d434d5…` reports the burn under policy
  **`8dfb447e…`** (the Fluid loan script's position NFT, `−1`) while the copy left in the wallet
  `addr_test1qr4rlljc…` is under policy **`eadc69a5…`**, untouched; the same transaction mints two
  Danogo tokens under `8de34f17…`. So journey 2 has **exactly the shape journey 4 shows**, and the
  pairing claim holds for the script-held token — see
  [`05` §2.2](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#22-the-three-fluid-tokens-minted-at-open-and-which-one-the-pairing-uses),
  where the same three policies are shown on mainnet.
- *Journey 4.* Koios confirms the shape directly: each refinance burns the **Fluid position token**
  held by the loan script (`8dfb447e….*`) and mints a fresh Dano loan token plus a Borrower NFT — but
  the **Fluid borrower NFTs** minted to the wallet at open (`eadc69a5….*`) are **not** burned and
  remain in the wallet after both loans are gone. Harmless (each sits in a 1.245590 ₳ UTxO), but the
  wallet shows tokens for positions that no longer exist.

What remains open is therefore not *which* token the pairing uses — the ledger answers that — but
whether leaving the `eadc69a5…` borrower NFT in the wallet after the position is gone is intended.
⬜ *For the team to confirm.*

---

## What the four sessions establish together

Four sessions, three Eternl accounts, one day, **123 QC checks, 123 hold**.

- **All four journeys are captured end to end.** Three sessions run *open → view → refinance → view →
  repay* on a single loan, from the first click to the settled repayment: journeys 1 and 2 in
  opposite asset directions, journey 3 by a second tester on a second wallet. Journey 4 covers
  *open → view → refinance*, twice, and says plainly that it does not cover repay.
- **What the interface quotes is what the chain records.** Across five refinances, the fee quoted is
  the fee paid to the lovelace (2.000000 each time), the collateral quoted is the collateral that
  arrives at the Dano contract to the decimal, the health factor quoted is the health factor the new
  loan carries, the resulting debt is debt + fee, and the pool disburses exactly settlement + fee
  (+ dust) every time. Across three repayments, the amount quoted is the amount paid, the collateral
  is released in full and the borrower's title to the loan is burned.
- **The collateral never returns to the borrower in between.** Every refinance moves it from the
  Fluid script to the Dano loan contract inside one transaction, in one block.
- **No capital of the borrower's own is needed — on the pools these sessions used.** In all five
  refinances here the pool funds both the settlement and the fee, and the borrower's outlay is the
  network fee plus a min-UTxO top-up, nothing more. This is a property of the Flexible Pool, not of
  refinancing as such: the one mainnet refinance drawing on the fixed-term staking contract makes the
  borrower fund 0.954728 ₳ of the fee out of pocket — correction
  [C-4](./08_CORRECTIONS.md#c4--corrected-the-borrower-contributes-only-the-network-fee-was-over-stated).
- **Defects are published, not edited out.** Journey 3's refinance failed to submit on the first
  attempt and needed a *Retry* (D-1); journey 4 records four smaller items. Two questions recur and
  are stated once, as OI-1 and OI-2, rather than four times as passes.
- **What none of this shows** is whether an independent user finds the interface intuitive. Journey 3
  was run by someone outside the walkthroughs, but **who the testers of journeys 3 and 4 were, what
  they were told and what they asked were not recorded** (§3, §4) — so no usability verdict is drawn
  from any of these sessions. The milestone's settlement evidence
  remains the fifteen mainnet transactions in
  [`00`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/00_POA_SUBMISSION_FORM.md) §A.
