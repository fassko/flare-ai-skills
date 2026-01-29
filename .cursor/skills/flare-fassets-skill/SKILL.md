---
name: flare-fassets
description: Provides domain knowledge and guidance for Flare FAssets—wrapped tokens (FXRP, FBTC, etc.), minting, redemption, agents, collateral, and smart contract integration. Use when working with FAssets, FXRP, FBTC, FAssets minting or redemption, Flare DeFi, agent/collateral flows, or Flare Developer Hub FAssets APIs and contracts.
---

# Flare FAssets

## What FAssets Are

FAssets is a **trustless, over-collateralized bridge** connecting non–smart-contract networks (XRP Ledger, Bitcoin, Dogecoin, Litecoin) to Flare. It creates **wrapped ERC-20 tokens** (FAssets) such as FXRP, FBTC, FDOGE that can be used in Flare DeFi or redeemed for the underlying asset.

**Powered by:**
- **FTSO (Flare Time Series Oracle):** decentralized price feeds
- **FDC (Flare Data Connector):** verifies off-chain actions (e.g. payments on other chains)

**Collateral:** Mix of stablecoin/ETH and native FLR/SGB; agents and a community collateral pool provide over-collateralization.

## Key Participants

| Role | Responsibility |
|------|-----------------|
| **Agents** | Hold underlying assets, provide collateral, redeem for users. Verified via governance. Use *work* (hot) and *management* (cold) addresses. Must meet **backing factor**. |
| **Users** | Mint (deposit underlying → get FAssets) or redeem (burn FAssets → get underlying). No restrictions. |
| **Collateral providers** | Lock FLR in an agent's pool; earn share of minting fees. |
| **Liquidators** | Burn FAssets for collateral when agent collateral falls below minimum; earn rewards. |
| **Challengers** | Submit proof of agent violations; earn from vault on successful challenge. Full liquidation stops agent from new minting. |

## FAsset Workflow

### Minting
1. User selects an agent and **reserves collateral** (pays fee in FLR/SGB).
2. User **sends underlying asset** (e.g. XRP) to the agent on the underlying chain (with payment reference).
3. **FDC verifies** the payment and produces attestation/proof.
4. User (or executor) calls **executeMinting** with proof → FAssets are minted on Flare.

**Fees:** Collateral Reservation Fee (CRF, native), Minting Fee (underlying), optional Executor Fee (native). If minting fails, CRF is not returned.

### Redemption
Users redeem FAssets for the original underlying asset at any time (flow is request → agent pays out on underlying chain).

### Core Vault (CV)
Per-asset vault that improves capital efficiency: agents can deposit underlying into the CV to free collateral. Multisig on the underlying network; governance can pause. Not agent-owned.

## Contracts and Addresses (Flare Mainnet)

| Contract | Address | Purpose |
|----------|---------|---------|
| AssetManagerController | `0x097B93eEBe9b76f2611e1E7D9665a9d7Ff5280B3` | FAssets controller; in Flare contracts registry |
| AssetManager XRP | `0x2a3Fe068cD92178554cabcf7c95ADf49B4B0B6A8` | Mint/burn FXRP, manage collateral |
| FXRP (token) | `0xAd552A648C74D49E10027AB8a618A3ad4901c5bE` | Wrapped XRP ERC-20 |

Get AssetManager and token addresses at runtime from the **Flare Contract Registry** when possible (different per network: Coston2, Songbird, Flare mainnet).

## Developer Integration (High Level)

1. **Reserve collateral:** Call `reserveCollateral(agentVault, lots, feeBIPS, executor)` on AssetManager. Pay CRF via `collateralReservationFee(lots)`. Use `CollateralReserved` event for `collateralReservationId`, payment reference, and deadlines.
2. **Underlying payment:** User sends underlying asset to agent's underlying-chain address with the **payment reference** from the event. Must complete before `lastUnderlyingBlock` and `lastUnderlyingTimestamp`.
3. **Proof:** Use FDC to get attestation/proof for the payment (e.g. Payment attestation type).
4. **Execute minting:** Call `executeMinting(proof, collateralReservationId)` on AssetManager.

**Agent selection:** Use `getAvailableAgentsDetailedList` (or equivalent), filter by free collateral lots and status, then by fee (e.g. `feeBIPS`). Prefer agents with status NORMAL.

**Prerequisites (from Flare docs):** Flare Hardhat Starter Kit, `@flarenetwork/flare-periphery-contracts`, and for XRP payments the `xrpl` package.

## Terminology

- **Underlying network / underlying asset:** Source chain and its native asset (e.g. XRPL, XRP).
- **Lot:** Smallest minting unit; size from AssetManager/FTSO (see "Read FAssets Settings" in reference).
- **Backing factor:** Minimum collateral ratio agents must maintain.
- **CRF:** Collateral Reservation Fee. **UBA:** Smallest unit of the underlying asset (e.g. drops for XRP).

## Minting dApps and Wallets

- Minting dApps: [Oracle Daemon](https://fasset.oracle-daemon.com/flare), [AU](https://fassets.au.cc)
- Wallets: Bifrost, Ledger, Luminite, OxenFlow (Flare + XRPL); MetaMask, Rabby, WalletConnect (Flare EVM); Xaman (XRPL). Dual-network wallets give the smoothest mint flow.

## When to Use This Skill

- Implementing or debugging FAssets minting/redemption (scripts, bots, dApps).
- Resolving agent selection, collateral, fees, or payment-reference flows.
- Integrating with AssetManager, AssetManagerController, or FAsset token contracts.
- Explaining FAssets, FXRP, FBTC, agents, or Core Vault to users or in docs.
- Following Flare Developer Hub FAssets guides and reference.

## Additional Resources

- Official docs and API/reference: [reference.md](reference.md)
- For detailed contract interfaces, mint/redeem scripts, and operational parameters, use the Flare Developer Hub links in reference.md.
