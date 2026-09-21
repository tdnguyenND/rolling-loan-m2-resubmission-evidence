# Milestone 2 — Proof of Achievement Resubmission

**Project Catalyst · Project 1400107 — Rolling Loan**
**Milestone 2 — Feature Integration** · resubmission after *Not Approved*
Milestone page → https://milestones.projectcatalyst.io/projects/1400107/milestones/2

**Rolling Loan** (called **Refinance via Dano** in the UI) — the proposal's name and the product's
name for the same capability. This package uses *Refinance via Dano*, or simply *refinance*.

---

## The two questions, answered in two lines

| The reviewer asked for | Where it is answered |
|---|---|
| **Evidence of testing by someone other than the developer**, completing a workflow without guidance | [`06` Journey 3](./06_UAT_Reports_Four_Journeys_M2.md#journey-3--a-second-tester-on-a-second-wallet) — a second tester, on their own wallet and their own Eternl installation, ran **all four journeys in one unbroken 7 min 29 s recording**, checked the result in their own wallet and on a public explorer, and hit a submit failure the delivery team had not (**D‑1**). We report what that session records, and claim nothing beyond it |
| **Each of the four journeys tested through the Eternl-connected front end, and the integration-test results** | [`04` §1.2](./04_USER_JOURNEYS_AND_APP_STATE.md#12-the-matrix) — one row per journey, each with its own interface capture **and** its own mainnet transaction · [`04` §1.5](./04_USER_JOURNEYS_AND_APP_STATE.md#15-current-integration-test-results) — the integration-test results as measured on 21 September 2026 |

Both answers are set out in full, with the evidence behind them, in the section
[*What the reviewer said*](#what-the-reviewer-said-and-where-each-objection-is-answered) below.

---

## What the reviewer said, and where each objection is answered

> **1.** *"The fact that a developer has successfully demonstrated the application does not
> automatically establish that the interface is intuitive to independent users […] there is no
> evidence to show that users even completed a specific workflow without external guidance."*

Taken on board. The previous submission's row *"Tester completed the flow without external guidance
✅ Yes"* was a self-assessment, and it is not carried as evidence here. What the testing sessions
record is kept, and is reported per journey: all four journeys were walked end-to-end through the
Eternl-connected front end, and **fifteen of those sessions settled on Cardano mainnet** — 7 Open,
5 Refinance, 3 Repay — from two separate wallets over eleven days. Everything below leads with
evidence that can be checked without us — the Cardano ledger, and the counterparty protocol's own
loan records — with the interface captures and signing dialogs itemised beside it. See
[`04` §1](./04_USER_JOURNEYS_AND_APP_STATE.md#1-the-four-approved-user-journeys-via-eternl) and
[`08_CORRECTIONS.md`](./08_CORRECTIONS.md) C‑2.

> **2.** *"…provide evidence demonstrating that the four approved user journeys (view, open, repay
> and refinance) were each successfully tested through the Eternl-connected front end, and provide
> the corresponding integration-test results."*

Answered per journey in
[`04` §1](./04_USER_JOURNEYS_AND_APP_STATE.md#1-the-four-approved-user-journeys-via-eternl) — a
matrix with two separate classes of evidence for each journey, plus the user path for each — and
evidenced by **fifteen** mainnet transactions instead of two: **7 Open** ([`05`
§1.1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans)),
**5 Refinance** ([`05` §1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions))
and **3 Repay** ([`05` §5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-three-direct-repayments-and-repayment-within-a-refinance)), the
post-settlement state in the application
([`04` §2](./04_USER_JOURNEYS_AND_APP_STATE.md#2-the-post-refinance-state-in-the-app-reconciled-to-the-ledger)),
and the counterparty protocol's own loan records
([`05` §5.3](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-three-direct-repayments-and-repayment-within-a-refinance)).

**Integration-test results**, measured on 21 September 2026 — the full table is
[`04` §1.5](./04_USER_JOURNEYS_AND_APP_STATE.md#15-current-integration-test-results):

| What was tested | Result |
|---|---|
| Transaction QC checks against the executed refinance (TC‑01 … TC‑25) | **24 of 25 hold** — TC‑24 fails on our own citation error, not a product defect ([`08` C‑1](./08_CORRECTIONS.md)) |
| Four end-to-end sessions on preprod, check by check | **123 QC checks, 123 hold** ([`06`](./06_UAT_Reports_Four_Journeys_M2.md)) |
| All 15 mainnet transactions, against the public ledger | every Plutus script execution `valid_contract = true` ([`05` §2](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#2-facts-that-hold-for-all-five-transactions)) |
| Repayment status, from the counterparty protocol | **7 of 7** loans `loan_repaid`, `remainingDebt: 0` ([`fluid-api/`](./fluid-api/)) |

The previous submission's “8 / 8” is withdrawn — [`08` C‑6](./08_CORRECTIONS.md); the
step-by-step detail behind the older figures is in
[`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) §2–§3, kept unedited as an
archive.

These figures are our own reporting about our own work, and are named as such; the primary
verifiable evidence is the ledger and Fluid's own records. **No automated-test pass rate is claimed
in this package** — the previous submission's “8 / 8” is withdrawn
([`08` C‑6](./08_CORRECTIONS.md)).

"Repay" — the journey the previous submission covered "implicitly" — has its own section:
[`05` §5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-three-direct-repayments-and-repayment-within-a-refinance).

---

## For the reviewer — read in this order

| # | Document | What it answers | Time |
|---|---|---|---|
| 1 | [`00_POA_SUBMISSION_FORM.md`](./00_POA_SUBMISSION_FORM.md) | The submission itself: the response to both objections, then the evidence behind each output | 8 min |
| 2 | [`06_UAT_Reports_Four_Journeys_M2.md`](./06_UAT_Reports_Four_Journeys_M2.md) | **The four journeys walked end-to-end, session by session** — including the one run by a second tester and the one signed in a second wallet brand. 123 QC checks | 12 min |
| 3 | [`04_USER_JOURNEYS_AND_APP_STATE.md`](./04_USER_JOURNEYS_AND_APP_STATE.md) | **The per-journey evidence matrix**, the integration-test results, and the resulting loans in the app matched to the ledger | 9 min |
| 4 | [`05_ONCHAIN_TRANSACTIONS_AND_REPAY.md`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md) | **Fifteen mainnet transactions** grouped by journey, the value flow of each, and the repayment evidence — three direct repayments, plus repayment within refinance | 14 min |
| 5 | [`07_ACCEPTANCE_CRITERIA_M2.md`](./07_ACCEPTANCE_CRITERIA_M2.md) | Every Output, Acceptance Criterion and Evidence item mapped to what this package can show — **including the two criteria it cannot fully meet** | 5 min |
| 6 | [`08_CORRECTIONS.md`](./08_CORRECTIONS.md) | Errors we found in **our own** previous submission, five of the six unprompted | 3 min |

**Only have ten minutes?** Read [`00` §C](./00_POA_SUBMISSION_FORM.md#output-4--ready-to-test-user-journeys-five-recorded-end-to-end-sessions)
— the four sessions in one table — then the journey matrix in
[`04` §1.2](./04_USER_JOURNEYS_AND_APP_STATE.md#12-the-matrix).

**Before you look for the refinance button:** on the mainnet app it is behind a rollout flag and is
**off by default**. Open `https://v3.danogo.io/?ff=fluid-refinance` once, or use the preprod app
where it ships on. Details, and how to read the flag back yourself in the browser console, in
[`07`](./07_ACCEPTANCE_CRITERIA_M2.md#where-the-app-is-and-how-to-reach-the-feature).

The integration-test results, as they stand today, are in
[`04` §1.5](./04_USER_JOURNEYS_AND_APP_STATE.md#15-current-integration-test-results) — one table,
including the one check that no longer holds. Read that rather than the old report.

The previous submission's own files — the integration test report, the transaction evidence and the
reviewer checklist — are kept **unedited** at their original paths as an **archive**, each with a
banner pointing at [`08_CORRECTIONS.md`](./08_CORRECTIONS.md).
[`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) still holds the step-by-step
detail behind those results, and is linked from `04` §1.5 where it is still good.

---

## The evidence in one screen

| Journey | In the interface | On the ledger |
|---|---|---|
| **View** | *My Account → Loans* on both signing wallets — [`04` §2](./04_USER_JOURNEYS_AND_APP_STATE.md#2-the-post-refinance-state-in-the-app-reconciled-to-the-ledger) | every displayed figure re-derived from the chain — [`04` §2.4](./04_USER_JOURNEYS_AND_APP_STATE.md#24-row-by-row-the-displayed-amount-is-the-ledger-amount) |
| **Open** | four journeys captured screen by screen, first click to settled loan, including the multi-pool case where one borrow opens two loans — [`00` §C](./00_POA_SUBMISSION_FORM.md#output-4--ready-to-test-user-journeys-five-recorded-end-to-end-sessions) | **7 mainnet Open transactions** — [`05` §1.1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans) |
| **Repay** | the *Repay* action in the loan sheet — [`04` §1.3](./04_USER_JOURNEYS_AND_APP_STATE.md#13-the-user-path-for-each-journey) | **3 direct mainnet Repay transactions**, plus the settlement leg inside every Refinance (the *Repay* step performed in the same transaction); Fluid's own API reports all seven loans repaid — [`05` §5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-three-direct-repayments-and-repayment-within-a-refinance) |
| **Refinance** | refinance card → Eternl dialog → confirmation → portfolio after — [`01` §1](./01_Integration_Test_Report_M2.md) | **5 mainnet refinances** — collateral in = out, source position NFT burned, Borrower NFT minted — [`05` §1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions) |

Three things hold across all of it:

- **Fifteen signed Cardano mainnet transactions**, two wallets, 18–28 August 2026 — every one built
  by this application (metadata label 674), every Plutus script execution `valid_contract = true`.
- **The pairing is the ledger's, not ours.** Each Fluid loan's position NFT **held by the loan
  script** (policy `30f1095a…`) has exactly one mint and one burn in its whole history: the mint is
  the Open transaction, the burn is the transaction that closed it. Open mints two sibling tokens of
  the same name under other Fluid policies — one to the settlement address, one to the borrower's
  wallet — which are not burned and which the pairing does not use
  ([`05` §2.2](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#22-the-three-fluid-tokens-minted-at-open-and-which-one-the-pairing-uses)).
  Fluid's own API independently reports the same loans as repaid, naming both hashes.
- **The interface quoted what settled.** Before signing `c426d9fa…` it showed a Fluid debt of 12 ADA
  and a fee of 2 ADA; on chain the settlement is 12.000102 ADA, the fee exactly 2.000000 ADA, and
  DJED 6.000000 is carried across.

The transaction-by-transaction detail is in
[`05`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md); the journey-by-journey detail, including the
integration-test results as they stand today, is in
[`04` §1](./04_USER_JOURNEYS_AND_APP_STATE.md#1-the-four-approved-user-journeys-via-eternl).

---

## Which environment each piece of evidence comes from

The package uses three environments, and never interchangeably:

| | Environment | When |
|---|---|---|
| **Settlement evidence** — the transactions everything rests on | **Cardano mainnet** | 18–28 August 2026 |
| **Interface walkthroughs** — the four journeys captured screen by screen | the **preprod app** (https://preprod.danogo.io) | 18 September 2026 |
| **Historical test report** (`01`, `02`, `03`, kept unedited) | the **mainnet app** (staging deployment, live mainnet contracts) | previous submission |

Where a document says "the live app", it means the front end the wallet was connected to in that
row — mainnet for the settlement evidence, preprod for the walkthroughs.

---

## Where the evidence comes from

Every factual claim in this package comes from one of two public APIs, and **neither is ours**.
Both are open endpoints: no key, no account, no cooperation from us.

| Source | What it settles | Credentials |
|---|---|---|
| **Koios** `api.koios.rest` | the transactions — what they burned, minted and paid, and what the chain holds today | none |
| **Fluid Tokens** `api.fluidtokens.com/wallet-lending-history` | the counterparty protocol's own record that each loan is repaid, with nothing outstanding — raw responses, headers and SHA‑256 sums kept in [`fluid-api/`](./fluid-api/), with the `curl` that repeats them | none |

No Danogo server, indexer or database appears anywhere in the figures below: the ledger data is
Cardano's, and the loan records are the lender's own.

---

## Terms and labels used across this package

**Nine terms.** Defined once, here, so no document has to stop and explain them.

| Term | What it means |
|---|---|
| **UTxO** | An on-chain transaction output: a parcel of value at an address, spendable once |
| **Mint / burn** | Creating or destroying a token. A burn of `−1` destroys that token permanently |
| **Position NFT** | The token that identifies one **Fluid** loan |
| **Borrower NFT** | The token that identifies one **Dano** loan, held by the borrower |
| **`valid_contract = true`** | The transaction was accepted by the contract's own on-chain validator (the Plutus script). A transaction whose validator rejected it cannot produce these outputs |
| **min-UTxO** | The small amount of ADA every output must carry by protocol rule. A deposit held by the output, not a fee paid to anyone |
| **Settlement leg** | The part of a *Refinance* that pays off the source loan — the *Repay* step, performed inside the *Refinance* rather than as a separate user action |

**Identifier prefixes.** Six numbering schemes run through this package; they do not share a
sequence, so `C‑4`, `D‑4` and `TC‑04` are three unrelated things.

| Prefix | What it numbers | Defined in |
|---|---|---|
| **C‑n** | a correction to our own previous submission | [`08`](./08_CORRECTIONS.md) |
| **D‑n** | a defect found during a UAT session | [`06`](./06_UAT_Reports_Four_Journeys_M2.md) |
| **OI‑n** | an open item recurring across sessions | [`06`](./06_UAT_Reports_Four_Journeys_M2.md#open-items-common-to-more-than-one-session) |
| **TC‑n**, **UATn‑TC‑n** | a manual QC check against a transaction | [`01` §2](./01_Integration_Test_Report_M2.md) · [`06`](./06_UAT_Reports_Four_Journeys_M2.md) |
| **FN‑**, **UI‑** | an automated test case | [`08` C‑6](./08_CORRECTIONS.md) |
| **AC1 … AC4** | a milestone acceptance criterion | [`07` §B](./07_ACCEPTANCE_CRITERIA_M2.md#b-acceptance-criteria) |
| **TX‑01 … TX‑05**, **O‑n**, **R‑A/B/C** | a refinance, an open, and a repayment form | [`05`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md) |
| **E1 / E2** | interface evidence (ours) vs on-chain evidence (anyone's) | [`04` §1.1](./04_USER_JOURNEYS_AND_APP_STATE.md#11-the-two-classes-of-evidence) |

---

## Package contents

```
00_POA_SUBMISSION_FORM.md            the text submitted to the Catalyst form, and the
                                     response to both objections
04_USER_JOURNEYS_AND_APP_STATE.md    the four journeys via Eternl, and the resulting
                                     loans in the app matched to the ledger
05_ONCHAIN_TRANSACTIONS_AND_REPAY.md fifteen mainnet transactions grouped by journey,
                                     per-transaction value flow, the repay journey in
                                     all three of its forms
06_UAT_Reports_Four_Journeys_M2.md   the four preprod sessions, step by step and check
                                     by check: journeys 1 and 2 in opposite asset
                                     directions, journey 3 by a second tester on a
                                     second wallet, journey 4 one borrow filling two
                                     pools; 123 QC checks
07_ACCEPTANCE_CRITERIA_M2.md         every milestone Output, Acceptance Criterion and
                                     Evidence item mapped to what this package can
                                     actually show — including the two it cannot
08_CORRECTIONS.md                    corrections to our own previous submission

screenshots/
  screens/                           the app's My Account - Loans, one per wallet
  fluid-dashboard/                   Fluid's own borrower dashboard: the loans marked REPAID
  cardanoscan/                       Cardanoscan, one per refinance transaction
  journey-1-borrow-ADA-collateral-USDM/    open + view + refinance + repay, screen by screen
  journey-2-borrow-USDM-collateral-ADA/    the same, in the opposite asset direction
  journey-3-borrow-USDM-collateral-ADA/    the same again, a second tester on a second wallet
  journey-4-borrow-ADA-collateral-USDM/    one borrow filling two pools, both loans refinanced
  *.png                              walkthrough captures of the manual refinance journey

videos/                              screen recordings of the four journeys above,
                                     first click to settled loan (`00` §C), plus
                                     `05…vespr….mp4` — open + refinance signed in a
                                     second wallet brand (`06` Journey 5)

fluid-api/                           the counterparty protocol's own API responses behind
                                     the "seven loans repaid" claim, byte for byte, with
                                     headers, SHA-256 sums and the curl that repeats them

previous submission, kept in place so its links still resolve, each with a banner:
  01_Integration_Test_Report_M2.md   the manual refinance journey, the transaction QC
                                     checks, and the automated integration-test results
  02_Mainnet_Transactions_M2.md      its two mainnet transactions
  03_Reviewer_Checklist_M2.md        its output / criterion / evidence mapping
                                     (superseded by 07 — several of its ticks rest
                                     on claims we have since withdrawn)
```

### The previous submission is still here

Those three files are the evidence the reviewer read the first time. They are **unedited** — we did
not quietly fix them — and each carries a banner pointing at
[`08_CORRECTIONS.md`](./08_CORRECTIONS.md), which lists every claim in them we have since corrected
or withdrawn, with the on-chain arithmetic. `03`'s banner additionally points at
[`07_ACCEPTANCE_CRITERIA_M2.md`](./07_ACCEPTANCE_CRITERIA_M2.md), which replaces its
criterion-to-evidence mapping. Their walkthrough screenshots in
[`screenshots/`](./screenshots/) remain valid and are still referenced by this package.

---

## Status

| | |
|---|---|
| Per-journey evidence matrix, each journey with its own evidence — own mainnet transactions for open / repay / refinance, ledger-reconciled screen capture for view | ✅ `04` §1 |
| On-chain evidence, independently verifiable | ✅ 15 transactions |
| Post-refinance state in the app, reconciled to the ledger | ✅ every displayed borrowed amount |
| Repay journey evidenced — **3** direct mainnet repayments on two paths (Fluid's and Danogo's own), plus repayment within refinance | ✅ `05` §5 |
| Repayment confirmed by the counterparty protocol's own records | ✅ 7/7 `loan_repaid` |
| Repayment from the borrower's own funds, on mainnet | ✅ `17c23dde…` · `ea823365…` · `77748bd9…` |
| Corrections to our own previous submission | ✅ 6 — C‑1 … C‑6, five of them unprompted |
| Integration-test results | ✅ published check by check, each with its public source — `04` §1.5 |
| Testing sessions, per journey, through the Eternl-connected front end | ✅ 15 settled on mainnet, 2 wallets, 11 days — `04` §1.3 |
| Interface walkthrough per journey | ✅ **all four captured**: refinance end-to-end including the Eternl signing dialog; **open** end-to-end in the four preprod walkthroughs — `00` Output 4, including the multi-pool case (`06` §4.1); **repay** end-to-end on three loans those walkthroughs created — `06` §§1.1, 2.1, 3.1 |

On the last two rows. Every journey was
walked end-to-end through the connected front end during this milestone — fifteen of those sessions
settled on mainnet. The package reports those sessions and rests its verifiable claims on what a
third party can check without us: the ledger and the counterparty protocol's records. The refinance
journey is captured end-to-end, including the Eternl signing dialog, and so is the **open** journey
— in the four walkthroughs added for this resubmission, which run *Open → View → Refinance → View*
(and *→ Repay* in three of the four)
on preprod with a screenshot of every screen and a screen recording of the whole session
([`00`](./00_POA_SUBMISSION_FORM.md) Output 4). **Repay** is captured screen by screen too, in three of them — the
quote, the wallet dialog burning the loan token, and the settled transaction — each closing the loan
that walkthrough had just opened and refinanced ([`06` Journey 1](./06_UAT_Reports_Four_Journeys_M2.md#journey-1--borrow-25-ada-against-100-fusdm),
[`06` Journey 2](./06_UAT_Reports_Four_Journeys_M2.md#journey-2--borrow-11-fusdm-against-100-ada), [`06` Journey 3](./06_UAT_Reports_Four_Journeys_M2.md#journey-3--a-second-tester-on-a-second-wallet)); only
[`06` Journey 3](./06_UAT_Reports_Four_Journeys_M2.md#journey-3--a-second-tester-on-a-second-wallet) has a screen recording that runs through the repayment, and the
journey's mainnet evidence is its three signed transactions.

---

## Why the settlement evidence is on mainnet

Mainnet is where the Fluid → Dano path exists as a real market: that is where Fluid's pools hold
real liquidity and where the collateral assets borrowers actually post live, so a refinance that
settles a real debt is executed there. Mainnet is not a shortcut here — it is the environment this
cross-protocol path lives in, and it produces the stronger evidence, because every transaction above
is public and immutable, and the transaction and repayment records are publicly verifiable.

Fluid's smart contracts on **preprod** were used for the four interface walkthroughs added to this
resubmission — open, refinance and repay, screen by screen, with preprod Cardanoscan links
([`00` §C](./00_POA_SUBMISSION_FORM.md#output-4--ready-to-test-user-journeys-five-recorded-end-to-end-sessions)).
The settlement evidence in this package is mainnet throughout. The previous submission stated more
absolutely that Fluid has no testnet deployment at all; that overstatement is withdrawn and recorded
in [`08_CORRECTIONS.md`](./08_CORRECTIONS.md) C‑5.
