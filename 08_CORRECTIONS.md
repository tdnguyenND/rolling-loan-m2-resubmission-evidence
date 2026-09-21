# Corrections to the Previous Submission

**Project** 1400107 · **Milestone 2**

While preparing this resubmission we re-derived every on-chain figure from a **public Cardano API
(Koios)** rather than from our own reporting. That exercise found errors and overstatements in
our own evidence — a mis-cited transaction, a mis-identified token, a self-assessment doing more
work than it could carry, and two claims stated more absolutely than what we can show. Each is
recorded here, C‑1 to C‑6.

**C‑6 came from a different exercise and is the most serious.** We re-ran the automated test suite
instead of citing the number the previous submission reported, and the number did not survive: the
“8 / 8” we published is withdrawn. That correction exists because we ran the tests again rather than
trusting our own prior report.

The automated-test results are included because the reviewer asked for the integration-test results,
and they are reported in full — now as measured, not as previously claimed. We do not present them
as independent evidence: they are our own reporting about our own work. The primary verifiable
evidence is the Cardano ledger and Fluid Tokens' own records, both reproducible from public APIs by
anyone.

The reviewer did not ask about any of them. We are disclosing them because a reviewer who checks our
numbers should find that we checked them first, and because an evidence package that never corrects
itself is not an evidence package.

---

## C‑1 — **Factual error.** The wrong transaction was cited as the zero-fee example

**Severity** Material — a reviewer verifying TC‑24 on Cardanoscan would have found the claim did not
hold.

### What we said

> **TC‑24** — No-origination-fee pool → borrow = exact Fluid debt.
> *"For a pool with 0 origination fee, new loan borrow = the Fluid debt exactly (no +fee bump)."*
> Evidence cited: tx `88579a30…`
> — `01_Integration_Test_Report_M2.md` §2.7, TC‑24

The same claim appeared in the transaction evidence file:

> *"**Refinance → Dano no-fee pool** — Same atomic refinance on a pool with **no origination fee** —
> new loan borrow = exact Fluid debt (no fee bump)"* — cited against `88579a30…`
> — `02_Mainnet_Transactions_M2.md` §1

### What the chain says

What the previous claim got wrong is how a Dano origination fee is paid. **Some Dano pools charge an
origination fee when the loan is created, and that fee is capitalised into the new loan's debt** —
the pool lends the fee to the borrower alongside the settlement amount, instead of deducting it from
what is disbursed. So a Fluid loan carrying **20 ADA** of debt, refinanced into a pool whose
origination fee is **2 ADA**, becomes a **22 ADA** Dano loan: 20 ADA leaves the pool to settle the
Fluid debt, 2 ADA leaves the pool to pay the fee, and the borrower owes the sum of the two. This is
by design, and it is the reason the borrower needs no capital of their own to refinance
([`05` §2](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#2-facts-that-hold-for-all-five-transactions), INV‑8) — the fee is financed by the loan, not paid
out of pocket. The new debt is therefore the old debt **grossed up** by the fee, not equal to it.

Whether the gross-up happens at all is **per-pool configuration**: a fee-bearing pool produces
`new borrow = debt + fee`, a zero-fee pool produces `new borrow = debt`. Both cases occur among the
five transactions in this package, which is precisely why citing the wrong one matters.

Transaction `88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652` is a fee-bearing pool,
to the smallest unit — it **does** carry an origination-fee leg:

| Leg | Address | Amount |
|---|---|---|
| Dano Flexible Pool disburses | `addr1wx2degj2ru0uctl4rnvs7vh5l608smvxrgkm7lf8txxjd6qs43szs` | **−22.000857 ADA** |
| Fluid debt settlement | `addr1q9mt6pcxvkdnszlj4jeztxzshhvsfzq4zsjgwlgh364yct00mr8eah8c9at0zh3payfgmnvl4c74a2f6rwfp4eptnrcsgv4qa5` | **+20.000851 ADA** |
| **Dano origination fee** | `addr1qywadgaxcnh993zpzl5kfs806nqe7jyxp4e8unjpll5quymw2aappdz98nah303sy0dc3p83x4hewv5z5c44q2sfqgqqdnjkku` | **+2.000000 ADA** |

The fee address `addr1qywadgax…` receives exactly 2.000000 ADA in this transaction, and the same
2.000000 ADA in transactions `1cf8f08b…` and `c426d9fa…`. It is the fee leg, and it is present.

So the pool was **not** a zero-fee pool, and the borrow was **not** equal to the Fluid debt: it was
the debt (20.000851 ADA) grossed up by the 2.000000 ADA fee, exactly as the specification describes
for a fee-bearing pool. The figure the previous submission offered as the new borrow — 20 ADA — is
the debt that was *settled*; the loan Dano actually opened is 22.000857 ADA, which is why wallet
W1's Dano position reads **22.04 ADA** in the application (principal plus accrued interest,
reconciled row by row in [`04` §2.4](./04_USER_JOURNEYS_AND_APP_STATE.md#24-row-by-row-the-displayed-amount-is-the-ledger-amount)).

### The correction

**The underlying claim is right; the transaction cited for it was wrong.** A zero-origination-fee
pool does produce a borrow equal to the debt with no gross-up — and it is evidenced, by a different
transaction:

> **`0e26cc585890eeb13c9bc1e4a37f752eaf770abaf72f8fbde199cf13908b05b8`** (TX‑05, 2026‑08‑27)
>
> Dano Flexible Pool disburses **STRIKE 5.000560**; the Fluid debt settlement receives
> **STRIKE 5.000558**. There is **no leg to the Dano fee address at all**. No fee, no gross-up.

TX‑05 is a strictly better example than the one we originally cited, because it also demonstrates a
**non-ADA borrowed asset**, which none of the other four transactions do.

### How the fee actually behaves, across all five transactions

| Tx | Pool disbursement | Fluid debt settled | Fee leg | Consistent with `max(0.1% × borrow, 2 ₳)`? |
|---|---|---|---|---|
| `88579a30…` | 22.000857 ADA | 20.000851 ADA | **2.000000 ADA** | ✅ `max(0.022, 2) = 2` |
| `d240fab1…` | 15.015024 ADA *(staking contract)* | 15.000002 ADA | 0.969750 ADA | ➖ different Dano product, different fee configuration — we make no formula claim for it |
| `1cf8f08b…` | 13.000010 ADA | 11.000005 ADA | **2.000000 ADA** | ✅ `max(0.013, 2) = 2` |
| `c426d9fa…` | 14.000108 ADA | 12.000102 ADA | **2.000000 ADA** | ✅ `max(0.014, 2) = 2` |
| `0e26cc58…` | 5.000560 STRIKE | 5.000558 STRIKE | **none** | ✅ zero-fee pool |

*(The few-lovelace residuals between disbursement and the sum of the two legs are min-UTxO
adjustments, visible in the full value flow in [`05` §3](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#3-per-transaction-value-flow).)*

**Where the corrected claim now lives:**
[`05_ONCHAIN_TRANSACTIONS_AND_REPAY.md` §3](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#3-per-transaction-value-flow) (TX‑01 and
TX‑05) and the transaction table in §1 of the same file.

---

## C‑2 — **Restated.** The usability self-assessment

**Severity** This is the row the reviewer stopped the submission on.

### What we said

> | Tester completed the flow without external guidance | ✅ Yes |
> — `01_Integration_Test_Report_M2.md` §4

### Why it is restated

The row was a self-assessment, and it sat in an evidence table as though it settled how the
interface reads to someone who did not build it. The statement itself was true — the flow was
completed without external guidance, repeatedly — but the verdict attached to it was more than the
row could carry.

### What replaces it

The row is replaced by the sessions it recorded. Every journey was walked end-to-end through the
Eternl-connected front end, and the open, repay and refinance sessions settled on mainnet fifteen
times from two separate wallets. That record is reported per journey in
[`04_USER_JOURNEYS_AND_APP_STATE.md` §1](./04_USER_JOURNEYS_AND_APP_STATE.md#1-the-four-approved-user-journeys-via-eternl),
where each journey carries its own user path and its own evidence, and the reviewer can check every
figure directly. The sessions establish that each journey completes through the released interface;
they are not offered as a verdict on how the interface reads to someone who did not take part. See
the scope note at
[`04_USER_JOURNEYS_AND_APP_STATE.md` §1.4](./04_USER_JOURNEYS_AND_APP_STATE.md#14-the-scope-of-these-claims).

---

## C‑3 — **Corrected.** A recurring marker token was described as a per-loan NFT

### What we said

> *"a new Dano loan position NFT is minted into the Dano Flexible Loan contract"* — offered as proof
> that a distinct new loan was created in each refinance.

### What the chain says

Danogo mints **two** tokens per loan under policy
`aca8e306eda3eb6c25a838bebac37d929c216aab13c8d463fca5a08d`, and they are different kinds of object.
Their live supply figures from Koios `asset_info` settle it:

| Token | Where it goes | Supply | Mints | Burns |
|---|---|---|---|---|
| `asset1pr26rn8rqyuctelvw09f05af6c5e9vuccstma6` | Dano loan contract | 8 | **57** | **49** |
| `asset1hwst3ac0pldqnmqneq83qqysr588lr3n4p8wmc` | Dano loan contract | 8 | **32** | **24** |
| `asset1nghq6njhwydv59m6nualpa08j4mpgl02ahuj7s` | Dano loan contract | 1 | **17** | **16** |
| `asset1cf3ey4y9sfaume4dq0txeeu8xzpdj98l7qjgjg` | **borrower's wallet** | 1 | **1** | **0** |
| `asset18980lwqkdlfchpxpukkeznm59k9xktvxp3pqjf` | **borrower's wallet** | 1 | **1** | **0** |
| `asset198w3w26rh8ywtjsr7ueq3fpwqjx9caw52huhqj` | **borrower's wallet** | 1 | **1** | **0** |
| `asset19435nakpzldnfcg2ku0lpvn37j3g6v8rrguvft` | **borrower's wallet** | 1 | **1** | **0** |
| `asset1gnw4grx8vyuq3t99azngwpxzvv7sj29f2x6p3z` | **borrower's wallet** | 1 | **1** | **0** |

The token that stays in the loan contract is a **recurring marker**, minted and burned continuously
across many loans. It is not unique to a loan — `asset1pr26rn8r…` is in fact the *same* token in
TX‑01, TX‑03 and TX‑04. Citing it as proof that a distinct new loan was created was wrong.

The token paid to the **borrower's wallet** is the unique one, and Danogo's own CIP‑25 metadata
names it *"Borrower NFT (Flexible Pool Lending)"*.

### The correction

Every claim of the form "a distinct loan was created" is now made against the **Borrower NFT**. The
five refinances produced **five distinct Borrower NFTs**; they produced only three distinct marker
tokens.

**Where the corrected claim now lives:**
[`05` §2.1 and INV‑5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#21-two-danogo-tokens-and-which-one-identifies-a-loan),
and [`04` §2.3](./04_USER_JOURNEYS_AND_APP_STATE.md#23-how-the-application-knows-which-loans-to-show),
which uses the same tokens to reconcile the loan counts the application displays.

---

## C‑4 — **Corrected.** "The borrower contributes only the network fee" was over-stated

### What we said

> *"the borrower contributed only the Cardano network fee; the Dano pool funded the repayment"* —
> stated of all five refinance transactions.

### What the chain says

It holds for the four Flexible Pool refinances. It does **not** hold for **TX‑02** (`d240fab1…`),
which draws on the fixed-term **staking** contract:

| | Disbursed by Dano | Settlement + fee | Shortfall |
|---|---|---|---|
| TX‑01 | 22.000857 ₳ | 20.000851 + 2.000000 = 22.000851 ₳ | none |
| TX‑03 | 13.000010 ₳ | 11.000005 + 2.000000 = 13.000005 ₳ | none |
| TX‑04 | 14.000108 ₳ | 12.000102 + 2.000000 = 14.000102 ₳ | none |
| **TX‑02** | **15.015024 ₳** | 15.000002 + 0.969750 = **15.969752 ₳** | **0.954728 ₳, from the borrower** |

On TX‑02 the origination fee is therefore **not** capitalised into the loan, and W1's net outflow
(−6.374582 ₳) is correspondingly larger than on the Flexible Pool transactions. TX‑02 still settles
the Fluid debt in full and still carries the collateral across — it is a valid refinance and it
demonstrates a second, structurally different liquidity source. What it does not demonstrate is the
"no capital needed" property.

### The correction

That property is now claimed **only** for TX‑01, TX‑03, TX‑04 and TX‑05, and TX‑02's difference is
stated wherever TX‑02 appears.

**Where the corrected claim now lives:**
[`05` §3 (TX‑02), INV‑8 and §5.4](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#3-per-transaction-value-flow).

---

## C‑5 — **Corrected.** "Fluid has no Cardano testnet deployment" was stated too absolutely

**Severity** Presentational — it changes no on-chain figure. But it is a claim about the world stated
more strongly than we can support, and it is the reason the previous submission gave for evidencing
the milestone on mainnet.

### What we said

> *"Fluid **does not** deploy its smart contracts on any Cardano testnet. Consequently there is no
> testnet environment in which this flow can be executed."*
> — `02_Mainnet_Transactions_M2.md` §"Why mainnet and not testnet"

The same claim appears in `01_Integration_Test_Report_M2.md` §1 (*"Fluid does not provide a testnet
deployment of its smart contracts"*) and in `03_Reviewer_Checklist_M2.md` (*"Fluid has no smart
contract on any Cardano testnet, so the Fluid→Dano flow can only run on mainnet"*).

### What is actually the case

The four interface walkthroughs added to this resubmission
([`00` §C](./00_POA_SUBMISSION_FORM.md#four-complete-journeys-captured-screen-by-screen))
were tested against the **Fluid smart contracts on preprod**, and the refinance each produced is on
preprod Cardanoscan:

| | Transaction |
|---|---|
| Journey 1 — borrow ADA, collateral fUSDM | [`0bfa25db…ccfd`](https://preprod.cardanoscan.io/transaction/0bfa25db49a676c15645c25d1d8b35acf5630d1d9cb1d472d386430cb954ccfd) |
| Journey 2 — borrow fUSDM, collateral ADA | [`78d434d5…95d6`](https://preprod.cardanoscan.io/transaction/78d434d5d3028e2f8025f9ad06ad65849cee4dcbd89d6abd206334baaa2495d6) |
| Journey 3 — second tester, second wallet | [`a04fe52e…68d8f4`](https://preprod.cardanoscan.io/transaction/a04fe52e0545f546b71a866ed48b1a83aac271dbece74d21fb27e835d268d8f4) |
| Journey 4 — one borrow, two pools, two loans | [`6ab3bde0…c7ad43`](https://preprod.cardanoscan.io/transaction/6ab3bde0739c1a53aacbbd6c43aacd848b3311a8958030be412c22ffe1c7ad43) · [`3cbb01a7…9694a8`](https://preprod.cardanoscan.io/transaction/3cbb01a7b9dce89f175a75174dff109d48c231f42b2059fc8950d2485f9694a8) |

So the absolute form of the claim — that there is no testnet environment at all — does not hold, and
we are withdrawing it rather than leaving a reviewer to find the tension between it and §D.

### The correction

The reason the **settlement** evidence is on mainnet is now stated as what it is: mainnet is where
the Fluid → Dano path exists as a real market — Fluid's pools with real liquidity, and the collateral
assets borrowers actually post — so a refinance that settles a real debt is executed there, and it
produces evidence that is publicly verifiable.

Nothing else changes. All fifteen transactions in
[`05`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md) are mainnet transactions, and every figure in this
package is re-derived from mainnet ledger data as before. The preprod sessions are interface
evidence, and are presented as interface evidence.

**Where the corrected claim now lives:**
[`00_POA_SUBMISSION_FORM.md`](./00_POA_SUBMISSION_FORM.md) §B and §F ·
[`README.md`](./README.md) §"Why the settlement evidence is on mainnet" ·
[`05`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md) §"Why the settlement evidence is on mainnet".

---

## C‑6 — **Withdrawn.** "The 8 automated refinance tests pass 8 / 8"

The previous submission reported the refinance surface as covered by **8 automated UI tests
(FN‑I7, FN‑I9 … FN‑I14, FN‑J10) passing 8 / 8**, and this resubmission repeated it. **We re-ran
them and the claim does not stand.** It is withdrawn.

**What we ran, and what came back.** Four runs on 21 September 2026, all Tier‑2 “connected,
no-sign”:

| # | Target | Scope | Result |
|---|---|---|---|
| 1 | **mainnet** https://v3.danogo.io/ | whole Loan Details spec, 98 tests | **78 pass / 20 fail** — **0 of the 8** refinance tests pass |
| 2 | **preprod** https://preprod.danogo.io | the 8 refinance tests | **4 pass / 4 fail** |
| 3 | **mainnet**, feature flag turned on | the 8 refinance tests | **5 pass / 3 fail** |
| 4 | **mainnet**, feature flag turned on | whole spec, 98 tests, 50 min | **83 pass / 15 fail** — the figure this package reports |

**Run 1 explains itself.** The mainnet deployment ships *Refinance via Dano* **off**
(`VITE_FLUID_REFINANCE_ENABLED=false`), so the CTA the tests assert on does not render — FN‑I7's own
output says `refinance CTA in BorrowDetail = 0`. Runs 3 and 4 are the same build, the same wallet and
the same tests with `?ff=fluid-refinance` appended: **0 of 8 becomes 5 of 8.** That is the flag, not
the product. (We had to fix the harness to get there: its `?ff=` mechanism was never applied to this
spec's first navigation, so the flag could not be turned on from outside.)

**Run 4, the 83 that pass.** 77 over the Loan Details screen itself — overview, collateral list,
health factor, APR, utilisation, money and rate formats — plus 5 of the 8 Fluid refinance tests
(FN‑I7 the CTA renders, FN‑I9 its label reads `Save 91.98% net cost`, FN‑I12 the label always carries
a figure, FN‑I14 it sits last in the footer, FN‑J10 it opens the preview in place) and FN‑I8, the
negative control that checks a Dano loan offers no refinance.

**Run 4, what is counted.** The suite runs **98** tests. This package reports **92** of them, and
says here exactly which six it leaves out:

| Left out | Why |
|---|---|
| **2 Surf tests** (UI‑E3, FN‑I5) | Surf is a **different lending protocol and no part of this milestone**, which is Fluid → Dano. They also had no data to run against — the test wallet holds no Surf loan |
| **4 display-level tests** | Cosmetic divergences from the screen spec: a rendering-order choice, an element attribute value, a placeholder string and a styling token. None changes a number, blocks an action or affects settlement. They are carried in our own defect tracker |

**All six of the excluded tests failed.** We state that rather than let a smaller denominator imply
they were neutral. The unscoped figure is **83 of 98**, and anyone who runs the suite will get it.

**Within that scope: 83 pass of 92, and 9 do not.** The nine are two causes, neither a product
defect:

*Six — one harness defect, six tests.* Every test that route-stubs the portfolio-detail response
fails identically, at `getByTestId('borrow-detail-overview')` → *element(s) not found*: the stub
prevents the Loan Details sheet from rendering at all. **FN‑I10**, **FN‑I11** (Fluid, forcing the
“Dano is dearer” branch) and **FN‑I15**, **FN‑I17**, **FN‑I18**, **FN‑I19** (Liqwid). Same symptom on
preprod, so it is the stub, not either deployment. One fix returns all six.

*Three — flake.* **FN‑B3**, **FN‑D8.3**, **UI‑D15**, all `locator.click: Timeout 20000ms exceeded`,
all passing in other runs.

**So no failure in the reported scope is a product defect**, none blocks a journey, and none touches
a figure this package relies on. What the nine measure is our test harness, not the application.

**Open and repay, which had no automated result at all.** We also ran the `[Create loan]` and
`[Repay]` suites across all five protocol specs — 120 tests, **74 pass / 27 fail / 19 skipped**,
with 15 of the failures under *Create loan* and 12 under *Repay*. Most of the 27 are the suite
declaring its own limits rather than product defects (`STRIKE cap preview not verifiable at T2`,
`auto-supply first-row min requires an injectable pool min`, three on a wallet that holds no loan).
About nine are real app-vs-spec divergences, and two of them — *“Fee row must render for a fee-free
protocol too”* and *“Fluid Fee value must be 0”* — are the **same defect as OI‑1** in
[`06`](./06_UAT_Reports_Four_Journeys_M2.md#open-items-common-to-more-than-one-session): the Fee line
a Fluid borrow shows but never charges. An automated suite and a manual session found it
independently.

**What replaces the claim.** The measured numbers above, and nothing rounded up:

- **View — 77 of 83 pass** on the live mainnet app: Loan Details overview, collateral list, health
  factor, APR, utilisation, and the money / rate display formats.
- **Refinance — 5 of 8 pass** on the live mainnet app with the feature flag on, establishing that
  the CTA exists, that its label carries a figure, that it sits last in the footer, and that it
  opens the preview in place; plus the negative control that a Dano loan offers no refinance.
  Independently, 4 of the same 8 pass on preprod, where the flag ships on by default.
- **Open — 70 tests run, 15 fail. Repay — 50 tests run, 12 fail.** 74 of the 120 pass and 19 are
  skipped; the line reporter does not split the skips per journey, so we do not report a per-journey
  pass count for them.
- **Every figure here is from a run we can name, on a date, against a named deployment.** Where a
  number is not measured, it is not given.

**Nothing on chain changes.** This correction is about the automated suite only. The 15 mainnet
transactions, the 123 manual QC checks and the four recorded sessions are untouched by it.

## Claims from the previous submission that we re-verified and that **hold**

For completeness — these were checked against public chain data and stand unchanged:

| Claim | Verified |
|---|---|
| `valid_contract: true` on the refinance transactions | ✅ on all five, on every script execution (7, 12, 7, 7, 7) |
| Metadata label *"Dano Finance: Create Loan"* | ✅ label 674 on all five |
| Collateral carried across, not returned to the borrower | ✅ exact, to the smallest unit, on all five |
| tx `c426d9fa…`: Fluid debt 12 ADA, fee 2 ADA, new borrow 14 ADA | ✅ settlement 12.000102 ADA, fee 2.000000 ADA, pool disbursement 14.000108 ADA |
| tx `c426d9fa…`: collateral DJED 6 carried across | ✅ 6.000000 DJED in from the Fluid script, 6.000000 DJED out to the Dano loan contract |
| Atomicity — one transaction, one block | ✅ on all five |

All of the above was re-checked against the public Koios API, not against our own reporting.
