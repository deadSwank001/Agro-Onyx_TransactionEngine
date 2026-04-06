# Agro-Onyx_TransactionEngine
Transaction Engine, Core Protocol Logic, and Routing Engine

Building the core logic for an aggregator on Solana that interacts with Onyx-coin and Uniswap requires a multi-step routing engine. Since Uniswap primarily operates on EVM-compatible chains (though it recently expanded support for Solana tokens via Jupiter integration), this engine must handle cross-chain state or utilize the Solana-native endpoints for these protocols.

Below is a Python implementation using solana-py and solders. This logic focuses on the Transaction Routing Engine, which fetches quotes from multiple sources, calculates the net return (Output - Gas), and selects the optimal path.

Implementation Highlights
Dual-Source Fetching: The find_best_route function uses asyncio.gather to ping both Onyx and Uniswap endpoints simultaneously. This minimizes the latency between the user clicking your UI's "hot-button" and the transaction being ready.

Net Return Logic: Instead of just looking at the out_amount, the engine subtracts the estimated_gas. On Solana, gas is usually low, but for complex cross-chain or "side-chain" logic, different programs can have significantly different compute unit (CU) costs.

Atomic Execution: The protocol logic is designed to be atomic. By using versioned transactions (available in the solders toolkit), you can bundle the search, the swap, and the bridge fee into a single transaction to ensure the user doesn't get "stuck" mid-swap.

For your frontend, ensure the landing.ts script passes the slippage_bps and amount_in parameters directly to this Python backend's v1/quote endpoint as defined in your repository structure.
