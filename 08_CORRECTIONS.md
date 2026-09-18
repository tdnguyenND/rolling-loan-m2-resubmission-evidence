# Corrections to the Previous Submission

**Project** 1400107 · **Milestone 2**

While preparing this resubmission we re-derived every on-chain figure from a **public Cardano API
(Koios)** rather than from our own reporting. That exercise found errors and overstatements in
our own evidence — a mis-cited transaction, a mis-identified token, a self-assessment doing more
work than it could carry, and two claims stated more absolutely than what we can show. Each is
recorded here, C‑1 to C‑5.

One further change is not a correction but is worth stating in the same place: this resubmission does
not rest any part of its case on test-suite figures. The previous submission's *"8/8 automated
refinance tests pass"* was true, but it is not what we are asking the reviewer to rely on. What we
are asking them to rely on is the Cardano ledger and Fluid Tokens' own records, both reproducible
from public APIs.

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
row could carry, and the reviewer was right to stop on it.

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

The two interface walkthroughs added to this resubmission
([`00` §D](./00_POA_SUBMISSION_FORM.md#d-interface-walkthroughs--two-complete-journeys-captured-screen-by-screen))
were tested against the **Fluid smart contracts on preprod**, and the transactions they produced are
on preprod Cardanoscan:

| | Transaction |
|---|---|
| Journey 1 — borrow ADA, collateral fUSDM | [`0bfa25db…ccfd`](https://preprod.cardanoscan.io/transaction/0bfa25db49a676c15645c25d1d8b35acf5630d1d9cb1d472d386430cb954ccfd) |
| Journey 2 — borrow fUSDM, collateral ADA | [`78d434d5…95d6`](https://preprod.cardanoscan.io/transaction/78d434d5d3028e2f8025f9ad06ad65849cee4dcbd89d6abd206334baaa2495d6) |

So the absolute form of the claim — that there is no testnet environment at all — does not hold, and
we are withdrawing it rather than leaving a reviewer to find the tension between it and §D.

### The correction

The reason the **settlement** evidence is on mainnet is now stated as what it is: mainnet is where
the Fluid → Dano path exists as a real market — Fluid's pools with real liquidity, and the collateral
assets borrowers actually post — so a refinance that settles a real debt is executed there, and it
produces evidence a third party can verify without our cooperation.

Nothing else changes. All fifteen transactions in
[`05`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md) are mainnet transactions, and every figure in this
package is re-derived from mainnet ledger data as before. The preprod sessions are interface
evidence, and are presented as interface evidence.

**Where the corrected claim now lives:**
[`00_POA_SUBMISSION_FORM.md`](./00_POA_SUBMISSION_FORM.md) §B and §F ·
[`README.md`](./README.md) §"Why the settlement evidence is on mainnet" ·
[`05`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md) §"Why the settlement evidence is on mainnet".

---

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
| Borrower needs no capital beyond the network fee | ⚠️ **holds for four of the five** — funded by the Dano pool on TX‑01, TX‑03, TX‑04 and TX‑05; TX‑02 is the exception, see C‑4 |
| Fluid has no Cardano testnet deployment, so mainnet is the only possible environment | ⚠️ **restated** — mainnet is where this path exists as a real market and where the evidence is third-party-verifiable; the absolute "no testnet deployment" form is withdrawn, see C‑5 |

All of the above was re-checked against the public Koios API, not against our own reporting.
