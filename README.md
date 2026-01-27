# MEVScan Claude Code Plugin

Claude Code plugin for MEV transaction analysis. Provides trace tree inspection, DEX pool lookup, and expert knowledge for identifying MEV patterns.

## Install

```
/plugin marketplace add https://github.com/matrooslabs/mevscan-claude-plugin
/plugin install mevscan
```

## What it provides

- **MCP tools** — `tx_tree` (transaction trace) and `get_pool` (DEX pool lookup) via a remote MCP server.
- **Transaction analysis skill** — teaches Claude how to parse traces, identify MEV patterns (sandwich, arbitrage, JIT liquidity, liquidation), and report on gas economics.

## Usage

Once installed, ask Claude to analyze any transaction:

```
Analyze transaction 0x...
```
