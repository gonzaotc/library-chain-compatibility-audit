# Deviation classes

Start list. Add classes that the chain docs show.

| Class | Search | Typical impact |
| - | - | - |
| Native token is also an ERC-20 (decimals can differ) | `msg.value`, `.balance`, `released()`, vault `asset()` | Native and token paths pay from one balance. Decimal mismatch |
| Value arrives without code execution | `receive()`, `fallback()` | Contracts get value they cannot reject. Donation paths |
| Transfers to `address(0)` or blocklisted addresses revert | `sendValue`, `call{value:`, batch `execute` | One recipient blocks a batch. Refunds and releases revert |
| SELFDESTRUCT rules beyond EIP-6780 | `selfdestruct`, deploy with value | Deploy with value reverts |
| Fixed or changed opcode values | `prevrandao`, `blobhash`, `blobbasefee`, `coinbase`, `basefee`, `chainid` | Randomness, fee math, cached domains |
| Missing opcodes | `mcopy`, `tstore`, `clz`, `push0` | Deploy or call fails |
| Precompiles | `ecrecover`, P256 `0x100`, modexp, BLS12-381 | Fallback paths or signature checks fail |
| System contracts | EIP-4788, EIP-2935, `blockhash` | Block hash and beacon proofs fail |
| Predeploys | Hard-coded addresses, ERC-1820, EntryPoint | Calls hit empty code |
| Block time and timestamp resolution | `block.number`, `block.timestamp`, `clock()` | Durations sized for 12 s blocks. Shared timestamps |
| Fees and gas | `gasleft()`, stipends, `tx.gasprice`, paymasters | Stipends fail. Prefund math is wrong |
| Contract size limit | Large contracts | Deploy fails |
| Sender changes (EIP-7702, call-from precompiles) | `msg.sender`, `tx.origin`, `_msgSender()` | Sender checks see an unexpected caller |
| Code or balance hidden from callers | `code.length`, `extcodehash`, `.balance` | `SafeERC20` and upgrade checks fail |
