# The Four Approved User Journeys, and the State They Left in the Application

**Project** 1400107 · **Milestone 2** · Rolling Loan / "Refinance via Dano"

> *"I would like you to provide evidence demonstrating that the four approved user journeys (view,
> open, repay and refinance) were each successfully tested through the Eternl-connected front end,
> and provide the corresponding integration-test results."* — reviewer

This file answers that request in two parts. **§1** takes the four journeys one at a time, with the
user's path through the interface and the evidence behind each. **§2** shows what the transactions
left behind — the resulting loans in the application — and reconciles every figure on that screen
against the Cardano ledger.

The on-chain half of the evidence is in
[`05_ONCHAIN_TRANSACTIONS_AND_REPAY.md`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md). The
integration-test results as they stand today are in
[§1.5](#15-current-integration-test-results) below; the manual refinance journey they came from,
step by step with screenshots, is in
[`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) §1, kept unedited as an
archive.

---

## 1. The four approved user journeys via Eternl

> *"I would like you to provide evidence demonstrating that the four approved user journeys (view,
> open, repay and refinance) were each successfully tested through the Eternl-connected front end,
> and provide the corresponding integration-test results."* — reviewer

### 1.1 The two classes of evidence

| Class | What it is | Who can check it |
|---|---|---|
| **E1 — captured in the interface** | The journey performed in the live app with the wallet connected, screenshotted and/or recorded. | **us** — we captured it |
| **E2 — On-chain settlement** | A Cardano mainnet transaction, publicly verifiable, accepted by live validators, carrying this application's own metadata under label 674. | **anyone**, from the chain |

E2 needs no cooperation from us. E1 does, and is marked as such wherever it appears. Where a journey
has no interface capture of its own, the cell says so rather than describing a screen — see §1.4.

### 1.2 The matrix

| Journey | E1 · Eternl front end | E2 · On-chain mainnet |
|---|---|---|
| **1. View** — see my loans, debt, collateral, health factor, APR | ✅ *My Account → Loans*, captured from the live app on two connected mainnet wallets — [§2.1](#21-the-two-screenshots) | ✅ every displayed borrowed amount reconciled to the ledger — [§2.4](#24-row-by-row-the-displayed-amount-is-the-ledger-amount) |
| **2. Open** — originate a loan against collateral | ✅ **full walkthrough captured, screen by screen** — *Borrow Market* with the amounts entered → Eternl signing *Dano Finance: Borrow from Fluid* → the new loan in *Portfolio* and *My Account*, in two opposite asset directions on preprod, and once at a size that fills two pools and opens two loans — [`00` §C](./00_POA_SUBMISSION_FORM.md#four-complete-journeys-captured-screen-by-screen) · the **seven mainnet** originations below have no sheet capture of their own — §1.4 · ✅ *corroborating:* the loans they opened are listed in the app and still open — §2.3 | ✅ **7 mainnet transactions of its own** — six *"Dano Finance: Borrow from Fluid"*, one *"Create Loan"* opening a Dano loan directly — [`05` §1.1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans) · ✅ each mints that loan's position token and locks its collateral · ✅ **5 distinct Borrower NFTs** minted by the refinances — [`05` §2.1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#21-two-danogo-tokens-and-which-one-identifies-a-loan) |
| **3. Repay** — settle a loan | ✅ **captured screen by screen** on preprod — the *Repay Loan* quote (*"27.000474 ADA … ↘ Repaid"*), the Eternl dialog burning the loan token and returning the collateral, and the settled transaction — **twice**, [`e134b85e…1416`](https://preprod.cardanoscan.io/transaction/e134b85eb5631391089598adefd8f06024307fc7d41ec4ac8d421276d8681416) and [`bae9a9c9…5b7d`](https://preprod.cardanoscan.io/transaction/bae9a9c9e0e4f63657d43cc21b4ab071e82d57919733ddb8908a958937c75b7d), each closing the very loan its walkthrough had just opened and refinanced — [`06` Journey 1](./06_UAT_Reports_Four_Journeys_M2.md#journey-1--borrow-25-ada-against-100-fusdm) §1.1 and [`06` Journey 2](./06_UAT_Reports_Four_Journeys_M2.md#journey-2--borrow-11-fusdm-against-100-ada) §2.1, steps 11–13 · ✅ **and once inside a continuous screen recording** — [`06` Journey 3](./06_UAT_Reports_Four_Journeys_M2.md#journey-3--a-second-tester-on-a-second-wallet) §3.1, where *Repay* is steps 9–13 of one unbroken 7 min 29 s take, [`a4bebdb7…3fbeed`](https://preprod.cardanoscan.io/transaction/a4bebdb7ca185ffc1ce4fc22873e9e3d9cefa80e0d1e0d8ad698e6b1813fbeed) · ⚠️ in journeys 1 and 2 the repay is screenshots only, not on those recordings, and no journey captures the app state *after* the repay · the signed mainnet repayment transactions are listed in [`05` §5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-two-direct-repay-flows-and-repayment-within-a-refinance) — §1.4 · ✅ *corroborating:* the settled loans are **absent** from the post-refinance screens — §2.5 · ✅ *corroborating:* **Fluid's own dashboard marks them `REPAID`** — [`05` §5.3](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#53-fluids-own-records-say-the-loans-are-repaid) | ✅ **3 mainnet transactions of its own**, debt paid from the borrower's own funds — `17c23dde…`, `ea823365…`, and **`77748bd9…` on Danogo's own `Repay Loan` path**, which burns the borrower's title to the loan — [`05` §5.5–§5.6](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#55-repaying-an-external-loan-on-mainnet-two-transactions) · ✅ **5 Fluid loans repaid in full** as the settlement leg of a refinance · ✅ the counterparty's own indexer reports **seven** loans `loan_repaid`, `remainingDebt: 0` |
| **4. Refinance** — roll a Fluid loan into a Dano loan | ✅ **full walkthrough**: refinance card and preview → **Eternl signing dialog with inputs and outputs** → confirmation → portfolio after, screenshotted end-to-end — [`01` §1](./01_Integration_Test_Report_M2.md) · ✅ **captured again, four times over, in the preprod session recordings** — [`06` At a glance](./06_UAT_Reports_Four_Journeys_M2.md#at-a-glance) · the **resulting** Dano loans shown in the app — §2 | ✅ **5 signed mainnet refinances**, 2 wallets, 3 collateral assets, 2 borrowed assets, 9 days — [`05` §1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions) |

### 1.3 The user path for each journey

Described as the *user's* path, because that is what is being evaluated — not the code path.

**Journey 1 — View**

```
  Open v3.danogo.io/?ff=fluid-refinance  →  Connect Wallet  →  Eternl  →  approve
      →  Portfolio                    : all positions, allocation %, borrow rows in negative tone
      →  My Account → Loans           : one row per open loan, per protocol
      →  Manage                       : Loan Details popup — debt, collateral list, APR,
                                        health factor with band label, utilisation
```

Nothing is signed. The user reads their real on-chain position. It is the journey where a wrong
number is most damaging and least visible, which is why §2.4 reconciles it figure by figure against
the ledger rather than merely screenshotting it.

**Journey 2 — Open**

```
  Borrow Market  →  pick a market  →  Borrow
      →  choose collateral, enter amount
      →  preview: max borrow, projected health factor, origination fee, loan impact
      →  Confirm  →  Eternl signature prompt  →  submit  →  confirmation + tx hash
```

On-chain this journey locks the collateral and mints the loan's position token. It ran **seven**
times on mainnet in its own right — six times against Fluid and once directly against a Dano pool —
and additionally as the second leg of each refinance. See
[`05` §1.1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans).

**Journey 3 — Repay**

```
  My Account → Loans  →  Manage  →  Repay
      →  amount pre-fills to min(wallet balance, full repayment)
      →  preview: new debt, new health factor, "↘ Repaid" badge on full repayment
      →  Confirm  →  Eternl signature prompt  →  submit  →  confirmation
```

The recorded repayment examples cover **Fluid** and **Dano** loans: this journey ran three times on
mainnet in its own right — twice against Fluid and once against Danogo's own contracts — and
additionally as the settlement leg of each refinance. Protocol-specific behaviour outside those
examples is outside the scope of this resubmission. See
[`05` §5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-two-direct-repay-flows-and-repayment-within-a-refinance).

**Journey 4 — Refinance**

```
  My Account → Loans  →  a Fluid loan row shows "Dano save 4.56% net cost"
      →  Manage  →  Loan Details  →  Refinance via Dano  (card expands in place)
      →  preview: Net cost (Fluid → Dano) 4.00% → −0.56%
                  Health Factor 1.77 Healthy → 1.52 Fair
                  Fee 2 ADA
                  ✓ Same loan, Same collateral
                  "You don't need extra funds to close your Fluid loan."
      →  Confirm  →  Eternl shows ONE transaction, labelled "Dano Finance: Create Loan"
      →  sign  →  "Transaction confirmed" + hash
      →  Portfolio: the Fluid position is gone; a Dano position stands in its place
```

The preview numbers above are from the journey that produced tx `c426d9fa…`. On-chain, that
transaction settled 12.000102 ADA of Fluid debt and paid exactly 2.000000 ADA of origination fee,
carrying DJED 6.000000 across. **The number the user was shown is the number that settled** — which
is the concrete form of "no data mismatch". Step by step, with screenshots, in [`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) §1.

### 1.4 The scope of these claims

Stated explicitly, so the reviewer does not have to work out where each claim stops:

1. **What the walkthroughs establish — and what they do not.** Every journey was walked end-to-end
   through the Eternl-connected front end during this milestone, and the open, repay and refinance
   sessions settled on mainnet fifteen times from two separate wallets. The previous submission's
   row *"Tester completed the flow without external guidance ✅ Yes"* was a self-assessment; this
   file does not carry it as a verdict, and instead reports the user path and the evidence for each
   journey, which the reviewer can check directly. See
   [`08_CORRECTIONS.md`](./08_CORRECTIONS.md) C‑2.
2. **Interface captures.** The refinance journey is captured end-to-end — loan row → refinance card
   → Eternl signing dialog with inputs and outputs → confirmation → portfolio after
   ([`01` §1](./01_Integration_Test_Report_M2.md)), and each of the four preprod session
   recordings walks the same flow end to end ([`06`](./06_UAT_Reports_Four_Journeys_M2.md#at-a-glance)).
   The previous submission's demo video is withdrawn as evidence — [`07` §A](./07_ACCEPTANCE_CRITERIA_M2.md#a-milestone-outputs).
   The **open** journey is captured end-to-end as well, in the four
   preprod walkthroughs added for this resubmission — [`00` §C](./00_POA_SUBMISSION_FORM.md#four-complete-journeys-captured-screen-by-screen) — which
   run *Open → View → Refinance → View* with a screenshot of every screen and a screen recording of
   the whole session, one of them at a size that fills **two** pools and opens **two** loans
   ([`06` Journey 4](./06_UAT_Reports_Four_Journeys_M2.md#journey-4--one-borrow-two-pools-two-loans)); the seven mainnet **Open** transactions themselves have
   no sheet capture and are carried by their transactions and by the resulting state in the app (§2).
   The **repay** journey is now captured screen by screen as well — quote, wallet dialog and settled
   transaction, on three of the loans the walkthroughs created
   ([`06` Journey 1](./06_UAT_Reports_Four_Journeys_M2.md#journey-1--borrow-25-ada-against-100-fusdm) §1.1 and [`06` Journey 2](./06_UAT_Reports_Four_Journeys_M2.md#journey-2--borrow-11-fusdm-against-100-ada) §2.1, steps 11–13,
   and [`06` Journey 3](./06_UAT_Reports_Four_Journeys_M2.md#journey-3--a-second-tester-on-a-second-wallet) §3.1 steps 10–13) — and only
   [`06` Journey 3](./06_UAT_Reports_Four_Journeys_M2.md#journey-3--a-second-tester-on-a-second-wallet) records it inside a screen recording; the app state after
   each repayment was not captured. Its mainnet evidence remains the three signed repayments in
   [`05` §5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-two-direct-repay-flows-and-repayment-within-a-refinance).
3. **Journeys are not double-counted.** Five transactions each perform a repay *and* a refinance.
   That is five refinances and five settlement legs, not ten demonstrations. The three separately
   evidenced **Repay** transactions are `17c23dde…`, `ea823365…` and `77748bd9…`; the seven
   separately evidenced **Open** transactions are in
   [`05` §1.1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans).
4. **"View" has no transaction by nature** — reading a position does not produce one. Its evidence
   is §2, where every displayed figure is re-derived from the ledger.
5. **On the word "Eternl":** the chain records that a wallet's key authorised each transaction, not
   which wallet software requested the signature. The Eternl signing dialog is directly visible in
   [`screenshots/03-eternl-inputs-outputs.png`](./screenshots/03-eternl-inputs-outputs.png) for the
   refinance journey. For the other journeys, "through the Eternl-connected front end" is our
   statement about how they were performed, and we mark it as ours rather than as something the
   ledger proves.
6. **The automated-test figures in [`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) §3 are our own reporting**, not
   third-party-checkable evidence. They are reported because the milestone asks for
   integration-test results; the primary verifiable evidence is the ledger and Fluid's own records.

---

---

### 1.5 Current integration-test results

Restated here so that nothing a reviewer needs to rely on lives only inside a superseded file.
[`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) is kept **unedited**, as an
archive of the previous submission; where its detail is still good it is linked below, and where it
is not, this table says so.

| What was tested | Current result | Detail |
|---|---|---|
| **25 transaction QC checks** against the executed refinance (TC‑01 … TC‑25) | **24 of 25 hold.** TC‑24 does **not**: the transaction it cites as the zero-origination-fee example, `88579a30…`, pays a **2 ADA** fee — [`08`](./08_CORRECTIONS.md) C‑1. The zero-fee case is `0e26cc58…`, which has no fee leg at all | [`01` §2](./01_Integration_Test_Report_M2.md) |
| **The refinance-surface tests** (FN‑I7, FN‑I9 … FN‑I14, FN‑J10), Tier‑2 with a connected wallet — **no-sign**: the wallet's balance/UTxO reads are real, but `signTx` and `submitTx` are stubbed, so nothing is signed or broadcast ([`07` §C](./07_ACCEPTANCE_CRITERIA_M2.md#c-what-the-automated-suite-does-and-does-not-prove)) | **All 7 in scope pass** on mainnet with the feature flag on; **0 of 8** with it off, which is how the deployment ships. The previous submission's **8 / 8 is withdrawn** — two of those eight had never reached their own assertion | [`08` C‑6](./08_CORRECTIONS.md) |
| **The Loan Details suite** — Tier‑2, against the live **mainnet** app with the flag on. 98 tests run, **87 in scope** (2 Surf and 4 Liqwid are other protocols, 5 are display-level — [`08` C‑6](./08_CORRECTIONS.md) says which and why) | **All 87 in scope have passed**, across three runs rather than one; C‑6 gives the sequence. Unscoped, the last full run was 83 of 98 | [`08` C‑6](./08_CORRECTIONS.md) |
| **The `[Create loan]` and `[Repay]` suites** — 120 tests across five protocol specs | **74 pass / 27 fail / 19 skipped**; 15 failures under *Create loan*, 12 under *Repay*. Two are the same defect as **OI‑1** — the Fee line a Fluid borrow shows but never charges | [`08` C‑6](./08_CORRECTIONS.md) |
| **Manual end-to-end refinance journey** on mainnet, step by step | pass — loan row → refinance card → Eternl signing dialog with inputs and outputs → confirmation → portfolio after | [`01` §1](./01_Integration_Test_Report_M2.md) |
| **Four manual end-to-end walkthroughs** on preprod, *Open → View → Refinance → View*, in opposite asset directions, on three Eternl accounts, one of them filling two pools | pass — and the fee, health factor and resulting debt the interface quoted **before** signing are the ones the chain holds afterwards | [`00` §C](./00_POA_SUBMISSION_FORM.md#four-complete-journeys-captured-screen-by-screen) · [`06` Journey 1](./06_UAT_Reports_Four_Journeys_M2.md#journey-1--borrow-25-ada-against-100-fusdm) · [`06` Journey 2](./06_UAT_Reports_Four_Journeys_M2.md#journey-2--borrow-11-fusdm-against-100-ada) · [`06` Journey 3](./06_UAT_Reports_Four_Journeys_M2.md#journey-3--a-second-tester-on-a-second-wallet) · [`06` Journey 4](./06_UAT_Reports_Four_Journeys_M2.md#journey-4--one-borrow-two-pools-two-loans) |
| **The four journeys, per journey** | evidence and result for each, one row apiece | §1.2 above |

**The automated suite reaches all four journeys.** All 87 Loan Details tests in scope have passed;
*open* ran 70 tests with 15 failing and *repay* 50 with 12, most of those the suite declaring its own
Tier‑2 limits. No product defect is outstanding inside the reported scope.
[`07` §B.2](./07_ACCEPTANCE_CRITERIA_M2.md#b2-ac4-what-is-automated-and-what-is-not) scores the
criterion as partly met on exactly that ground, and
[`08` C‑6](./08_CORRECTIONS.md) records why the previously published “8 / 8” is withdrawn.

The first three rows are our own reporting about our own work, and are named as such. The
primary verifiable evidence is the ledger and the counterparty's records — column E2 of §1.2.

## 2. The post-refinance state in the app, reconciled to the ledger

[`05_ONCHAIN_TRANSACTIONS_AND_REPAY.md`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md) proves what the five transactions **did**. This section
proves what they **left behind**, and that the numbers the application shows a user today are the
numbers the Cardano ledger actually holds. A transaction hash shows that a validator accepted
something; it does not show that the borrower can then open the app, see their loan, and recognise
it as theirs.

Everything below is read from the public **Koios API**, as **live** state rather than as history —
so the loans age: the principal is fixed, the accrued interest grows.

### 2.1 The two screenshots

Captured on **2026‑09‑10** from the deployed application, with each wallet connected through the
CIP‑30 browser extension. Both are unedited full-window captures — address bar included, nothing
removed. Every figure reconciled in §2.4 is read from the **Cardano ledger**, not from the
application, so the reconciliation does not depend on which host served the page.

| Screenshot | Wallet shown in the app | Loans listed |
|---|---|---|
| [`my-account-loans-W1`](./screenshots/screens/my-account-loans-W1-addr1q80xx.png) | `addr1q80…edcr` | **2** |
| [`my-account-loans-W2`](./screenshots/screens/my-account-loans-W2-addr1q8ex.png) | `addr1q8e…4rfq` | **4** |

![My Account — wallet W1, two loans](./screenshots/screens/my-account-loans-W1-addr1q80xx.png)

![My Account — wallet W2, four loans](./screenshots/screens/my-account-loans-W2-addr1q8ex.png)

### 2.2 The wallets in the screenshots are the wallets that signed

The truncated addresses in the app's header expand to the two addresses that appear as signers in
the five mainnet transactions:

| | Address, in full | Signed |
|---|---|---|
| **W1** | `addr1q8009gf2f66x5nnk3xd7f3kagn3avqtyhk5uf4zhnejmjrw7lqahdkjjknfuxdj9kevvyqmlu3zyx3x547dqw2pevx0scdedcr` | TX‑01, TX‑02 |
| **W2** | `addr1q8epy5jaharr0857d0lcwhlyg7tnarlajcml084mlhv92h39xk00fdnqnawyvkcs43kmt7hv4uqwetw9yd6lkjl5vxhs4p4rfq` | TX‑03, TX‑04, TX‑05 |

Both are visible on Cardanoscan against the transactions in
[`05` §1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions).

### 2.3 How the application knows which loans to show

Each refinance transaction mints **two** tokens under the Danogo policy
`aca8e306eda3eb6c25a838bebac37d929c216aab13c8d463fca5a08d`:

- one that stays in the **Dano loan contract** — a **recurring marker** for loan UTxOs in that
  contract, which is *not* unique to one loan (`asset1pr26rn8r…` has 57 mints and 49 burns, and is
  the same token in three of the five refinances), and
- one that is paid **to the borrower's own wallet** — unique, one mint, no burn while the loan is
  open.

The distinction matters, and our previous submission did not draw it — see
[`05` §2.1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#21-two-danogo-tokens-and-which-one-identifies-a-loan)
and [`08_CORRECTIONS.md`](./08_CORRECTIONS.md) C‑3.

The second is the borrower's title to the loan. Danogo's own CIP‑25 metadata under the same policy
names it, verbatim:

> **`"name": "Borrower NFT (Flexible Pool Lending)"`**
> **`"description": "An NFT representing the loan from the Danogo Flexible Pool Lending"`**

So the "Loans" list is not a report the backend composes at will. It is the set of Borrower NFTs the
connected wallet holds — a count held on chain, not in our database, and countable by anyone.

| Wallet | Borrower NFTs held on chain | "Loans" badge in the screenshot | |
|---|---|---|---|
| W1 | 2 | **2** | ✅ |
| W2 | 4 | **4** | ✅ |

### 2.4 Row by row: the displayed amount is the ledger amount

The principal is what the Dano liquidity source disbursed on chain; the accrual is simple interest
at **the APR the application itself displays in that row**; the result is truncated to two decimals,
which is the display rule the application follows across all five rows.

**Wallet W1**

| UI row | Borrowed (displayed) | APR (displayed) | Created by | Principal on chain | Age at capture | Principal + accrual | |
|---|---|---|---|---|---|---|---|
| 1 | 15.07 ADA | 8.47% | [`d240fab1…`](https://cardanoscan.io/transaction/d240fab1d260b8553a60bf5bae7eb4a1500f9011446a5f01a3c156d6f0dad84c) | 15.015024 ADA | 16.8 d | 15.0736 → **15.07** | ✅ |
| 2 | 22.04 ADA | 3.07% | [`88579a30…`](https://cardanoscan.io/transaction/88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652) | 22.000857 ADA | 21.8 d | 22.0413 → **22.04** | ✅ |

**Wallet W2**

| UI row | Borrowed (displayed) | APR (displayed) | Created by | Principal on chain | Age at capture | Principal + accrual | |
|---|---|---|---|---|---|---|---|
| 1 | 14.01 ADA | 3.07% | [`c426d9fa…`](https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c) | 14.000108 ADA | 15.6 d | 14.0185 → **14.01** | ✅ |
| 2 | 13.01 ADA | 3.07% | [`1cf8f08b…`](https://cardanoscan.io/transaction/1cf8f08b65574186d4d53c6288e848207b1bda42040e1a97e519839c57549f10) | 13.000010 ADA | 15.9 d | 13.0174 → **13.01** | ✅ |
| 3 | 5.01 **STRIKE** | 5.00% | [`0e26cc58…`](https://cardanoscan.io/transaction/0e26cc585890eeb13c9bc1e4a37f752eaf770abaf72f8fbde199cf13908b05b8) | 5.000560 STRIKE | 13.9 d | 5.0101 → **5.01** | ✅ |
| 4 | 321.78 USDA | 7.00% | `67833d58…`, 2025‑10‑12 | — | 333 d | **not part of this milestone** | — |

Five rows, five transactions, five matches — including the non-ADA one. The values were not chosen
to fit: the principals are odd numbers produced by min-UTxO arithmetic (`22.000857`, `15.015024`,
`13.000010`), and each still lands on the displayed figure once its own APR and its own age are
applied.

**About W2's fourth loan.** W2 holds four Borrower NFTs, so the app correctly shows four. The fourth
is the **USDA** loan, minted on **2025‑10‑12** by transaction
`67833d580982a9be309fa322d81951acd8c4c82166e676cbde88c1478e60099a` — almost a year before this
milestone's work. It is **not** offered as evidence for anything here and is not reconciled above.
It is named only so the count adds up without a gap.

### 2.5 What is *absent* from the screenshots, and why that is the point

Neither account lists a **Fluid** loan. That is not a rendering choice — those loans no longer exist
anywhere on Cardano. Fluid mints a position NFT when a loan opens;
[`05` §2](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#2-facts-that-hold-for-all-five-transactions) shows all
five being burned inside the refinance transactions. The consequence, today:

| Refinance tx | Fluid position NFT | Total supply on chain, now |
|---|---|---|
| `88579a30…` | `asset128rrfu48vwdclrdxe4hxqhq6hnd5q4h8glf7uq` | **0** |
| `d240fab1…` | `asset12nvvpmrv0qmeh9dz4xcnvx85lknrrzy9rh7uha` | **0** |
| `1cf8f08b…` | `asset1zxnfclhy8tfde90aduttcpp20c7s8r67slv9vr` | **0** |
| `c426d9fa…` | `asset16hjjsk7mzedy20a27aqe5etg0a6v3g7cdlglac` | **0** |
| `0e26cc58…` | `asset1hc8hc7kt4ksrx2ww55gw7l7g9kznsas6csadwx` | **0** |

A supply of zero is a stronger statement than a burn in a historical transaction: the token has not
reappeared, in any wallet, in any UTxO, since. This is the **repay** journey seen from the user's
side — the debt the borrower had before is not there afterwards, and the replacement debt is listed
with the right number against it.

### 2.6 What this section does not claim

- **What the screenshots are.** They were captured by us, from wallets we control, and they evidence
  the *view* journey and the state of the chain: the loans listed, and every displayed amount
  reconciled to the ledger. They are reported as that, and the scope note is in §1.4.1 and
  [`08_CORRECTIONS.md`](./08_CORRECTIONS.md) C‑2.
- **The borrowed amounts are reconciled, not the USD figures.** The dollar values in the screenshots
  are oracle-priced at render time and cannot be derived from a transaction hash. The native-unit
  amounts can, and those are what §2.4 checks.
- **Collateral is stated from the origination transaction, not the current UTxO.** A loan's
  collateral can be modified after it is opened.
- **W2's fourth loan predates this milestone** and is excluded from every count and claim above.

### 2.7 What this section rests on

Four claims, all read from `api.koios.rest`:

| Claim | Source |
|---|---|
| The Fluid loan position NFT burned by each refinance has **total supply 0** today | asset supply, live |
| The Dano Borrower NFT minted by each refinance is **still held by the signing wallet** | the wallet's current assets |
| The number of Borrower NFTs each wallet holds equals the **"Loans N"** badge in its screenshot | the wallet's current assets |
| The amount the application **displayed** equals the on-chain principal plus simple interest at the APR the application displayed, truncated to 2 dp | the principals in [`05` §3](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#3-per-transaction-value-flow) and each transaction's timestamp |

The Fluid policy id used for the supply figures is the one read out of the burn records in the
transactions themselves, not a constant carried in from elsewhere.

---
