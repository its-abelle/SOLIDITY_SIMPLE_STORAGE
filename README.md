# Foundry Simple Storage

A simple Solidity project using Foundry that demonstrates storing and retrieving a user's favorite number.

This repository contains a minimal smart contract, a deployment script, and Foundry configuration to build, test and deploy locally using Anvil.

Summary
- Contract: `src/SimpleStorage.sol`
  - Stores a single `uint256` favorite number and exposes `store` and `retrieve` functions.
  - Keeps a list of people with `Person { uint256 favoriteNumber; string name; }` and a `mapping(string => uint256) nameToFavoriteNumber`.
- Deployment script: `script/DeploySimpleStorage.s.sol` (uses `forge-std/Script.sol`)

Prerequisites
- Foundry (forge, cast, anvil). Install with:
  ```bash
  curl -L https://foundry.paradigm.xyz | bash
  foundryup
  ```
- (Optional) VS Code for editing and the Remote - WSL extension if using WSL.

Quick usage

1. Build

```bash
forge build
```

2. Run local node (Anvil)

```bash
anvil
```

3. Run tests

```bash
forge test
```

4. Deploy locally

Start `anvil` in one terminal, then in another run:

```bash
# local RPC + direct private key
forge script script/DeploySimpleStorage.s.sol:DeploySimpleStorage \
  --rpc-url http://127.0.0.1:8545 \
  --broadcast \
  --private-key <your_private_key>
```

Environment variables
- Recommended to use a `.env` file for `RPC_URL` and `PRIVATE_KEY` (do NOT commit `.env`):

```
PRIVATE_KEY=012345... (64 hex characters, no 0x prefix)
RPC_URL=http://127.0.0.1:8545
```

Important notes / Troubleshooting
- Private key format: must be exactly 64 hex characters (no `0x`). If Forge reports `Failed to decode private key`, check for hidden Windows CRLF characters in `.env`.
  - Remove CRLF from `.env`:
    ```bash
    sed -i 's/\r$//' .env
    ```
  - Or re-export the key in your shell (no trailing newline):
    ```bash
    export PRIVATE_KEY=$(tr -d '\r' < .env | sed -n 's/^PRIVATE_KEY=//p')
    ```
- If Forge cannot connect to the RPC (`Connection refused`), ensure `anvil` is running and listening on the configured port.
- If WSL cannot resolve hosts (`curl: Could not resolve host`), fix DNS for WSL by setting `/etc/resolv.conf` and disabling auto generation (see WSL docs) or run `wsl --shutdown` and reopen WSL.

Security
- Never commit private keys or `.env` to version control. Add `.env` to `.gitignore`.

Project structure

```
.
├─ src/SimpleStorage.sol        # main contract
├─ script/DeploySimpleStorage.s.sol
├─ test/                       # tests (none present in our case)
├─ foundry.toml
└─ README.md
```



License

MIT
