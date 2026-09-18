# Milestone 2 — User Acceptance Test Report · Journey 3

**Feature:** Rolling Loan ("Refinance via Dano") — **open**, **view**, **refinance** and **repay** a
loan from the Danogo front end with the user's own wallet.
**Journey:** borrow **11 fUSDM** from Fluid against **100 ADA** collateral, refinance it into a Dano
Finance loan, then repay that loan in full and unlock the collateral — the same four journeys as
[`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Report_Journey_1_M2.md)
and [`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_UAT_Report_Journey_2_M2.md),
**run by a second tester on a different wallet**.
**Environment:** front end https://preprod.danogo.io, running against **Cardano preprod** and the
**Fluid preprod** smart contracts. Wallet: **Eternl v2.1.5.0**, account *Ngan Wallet (#0)*, address
`addr_test1qqv2pd75qaeye7w8fcddtwd8uk8wmxgr979wx4zcjknswfjpq4xedf86fkzd7ln9escmvdd4s5enl7ma50q4tk3dn8ys027ghs`
— **a different wallet, and a different Eternl installation, from the two walkthroughs in
[`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Report_Journey_1_M2.md) and [`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_UAT_Report_Journey_2_M2.md)**.
**Date:** 2026-09-18, one continuous session of **7 min 29 s**, 08:22:02 → 08:29:31 UTC.
**Screen recording (whole session, unbroken):** [`03-open-refinance-repay-borrow-USDM-collateral-ADA.webm`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/03-open-refinance-repay-borrow-USDM-collateral-ADA.webm)
**Screenshots (frames from that recording):** [`screenshots/journey-3-borrow-USDM-collateral-ADA/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/journey-3-borrow-USDM-collateral-ADA)

**Tester:** ⬜ *to fill:* `____________________` — relation to the delivery team: ⬜ `____________`
**Instructions given:** ⬜ *to fill:* goals only / step-by-step / other · **questions asked during
the session:** ⬜ `___`

> **What this report claims.** That this session happened, in this interface, on this date, from this
> wallet, and that what the interface displayed matches what the chain recorded. Every on-chain
> figure below was re-derived from the public **Koios** API on preprod, not from our backend, and
> every transaction can be opened on preprod Cardanoscan.
>
> **What it does not claim.** The two lines above are left blank on purpose. This session was run by
> someone who did not run the earlier walkthroughs, but a usability claim depends on *who* the
> tester is and *what they were told* — so this package states those facts rather than inferring a
> verdict from them. Until they are filled in, this report is evidence that the four journeys can be
> completed by a second person on a second wallet, and of the defect in §4, and nothing more.

---

## 1. The session, step by step

Timestamps are positions in the recording. *Open* is steps 1–3, *View* is steps 3–4, *Refinance* is
steps 5–8, and *Repay* is steps 9–13.

| # | Time | Step | Expected | Actual result | Screenshot |
|---|---|---|---|---|---|
| 1 | 0:24 | **Borrow Market** — ADA as collateral, Fluid pool, amounts entered | a *Loan Impact* preview before anything is signed | Fluid Flexible **4.00%** (67% LTV) selected; **You Borrow 11 fUSDM** ($11.00), **Collateral 100 ADA** ($30.50, of 1,119.47 available); *Loan Impact* — APR **4.00%**, 2.04%/month, New Collateral $30.50, **New Health Factor 4.00 Healthy**, Deposit 5 ADA, Fee 5 fUSDM ($5.00) | [`01`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/01-borrow-market-11-fUSDM-100-ADA.png) |
| 2 | 0:36 | **Create Loan** → sign in Eternl | one transaction, memo *"Dano Finance: Borrow from Fluid"* | Eternl: 1 transaction to confirm, memo **Dano Finance: Borrow from Fluid**; settled as `69e04600…3b84b8`, block **5,190,728**, 08:23:32 UTC | [`02`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/02-eternl-sign-open.png) |
| 3 | 1:40 | **Portfolio** *(View)* | the new Fluid position appears | wallet `addr_tes…7ghs`; **fUSDM · Fluid Borrow −11 (−$11.00)**; fUSDM · Dano Supply 100 | [`03`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/03-portfolio-after-open.png) |
| 4 | 2:00 | **Loan Details → Refinance via Dano** *(View → Refinance)* | the quote, in full, before signing | Fluid loan: Total Debt **$11.00 / 11 fUSDM**, HF **3.63 Healthy**, APR **4.00%**, collateral **ADA 100 ($30.50)**. Card: **Save 9.84% net cost**; Net cost **4.00% → −5.84%**; Health Factor **3.63 Healthy → 1.87 Healthy**; ✓ *Same loan, Same collateral*; **Fee 2 fUSDM** | [`04`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/04-loan-details-refinance-quote.png) |
| 5 | 2:28 | **Confirm** → sign in Eternl | one transaction, memo *"Dano Finance: Create Loan"* | Eternl: 1 transaction, tags **Contract · Mint · Burn · Memo**, memo **Dano Finance: Create Loan**, signing wallet *Ngan Wallet (#0)* | [`05`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/05-eternl-sign-refinance.png) |
| 6 | 2:46 | **The submit fails** | — | the app shows **"Submit failed: Your wallet may not have finished syncing. Please wait a moment or reload the page, then try again."** with **Retry** and **Close** — see §4, D-1 | [`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-3-borrow-USDM-collateral-ADA/06-submit-failed-retry-prompt.png) |
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

---

## 2. Detailed verification (QC)

All on-chain figures re-derived from the public **Koios** preprod API
(`/tx_info`, `/tx_utxos`, `/tx_metadata`).

### 2.1 Preview values (UI correctness)
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT3-TC-01 | Savings label = net-cost delta | 4.00% − (−5.84%) = **9.84%** | "Save 9.84% net cost" | `04` | ✅ |
| UAT3-TC-02 | Health factor before → after | 3.63 Healthy → 1.87 Healthy | as quoted | `04` | ✅ |
| UAT3-TC-03 | Origination fee shown in the borrowed asset | 2 fUSDM | Fee 2 fUSDM | `04` | ✅ |
| UAT3-TC-04 | Resulting debt = debt + fee | 11 + 2 = **13 fUSDM** | Loan Details after: 13 fUSDM | `04`, `09` | ✅ |

### 2.2 Signing & submission
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT3-TC-05 | Each action builds **one** transaction with a naming memo | memo per action under metadata 674 | open *Borrow from Fluid*, refinance *Create Loan*, repay *Repay Loan* — all three confirmed by Koios `/tx_metadata` | `02`, `05`, `11` | ✅ |
| UAT3-TC-06 | A failed submit does not produce a partial or duplicate transaction | at most one refinance on chain | the wallet's whole history in this window is **three** transactions — one open, one refinance, one repay | Koios `/address_txs` | ✅ |
| UAT3-TC-07 | The failure is recoverable from inside the app | Retry completes the action | Retry → one more signature → settled in block 5,190,735, ~2½ minutes after the first attempt | `06`, `07` | ✅ *(with the defect in §4)* |
| UAT3-TC-08 | Accepted by the validators | scripts pass, within budget | refinance 5,338 B, 3 in → 6 out; repay ex-units 2.4M mem · 764.9M steps (well inside the limit) | `13` | ✅ |

### 2.3 Open — what the chain recorded
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT3-TC-09 | The collateral quoted is the collateral locked | 100 ADA into the Fluid loan script | **100.000000 ₳** at `addr_test1zpyg4esucs…` with the loan's position token | Koios | ✅ |
| UAT3-TC-10 | The amount quoted is the amount borrowed | 11 fUSDM disbursed by the Fluid pool | pool fUSDM **834.000000 → 823.000000** = **11.000000 disbursed** | Koios | ✅ |
| UAT3-TC-11 | The borrower's own outlay is the collateral plus costs | ≈ 100 ADA + min-UTxO + fee | wallet ADA falls by **101.988262**, of which **100.000000** is the collateral now locked | Koios | ✅ |

### 2.4 Refinance — what the chain recorded
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT3-TC-12 | The Fluid loan UTxO is consumed with its collateral | 100 ADA leaves the Fluid script | input `addr_test1zpyg4esucs…` **100.000000 ₳** + position token | Koios | ✅ |
| UAT3-TC-13 | **Collateral continuity** | the same 100 ADA arrives at the Dano loan contract | output `addr_test1zzx7xnch…` **100.000000 ₳** + new loan token `991d751a…` | Koios | ✅ |
| UAT3-TC-14 | The Fluid debt is settled in the same transaction | the lender is paid in full | **fUSDM 11.000002** to `addr_test1qr9ew225…` | Koios | ✅ |
| UAT3-TC-15 | Quoted fee = fee paid | 2 fUSDM → exactly 2.000000 on chain | **fUSDM 2.000000** to the fee address `addr_test1qrrrfm89…` | `04`, Koios | ✅ |
| UAT3-TC-16 | The pool disbursed exactly what the loan is for | disbursement = settlement + fee (+ dust) | pool fUSDM **5,464.211244 → 5,451.211236** = **13.000008** = 11.000002 + 2.000000 + 0.000006 | Koios | ✅ |
| UAT3-TC-17 | The borrower needs no capital of their own | only fee and min-UTxO leave the wallet | wallet net **−4.688518 ₳** = network fee 1.572388 + 3.116130 of min-UTxO ADA; **no fUSDM leaves the wallet** | Koios | ✅ |

### 2.5 Repay — what the chain recorded
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT3-TC-18 | Quoted repay amount = amount paid | 13.00001 fUSDM quoted | pool **+12.999904** + fee address **+0.000105** = **13.000009 fUSDM**, the figure the wallet also shows | `10`, `12`, Koios | ✅ |
| UAT3-TC-19 | Full repayment releases all the collateral | 100 ADA returns, less the transaction's costs | wallet **+97.587398 ₳** = 100 − 1.266142 network fee − 1.146460 to the fee address | Koios | ✅ |
| UAT3-TC-20 | The UTxO consumed is the one the refinance created | same contract, same loan token | input `addr_test1zzx7xnch…` 100 ₳ + loan token `991d751a…`, both created by `a04fe52e…` | Koios | ✅ |
| UAT3-TC-21 | The borrower's title to the loan is burned | the Borrower NFT the refinance paid to the wallet is destroyed | `185a63e4…` is an **input** and appears in **no output** — the wallet's own history shows it as **−1** | `12`, Koios | ✅ |
| UAT3-TC-22 | Only the intended loan is touched | the tester's other positions are untouched | their second Dano loan (203.32759 ADA) is still open at the end of the session — §3 | `08`, Koios | ✅ |

**22 checks, 22 hold.**

---

## 3. What this session covers

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

---

## 4. Defects and friction found

| ID | Severity | What happened |
|---|---|---|
| **D-1** | **major (recoverable)** | **The first refinance submit failed.** After the signature, the app showed *"Submit failed: Your wallet may not have finished syncing. Please wait a moment or reload the page, then try again."* The tester clicked **Retry**, signed a second time, and the refinance settled. Cost: one extra signature and about 2½ minutes. Nothing was double-submitted — the chain holds exactly one refinance. The message is honest but puts the cause on the user's wallet; the tester had no way to tell whether the first signature had cost them anything. ⬜ *root cause to fill* |
| **D-2** | minor | While the repay preview loads, the dialog reads *"Minimum amount to repay is **--** fUSDM"* and shows skeleton bars for several seconds before the real numbers appear (`10` is the loaded state; the skeleton is visible in the recording at 5:08–5:12). |
| **D-3** | observation | The repay of the Dano loan took **~1 min 40 s** between signing and the confirmation clearing (5:35 → 7:08), with the dialog showing *"Waiting for confirmation…"* throughout. The block itself settled at 08:29:15; the wait is the app polling, not the chain. |
| **F-1** | carried over | The Borrow Market *Loan Impact* block again quoted **Deposit 5 ADA** and **Fee 5 fUSDM ($5.00)** at open, while the loan that resulted was **11 fUSDM of debt for 11.000000 fUSDM disbursed** — no fee deducted, none capitalised. Same open item as [`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Report_Journey_1_M2.md) F-1 and [`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_UAT_Report_Journey_2_M2.md) F-1. |

### Stability
| Check | Result |
|---|---|
| Crash, white screen or unrecoverable state | ✅ none — the one failure (D-1) recovered inside the app |
| Values displayed matched the chain | ✅ fee, health factor, debt, collateral and repayment all match — §2 |
| Wallet signing | ✅ all three actions signed and submitted (four signatures, because of D-1) |
| Each action reached a confirmation the tester could verify independently | ✅ wallet history and public explorer, steps 12–13 |

---

## 5. Conclusion

A second tester, on a second wallet and a second Eternl installation, completed **all four approved
journeys on one loan** through the preprod front end in a single 7½-minute session, and verified the
result themselves in their wallet and on a public block explorer. Twenty-two QC checks hold: the
collateral quoted is the collateral locked, the fee quoted is the fee paid, the health factor quoted
is the health factor the loan carries, the pool disbursed exactly settlement + fee, and the
repayment released the collateral and burned the borrower's title to the loan.

The session also produced the first **defect** in this package's UAT evidence: the refinance failed
to submit on the first attempt and needed a retry (D-1). It is reported here rather than edited out,
and the recording shows it in full.
