# Milestone 2 — Acceptance Criteria, mapped to this resubmission

This is the mapping for the **resubmission**. It replaces
[`03_Reviewer_Checklist_M2.md`](./03_Reviewer_Checklist_M2.md), which is the previous submission's
mapping and is kept unedited as an archive — several of its ✅ rest on claims we have since withdrawn
([`08_CORRECTIONS.md`](./08_CORRECTIONS.md)).

Every row below points at evidence a third party can check without us, and lists **only what that
evidence shows**. Where an artifact does not support a claim, the claim is not made. **§E** lists the
known issues found while producing this package, each with its established cause.

The previous submission's self-assessments — that the interface is intuitive, and that the automated
refinance suite passed 8 / 8 — **are withdrawn and are not replaced by new self-assessments**
([`08` C‑2, C‑6](./08_CORRECTIONS.md)). What this package offers instead is execution evidence and
published checks, each re-derivable from a public source.

"Rolling Loan" = the **Refinance via Dano** feature.

### Where the app is, and how to reach the feature

| | URL | *Refinance via Dano* | Used for |
|---|---|---|---|
| **Mainnet app** — staging deployment, live mainnet contracts | https://v3.danogo.io/ | **off by default** — open **https://v3.danogo.io/?ff=fluid-refinance** | the 15 settled transactions, 18–28 August 2026 |
| **Preprod app** | https://preprod.danogo.io | **on by default** — nothing to append | the 4 screen-by-screen walkthroughs, 18 September 2026 |

**Read this before looking for the button.** *Refinance via Dano* is behind a rollout flag,
`VITE_FLUID_REFINANCE_ENABLED`, staged per protocol. On the mainnet deployment it is **off**, so a
reviewer who opens https://v3.danogo.io/ and goes to a Fluid loan **will not see the refinance
CTA**. Appending `?ff=fluid-refinance` **once** turns it on; the override is stored in
`sessionStorage` and persists for the rest of that browser tab, including in-app navigation. On
preprod the flag ships on, which is why the walkthroughs in [`06`](./06_UAT_Reports_Four_Journeys_M2.md)
show the card without any URL parameter.

*The reviewer does not have to take our word for either.* Each deployment injects its own runtime
configuration into the page at load time, and the flag is in it in plain text. To read it on either
site: open the site, then in the browser's developer tools console evaluate

```js
window.__runtimeConfig.VITE_FLUID_REFINANCE_ENABLED
```

On the mainnet app it returns `"false"`; on preprod, `"true"`. (The same object carries other
deployment settings, so we do not link it directly; the console read above returns only this flag.)

**What the flag gates is only the way in.** It controls the *Refinance via Dano* CTA on Loan Details
and the savings badge in *My Account* / *Portfolio* — nothing else. **View, open and repay are not
behind any flag** and are reachable on both deployments as shipped.

---

## A. Milestone Outputs

| Output | Satisfied by | Status |
|---|---|---|
| Integrated front-end with loan actions (view, open, repay, refinance) | **15 mainnet transactions** signed from the app — 7 open, 5 refinance, 3 repay — [`05` §1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions), [§1.1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans), [§5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-three-direct-repayments-and-repayment-within-a-refinance) · all four actions captured screen by screen in [`06`](./06_UAT_Reports_Four_Journeys_M2.md) | ✅ |
| Completed connection to back-end API endpoints | every quoted figure re-derived from the ledger and found to match — [`04` §2](./04_USER_JOURNEYS_AND_APP_STATE.md#2-the-post-refinance-state-in-the-app-reconciled-to-the-ledger); Fluid's own API independently reports the same loans repaid — [`05` §5.3](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#53-fluids-own-records-say-the-loans-are-repaid) | ✅ |
| Ready-to-test user journeys | four complete sessions on three Eternl accounts, each with a screen recording, every signing dialog and every explorer record — [`06`](./06_UAT_Reports_Four_Journeys_M2.md), **123 QC checks, 123 hold** | ✅ |
| Demo video | **four unedited screen recordings**, one per journey — continuous takes from the first click, 3 min 33 s – 7 min 29 s, one of them ([`03`](./videos/03-open-refinance-repay-borrow-USDM-collateral-ADA.mp4)) covering all four journeys end to end in a single unbroken session — [`videos/`](./videos/). Every frame is cross-referenced step by step in [`06`](./06_UAT_Reports_Four_Journeys_M2.md), so any figure on screen can be checked against the chain | ✅ |

> **On the demo video.** The link in the previous submission, https://youtu.be/z07TxLJLC2w, was a
> developer walking through the feature — the first thing the reviewer rejected. **It is withdrawn as
> evidence.** What stands in its place is the four session recordings above: unedited, full-length,
> and showing the interface being operated rather than described.

## B. Acceptance Criteria

| # | Criterion | What we can show | Status |
|---|---|---|---|
| **AC1** | View / open / repay / refinance via **Eternl** wallet | all four exercised through Eternl: 15 mainnet transactions from 2 wallets, plus 4 preprod sessions on 3 Eternl accounts and 2 Eternl versions (v2.1.7.1, v2.1.5.0) — [`06` At a glance](./06_UAT_Reports_Four_Journeys_M2.md#at-a-glance). **View** produces no transaction by nature; its evidence is the app's own screens reconciled to the ledger. *Open* and *refinance* were also exercised in **Vespr**, a second wallet brand ([`06` Journey 5](./06_UAT_Reports_Four_Journeys_M2.md#journey-5--open--refinance-through-a-second-wallet-brand-vespr)) | ✅ |
| **AC2** | Back-end correct contract interactions for all loan states | every Plutus script execution in all 15 transactions returned `valid_contract = true`; inputs, outputs, mints and burns re-derived from the public **Koios** API — [`05` §2](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#2-facts-that-hold-for-all-five-transactions), [§3](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#3-per-transaction-value-flow) | ✅ |
| **AC3** | *"Integration must be **stable**, **intuitive**, and **free from blocking UI/UX issues** across **supported wallets**"* — quoted as written | **Stability**: no crash, no hang and no lost state in any of the five recorded sessions; the four that carry a QC checklist hold **123 of 123 checks**, each re-derived from the public ledger. **Wallets exercised**: **Eternl** on all four journeys, across three accounts and two versions (v2.1.7.1, v2.1.5.0); **Vespr** on *open* and *refinance* — [**§B.1**](#b1-who-ran-the-sessions-stability-and-the-wallets-exercised) | evidence offered |
| **AC4** | Major journeys covered by integration tests | All four journeys executed end to end and verified check by check against the public ledger: **123 session QC checks** (123 hold) and **25 transaction QC checks** (24 hold), each published with the figure it asserts and the source it was re-derived from — [**§B.2**](#b2-the-integration-testing-this-package-publishes) | evidence offered |

### B.1 Who ran the sessions, stability, and the wallets exercised

**Who operated the four recorded sessions.** All four were run by **people from outside the company**
— three people across three wallet accounts — none of whom took part in building the feature, each
given a goal only, with no step-by-step instructions and no one guiding them during the run.

This rests on a statement by the delivery team; there is no test plan, brief, assignment message or
tester name to cite, and an earlier version of this package described journeys 1, 2 and 4 as
delivery-team runs. The change is logged as [`08` C‑7](./08_CORRECTIONS.md). What the recordings show
without relying on it: three accounts, two Eternl installations, and four operators who behaved
differently from one another ([`06`](./06_UAT_Reports_Four_Journeys_M2.md)).

**Stability.** No crash, no hang and no lost state in any of the five recorded sessions. The four
that carry a QC checklist hold **123 of 123 checks**; the Vespr session has no checklist of its own.
On every settled loan the figures the interface displays reconcile to the ledger —
fee, collateral, health factor and resulting debt, to the smallest unit, in all five preprod
refinances and all three preprod repayments ([`06`](./06_UAT_Reports_Four_Journeys_M2.md)).

**Wallets exercised.**

| Wallet | Journeys | Evidence |
|---|---|---|
| **Eternl** | view · open · repay · refinance | three accounts, two separate installations, versions **v2.1.7.1** and **v2.1.5.0** (journey 4's not recorded), on both mainnet and preprod — [`06` At a glance](./06_UAT_Reports_Four_Journeys_M2.md#at-a-glance) |
| **Vespr** | open · refinance | one 2 min 25 s recorded session on preprod; 13 Plutus script executions across the two transactions, all `valid_contract = true`; 200.000000 ₳ of collateral carried across to the lovelace — [`06` Journey 5](./06_UAT_Reports_Four_Journeys_M2.md#journey-5--open--refinance-through-a-second-wallet-brand-vespr) |

No claim is made for any wallet not in this table, and none is made about *view* or *repay* in Vespr,
which that session did not exercise.

**One defect occurred during the sessions and is published rather than edited out.** In journey 3 the
first refinance submit failed after the signature; the tester pressed **Retry** in the app, signed
again, and it settled. The ledger carries **exactly one** transaction for that refinance and no funds
were lost. The cause is established: the ledger rejected the first submission with
`ScriptsNotPaidUTxO` because the wallet offered a collateral UTxO its own cache had not refreshed
since the open 1 min 50 s earlier. **No fix has shipped** —
[`06` Root causes](./06_UAT_Reports_Four_Journeys_M2.md#root-causes-established-after-the-sessions).

### B.2 The integration testing this package publishes

Every check below is published with the figure it asserts and the public source that figure was
re-derived from. Checks that do not hold are published as failures.

| What was tested | Result | Detail |
|---|---|---|
| **Four end-to-end sessions on preprod** — each covering open, view and refinance; three of them repay as well | **123 QC checks, 123 hold** | [`06`](./06_UAT_Reports_Four_Journeys_M2.md) |
| **The executed mainnet refinance, transaction by transaction** (TC‑01 … TC‑25) | **24 of 25 hold.** TC‑24 does not: it asserts that `88579a30…` carries no origination-fee leg, and that is the wrong transaction to cite for the zero-fee case — an error in our own evidence, not a product defect ([`08` C‑1](./08_CORRECTIONS.md)) | [`01` §2](./01_Integration_Test_Report_M2.md) |
| **All 15 mainnet transactions**, against the public ledger | every Plutus script execution returned `valid_contract = true`; inputs, outputs, mints and burns re-derived from **Koios** | [`05` §2](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#2-facts-that-hold-for-all-five-transactions) |
| **Repayment status, from the counterparty protocol** | **7 of 7** Fluid loans reported `loan_repaid`, `remainingDebt: 0` | [`05` §5.3](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#53-fluids-own-records-say-the-loans-are-repaid) · [`fluid-api/`](./fluid-api/) |
| **What the interface quoted before signing, against what settled** | fee, collateral, health factor and resulting debt match to the smallest unit, every time | [`06`](./06_UAT_Reports_Four_Journeys_M2.md) |

Per journey, the execution evidence is:

| Journey | Executed and verified |
|---|---|
| **View** | every displayed figure — debt, collateral, APR, health factor — reconciled to the ledger, in all four sessions and on two mainnet wallets ([`04` §2](./04_USER_JOURNEYS_AND_APP_STATE.md#2-the-post-refinance-state-in-the-app-reconciled-to-the-ledger)) |
| **Open** | **7 mainnet originations** ([`05` §1.1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans)) + 4 preprod walkthroughs, including the multi-pool case |
| **Repay** | **3 mainnet repayments** ([`05` §5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-three-direct-repayments-and-repayment-within-a-refinance)) + 3 preprod repayments, one inside a continuous recording |
| **Refinance** | **5 mainnet refinances** ([`05` §1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions)) + 5 preprod refinances |

**The previous submission's “8 / 8” for the automated refinance suite is withdrawn**, and **no
automated-test pass rate is claimed in its place** ([`08` C‑6](./08_CORRECTIONS.md)).

## C. Evidence of Completion

| # | Evidence item | Artifact | Status |
|---|---|---|---|
| 1 | Published integration test report (successful user journeys) | the four sessions check by check: [`06`](./06_UAT_Reports_Four_Journeys_M2.md) · transaction-level QC: [`01` §2](./01_Integration_Test_Report_M2.md) · current results in one table: [`04` §1.5](./04_USER_JOURNEYS_AND_APP_STATE.md#15-current-integration-test-results) | ✅ |
| 2 | Demo video of workflows | five unedited session recordings, [`videos/`](./videos/) | ✅ |
| 3 | Links to deployments showing correct contract interactions | [`02_Mainnet_Transactions_M2.md`](./02_Mainnet_Transactions_M2.md) and, with the full value flow, [`05`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md) | ✅ |

## D. Known issues found while producing this package

Each was found during the recorded sessions or while re-deriving figures from public sources, and
each is published with its established cause rather than left for a reviewer to find.

| | Where |
|---|---|
| **D‑1 — journey 3's submit failure.** Cause established: the ledger rejected the first submission with `ScriptsNotPaidUTxO` on a stale collateral UTxO the wallet supplied. **Impact bounded and checked on chain: no funds lost, nothing double-submitted, exactly one transaction on chain**, and it recovered inside the app on *Retry* | [`06` Root causes](./06_UAT_Reports_Four_Journeys_M2.md#root-causes-established-after-the-sessions) |
| **OI‑1 — the *Deposit* / *Fee* preview lines at open reconcile to nothing on chain.** Cause established: both are pool-configuration fields, not transaction values. **Impact: display-only — no borrower is ever charged either figure, verified on chain in all four sessions** (debt afterwards equals the amount borrowed; no payment reaches any fee address at open). The fee the user is actually charged, on the refinance, reconciles five times out of five | [`06` Root causes](./06_UAT_Reports_Four_Journeys_M2.md#root-causes-established-after-the-sessions) |
| **OI‑2 — the Fluid borrower NFT is left in the wallet after the position is gone.** Reproduced through two different wallet applications, so it is a property of the counterparty protocol, not of one wallet. Intended or not, unconfirmed — a question for **Fluid**, not this codebase | [`06` OI‑2](./06_UAT_Reports_Four_Journeys_M2.md#open-items-common-to-more-than-one-session) |
| **D‑6 — Loan Details labels a health factor of 197 and of 32.3 both *Healthy*.** Not a computation error: the band is absolute, `HEALTHY` above 1.6. Open as a product decision | [`06` Root causes](./06_UAT_Reports_Four_Journeys_M2.md#root-causes-established-after-the-sessions) |
| **TC‑24 does not hold** — our own citation error, not a product defect | [`08` C‑1](./08_CORRECTIONS.md) |
| **The Vespr session covers *open* and *refinance* only**, with no screenshot set and no QC checklist of its own. Nothing in this package claims *view* or *repay* in Vespr | [§B.1](#b1-who-ran-the-sessions-stability-and-the-wallets-exercised) |
