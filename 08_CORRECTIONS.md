# Annex H — Corrections to the Previous Submission

**Project** 1400107 · **Milestone 2**

While preparing this resubmission we re-derived every on-chain figure from a **public Cardano API
(Koios)** rather than from our own reporting. That exercise found one factual error, and one claim
that should never have been offered as evidence. Both are recorded here.

A third change is not a correction but is worth stating in the same place: this resubmission does
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
> — `01_Integration_Test_Report_M2.md` §2.7

The same claim appeared in the transaction annex:

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
([Annex D](./04_ONCHAIN_TRANSACTION_LEDGER.md) INV‑8) — the fee is financed by the loan, not paid
out of pocket. The new debt is therefore the old debt **grossed up** by the fee, not equal to it.

Whether the gross-up happens at all is **per-pool configuration**: a fee-bearing pool produces
`new borrow = debt + fee`, a zero-fee pool produces `new borrow = debt`. Both cases occur among the
five transactions in this package, which is precisely why citing the wrong one matters.

Transaction `88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652` is a fee-bearing pool,
to the smallest unit — it **does** carry an origination-fee leg:

| Leg | Address | Amount |
|---|---|---|
| Dano Flexible Pool disburses | `addr1wx2degj2ru0uctl4rnvs7vh5l608smvxrgkm7lf8txxjd6qs43szs` | **−22.000857 ADA** |
| Fluid debt settlement | `addr1q9mt6pcxvkdnszlj4jeztxzshhvsfzq4zsjgwlgh364yct00mr8eah8c9at` | **+20.000851 ADA** |
| **Dano origination fee** | `addr1qywadgaxcnh993zpzl5kfs806nqe7jyxp4e8unjpll5quymw2aappdz98na` | **+2.000000 ADA** |

The fee address `addr1qywadgax…` receives exactly 2.000000 ADA in this transaction, and the same
2.000000 ADA in transactions `1cf8f08b…` and `c426d9fa…`. It is the fee leg, and it is present.

So the pool was **not** a zero-fee pool, and the borrow was **not** equal to the Fluid debt: it was
the debt (20.000851 ADA) grossed up by the 2.000000 ADA fee, exactly as the specification describes
for a fee-bearing pool. The figure the previous submission offered as the new borrow — 20 ADA — is
the debt that was *settled*; the loan Dano actually opened is 22.000857 ADA, which is why wallet
W1's Dano position reads **22.04 ADA** in the application (principal plus accrued interest,
reconciled row by row in [Annex F](./06_POST_STATE_UI_RECONCILIATION.md) §4).

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
adjustments, visible in the full value flow in [Annex D](./04_ONCHAIN_TRANSACTION_LEDGER.md) §4.)*

**Where the corrected claim now lives:** [Annex D](./04_ONCHAIN_TRANSACTION_LEDGER.md) §4 (TX‑01 and
TX‑05) and the ledger table in §2.

---

## C‑2 — **Restated.** The usability self-assessment

**Severity** This is the row the reviewer rejected the submission over.

### What we said

> | Tester completed the flow without external guidance | ✅ Yes |
> — `01_Integration_Test_Report_M2.md` §4

### Why it is restated

The "tester" was a member of the project team. The statement was true — the flow was completed
without external guidance, repeatedly — but the column it sat in read as a verdict on usability,
which implied an independence that did not exist. The reviewer was right to reject it on that basis.

The row is restated as what it records: **internal testing**. Every journey was walked end-to-end
through the connected wallet, and six of those sessions settled on mainnet and are listed in
[Annex D](./04_ONCHAIN_TRANSACTION_LEDGER.md). It stands as evidence that each journey completes
through the released interface, and it is not offered as a measure of how the interface reads to
someone who did not build it — see [Annex A](./01_REVIEWER_RESPONSE.md), Objection 1.

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
| Borrower needs no capital beyond the network fee | ✅ the settlement leg is funded by the Dano pool on all five |
| Fluid has no Cardano testnet deployment, so mainnet is the only possible environment | ✅ unchanged; no testnet Fluid deployment exists |

All of the above was re-checked against the public Koios API, not against our own reporting.
