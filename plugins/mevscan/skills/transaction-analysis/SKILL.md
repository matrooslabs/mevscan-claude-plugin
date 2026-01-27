# Transaction Analysis

Analyze Ethereum transactions for MEV patterns using MEVScan trace data.

## Tools

**`tx_tree`** `{ tx_hash }` — Returns a row from `brontes.tree`: `block_number`, `tx_hash`, `tx_idx`, `from`, `to`, `gas_details` (tuple: `coinbase_transfer`, `priority_fee`, `gas_used`, `effective_gas_price`), `trace_nodes.*` (parallel arrays: `trace_address`, `action_type`, `from`, `to`, `value`, `input`, `output`, `gas_used`, `decoded_function_name`, `decoded_inputs`, `decoded_outputs`), `timeboosted`.

**`get_pool`** `{ pool_address }` — Returns a row from `ethereum.pools`: `protocol`, `protocol_subtype`, `address`, `tokens`, `curve_lp_token`, `init_block`.

## Workflow

1. Call `tx_tree` with the hash.
2. Parse `trace_nodes.*` — each index is one internal call. `trace_address` encodes tree position (empty = root, `[0]` = first child, `[0,1]` = second child of first child).
3. For `to` addresses with swap-like calls, call `get_pool` to identify protocol and tokens.
4. Classify MEV type, compute gas economics, and report.

## Report format

### Overview

Describe briefly what the transaction is doing, including the main protocols involved. Give high-level summary.

### Phase based analysis report

Categorize the actions taken by the transaction into phases such as:

1. **Setup Phase**: Initial actions to prepare for the main operation.
2. **Swap Phase**: Core actions involving token swaps or liquidity provision. Provide a table to the user with following columns:
- `Step`: Description of the action (e.g., "Swap on Uniswap V3").
- `Protocol`: The protocol involved (e.g., "Uniswap V3").
- `Tokens Involved`: List of tokens involved in the action.
- `Execution Price`: Price of the pool at which the swap was executed.
3. **Cleanup Phase** (Optional): Final actions to settle flashloans or collect profit to EOA, etc.

### Profit Calculation

Using the state differences before and after the transaction, calculate the profit made by the transaction in terms of the main token that has been spent and received. Present this in a clear tabular format with following columns:
- `Item`: Description of the item (e.g., "Initial ETH Spent", "Final ETH Received").
- `Amount`: The amount associated with the item.

The item must include gross profit and net profit after gas fees. The profit should be in terms of USD at the time of transaction.
