# tari.js

> **🚀 The complete TypeScript toolkit for building on Tari** — Connect any wallet, query the blockchain, and create powerful dApps with confidence.

[![npm version](https://badge.fury.io/js/@tari-project%2Ftarijs.svg)](https://badge.fury.io/js/@tari-project%2Ftarijs)
[![License](https://img.shields.io/badge/license-BSD%203--Clause-blue.svg)](LICENSE)
[![Documentation](https://img.shields.io/badge/docs-tari--project.github.io-brightgreen)](https://tari-project.github.io/tari.js/)

**✨ What makes tari.js special?**
- **🔌 Universal Wallet Support** — MetaMask, Wallet Daemon, Universe, WalletConnect — all with one API
- **🛡️ Privacy-First** — Built-in confidential transactions and zero-knowledge proofs
- **📱 Developer Friendly** — Full TypeScript support, intuitive APIs, comprehensive docs
- **⚡ Production Ready** — Battle-tested, optimized, and actively maintained

## Installing Tari.js

Tari.js provides the following packages:

| Package Name                           | Description                                                |
|----------------------------------------|------------------------------------------------------------|
| @tari-project/tarijs-all               | Everything: core, signers, providers, builders, permissions|
| @tari-project/tarijs                   | Core Tari.js types and helpers                             |
| @tari-project/tarijs-builders          | Transaction builder (fluent API)                           |
| @tari-project/indexer-provider         | Read-only blockchain provider                              |
| @tari-project/wallet-daemon-signer     | Wallet Daemon signer & provider                            |
| @tari-project/metamask-signer          | MetaMask signer                                            |
| @tari-project/tari-universe-signer     | Tari Universe wallet signer                                |
| @tari-project/wallet-connect-signer    | WalletConnect signer                                       |
| @tari-project/tari-permissions         | Permissions utility                                        |
| @tari-project/typescript-bindings      | TypeScript type definitions and low-level structures       |
| @tari-project/tarijs-types             | Re-exports typescript-bindings while adding tari.js specific types |

### Recommended: Use the Main Package

This installs everything (providers, signers, core modules, builders, types and permissions) with one package:

```sh
npm install @tari-project/tarijs
```

You can then import any of the required packages in your application:

```typescript
import {
  IndexerProvider,
  WalletDaemonTariSigner,
  TariUniverseSigner,
  WalletConnectTariSigner,
  TransactionBuilder,
  TariPermissions,
} from "@tari-project/tarijs";
```

If you are interested in development on the bleeding edge, you can install the following:

```sh
npm install @tari-project/tarijs-all
```

This will install ALL tari.js modules. Some of these may be under active development, experimental or not properly documented.

### Optional: Install only the modules you require

If you require a minimal or custom setup, you can install the packages individually. This is not recommended unless you have a firm understanding of the purpose and function of the different packages.

Example of dependencies if you just wish to query Ootle states on the chain:

```sh
npm install @tari-project/indexer-provider
npm install @tari-project/wallet-daemon-signer   # Only if you need wallet data models/types
```

#### Core Packages

- **@tari-project/tarijs-builders**  
  ```sh
  npm install @tari-project/tarijs-builders
  ```

#### Providers
- **@tari-project/indexer-provider**  
  ```sh
  npm install @tari-project/indexer-provider
  ```

- **@tari-project/wallet-daemon-signer**  
  ```sh
  npm install @tari-project/wallet-daemon-signer
  ```

#### Signers
- **@tari-project/metamask-signer**  
  ```sh
  npm install @tari-project/metamask-signer
  ```

- **@tari-project/tari-universe-signer**  
  ```sh
  npm install @tari-project/tari-universe-signer
  ```

- **@tari-project/wallet-connect-signer**  
  ```sh
  npm install @tari-project/wallet-connect-signer
  ```

### Helpers

- **@tari-project/tari-permissions**  
  ```sh
  npm install @tari-project/tari-permissions
  ```


## 🎯 Quick Start (5 minutes)

Get your first Tari app running in minutes:

```bash
# Install tari.js
npm install @tari-project/tarijs-all

# Run your first connection test
node -e "
import { WalletDaemonTariSigner, TariPermissions } from '@tari-project/tarijs-all';
const wallet = await WalletDaemonTariSigner.buildFetchSigner({
  serverUrl: 'http://localhost:18103',
  permissions: new TariPermissions()
});
console.log('Connected to Tari!');
"
```

**👉 [Complete Installation Guide](https://tari-project.github.io/tari.js/docs/installation) | [5-Minute Tutorial](https://tari-project.github.io/tari.js/docs/guides/getting-started-tutorial)**

## 🏗️ What You Can Build

### 🪙 **DeFi Applications**
```typescript
// Transfer tokens
const transaction = new TransactionBuilder()
  .feeTransactionPayFromComponent(account.address, "100")
  .callMethod({
    componentAddress: account.address,
    methodName: 'withdraw'
  }, [{ type: 'Amount', value: '1000' }])
  .build();
```

### 🎮 **Gaming & NFTs**  
```typescript
// Call a smart contract function
const transaction = new TransactionBuilder()
  .feeTransactionPayFromComponent(account.address, "100")
  .callFunction({
    templateAddress: nftTemplate.address,
    functionName: 'mint_nft'
  }, [{ name: 'metadata', value: { name: "Epic Sword", rarity: "legendary" } }])
  .build();
```

### 💼 **Enterprise Solutions**
```typescript
// Multiple operations in one transaction
const transaction = new TransactionBuilder()
  .feeTransactionPayFromComponent(account.address, "100")
  .callMethod({ componentAddress: account1, methodName: 'withdraw' }, [amount1])
  .callMethod({ componentAddress: account2, methodName: 'withdraw' }, [amount2])
  .build();
```

## 🔗 Supported Wallets

| Wallet | Best For | Status |
|--------|----------|--------|
| **🖥️ [Tari Wallet Daemon](https://tari-project.github.io/tari.js/docs/signers/wallet-daemon)** | Servers, advanced features | ✅ Production |
| **🦊 [MetaMask](https://tari-project.github.io/tari.js/docs/signers/metamask)** | Browser apps, familiar UX | ✅ Production |
| **🌌 [Tari Universe](https://tari-project.github.io/tari.js/docs/signers/tari-universe)** | Mobile, native experience | ✅ Production |
| **📱 [WalletConnect](https://tari-project.github.io/tari.js/docs/signers/wallet-connect)** | Cross-platform, mobile-first | ✅ Production |

## 📚 Documentation Hub

### 🚀 **Get Started**
- **[Installation Guide](https://tari-project.github.io/tari.js/docs/installation)** — Set up your development environment
- **[First App Tutorial](https://tari-project.github.io/tari.js/docs/guides/getting-started-tutorial)** — Build a working wallet app
- **[Provider vs Signer](https://tari-project.github.io/tari.js/docs/provider-vs-signer)** — Understand the core concepts

### 📖 **Guides & Examples**
- **[Wallet Integration](https://tari-project.github.io/tari.js/category/signers)** — Connect different wallet types
- **[Transaction Building](https://tari-project.github.io/tari.js/docs/wallet/submit-transaction/transaction-builder/)** — Create complex transactions
- **[Production Deployment](https://tari-project.github.io/tari.js/docs/guides/production-deployment)** — Go live with confidence

### 🔧 **Reference**
- **[Complete API Reference](https://tari-project.github.io/tari.js/docs/api-reference)** — Every method documented
- **[Troubleshooting](https://tari-project.github.io/tari.js/docs/troubleshooting)** — Common issues & solutions
- **[Templates & Examples](https://github.com/tari-project/tari.js/tree/main/examples)** — Copy-paste code snippets

---

## 🛠️ Contributing & Development  

**Want to contribute?** We'd love your help! Here's how to get started:

### 🚀 **Quick Development Setup**

```bash
# 1. Clone with required dependencies
git clone https://github.com/tari-project/tari.js
git clone https://github.com/tari-project/tari-ootle ../tari-ootle

# 2. Install toolchain
curl -fsSL https://moonrepo.dev/install/proto.sh | bash
proto use

# 3. Build everything
pnpm install
moon :build

# or target individual packages with 
moon <package>:build
```

### 🧪 **Run the Example App**
```bash
cd packages/tarijs/example
cp .env.example .env    # Configure your wallet endpoints
pnpm run dev           # Start development server
```

### 📦 **Docker Development**
```bash
docker build -t tarijs .
docker create --name tarijs-build tarijs
docker cp tarijs-build:/app/combined_dist/ ./dist
```

### 📖 **Documentation Development**
```bash
moon tari-docs:start   # http://localhost:3000/tari.js/
```

**Need help getting started?** Check our **[Contributing Guide](https://tari-project.github.io/tari.js/docs/contributing)** or ask in [GitHub Discussions](https://github.com/tari-project/tari.js/discussions).

## 🤝 Community & Support

- **📚 [Complete Documentation](https://tari-project.github.io/tari.js/)** — Everything you need to know
- **💬 [GitHub Discussions](https://github.com/tari-project/tari.js/discussions)** — Ask questions, share ideas  
- **🐛 [Issue Tracker](https://github.com/tari-project/tari.js/issues)** — Report bugs, request features
- **🔧 [Troubleshooting Guide](https://tari-project.github.io/tari.js/troubleshooting)** — Common issues & solutions
- **🎮 [Example Apps](https://github.com/tari-project/tari.js/tree/main/examples)** — See tari.js in action

## 📄 License

This project is licensed under the **BSD 3-Clause License** — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**Built with ❤️ by the [Tari Project](https://tari.com)**

[Website](https://tari.com) • [Blog](https://blog.tari.com) • [Twitter](https://twitter.com/tari) • [Discord](https://discord.gg/tari)

</div>
