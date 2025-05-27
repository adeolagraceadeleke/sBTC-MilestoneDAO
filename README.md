
---

# sBTC-MilestoneDAO - GrantPool Smart Contract

## Overview

This smart contract facilitates a decentralized **grant allocation and milestone-based disbursement** system on the Stacks blockchain. Leveraging SIP-010 compliant fungible tokens, it enables:

* Grant pool creation
* Proposal submission with milestone tracking
* Community voting on proposals
* Conditional approval and milestone completion

## Features

* ✅ SIP-010 Fungible Token Integration
* 📦 Grant Pool Management
* 📄 Proposal Submission with Milestones
* 🗳️ Community Voting Mechanism
* 🧾 Proposal Finalization by Pool Owner
* 📌 Milestone Verification and Completion

---

## 📦 Contract Components

### Traits

#### `ft-trait`

SIP-010 compliant trait interface for interacting with fungible tokens:

```clojure
(define-trait ft-trait
  ...
)
```

---

## 🗃️ Data Structures

### Constants

* `contract-owner`: The account that deployed the contract
* Various `err-*` constants for error handling

### Maps

| Map            | Description                                                        |
| -------------- | ------------------------------------------------------------------ |
| `grant-pools`  | Stores details of grant pools (owner, total amount, token, status) |
| `proposals`    | Stores proposals submitted to pools                                |
| `votes`        | Tracks individual votes per proposal                               |
| `vote-tallies` | Tallies votes for each proposal                                    |

### Data Variables

* `current-pool-id`: Counter for pools
* `current-proposal-id`: Counter for proposals
* `minimum-grant-amount`: Lower bound on proposal amount
* `maximum-grant-amount`: Upper bound on proposal amount
* `minimum-votes-required`: Threshold of total votes for finalization
* `quorum-threshold`: % of positive votes needed for approval

---

## ⚙️ Functionality

### 🔹 Grant Pool Management

#### `create-grant-pool(total-amount, token-contract)`

Creates a new grant pool with a specific SIP-010 token.

* Only callable by the contract owner.
* Requires a valid token balance.

---

### 📄 Proposal System

#### `submit-proposal(pool-id, requested-amount, milestones)`

Submit a funding proposal to a specific grant pool with defined milestones.

* Milestones must not be empty and should total the requested amount.

#### `finalize-proposal(proposal-id)`

Finalizes a proposal based on community votes.

* Callable only by the grant pool owner.
* Applies quorum logic to determine if approved or rejected.

#### `complete-milestone(proposal-id, milestone-index)`

Marks a milestone as completed.

* Callable only by the original proposal applicant.
* Validates index and proposal status.

---

### 🗳️ Voting

#### `vote-on-proposal(proposal-id, in-favor)`

Allows users to cast a vote on a proposal.

* Each user can vote only once per proposal.

#### `get-vote-counts(proposal-id)`

Returns the current vote tally for a proposal.

---

## 🧪 Validation Helpers (Private)

* `validate-pool-id(pool-id)`
* `validate-proposal-id(proposal-id)`
* `validate-amount(amount)`
* `validate-milestones(milestones)`

---

## 🔐 Error Codes

| Code                                | Description                        |
| ----------------------------------- | ---------------------------------- |
| `err-owner-only (err u100)`         | Only owner can perform this action |
| `err-not-found (err u101)`          | Resource not found                 |
| `err-unauthorized (err u102)`       | Unauthorized access                |
| `err-invalid-state (err u103)`      | Invalid state transition           |
| `err-insufficient-funds (err u104)` | Not enough tokens                  |
| `err-invalid-amount (err u105)`     | Amount not within limits           |
| `err-invalid-token (err u106)`      | Token contract issue               |
| `err-invalid-milestone (err u107)`  | Malformed milestone list           |
| `err-insufficient-votes (err u108)` | Not enough votes cast              |
| `err-no-votes (err u109)`           | No votes recorded                  |

---
