# library-chain-compatibility-audit

Checks if a Solidity library works unchanged on an EVM chain that deviates from Ethereum.

## Use

```
/library-chain-compatibility-audit <library repo> on <chain>, docs at <URL>
```

Example: `/library-chain-compatibility-audit openzeppelin-contracts on Arc, docs at https://docs.arc.io`

## Needs

- The library repo, checked out locally.
- Chain-aware tooling, for example `arc-forge`, `arc-anvil` and a fork RPC.

## Flow

1. Reads the chain docs and lists the deviations.
2. In the background, runs the library's own tests on Ethereum and on the chain, if tooling allows.
3. Lists candidate contracts by category.
4. Reruns existing tests with chain assets (for example, real USDC instead of a mock).
5. Subagents write and run targeted tests per candidate.
6. Writes `REPORT.md` to the folder you give, else `audit/<chain>/`, and replies with the executive summary.

It runs without questions. It asks only if it cannot continue, for example when the chain name is missing. It resumes from `STATUS.md` in that folder.
