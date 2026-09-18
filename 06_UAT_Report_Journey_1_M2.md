# Milestone 2 — User Acceptance Test Report · Journey 1

**Feature:** Rolling Loan ("Refinance via Dano") — **open**, **view**, **refinance** and **repay** a
loan from the Danogo front end with the user's own wallet.
**Journey:** borrow **25 ADA** from Fluid against **100 fUSDM** collateral, refinance it into a Dano
Finance loan, then **repay that Dano loan in full and unlock the collateral** — *open → view →
refinance → view → repay*, all four approved journeys in one thread on one loan.
**Environment:** front end https://preprod.danogo.io, running against **Cardano preprod** and the
**Fluid preprod** smart contracts. Signed with the **Eternl** wallet (v2.1.7.1), account *1kang (#0)*,
address `addr_test1qr4rlljcxqj8dwyf076u3g3lh3p879mvkr9qdhu9kmquqejlawjwdzr7ul3pseee3j05kalkqhququxqqum4j83w4n8qf5g494`.
**Date:** 2026-09-18. Open 04:51:28 and refinance 04:54:27 (wallet clock); the repay settled later
the same day, in block 5,190,592 at 2:29:37 PM (explorer clock).
**Screen recording of the whole session:** [`01-open-and-refinance-borrow-ADA-collateral-USDM.webm`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/01-open-and-refinance-borrow-ADA-collateral-USDM.webm)
**Screenshots:** [`screenshots/journey-1-borrow-ADA-collateral-USDM/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/journey-1-borrow-ADA-collateral-USDM)

> **What this session is, and what it is not.** It was run by the delivery team, not by an
> independent tester. It is therefore **not** offered as evidence that the interface is intuitive to
> someone who has never seen it — that is the claim the previous submission made and the review
> rejected ([`08_CORRECTIONS.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/08_CORRECTIONS.md) C-2). What it does establish, and what every
> row below is checkable against, is the **complete click path of each journey** and the agreement
> between **what the interface promised before the signature** and **what the chain recorded after
> it**. Nothing between the first click and the settled loan is omitted.
>
> **Why preprod.** This is interface evidence. The milestone's settlement evidence is the fifteen
> **mainnet** transactions in [`00`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/00_POA_SUBMISSION_FORM.md) §A, which anyone can verify
> without us.

---

## 1. The journey, step by step

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

**Refinance transaction:** https://preprod.cardanoscan.io/transaction/0bfa25db49a676c15645c25d1d8b35acf5630d1d9cb1d472d386430cb954ccfd

**Repay transaction:** https://preprod.cardanoscan.io/transaction/e134b85eb5631391089598adefd8f06024307fc7d41ec4ac8d421276d8681416

A Fluid loan opened at 04:51:28 was refinanced into a Dano Finance loan at 04:54:27 in **one signed
transaction** — the Fluid debt settled, a new Dano loan opened, and the 100 fUSDM collateral moved
across without ever returning to the wallet. Later the same day that Dano loan was **repaid in
full**: the loan UTxO the refinance had created was consumed, its loan token burned, and the 100
fUSDM collateral released back to the borrower. **One loan, followed from origination to
settlement, through all four journeys.**

---

## 2. Detailed verification (QC) — what was quoted against what settled

Sections 2.1–2.6 check the refinance `0bfa25db…954ccfd`; section 2.7 checks the repayment
`e134b85e…681416` that closed the same loan.
Each has an oracle and is backed by a screenshot from this session; the on-chain column is
Cardanoscan's own rendering of the transaction, not our backend.

### 2.1 Preview values (UI correctness)
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT1-TC-01 | Savings label = net-cost delta | 4.00% − 0.68% = **3.32%** → "Save 3.32% net cost" | "Dano save 3.32% net cost" on the row; "Save 3.32% net cost" on the card | `04`, `05` | ✅ |
| UAT1-TC-02 | Net cost before → after | source 4.00% → Dano 0.68% | 4.00% → 0.68% | `05` | ✅ |
| UAT1-TC-03 | Health factor before → after | 7.27 Healthy → 1.21 Fair | 7.27 Healthy → 1.21 Fair | `05` | ✅ |
| UAT1-TC-04 | Origination fee shown | 2 ADA | Fee 2 ADA | `05` | ✅ |
| UAT1-TC-05 | "Same loan, Same collateral" and the no-extra-funds note | both shown | both shown | `05` | ✅ |

### 2.2 Signing & submission
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT1-TC-06 | Confirm builds **one** transaction and opens the wallet | 1 transaction, memo "Dano Finance: Create Loan" | Eternl shows 1 transaction, memo *Dano Finance: Create Loan*, tagged Mint **and** Burn | `06` | ✅ |
| UAT1-TC-07 | The transaction id is visible **before** signing and is the one that settled | id shown in the dialog = id in the wallet history = id on the explorer | `0bfa25db…954ccfd` in all three | `06`, `07`, `08` | ✅ |
| UAT1-TC-08 | The hash resolves on a public explorer | preprod Cardanoscan shows the transaction | present, 3 in → 6 out | `08` | ✅ |
| UAT1-TC-09 | Included in a block and confirmed | one block, confirmations accruing | **block 5,189,867**, 180 confirmations at capture | `07` | ✅ |

### 2.3 Inputs consumed (source side)
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT1-TC-10 | The Fluid loan UTxO is consumed with its collateral | input spends fUSDM 100.000000 from the Fluid loan address | input **#0** `addr_test1zpyg4esu…n47ypv`, **fUSDM 100,000,000**, −2.25413 ₳ | `08` | ✅ |
| UAT1-TC-11 | The Dano pool UTxO is consumed | pool input spends its market token and dADA | `addr_test1wpuxsu23…5z3psh` −9,313.024336 ₳, *Float ADA Market Token* −1, dADA −12,693,629,080 | `06`, `08` | ✅ |
| UAT1-TC-12 | The Fluid debt is **not** paid from the borrower's own funds | the settlement output is funded by the pool | pool net **−27.000017 ₳** (9,313.024336 − 9,286.024319) covers the 25.000003 ₳ settlement and the 2.000000 ₳ fee; the borrower's own net outlay is **t₳4.31** = network fee 1.55990 + the 2.74587 ₳ that tops the loan UTxO from 2.25413 to 5.00000 | `06`, `08` | ✅ |

### 2.4 Outputs created (target side)
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT1-TC-13 | A new Dano loan UTxO is created | output at the Dano loan contract holds ADA + the collateral + a loan token | output **#3** `addr_test1zzx7xnch…56t24n`: **+5.0 ₳**, **fUSDM 100,000,000**, token `144020f…966` +1 | `08` | ✅ |
| UAT1-TC-14 | The Fluid debt is settled in the same transaction | one output pays the lender the full debt | output **#1 +25.000003 ₳** to `addr_test1qr9ew225…s2n6em` | `08` | ✅ |
| UAT1-TC-15 | The origination fee is a separate leg | one output of exactly 2 ADA to the fee address | output **#4 +2.0 ₳** | `08` | ✅ |

### 2.5 Conservation & "the number shown is the number that settled"
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT1-TC-16 | **Collateral continuity** | fUSDM in = fUSDM out to the Dano loan; not returned to the borrower | 100,000,000 in (#0) = 100,000,000 out (#3) | `08` | ✅ |
| UAT1-TC-17 | **Atomicity** | one transaction, one block, inputs balance outputs | Σ in 10,483.165924 ₳ = Σ out 10,481.60602 ₳ + fee; block 5,189,867 | `08` | ✅ |
| UAT1-TC-18 | Quoted fee = fee paid | 2 ADA quoted → 2.000000 ₳ on chain | 2 ADA (`05`) = +2.0 ₳ (`08` #4) | `05`, `08` | ✅ |
| UAT1-TC-19 | Quoted health factor = health factor after | 1.21 Fair → 1.21 Fair | 1.21 Fair quoted, 1.21 Fair on the settled loan | `05`, `09` | ✅ |
| UAT1-TC-20 | Resulting debt = debt + fee | 25 + 2 = **27 ADA** | 27 ADA in Loan Details and in Portfolio | `09`, `10` | ✅ |
| UAT1-TC-21 | The pool disbursed exactly what the loan is for | disbursement = settlement + fee | **27.000017 ₳** disbursed = 25.000003 + 2.000000 + 0.000014 dust to the borrower | `08` | ✅ |
| UAT1-TC-22 | Network fee is separate from the origination fee | ~1.56 ₳ network vs 2 ADA origination | t₳1.55990 network fee, 2.0 ₳ origination | `07`, `08` | ✅ |

### 2.6 Post-state
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT1-TC-23 | The Fluid position is settled | no Fluid borrow row remains | *Fluid · Borrow* absent after the refinance | `03` → `10` | ✅ |
| UAT1-TC-24 | A Dano position stands in its place | one Dano borrow row, −27 ADA | **Dano · Borrow −27 (−$8.23)** | `10` | ✅ |
| UAT1-TC-25 | The APR moves to the rate the quote named | the ADA pool's 3.21% | Loan Details after: **APR 3.21%**, the same rate the Borrow Market listed for that pool | `01`, `09` | ✅ |

### 2.7 The repayment that closed the loan
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT1-TC-26 | Quoted repay amount = amount actually paid | 27.000474 ADA quoted → the same, split between pool and fee address | pool **+25.581001 ₳** + fee address **+1.419472 ₳** = **27.000473 ₳**, one lovelace of rounding from the quote | `11`, `13` | ✅ |
| UAT1-TC-27 | Full repayment releases **all** the collateral | fUSDM 100.000000 returns to the borrower | loan contract sends **fUSDM −100,000,000**; borrower receives **fUSDM +100,000,000** | `12`, `13` | ✅ |
| UAT1-TC-28 | The UTxO consumed is the one the refinance created | same address, same loan token as output #3 of `0bfa25db…` | `addr_test1zzx7xn…56t24n` with token `144020f…966`, both created by the refinance | `08`, `13` | ✅ |
| UAT1-TC-29 | The borrower's title to the loan is burned | the token the refinance paid to the wallet is destroyed | Eternl shows `18a7d7a6…4ddd195e` **−1**, the same token it showed **+1** at step 6; Cardanoscan reports **2 mints & burns** | `06`, `12`, `13` | ✅ |
| UAT1-TC-30 | The borrower's net outlay reconciles | debt paid + network fee − min-UTxO returned | **−23.252978 ₳** = 27.000473 paid + 1.252505 fee − 5.000000 returned from the loan UTxO | `12`, `13` | ✅ |
| UAT1-TC-31 | Built by this application | metadata label 674 naming the action | label **674 · CIP-20** — `{"msg": ["Dano Finance: Repay Loan"]}` | `13` | ✅ |

**31 checks, 31 hold** — 25 on the refinance, 6 on the repayment.

*Not captured:* no screenshot of the portfolio **after** the repayment was taken, so this report does
not show the app's post-repay state; what it shows is the quote, the signature, and the settled
transaction.

---

## 3. What this session covers, and what it does not

| Journey | Covered here | How |
|---|---|---|
| **Open** | ✅ end to end | steps 1–3: amounts entered, wallet dialog, resulting position |
| **View** | ✅ end to end | step 4 and steps 9–10: the loan list and the loan details, before and after |
| **Refinance** | ✅ end to end | steps 5–10, with the transaction on a public explorer |
| **Repay** | ✅ end to end, on the loan this session created | steps 11–13: the *Repay Loan* quote, the wallet dialog burning the loan token, and the settled transaction on a public explorer. The wallet's list at step 7 additionally records **three earlier *"Dano Finance: Repay Loan"* transactions** from the same wallet — 04:47:17 (+t₳2.58), 04:48:21 (+t₳97.59) and 04:48:50 (+t₳2.58, **Borrower NFT −1**, fUSDM −12,354,268) — which are not part of this walkthrough. The repay journey's **mainnet** evidence is in [`05` §5](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-two-direct-repay-flows-and-repayment-within-a-refinance) |

---

## 4. Stability, and the one thing not reconciled

| Check | Result |
|---|---|
| No crash, white screen or unrecoverable state during the session | ✅ none |
| Every value the interface displayed matched the chain — §2.5 | ✅ fee, health factor, debt, collateral and disbursement all match |
| Wallet signing worked on every signature | ✅ all three signed and submitted, all three confirmed |
| Each transaction reached a confirmation with a hash the explorer resolves | ✅ |
| Whether an independent user finds this interface intuitive | **not tested — see the note at the top of this report** |

**F-1 · Open item — the *Fee* line in the Borrow Market preview is not reconciled.** At step 1 the
*Loan Impact* block quoted **Deposit 5 ADA** and **Fee 5 ADA ($1.52)**. Against a 25 ADA borrow the
wallet's net change on the open was **+t₳20.71**, a difference of 4.29 ADA, and the debt the app
showed afterwards was **25 ADA**, not 30. The open transaction's UTxO breakdown was not captured in
this session, so what those two quoted lines correspond to on chain is **not established here**. It
is recorded as an open item rather than as a pass. The same line is queried in Journey 2
([`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_UAT_Report_Journey_2_M2.md) F-1), where the borrower received the full 11 fUSDM
borrowed with no fee deducted.

**F-2 · Minor.** The Borrow Market quoted a *New Health Factor* of **7.20**; the loan list showed
**7.27** three minutes later. Consistent with price and interest movement between the two screens,
not investigated further.

---

## 5. Conclusion

**All four approved journeys were completed end to end on a single loan**, through the
Eternl-connected front end, screen by screen: opened on Fluid, read in *My Account*, refinanced into
Dano, and repaid in full with the collateral released. The refinance settled in
one transaction on preprod: the Fluid loan UTxO consumed, the 100 fUSDM collateral carried into the
Dano loan contract, the 25.000003 ₳ debt settled from the pool's disbursement and 2.000000 ₳ paid to
the fee address — the same 2 ADA fee, the same 1.21 health factor and the same 27 ADA of debt that
the interface had quoted before the signature. The repayment then consumed exactly that loan UTxO, burned the loan token
the refinance had paid to the wallet, returned all 100 fUSDM of collateral, and paid 27.000473 ₳
against the 27.000474 ₳ the app had quoted. Thirty-one QC checks hold; one preview line (F-1) is
recorded as unreconciled rather than as a pass. No usability verdict is drawn from this session.
