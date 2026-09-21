# Milestone 2 — Acceptance Criteria, mapped to this resubmission

This is the mapping for the **resubmission**. It replaces
[`03_Reviewer_Checklist_M2.md`](./03_Reviewer_Checklist_M2.md), which is the previous submission's
mapping and is kept unedited as an archive — several of its ✅ rest on claims we have since withdrawn
([`08_CORRECTIONS.md`](./08_CORRECTIONS.md)).

Every row below points at evidence a third party can check without us. Where a criterion is met with
open items still attached, the row says which ones and links them; **§E** lists everything this
package leaves open, including the one thing it cannot evidence at all.

**Two criteria are not fully met, and neither is marked as if it were.**

- **AC3** asks for an integration that is *stable, intuitive, and free from blocking UI/UX issues
  across supported wallets*. Stability we can show, and two wallet brands are now exercised — Eternl
  across all four journeys, Vespr across *open* and *refinance*. **Whether the interface is intuitive
  we cannot show**: no independent-user study was run. Scored clause by clause in
  [§B.1](#b1-ac3-clause-by-clause), carried as **⚠️ partly met**.
- **AC4** asks for the major journeys to be *covered by integration tests*. All four journeys were
  executed and checked, and all four now carry automated results — on Loan Details, **87 of 87
  in-scope tests have passed, across three runs rather than one**; the separate suites ran 70 tests
  over *open* and 50 over *repay*, with 15 and 12 failing. So the coverage is real but uneven. The
  previous submission's “8 / 8” for refinance is withdrawn ([`08` C‑6](./08_CORRECTIONS.md)). Scored in
  [§B.2](#b2-ac4-what-is-automated-and-what-is-not), carried as **⚠️ partly met**.

Both are also in §E. We would rather hand the reviewer the two gaps named than a full column of
ticks they have to take apart themselves.

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
| Demo video | **four unedited screen recordings**, one per journey — continuous takes from the first click, 3 min 50 s – 7 min 29 s, one of them ([`03`](./videos/03-open-refinance-repay-borrow-USDM-collateral-ADA.mp4)) covering all four journeys end to end in a single unbroken session — [`videos/`](./videos/). Every frame is cross-referenced step by step in [`06`](./06_UAT_Reports_Four_Journeys_M2.md), so any figure on screen can be checked against the chain | ✅ |

> **On the demo video.** The link in the previous submission, https://youtu.be/z07TxLJLC2w, was a
> developer walking through the feature — the first thing the reviewer rejected. **It is withdrawn as
> evidence.** What stands in its place is the four session recordings above: unedited, full-length,
> and showing the interface being operated rather than described.

## B. Acceptance Criteria

| # | Criterion | What we can show | Status |
|---|---|---|---|
| **AC1** | View / open / repay / refinance via **Eternl** wallet | all four exercised through Eternl: 15 mainnet transactions from 2 wallets, plus 4 preprod sessions on 3 Eternl accounts and 2 Eternl versions (v2.1.7.1, v2.1.5.0) — [`06` At a glance](./06_UAT_Reports_Four_Journeys_M2.md#at-a-glance). **View** produces no transaction by nature; its evidence is the app's own screens reconciled to the ledger. *Open* and *refinance* were also exercised in **Vespr**, a second wallet brand ([`06` Journey 5](./06_UAT_Reports_Four_Journeys_M2.md#journey-5--open--refinance-through-a-second-wallet-brand-vespr)) | ✅ |
| **AC2** | Back-end correct contract interactions for all loan states | every Plutus script execution in all 15 transactions returned `valid_contract = true`; inputs, outputs, mints and burns re-derived from the public **Koios** API — [`05` §2](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#2-facts-that-hold-for-all-five-transactions), [§3](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#3-per-transaction-value-flow) | ✅ |
| **AC3** | *"Integration must be **stable**, **intuitive**, and **free from blocking UI/UX issues** across **supported wallets**"* — the criterion as written, not narrowed | scored clause by clause in [**§B.1**](#b1-ac3-clause-by-clause) immediately below. Of the four clauses **one is met outright**, two are met with named gaps — including **one defect that did block until the user pressed *Retry*** — and **one has no evidence at all** | ⚠️ **partly met** |
| **AC4** | Major journeys covered by integration tests | **All four journeys now carry automated results.** On the Loan Details suite, **87 of 87 in-scope tests have passed — across three runs, not one**; the last full-suite run was **83 of 98**. [`08` C‑6](./08_CORRECTIONS.md) gives the sequence and names the eleven tests left out of scope and why. **Open: 70 run, 15 fail. Repay: 50 run, 12 fail** — mostly the suite declaring its own Tier‑2 limits. Scored in [**§B.2**](#b2-ac4-what-is-automated-and-what-is-not) | ⚠️ **partly met** |

### B.1 AC3, clause by clause

AC3 is the only criterion this package cannot tick. It is four requirements in one sentence, and they
do not have the same answer, so each is scored separately rather than averaged into a single mark.

| Clause | Verdict | Status |
|---|---|---|
| **stable** | No crash, no hang, no lost state in any of the four sessions, across 123 QC checks | ✅ |
| **free from blocking UI/UX issues** | One defect blocked the journey until the user pressed *Retry* in the app — [below](#the-blocking-defect-d1) | ⚠️ **one blocking-but-recoverable defect** |
| **intuitive** | No independent-user study was run — [below](#the-clause-with-no-evidence-intuitive) | ❌ **not evidenced** |
| **across supported wallets** | Two brands: Eternl across all four journeys, Vespr across two — [below](#supported-wallets-two-brands) | ⚠️ **two brands; one on two journeys only** |

##### stable

No crash, no hang and no lost state in any of the four sessions, across **123 QC checks**. On every
settled loan the figures the interface displays reconcile to the ledger — fee, collateral, health
factor and resulting debt, to the lovelace, in all five refinances and all three repayments
([`06`](./06_UAT_Reports_Four_Journeys_M2.md)).

##### The blocking defect (D‑1)

**One issue was blocking until the user pressed a button in the app.** In journey 3 the first
refinance submit failed after the signature — *"Submit failed: Your wallet may not have finished
syncing…"* — and the journey could not proceed until the tester pressed **Retry**, which succeeded.
We grade it **major (recoverable)**, and we do not claim this clause is clean.

What bounds it: it recovered **inside the app**, with no reload and no re-entry of data; **no funds
were lost**; and the chain carries **exactly one** transaction for that refinance, so there was no
double-submit.

**The cause is established.** The ledger rejected the submission with `ScriptsNotPaidUTxO` — over
CIP‑30 `getCollateral()` the wallet offered a collateral UTxO its own cache had not refreshed since
the open **1 min 50 s earlier**. It is the wallet's selection, but the product's consequence, and
**no fix has shipped** ([`06` Root causes](./06_UAT_Reports_Four_Journeys_M2.md#root-causes-established-after-the-sessions)).

**Separately, and not blocking:** the *Deposit* / *Fee* lines in the pre-signature preview at open
reconcile to nothing on chain, in **all four** sessions — on the very screen the user signs from
([`06` OI‑1](./06_UAT_Reports_Four_Journeys_M2.md#open-items-common-to-more-than-one-session)).

##### The clause with no evidence: intuitive

**Nothing.** No independent-user study was run. Journeys 1, 2 and 4 were run by the delivery team,
who wrote the feature. Journey 3 was run by a second person — but their instructions, their
questions and their relation to the team were not recorded, so it does not substitute for a study
([`06` §3](./06_UAT_Reports_Four_Journeys_M2.md#journey-3--a-second-tester-on-a-second-wallet)). The
previous submission's self-assessment of this clause **is withdrawn** ([`08` C‑2](./08_CORRECTIONS.md)).

##### Supported wallets: two brands

**Eternl** — across three accounts and two separate installations, on both mainnet and preprod;
versions **v2.1.7.1** (journeys 1–2) and **v2.1.5.0** (journey 3), journey 4's not recorded
([`06` At a glance](./06_UAT_Reports_Four_Journeys_M2.md#at-a-glance)).

**Vespr** — signed an *open* and a *refinance* on preprod in one 2 min 26 s recorded session:
[`b2937327…7bf4`](https://preprod.cardanoscan.io/transaction/b2937327cc0661395029834652ad9f076eed677248f95f51fcdb9f76a2ab7bf4),
13 Plutus script executions across the two, all `valid_contract = true`, 200.000000 ₳ of collateral
carried across to the lovelace ([`06` Journey 5](./06_UAT_Reports_Four_Journeys_M2.md#journey-5--open--refinance-through-a-second-wallet-brand-vespr)).

⚠️ The Vespr evidence covers **two journeys, not four** — no *view*, no *repay*, no screenshot set
and no QC checklist. Lace, Typhon and Nami are untried.

We would rather report AC3 as **partly met** with the clauses named than tick it on the two clauses
that pass. Both open clauses are in §E, and nothing elsewhere in this package claims either of them.

### B.2 AC4, what is automated and what is not

The reviewer's second reason for *Not Approved* was per-journey evidence **and** the corresponding
integration-test results. The first half is answered in full; the second half is answered for one
journey out of four, and this table says so rather than averaging the two together.

| Journey | Automated integration tests | Executed and checked by hand |
|---|---|---|
| **View** | ✅ **all 80 in scope pass** — Loan Details overview, collateral list, health factor, APR, utilisation, money and rate formats, plus the negative control FN-I8, against the live **mainnet** app (21 Sep 2026). Two needed a retry; both are `locator.click` timeouts, not defects | ✅ every displayed figure — debt, collateral, APR, health factor — reconciled to the ledger, in all four sessions and on two mainnet wallets ([`04` §2](./04_USER_JOURNEYS_AND_APP_STATE.md#2-the-post-refinance-state-in-the-app-reconciled-to-the-ledger)) |
| **Open** | ⚠️ **70 tests run, 15 fail** — the `[Create loan]` suite across all five protocol specs. Most failures are the suite declaring its own Tier‑2 limits, not product defects; details in [`08` C‑6](./08_CORRECTIONS.md) | ✅ 4 preprod walkthroughs + **7 mainnet originations** ([`05` §1.1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans)) |
| **Repay** | ⚠️ **50 tests run, 12 fail** — the `[Repay]` suite across all five protocol specs. Two of the failures are the **same defect as OI‑1**, found independently of the manual sessions; details in [`08` C‑6](./08_CORRECTIONS.md) | ✅ 3 preprod repayments, one of them inside a continuous recording, + **3 mainnet repayments** ([`05` §5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-three-direct-repayments-and-repayment-within-a-refinance)) |
| **Refinance** | ✅ **all 7 Fluid tests in scope pass** on the live **mainnet** app with the feature flag on — the CTA renders, its label carries a figure in both directions, it sits last in the footer, and it opens the preview in place. The previous submission's “8 / 8” **is withdrawn** — [`08` C‑6](./08_CORRECTIONS.md). Test-by-test below the table | ✅ 5 preprod refinances + **5 mainnet refinances** ([`05` §1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions)) |


**The seven refinance tests, and what is not among them.** FN‑I7 (the CTA renders), FN‑I9
(`Save 91.98% net cost`), FN‑I10 and FN‑I11 (the dearer branch, `+1.30% net cost`, reached for the
first time after a harness fix), FN‑I12 (the label always carries a figure), FN‑I14 (last in the
footer) and FN‑J10 (opens the preview in place). The negative control **FN‑I8** — that a Dano loan
offers no refinance — also passes, but asserts on the Loan Details screen and is counted under
*View*, so it is not one of the seven. One further Fluid test is out of scope as display-level, and
the four Liqwid refinance tests are out of scope as a different protocol
([`08` C‑6](./08_CORRECTIONS.md)).

So the honest reading of AC4 is: **all four journeys are covered by execution and all four now carry
automated results, and the coverage is uneven.** The Loan Details suite is clean within its
reported scope — a scope that excludes eleven tests, and a figure that stands on three runs rather
than one; *open* and *repay* are not clean, at 15 of 70 and 12 of 50 failing.
Nothing in this package presents the 123 manual checks
as automated results, and nothing rounds a partial suite up to a full one — the figure the previous
submission published, “8 / 8”, is withdrawn in [`08` C‑6](./08_CORRECTIONS.md) rather than repeated.

Two pieces of work would close this row, and both are ours: give the harness a real Dano pool id so
the four Liqwid refinance tests can synthesise their target, and run the whole suite once more so
the 87 stand on a single run rather than three. The two blockers that stopped these suites
running at all — the `?ff=` flag never reaching the spec's first navigation, and a missing wallet
fallback that turned a misconfigured run into a silent “98 skipped” — are already fixed.

## C. What the automated suite does and does not prove

This is stated up front because the distinction matters and is easy to miss.

The refinance tests (FN-I7, FN-I9 … FN-I14, FN-J10) and the 80 passing View tests all run in the
harness's **Tier 2, "connected, no-sign"** mode:

- **Real, live data.** A real Cardano **mainnet** wallet is bridged into the page over CIP-30.
  `getBalance`, `getUtxos` and the address calls are served from real chain state via Blockfrost, so
  the app loads real positions and computes its quotes from them. It is not a mock wallet.
- **No signing, no broadcast.** `signTx` and `submitTx` are **stubbed** — `signTx` returns a fixed
  witness and `submitTx` returns a placeholder hash. Nothing is signed. Nothing reaches the chain.

So the suite evidences that **the refinance surface renders correctly from live data, that the
figures it quotes are computed correctly, and that the app reaches the point of asking for a
signature**. It does **not** evidence that a transaction signs, submits and settles.

That last part is evidenced by something stronger: the **15 mainnet transactions**, each signed by
hand through Eternl and each independently verifiable by anyone, with no test harness in the path —
[`00` §A](./00_POA_SUBMISSION_FORM.md#a-feature-integration).

## D. Evidence of Completion

| # | Evidence item | Artifact | Status |
|---|---|---|---|
| 1 | Published integration test report (successful user journeys) | current results: [`04` §1.5](./04_USER_JOURNEYS_AND_APP_STATE.md#15-current-integration-test-results) · the four sessions check by check: [`06`](./06_UAT_Reports_Four_Journeys_M2.md) · previous submission's report kept as an archive: [`01`](./01_Integration_Test_Report_M2.md) | ✅ |
| 2 | Demo video of workflows | four unedited session recordings, [`videos/`](./videos/) | ✅ |
| 3 | Links to deployments showing correct contract interactions | [`02_Mainnet_Transactions_M2.md`](./02_Mainnet_Transactions_M2.md) and, with the full value flow, [`05`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md) | ✅ |

## E. What is open, stated here rather than left to be found

| | Where |
|---|---|
| Is the interface intuitive to an independent user — **not evidenced**, and not claimed anywhere in this package. The previous submission's self-assessment is withdrawn. This is one clause of **AC3**, which is carried as partly met for exactly this reason | [§B.1](#b1-ac3-clause-by-clause) · [`08` C-2](./08_CORRECTIONS.md) |
| The *Deposit* / *Fee* preview lines at open reconcile to nothing on chain. **Cause established**: both are pool-configuration fields, not transaction values — the *Fee* applies a 1.0% / 5-minimum origination model to a Fluid borrow, which the source itself says charges none. Display-only, never charged, **no fix shipped** | [`06` Root causes](./06_UAT_Reports_Four_Journeys_M2.md#root-causes-established-after-the-sessions) |
| The Fluid borrower NFT is left in the wallet after the position is gone — intended or not, unconfirmed. A question for the **counterparty protocol**, not this codebase | [`06` OI-2](./06_UAT_Reports_Four_Journeys_M2.md#open-items-common-to-more-than-one-session) |
| Loan Details labels a health factor of **197** and of **32.3** both *Healthy* (D-6). Not a computation error — the band is absolute, `HEALTHY` above 1.6 — but **open as a product decision**, and the one item in these sessions that touches how understandable the screen is | [`06` Root causes](./06_UAT_Reports_Four_Journeys_M2.md#root-causes-established-after-the-sessions) |
| Journey 3's submit failure (D-1). **Cause established**: ledger `ScriptsNotPaidUTxO` on a stale collateral UTxO the wallet supplied; recovered by *Retry*, exactly one transaction on chain. **No fix shipped** | [`06` Root causes](./06_UAT_Reports_Four_Journeys_M2.md#root-causes-established-after-the-sessions) |
| TC-24 does not hold — our own citation error, not a product defect | [`08` C-1](./08_CORRECTIONS.md) |
| Two wallet brands are exercised — Eternl across all four journeys, **Vespr across *open* and *refinance* only** ([`06` Journey 5](./06_UAT_Reports_Four_Journeys_M2.md#journey-5--open--refinance-through-a-second-wallet-brand-vespr)). Lace, Typhon and Nami are untried, and the Vespr session has no screenshot set and no QC checklist | [§B.1](#b1-ac3-clause-by-clause) |
| **Open** and **repay** now have automated results — 70 and 50 tests run, 15 and 12 failing — but most of those failures are the suite declaring its own Tier‑2 limits rather than product verdicts | [§B.2](#b2-ac4-what-is-automated-and-what-is-not) |
| The 87 in-scope Loan Details tests have all passed, but across **three runs**, not one — no single run since the 21 September harness fix covers all 87 | [`08` C‑6](./08_CORRECTIONS.md) |
| All eight refinance tests are annotated `KNOWN-FAIL (finding, app-vs-spec)` in their own source — they were written to record a divergence from the screen spec, not to pass. The previous submission cited them as a passing suite | [`08` C‑6](./08_CORRECTIONS.md) |
