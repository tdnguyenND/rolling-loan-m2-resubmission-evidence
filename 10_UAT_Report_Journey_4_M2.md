# Milestone 2 — User Acceptance Test Report · Journey 4

**Feature:** Rolling Loan ("Refinance via Dano") — **open**, **view** and **refinance** a loan from
the Danogo front end with the user's own wallet.
**Journey:** borrow **905.004 ADA** from Fluid against **98,327.69 fUSDM** collateral — an amount
large enough that it **fills two Fluid pools and opens two separate loans** — then refinance **both**
of them into Dano Finance loans. This is the *open → view → refinance* path of
[`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Report_Journey_1_M2.md),
[`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_UAT_Report_Journey_2_M2.md)
and [`09`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/09_UAT_Report_Journey_3_M2.md),
**at a size that exercises the multi-pool case**, on a **third wallet**. It does **not** cover repay —
see §3.
**Environment:** front end https://preprod.danogo.io, running against **Cardano preprod** and the
**Fluid preprod** smart contracts. Wallet: Eternl, account *Deploy Oracle (#0)*, address
`addr_test1qr20zc2v0fyxwjf42jy8vjctfpqrc9f3md3cwe6fccsza9g5q42ut28rqch23fj0j4hp479jdkeyn2mpv8tmxk3h5jeqsm9xmy`
(the interface shows it as `addr_tes…9xmy`) — **a third wallet**, different from the one used in
[`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Report_Journey_1_M2.md)/[`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_UAT_Report_Journey_2_M2.md)
and from the one in [`09`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/09_UAT_Report_Journey_3_M2.md).
**Date:** 2026-09-18, one continuous session of **3 min 50 s**, ≈08:33 → 08:37 UTC.
**Screen recording (whole session, unbroken):** [`04-open-and-two-refinances-borrow-ADA-collateral-USDM.mp4`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/04-open-and-two-refinances-borrow-ADA-collateral-USDM.mp4)
**Screenshots (frames from that recording):** [`screenshots/journey-4-borrow-ADA-collateral-USDM/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/journey-4-borrow-ADA-collateral-USDM)
— each frame is unaltered except that the browser's **bookmarks bar** has been cropped out; the
address bar is kept, and the recording linked above is the unedited source for every one of them.

**Tester:** ⬜ *to fill:* `____________________` — relation to the delivery team: ⬜ `____________`
**Instructions given:** ⬜ *to fill:* goals only / step-by-step / other · **questions asked during
the session:** ⬜ `___`

> **What this report claims.** That this session happened, in this interface, on this date, from this
> wallet, and that what the interface displayed matches what the chain recorded. Every on-chain
> figure below was re-derived from the public **Koios** API on preprod, not from our backend, and
> every transaction can be opened on preprod Cardanoscan.
>
> **What it does not claim.** The two lines above are left blank on purpose, as in
> [`09`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/09_UAT_Report_Journey_3_M2.md):
> a usability claim depends on *who* the tester is and *what they were told*, so this package states
> those facts rather than inferring a verdict from them. It also does **not** claim repay coverage —
> this session never repaid, and both Dano loans were still open when the recording stopped.

---

## 1. The session, step by step

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

---

## 2. Detailed verification (QC)

All on-chain figures re-derived from the public **Koios** preprod API
(`/address_txs`, `/tx_info`, `/tx_utxos`, `/tx_metadata`).

### 2.1 Preview values (UI correctness)
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
here; that check is made in
[`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Report_Journey_1_M2.md),
[`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_UAT_Report_Journey_2_M2.md) and
[`09`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/09_UAT_Report_Journey_3_M2.md),
where the loan is opened again after the refinance.

### 2.2 Signing & submission
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT4-TC-11 | Each action builds **one** transaction with a naming memo | memo per action under metadata label 674 | open **"Dano Finance: Borrow from Fluid"**, both refinances **"Dano Finance: Create Loan"** — all three confirmed by Koios `/tx_metadata` | `02`, `06`, `11` | ✅ |
| UAT4-TC-12 | The net movement Eternl shows is the net movement the chain records | +t₳ 896 / −t₳ 4.36 / −t₳ 4.36 | **+896.997892 ₳**, **−4.364925 ₳**, **−4.355241 ₳** | `02`, `06`, `11`, Koios | ✅ |
| UAT4-TC-13 | The asset shown in the signing dialog is the token actually minted | the Borrower NFT of that refinance | `04df3b105287bd665af89dd3c7192aa3be444cb047398ebb221d7cc6` and `589391707a6d5d06036d61c9f1d82ec851f1a3ef983ff308ef555fdb`, both minted **+1** under policy `8de34f17…` | `06`, `11`, Koios | ✅ |
| UAT4-TC-14 | Nothing is double-submitted | one transaction per action | the wallet's whole history in this window is **exactly three** transactions — one open, two refinances | Koios `/address_txs` | ✅ |
| UAT4-TC-15 | Accepted by the validators, within budget | scripts pass | open 5,188 B / fee 0.894608 ₳ (11 in → 5 out); refinance 1 6,642 B / 1.619065 ₳ (3 in → 6 out); refinance 2 6,744 B / 1.627078 ₳ (4 in → 6 out) | Koios | ✅ |

### 2.3 Open — what the chain recorded
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT4-TC-16 | One borrow, two pools, two loans | two pool UTxOs consumed, two loan UTxOs created | pool UTxOs of **885.004000 ₳** and **20.000000 ₳** consumed and their pool tokens burned; **two** loan UTxOs created at `addr_test1zpyg4esucs…`, each with its own position token under policy `8dfb447e…` | Koios | ✅ |
| UAT4-TC-17 | The wallet gives up exactly the quoted collateral | −98,327.69 fUSDM | wallet fUSDM **9,878,327.691214 → 9,780,000.001214** = **−98,327.690000** | Koios | ✅ |
| UAT4-TC-18 | The borrower receives the borrow less chain costs only | no origination fee to any Dano or Fluid address | **+896.997892 ₳** = 905.004000 − 0.894608 (network fee) − 2.586000 (two lender-NFT min-UTxOs) − 4.525500 (two collateral min-UTxOs). **Nothing else left the wallet** — see D-1 | Koios | ✅ |
| UAT4-TC-19 | Each loan is titled to somebody | a lender NFT and a borrower NFT per loan | two lender NFTs (`bcd713bb…`) to `addr_test1qr9ew225…`, two borrower NFTs (`eadc69a5…`) to the tester's wallet | Koios | ✅ |

### 2.4 Refinance — first loan (20 ADA), `6ab3bde0…`
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT4-TC-20 | The Fluid loan UTxO is consumed with its collateral | that loan's collateral leaves the Fluid script | input `addr_test1zpyg4esucs…` **2.254130 ₳ + 2,172.978020 fUSDM** + position token `8dfb447e….4a22b7f2…`, which is **burned** | Koios | ✅ |
| UAT4-TC-21 | **Collateral continuity** | the same fUSDM arrives at the Dano loan contract | output `addr_test1zzx7xnch…` **5.000000 ₳ + 2,172.978020 fUSDM** — the same amount, to the decimal | Koios | ✅ |
| UAT4-TC-22 | The Fluid debt is settled in the same transaction | the lender is paid in full | **20.000003 ₳** to `addr_test1qr9ew225…` | Koios | ✅ |
| UAT4-TC-23 | Quoted fee = fee paid | 2 ADA → exactly 2.000000 on chain | **2.000000 ₳** to the fee address `addr_test1qrrrfm89…` | `05`, Koios | ✅ |
| UAT4-TC-24 | The Dano pool disbursed exactly what the loan is for | disbursement = settlement + fee (+ dust) | pool **9,311.605320 → 9,289.605307 ₳** = **22.000013** = 20.000003 + 2.000000 + 0.000010 | Koios | ✅ |
| UAT4-TC-25 | The borrower needs no capital of their own | only fee and min-UTxO leave the wallet | wallet net **−4.364925 ₳** = 1.619065 network fee + 2.745870 min-UTxO top-up − 0.000010 dust returned; **no fUSDM leaves the wallet** | Koios | ✅ |
| UAT4-TC-26 | The borrower gets title to the new loan | a Borrower NFT is paid to the wallet | `8de34f17….04df3b10…` minted **+1** to the tester's address — the id Eternl displayed before signing | `06`, Koios | ✅ |

### 2.5 Refinance — second loan (885 ADA), `3cbb01a7…`
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

### 2.6 The interface against the chain (View)
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT4-TC-35 | The portfolio's split between protocols matches the chain | one loan's collateral at Dano, one still at Fluid | UI: *Fluid · Supply* **$96,154.71**, *Dano · Supply* **$2,172.98**; chain: **96,154.711980** fUSDM at the Fluid loan script, **2,172.978020** at the Dano loan contract — equal to the cent | `08`, Koios | ✅ |
| UAT4-TC-36 | A refinanced loan leaves the Fluid loan list | one row left, the 885 ADA loan | *ADA · Loans* shows a single row after the first refinance | `04`, `09` | ✅ |
| UAT4-TC-37 | Interest accrues between open and refinance | settlement slightly above principal, of the order of 4.00%/yr | 20.000000 → **20.000003** over 80 s and 885.004000 → **885.004206** over 195 s; 4.00% simple interest over those spans would be 0.000002 and 0.000219 | Koios | ✅ |

**37 checks, 37 hold.**

---

## 3. What this session covers

| Journey | Covered | How |
|---|---|---|
| **Open** | ✅ | steps 1–3, settled as `5527f624…` — and specifically the **multi-pool** open: one borrow, two loans |
| **View** | ✅ | steps 4, 8, 9 — loan list before and after, loan details on both loans, portfolio split across two protocols |
| **Refinance** | ✅ | steps 5–7 (`6ab3bde0…`) and 10–11 (`3cbb01a7…`) — **two** refinances in one session |
| **Repay** | ❌ | **not exercised.** Both Dano loans were still open when the recording stopped. Repay evidence is in [`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Report_Journey_1_M2.md) §1 steps 11–13, [`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_UAT_Report_Journey_2_M2.md) §1 steps 11–13 and [`09`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/09_UAT_Report_Journey_3_M2.md) §1 steps 10–13 |
| **Self-verification** | ❌ | the tester did not open their wallet history or a block explorer during the session; the on-chain column above is our verification, done afterwards from Koios |

**What this journey adds that the other three do not:** the **multi-pool open** — a single borrow that
the interface warns may "fill several pools and open more than one loan", and that does exactly that
— and the **second refinance in the same session**, showing that refinancing one loan leaves the
tester's other positions untouched (UAT4-TC-35, TC-36).

---

## 4. Defects and friction found

| ID | Severity | What happened |
|---|---|---|
| **D-1** | minor (carried over as **F-1**) | The Borrow Market *Loan Impact* block quoted **Deposit 5 ADA** and **Fee 9.05 ADA ($2.76)** before the borrow. Neither appears on chain: the whole difference between the **905.004000 ₳** the pools paid out and the **896.997892 ₳** the wallet received is network fee (0.894608) plus min-UTxO (7.111500), and the resulting debt is **905.004 ADA**, not 914.054. Same open item as [`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Report_Journey_1_M2.md) F-1, [`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_UAT_Report_Journey_2_M2.md) F-1 and [`09`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/09_UAT_Report_Journey_3_M2.md) F-1 — now with a third denomination (ADA) and a much larger loan. ⬜ *root cause to fill* |
| **D-2** | minor (carried over as **F-2**) | The two **Fluid borrower NFTs** minted at open (`eadc69a5….4a22b7f2…` and `eadc69a5….647d5a96…`) are **still in the wallet** after both refinances — neither refinance burns them, although both Fluid loans are gone. Harmless (each sits in a 1.245590 ₳ UTxO) but the wallet now shows two tokens for positions that no longer exist. |
| **D-3** | minor | The repay/loan dialogs and the loan list show the second loan as **885 ADA**; its actual principal is **885.004000 ADA**, and it settled at **885.004206**. Rounding in the display only — every figure the user confirms before signing is exact. |
| **D-4** | observation | Both Dano loan UTxOs carry a loan token with the **same** asset name (`8de34f17….ea5041c0ae6b0969…`); the two loans are told apart by their distinct **Borrower NFTs** (`04df3b10…`, `58939170…`). This looks like a pool identifier rather than a per-loan id, and the loans are separable either way — ⬜ *for the team to confirm as intended*. |
| **D-5** | observation | Loan Details quotes the health factor falling from **197** to **32.3** / **35.5** on refinance, with *both* labelled **Healthy**, next to the line *"Same loan, Same collateral"*. Nothing in the card explains why an unchanged position's health factor drops six-fold; a user cannot tell from the screen whether the two numbers are on the same scale. This session did not re-open the Dano loans, so the resulting health factor is not confirmed here either — ⬜ *for the team to explain in the UI, or correct*. |

### Stability
| Check | Result |
|---|---|
| Crash, white screen or unrecoverable state | ✅ none — no failed submit in this session, unlike [`09`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/09_UAT_Report_Journey_3_M2.md) D-1 |
| Values displayed matched the chain | ✅ borrow, collateral, fee, debt, collateral split and both settlements all match — §2 |
| Wallet signing | ✅ all three actions signed and submitted, three signatures for three transactions |
| Multi-pool handling | ✅ the interface warned, opened two loans, listed them separately, and refinanced them one at a time without touching the other |

---

## 5. Conclusion

A third wallet completed **open, view and refinance** through the preprod front end in a single
3 min 50 s session, at a size that forced the **multi-pool** path: one 905.004 ADA borrow filled two
Fluid pools and opened two loans, and both were then refinanced into Dano Finance loans. Thirty-seven
QC checks hold: the amount quoted is the amount disbursed, the collateral quoted is the collateral
locked, each loan's collateral arrives at the Dano contract to the decimal, the 2 ADA fee quoted on
each card is the 2 ADA paid, the Dano pool disbursed exactly settlement + fee on both, and the
portfolio's split between Fluid and Dano matches the chain to the cent.

This journey does **not** cover repay, and the tester did not verify the result themselves — both are
stated plainly in §3 rather than papered over. Five items are recorded in §4, of which two (D-1, D-2)
are open questions this package has already raised in the other three journeys.
