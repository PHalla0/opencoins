# OpenCoins Launchpad MCP

An [OpenCode.ai](https://opencode.ai) plugin that enables AI-assisted token deployment on EVM and Solana networks with built-in launchpad functionality and automated fee collection.

## Features

🚀 **Multi-Chain Support**

- Deploy tokens on EVM networks (Ethereum, BSC, Polygon, Arbitrum, Optimism, Base)
- Deploy tokens on Solana (mainnet, devnet, testnet)
- Support for both mainnet and testnet deployments

💧 **Automated Liquidity Pool Creation**

- **EVM**: Automatic pool creation on Uniswap V2 and compatible DEXes
- **Solana**: Multi-DEX support (Raydium, Meteora, Jupiter) with guided setup
- **Auto fee exclusion**: Pools automatically excluded from transfer fees
- One-click deployment with liquidity in the same transaction

💰 **Automated Fee Collection**

- Built-in 1% transfer fee on all token transfers
- EVM: Fee mechanism integrated into smart contract
- Solana: Uses Token-2022 Transfer Fee Extension
- Fees automatically sent to your configured wallet
- **Smart pools excluded** from fees to prevent drainage

🤖 **AI-Powered Deployment**

- Natural language commands to deploy tokens
- Interactive wizard guides you step-by-step
- Automated parameter validation and error handling
- Comprehensive deployment logging

🔒 **Security First**

- Input validation for all parameters
- Private key handling best practices
- Automatic pool fee exclusion
- Open-source and auditable code

## Installation

### Option 1: From npm (Recommended - Easy!)

Simply add to your OpenCode config file (`~/.opencode/config.json`):

```json
{
  "plugins": ["@opencoins/launchpad-plugin"]
}
```

**That's it!** OpenCode.ai will:

- Download the plugin from npm
- Install dependencies automatically
- Use the pre-compiled code
- Ready to use immediately

No build required! 🎉

### Option 2: From Source (For Development)

If you want to modify the plugin or contribute:

```bash
# Clone the repository
git clone https://github.com/singulargh/opencoins.git
cd opencoins

# Install dependencies
npm install

# Build the plugin
npm run build
```

Add to your OpenCode configuration file (`~/.opencode/config.json`):

```json
{
  "plugins": ["file:///path/to/opencoins"]
}
```

#### From npm (Coming Soon)

```bash
npm install @opencoins/launchpad-plugin
```

### 2. Configure Your Fee Collector Addresses (Plugin Creator Only)

> [!IMPORTANT]
> **If you're a user of this plugin**: Fee collector addresses are already configured. Skip this step.
>
> **If you're forking this plugin**: Edit `src/config.ts` and add your wallet addresses:

```typescript
export const PLUGIN_CONFIG = {
  FEE_COLLECTOR_EVM: "0xYourEvmAddress",
  FEE_COLLECTOR_SOLANA: "YourSolanaAddress",
  // ...
};
```

**Optional RPC Configuration** (for all users):

Copy `.env.example` to `.env` if you want to use custom RPC endpoints:

```bash
cp .env.example .env
```

## Usage

### 🎯 Recommended: Guided Token Launch (Interactive Wizard)

The easiest way to launch a token! The wizard guides you step-by-step:

```
"I want to launch a token"
"Help me create a new token"
"Start the token wizard"
```

The AI will:

1. Ask for blockchain choice (EVM or Solana)
2. Help you select a network
3. Guide you through token parameters (name, symbol, supply)
4. Request deployment credentials
5. **Ask if you want to create a liquidity pool** 💧
6. Collect pool parameters (token amount, ETH/SOL amount)
7. **Deploy token and create pool (if requested)** 🚀

#### Liquidity Pool Creation

The wizard now includes an **optional liquidity pool creation** step:

**For EVM Networks (Ethereum, BSC, Polygon, etc.):**

- Automatically creates pools on **Uniswap V2** and compatible DEXes
- Approves token spending and adds liquidity in one flow
- Returns pair address and LP token information

**For Solana:**

- Provides guidance for manual **Raydium** pool creation
- Displays all necessary information (mint address, amounts)
- Links to Raydium UI for easy setup

📖 **Full guide**: See [docs/GUIDED_LAUNCH.md](docs/GUIDED_LAUNCH.md)

---

### Advanced: Direct Deployment Tools

For experienced users who know exactly what they want:

Use natural language with OpenCode.ai:

```
Deploy an ERC-20 token called "MyToken" with symbol "MTK" and 1 million supply on Sepolia testnet
```

Or use the tool directly:

```typescript
{
  "tool": "deploy-evm-token",
  "args": {
    "network": "sepolia",
    "name": "MyToken",
    "symbol": "MTK",
    "decimals": 18,
    "totalSupply": "1000000",
    "privateKey": "0x..."
  }
}
```

**Response:**

```json
{
  "success": true,
  "message": "Token MyToken (MTK) deployed successfully on Sepolia Testnet",
  "tokenAddress": "0x...",
  "transactionHash": "0x...",
  "blockExplorer": "https://sepolia.etherscan.io/address/0x...",
  "network": "Sepolia Testnet",
  "deployer": "0x...",
  "feeCollector": "0x...",
  "feePercentage": "1%"
}
```

### Deploy Solana Token

```
Create a Solana token named "SolToken" with symbol "SOL" and 10 million supply on devnet
```

Or use the tool:

```typescript
{
  "tool": "deploy-solana-token",
  "args": {
    "network": "devnet",
    "name": "SolToken",
    "symbol": "SOL",
    "decimals": 9,
    "supply": "10000000",
    "keypair": "[1,2,3,...]" // JSON array from keypair file
  }
}
```

### Get Token Information

```
Show me information about the token at address 0x... on Ethereum
```

Or:

```typescript
{
  "tool": "get-token-info",
  "args": {
    "address": "0x...",
    "network": "ethereum",
    "chain": "evm"
  }
}
```

## Supported Networks

### EVM Networks

**Mainnets:**

- `ethereum` - Ethereum Mainnet
- `bsc` - BNB Smart Chain
- `polygon` - Polygon
- `arbitrum` - Arbitrum One
- `optimism` - Optimism
- `base` - Base

**Testnets:**

- `sepolia` - Sepolia Testnet
- `bscTestnet` - BSC Testnet
- `mumbai` - Mumbai Testnet

### Solana Networks

- `mainnet` - Solana Mainnet Beta
- `devnet` - Solana Devnet
- `testnet` - Solana Testnet

## DEX Integration & Liquidity Pools

### Automated Pool Creation (EVM)

The plugin integrates with **Uniswap V2** and compatible DEXes to automatically create liquidity pools:

**Supported DEXes by Network:**

- **Ethereum**: Uniswap V2
- **Sepolia**: Uniswap V2 Testnet
- **BSC**: PancakeSwap
- **Polygon**: QuickSwap
- **Arbitrum**: SushiSwap
- **Optimism**: SushiSwap
- **Base**: BaseSwap

**What happens automatically:**

1. ✅ Token approval for DEX router
2. ✅ Pair creation (if doesn't exist)
3. ✅ Liquidity addition with your specified amounts
4. ✅ LP tokens sent to your wallet

**Requirements:**

- Token balance for pool
- Native currency (ETH/BNB/MATIC) for pool
- Additional gas fees

### Manual Pool Creation (Solana)

For Solana tokens, the wizard lets you **choose your preferred DEX** and provides guidance:

**Supported DEXes:**

- **Raydium** - Most popular, highest liquidity
- **Meteora** - Dynamic liquidity pools with flexible fees
- **Jupiter** - Aggregator with pool creation capabilities

**What the wizard does:**

- Asks which DEX you prefer
- Displays your token mint address
- Shows recommended amounts
- Links directly to the selected DEX's pool creation UI
- Provides step-by-step instructions

> [!NOTE]
> Solana DEX pool creation requires OpenBook/Serum market setup (for Raydium) or other complex configurations. The wizard makes this process as easy as possible by providing all necessary information and linking to the appropriate platform.

## Fee Mechanism (Launchpad Service Model)

### How It Works

**This plugin operates as a launchpad service.** Every token deployed through this plugin automatically sends 1% of all transfers to the plugin creator as a service fee.

### EVM (Ethereum, BSC, Polygon, etc.)

The deployed ERC-20 contract includes a built-in 1% transfer fee:

- **Automatic Deduction**: On every transfer, 1% is automatically deducted
- **Fee Recipient**: Sent to the plugin creator's address (hardcoded in `src/config.ts`)
- **Cannot Be Changed**: Fee recipient is immutable once contract is deployed
- **Exclusions**: Plugin creator can exclude specific addresses (e.g., DEX pools) if needed

### Solana

Uses Token-2022's Transfer Fee Extension:

- **Transfer Fee Config**: 1% (100 basis points) on all transfers
- **Withheld Fees**: Fees are withheld in token accounts
- **Fee Withdrawal**: Plugin creator can withdraw accumulated fees
- **Cannot Be Changed**: Fee configuration is immutable once token is created
- **Standard Compliant**: Follows SPL Token-2022 specification

### Why This Model?

The launchpad service model ensures:

- ✅ Sustainable development and maintenance of the plugin
- ✅ Professional smart contracts with security best practices
- ✅ Ongoing support and updates
- ✅ Fair compensation for providing the infrastructure

## Security Considerations

> [!WARNING]
> **Private Key Safety**
>
> - Never commit `.env` files to version control
> - Use environment variables or secure key management
> - Consider using hardware wallets for production deployments

> [!CAUTION]
> **Smart Contract Audits**
>
> - The provided contracts are templates
> - Always audit contracts before mainnet deployment
> - Consider professional security audits for large deployments

> [!IMPORTANT]
> **Fee Disclosure**
>
> - Clearly communicate the 1% fee to token holders
> - Consider providing fee exclusion for liquidity pools
> - Ensure compliance with local regulations

## Development

### Build

```bash
npm run build
```

### Watch Mode

```bash
npm run dev
```

### Testing

```bash
# Run all tests
npm test

# Run integration tests
npm run test:integration
```

### Compile Solidity Contracts

```bash
npm run compile-contracts
```

## Documentation

**Getting Started:**

- [Guided Token Launch](docs/GUIDED_LAUNCH.md) - Step-by-step wizard
- [Quick Start](docs/QUICKSTART.md) - Get running fast
- [Usage Guide](docs/USAGE.md) - Detailed examples

**Technical:**

- [Smart Contracts](docs/CONTRACTS.md) - Contract architecture & fees
- [Security](docs/SECURITY.md) - Security best practices
- [Configuration](docs/CONFIGURATION.md) - Advanced configuration
- [Deployment](docs/DEPLOYMENT.md) - Production deployment

**Development:**

- [NPM Publication](docs/NPM_PUBLICATION.md) - Publishing guide
- [Git Guidelines](docs/WHAT_TO_COMMIT.md) - Commit standards

## Contributing

We welcome all contributions! 🎉

- 🐛 Bug reports and fixes
- 💡 Feature suggestions and implementations
- 📝 Documentation improvements
- ✨ Code enhancements

**Get started:** Read our [Contributing Guide](CONTRIBUTING.md) for guidelines on:

- Development setup
- Coding standards
- Pull request process
- Testing requirements

## License

MIT License - see [LICENSE](LICENSE) for details

## Support

- GitHub Issues: [https://github.com/singulargh/opencoins/issues](https://github.com/singulargh/opencoins/issues)
- Documentation: [https://github.com/singulargh/opencoins/docs](https://github.com/singulargh/opencoins/docs)

## Disclaimer

This software is provided "as is" without warranty. Use at your own risk. Always conduct thorough testing and audits before deploying to production networks.
