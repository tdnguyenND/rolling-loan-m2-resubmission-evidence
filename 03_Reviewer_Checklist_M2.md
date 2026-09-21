> ### ⚠️ Superseded — this file is from the **previous** Milestone 2 submission
>
> It is kept at its original path so that links from the previous Proof of Achievement still
> resolve. It has **not** been edited, so anything it says that we later found to be wrong is
> still wrong here — deliberately.
>
> **Start at [`README.md`](./README.md)** for the resubmission.
> **The mapping below is superseded by
> [`07_ACCEPTANCE_CRITERIA_M2.md`](./07_ACCEPTANCE_CRITERIA_M2.md)**, which maps the same Outputs,
> Acceptance Criteria and Evidence items to what the resubmission can actually show. Two rows below
> are ticked ✅ on grounds the resubmission no longer stands on:
>
> - **AC3** is ticked here on the strength of the **usability self-assessment that has since been
>   withdrawn** (see [`08` C‑2](./08_CORRECTIONS.md)). The resubmission makes no usability claim; it
>   offers the stability evidence and the wallets actually exercised —
>   [`07` §B.1](./07_ACCEPTANCE_CRITERIA_M2.md#b1-stability-and-the-wallets-exercised).
> - **AC4** is ticked here on **8/8 automated refinance tests** alone. **That figure is withdrawn**
>   and **nothing replaces it**: all eight carry a `KNOWN-FAIL` annotation in their own source and two
>   never reached their own assertion. See [`08` C‑6](./08_CORRECTIONS.md); what the resubmission
>   offers instead is published check by check in
>   [`07` §B.2](./07_ACCEPTANCE_CRITERIA_M2.md#b2-the-integration-testing-this-package-publishes).
> - The **Demo video** rows point at https://youtu.be/z07TxLJLC2w, a developer walking through the
>   feature. That is **withdrawn as evidence**; the resubmission offers five unedited session
>   recordings instead — [`videos/`](./videos/), cross-referenced in
>   [`06`](./06_UAT_Reports_Four_Journeys_M2.md).
> **[`08_CORRECTIONS.md`](./08_CORRECTIONS.md)** lists, with the on-chain arithmetic, every claim in
> this file that we have since corrected or withdrawn.

---

# Milestone 2 — Reviewer Checklist

Maps every milestone Output, Acceptance Criterion, and Evidence item to the concrete artifact that
satisfies it. "Rolling Loan" = the "Refinance via Dano" feature.

## A. Milestone Outputs

| Output | Satisfied by | Status |
|---|---|---|
| Integrated front-end with loan actions (view, open, repay, refinance) | Live app https://v3.danogo.io/ ; [`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) §1–§2 | ✅ |
| Completed connection to back-end API endpoints | `01` §1 (journey exercises the APIs); `02` §3 (contract interaction) | ✅ |
| Ready-to-test user journeys | `01` §1 (manual journey), §3 (automated tests) | ✅ |
| Demo video | https://youtu.be/z07TxLJLC2w | ✅ |

## B. Acceptance Criteria

| # | Criterion | Satisfied by | Status |
|---|---|---|---|
| AC1 | View / open / repay / refinance via **Eternl** wallet | `01` §1 (manual journey via Eternl); demo video | ✅ |
| AC2 | Back-end correct contract interactions for all loan states | `02` — mainnet tx `c426d9fa…`, `valid_contract: true`; `01` §2 (input/output & asset-conservation verification) | ✅ |
| AC3 | Interface stable (no crash / data mismatch) and intuitive | `01` §4 | ✅ |
| AC4 | Major journeys covered by integration tests | `01` §3 — 8/8 automated refinance tests pass | ✅ |

## C. Evidence of Completion

| # | Evidence item | Artifact | Status |
|---|---|---|---|
| 1 | Published integration test report (successful user journeys) | [`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) | ✅ |
| 2 | Demo video of workflows | https://youtu.be/z07TxLJLC2w | ✅ |
| 3 | Links to deployments showing correct contract interactions | [`02_Mainnet_Transactions_M2.md`](./02_Mainnet_Transactions_M2.md) | ✅ |

## D. Reviewer notes addressed proactively

| Potential concern | Our response |
|---|---|
| "Evidence should be on testnet, not mainnet" | Fluid has no smart contract on any Cardano testnet, so the Fluid→Dano flow can only run on mainnet; the mainnet tx is public and independently verifiable |
| "Is this just a repeat of M1?" | M1 proved the mechanism between two Dano contracts; M2 delivers the real cross-protocol case (Fluid → Dano) integrated end-to-end |
| "How is on-chain success verified?" | Public Cardanoscan tx `c426d9fa…`: Fluid loan UTxO consumed, Dano Float loan UTxO created, DJED collateral carried, `valid_contract: true` |
