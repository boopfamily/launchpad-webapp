# Boop Family contract package

This directory is the self-contained, GitHub-ready contract package for Boop
Family. The historical deployed source remains outside this package and is not
modified.

Build this directory as the Foundry repository root:

```bash
cd artifacts/boopfamily/contracts
forge build
```

The verification package intentionally uses repository-relative source names:

```text
src/BoopFactory.sol
src/BoopFamilyToken.sol
src/interfaces/IPancakeV3.sol
src/lib/TickMath.sol
lib/openzeppelin-contracts/contracts/...
```

Do not build this package from the monorepo root and do not use workspace
absolute paths in remappings. Those paths leak into Solidity Standard JSON
verification and make BscScan show `.local/conversation-workspace/...`.

Required corrected deployment rules from `boopfamilycorrecting.md`:

- canonical WBNB only
- fixed 1B supply, 0.001 BNB launch fee, Pancake V3 fee tier 10000
- factory-controlled launch price and liquidity range
- fresh price authorization
- enforced 0x5555 CREATE2 suffix
- no creator-controlled tick, positions, quote token, fee tier, spacing, or fee-purpose mode
- delayed exact-operation liquidity recovery via `decreaseLiquidityAndCollect`
- recovery delay defaults to `86400` seconds and can be changed repeatedly by
  the owner with `setLiqRecDel(uint256)`; values below `60` seconds are rejected
- changing the global delay affects future schedules; an existing schedule
  keeps its recorded `executeAfter` and must be cancelled and rescheduled if
  the owner wants a different deadline

The corrected source is intentionally kept separate from the historical
deployment records. A future deployment must be made from this package after
the GitHub fork is connected and the source tree has been reviewed.
