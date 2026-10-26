# Tooling pitfalls

Check these before you trust a failure.

## Foundry fork

- Prove chain mode with a probe test. Some tools do not show the active mode.
- `evm_version` sets only the compiler target. The chain spec or `hardfork` sets the rules.
- A test that reads files needs `fs_permissions` in `foundry.toml`.
- `foundry-rs/foundry-toolchain` in CI installs upstream Foundry, which runs Ethereum rules.
- Parallel runs in one project collide on `out/` and `cache/`. Use one project per agent, or set `FOUNDRY_OUT` and `FOUNDRY_CACHE_PATH`.
- Pin `--fork-block-number` so RPC responses cache. Public RPCs rate-limit parallel forks.

## Hardhat

- The in-process network simulates only Ethereum and OP Stack. Point Hardhat at a chain-mode node over HTTP.
- A node replies to a send before it mines. Tests that read state without `wait()` read stale state, and fixtures snapshot too early. Fix: a network `onRequest` hook that waits for the receipt after each send, except when automine is off.
- Mocha does not exit after the summary over HTTP. Stop it after `passing` prints.
- Parallel runs race on the compile cache. Compile once, then use `--no-compile`.
- Wall-clock tests are flaky on a node. Rerun chain-only failures.

## Comparison

- Compare Ethereum mode and chain mode of the same tool, never a node against the in-process network.
- Restricted test addresses can differ between testnet and mainnet.
