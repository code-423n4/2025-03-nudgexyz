# Nudge.xyz audit details

- Total Prize Pool: $20,000 in USDC
  - HM awards: up to $17,000 USDC
    - If no valid Highs or Mediums are found, the HM pool is $0
  - Judge awards: $1,500 in USDC
  - Validator awards: $1,000 USDC
  - Scout awards: $500 in USDC
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts March 17, 2025 20:00 UTC
- Ends March 24, 2025 20:00 UTC

*There are no QA awards for this audit.*

**Note re: risk level upgrades/downgrades**

Two important notes about judging phase risk adjustments:

- High- or Medium-risk submissions downgraded to Low-risk (QA) will be ineligible for awards.
- Upgrading a Low-risk finding from a QA report to a Medium- or High-risk finding is not supported.

As such, wardens are encouraged to select the appropriate risk level carefully during the submission phase.

## Automated Findings / Publicly Known Issues

The 4naly3er report can be found [here](https://github.com/code-423n4/2025-03-nudgexyz/blob/main/4naly3er-report.md).

The Slither report can be found [here](https://github.com/code-423n4/2025-03-nudgexyz/blob/main/slither.txt).

*Note for C4 wardens: Anything included in this `Automated Findings / Publicly Known Issues` section is considered a publicly known issue and is ineligible for awards.*

- Any issue already mentioned in the first audit of our smart contracts and that we acknowledged.
- No time-lock for admin functions or role management.

# Overview

Nudge is a Reallocation Marketplace that helps protocols incentivize asset movement across blockchains and ecosystems.
Nudge empowers protocols and ecosystems to grow assets, boost token demand, and motivate users to reallocate assets within their wallets—driving sustainable, KPI-driven growth.

With Nudge, protocols can create and fund campaigns that reward users for acquiring and holding a specific token for at least a week. Nudge smart contracts acts as an escrow for rewards, while its backend system monitors participants' addresses to ensure they maintain their holdings of said token for the required period. Nudge provides an all-in-one solution, eliminating the need for any technical implementation by protocols looking to run such incentivisation campaigns.

For more details please see: [What is Nudge?](https://docs.nudge.xyz/)

### Setup

```bash
pnpm install
forge install
```

### Running Tests

```bash
forge test
```

### Audits

[Oak Security Audit Report](https://github.com/oak-security/audit-reports/blob/main/Nudge/2025-03-07%20Audit%20Report%20-%20Nudge%20Campaigns.pdf) - Published on March 7, 2025

Since the Oak Security Audit was completed, we have made the following 2 changes:

1. We added a function to rescue tokens (function rescueTokens(address token)). Without this function, if someone were to send tokens (other than the reward tokens) to a campaign contract, they would become stuck and therefore lost: Commit [23a8098f84d1100baee349be0f33344b68dccf2a](https://github.com/violetprotocol/nudge-smart-contracts/commit/23a8098f84d1100baee349be0f33344b68dccf2a).

2. We changed the logic for campaigns where the campaign admin does not want them to start immediately upon deployment. In this case, the address with the NUDGE_ADMIN_ROLE had to call setIsCampaignActive() to activate the campaign once the startTimestamp was reached. With the change introduced by Commit [e0fe46913140110ba6c1fa68a63c7a41a6dd4db2](https://github.com/violetprotocol/nudge-smart-contracts/commit/e0fe46913140110ba6c1fa68a63c7a41a6dd4db2), the campaign will automatically be turned "active" once the startTimestamp is reached by a call to handleReallocation(). Importantly, a campaign can still be set to active or inactive manually.

## Links

- **Previous audits:** [Oak Security Audit Report](https://github.com/oak-security/audit-reports/blob/main/Nudge/2025-03-07%20Audit%20Report%20-%20Nudge%20Campaigns.pdf)
- **Documentation:** <https://docs.nudge.xyz>, with the most relevant section for the audit being <https://docs.nudge.xyz/technical-documentation/smart-contracts>
- **Website:** <https://nudge.xyz/>
- **X/Twitter:** <https://x.com/nudgexyz>

**Examples of real transactions (tested on Base):**
- New campaign participation: https://basescan.org/tx/0xe11e8013db2118413b91e9281dd253ab0cb4dcb3782567262ab362aebdb7d033
- Reward claiming: https://basescan.org/tx/0x659c2794a62823a0c930b6ce1e9cef6f8d4889278669b036b1e8d28b9704539a
---

# Scope

*See [scope.txt](https://github.com/code-423n4/2025-03-nudgexyz/blob/main/scope.txt)*

### Files in scope

| File   | Logic Contracts | Interfaces | nSLOC | Purpose | Libraries used |
| ------ | --------------- | ---------- | ----- | -----   | ------------ |
| /src/campaign/NudgeCampaign.sol | 1| **** | 272 | |@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol<br>@openzeppelin/contracts/access/AccessControl.sol<br>@openzeppelin/contracts/utils/math/Math.sol<br>@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol|
| /src/campaign/NudgeCampaignFactory.sol | 1| **** | 164 | |@openzeppelin/contracts/access/AccessControl.sol<br>@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol<br>@openzeppelin/contracts/utils/Create2.sol|
| /src/campaign/NudgePointsCampaigns.sol | 1| **** | 129 | |@openzeppelin/contracts/token/ERC20/IERC20.sol<br>@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol<br>@openzeppelin/contracts/access/AccessControl.sol<br>@openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol|
| /src/campaign/interfaces/IBaseNudgeCampaign.sol | ****| 1 | 31 | ||
| /src/campaign/interfaces/INudgeCampaign.sol | ****| 1 | 22 | ||
| /src/campaign/interfaces/INudgeCampaignFactory.sol | ****| 1 | 4 | |@openzeppelin/contracts/access/IAccessControl.sol|
| /src/campaign/interfaces/INudgePointsCampaign.sol | ****| 1 | 19 | ||
| **Totals** | **3** | **4** | **641** | | |

### Files out of scope

*See [out_of_scope.txt](https://github.com/code-423n4/2025-03-nudgexyz/blob/main/out_of_scope.txt)*

| File         |
| ------------ |
| ./src/mocks/MockTokenDecimals.sol |
| ./src/mocks/TestERC20.sol |
| ./src/mocks/TestUSDC.sol |
| ./src/test/NudgeCampaign.t.sol |
| ./src/test/NudgeCampaignAdmin.t.sol |
| ./src/test/NudgeCampaignFactory.t.sol |
| ./src/test/NudgeCampaignReallocation.t.sol |
| ./src/test/NudgePointsCampaigns.t.sol |
| ./src/test/NudgePointsCampaignsHandleReallocationTest.t.sol |
| Totals: 9 |

## Scoping Q &amp; A

### General questions

| Question                                | Answer                       |
| --------------------------------------- | ---------------------------- |
| ERC20 used by the protocol              |       Any (all possible ERC20s)             |
| Test coverage                           | 96%                        |
| ERC721 used  by the protocol            |          None           |
| ERC777 used by the protocol             |          None            |
| ERC1155 used by the protocol            |          None           |
| Chains the protocol will be deployed on | We will launch on Ethereum, but we will also have cross-chain reallocations to Base, Arbitrum, Polygon, Mantle and Optimism, and potentially other EVM-compatible chains in the future (TBD) |

### ERC20 token behaviors in scope

| Question                                                                                                                                                   | Answer |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| [Missing return values](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#missing-return-values)                                                      |   Out of scope  |
| [Fee on transfer](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#fee-on-transfer)                                                                  |  Out of scope  |
| [Balance changes outside of transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#balance-modifications-outside-of-transfers-rebasingairdrops) | Out of scope    |
| [Upgradeability](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#upgradable-tokens)                                                                 |   Out of scope  |
| [Flash minting](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#flash-mintable-tokens)                                                              | Out of scope    |
| [Pausability](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#pausable-tokens)                                                                      | Out of scope    |
| [Approval race protections](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#approval-race-protections)                                              | In scope    |
| [Revert on approval to zero address](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-approval-to-zero-address)                            | In scope    |
| [Revert on zero value approvals](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-approvals)                                    | In scope    |
| [Revert on zero value transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-transfers)                                    | In scope    |
| [Revert on transfer to the zero address](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-transfer-to-the-zero-address)                    | In scope    |
| [Revert on large approvals and/or transfers](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-large-approvals--transfers)                  | In scope    |
| [Doesn't revert on failure](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#no-revert-on-failure)                                                   |  In scope   |
| [Multiple token addresses](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#revert-on-zero-value-transfers)                                          | Out of scope    |
| [Low decimals ( < 6)](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#low-decimals)                                                                 |   In scope  |
| [High decimals ( > 18)](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#high-decimals)                                                              | Out of scope    |
| [Blocklists](https://github.com/d-xo/weird-erc20?tab=readme-ov-file#tokens-with-blocklists)                                                                | Out of scope    |

### External integrations (e.g., Uniswap) behavior in scope

| Question                                                  | Answer |
| --------------------------------------------------------- | ------ |
| Enabling/disabling fees (e.g. Blur disables/enables fees) | No   |
| Pausability (e.g. Uniswap pool gets paused)               |  No   |
| Upgradeability (e.g. Uniswap gets upgraded)               |   No  |

### EIP compliance checklist

N/A

# Additional context

## Main invariants

### Solvency Invariants

Token Balance Integrity

- `rewardToken.balanceOf(campaign) >= pendingRewards + accumulatedFees`
- *Ensures the contract always maintains sufficient reward tokens to cover all pending rewards and accumulated fees.*

Protocol Solvency

- For any active participation p:`p.rewardAmount <= rewardToken.balanceOf(campaign) - (pendingRewards - p.rewardAmount) - accumulatedFees`
- *Guarantees that any individual user's reward can be fully covered by the contract's current token balance, after accounting for fees*

### State Consistency Invariants

Participation State Consistency

- State transitions only occur from `PARTICIPATING` → `CLAIMED` or `PARTICIPATING` → `INVALIDATED`
- Once in `CLAIMED` or `INVALIDATED` state, a participation cannot change state

Participation Tracking Consistency

- `sum(participations[i].toAmount for all i from 1 to pID) == totalReallocatedAmount` *Ensures the sum of all participation amounts matches the tracked total reallocated amount*
- pendingRewards == sum of all participation.rewardAmount where participation.status == PARTICIPATING
The pendingRewards value must equal the sum of reward amounts for all participations in the PARTICIPATING state.
- distributedRewards == sum of all participations.rewardAmount where participation.status == CLAIMED

    The distributedRewards value must equal the sum of reward amounts for all participations in the CLAIMED state.

### Claiming Rules Invariants

Claim Conditions:

- User can only claim rewards for their own participations
- User can only claim rewards after holdingPeriodInSeconds has elapsed
- User can only claim rewards for participations in PARTICIPATING state
- User can only claim rewards for a participation once

## Attack ideas (where to focus for bugs)

We’ll call any type of user of the protocol, a “user” (campaign administrators, end users a.k.a campaign participants, the Nudge team…), collectively referred as “users”.

Here are our main concerns in order from most critical to less critical:

- Loss of funds by any user.
- Accounting and calculation logic: a user receives significantly less, or more, tokens than they should.
- A campaign participant is able to claim rewards without fulfilling the conditions of the campaign. For example, claiming rewards before the end of the holding period.
- Loss of access and permissions granted to the Nudge team.
- Loss of access and permissions granted to the campaign administrator.
- A malicious actor gaining control to a functionality they shouldn’t have access to.
- Service halting (e.g denial of service attacks), rendering critical functionalities of the protocol unusable by users.

## All trusted roles in the protocol

We are using role-based access control in the protocol. Below are the roles that can be found, and how they will be assigned initially:

| Role                                | Description                       |
| --------------------------------------- | ---------------------------- |
| NUDGE_ADMIN_ROLE | This role is given to a multisig (Safe) wallet controlled by Nudge.|
| DEFAULT_ADMIN_ROLE | Similar to NUDGE_ADMIN_ROLE, initially.|
| NUDGE_OPERATOR_ROLE | This role is given to one of our Relayers, submitting transactions programmatically.|
| SWAP_CALLER_ROLE | This role is initially given to one of Li.fi’s contracts (called Executor).|
| CAMPAIGN_ADMIN_ROLE | This role is given to the administrator of a campaign. It is campaign-specific.|

## Describe any novel or unique curve logic or mathematical models implemented in the contracts

N/A

## Running tests

![](https://github.com/user-attachments/assets/c12f4b49-fe5f-48bd-8519-819e5b353c2a)


## Miscellaneous

Employees of Nudge and employees' family members are ineligible to participate in this audit.

Code4rena's rules cannot be overridden by the contents of this README. In case of doubt, please check with C4 staff.
