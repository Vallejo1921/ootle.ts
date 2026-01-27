---
sidebar_position: 4
title: MetaMask Integration
description: Connect to MetaMask with the Tari snap for seamless browser-based wallet integration
---

# MetaMask Signer Integration 🦊

> **Browser-native wallet experience** — Connect your web apps to MetaMask using the Tari snap for familiar, secure wallet interactions.

The MetaMask signer enables your dApps to integrate with MetaMask through the official **Tari snap**, providing users with a familiar wallet experience while accessing Tari's advanced privacy features.

## ✨ **Why Choose MetaMask?**

- **🔒 Familiar Security** — Users trust MetaMask's proven security model
- **🌐 Browser Native** — No additional software installation required
- **📱 Mobile Ready** — Works on MetaMask mobile app
- **🔗 Easy Integration** — Simple API for web developers
- **🛡️ Privacy Enabled** — Full access to Tari's confidential transactions

## 🚀 **Quick Start**

### **Prerequisites**

Before using the MetaMask signer, ensure:

- **MetaMask Flask** installed (developer version required for snaps)
- **Tari snap** installed in MetaMask
- **Modern browser** with MetaMask extension support

:::tip Install MetaMask Flask
Regular MetaMask doesn't support snaps yet. Download [MetaMask Flask](https://metamask.io/flask/) for snap functionality.
:::

### **Installation**

```bash npm2yarn
npm install @tari-project/tarijs @tari-project/metamask-signer
```

### **Basic Connection**

```typescript
import { MetaMaskSigner } from '@tari-project/metamask-signer';

// Check if MetaMask is available
if (typeof window.ethereum === 'undefined') {
  throw new Error('MetaMask not installed. Please install MetaMask Flask.');
}

// Create signer instance
const signer = new MetaMaskSigner();

// Connect to MetaMask (will prompt for snap installation)
await signer.connect();

// Verify connection
if (signer.isConnected()) {
  console.log('✅ Connected to MetaMask with Tari snap!');
}
```

## 🔧 **Configuration Options**

### **Advanced Configuration**

```typescript
import { MetaMaskSigner } from '@tari-project/metamask-signer';

const signer = new MetaMaskSigner({
  // Snap configuration
  snapId: 'npm:@tari-project/wallet-snap', // Default snap ID
  snapVersion: '^0.1.0', // Snap version to install
  
  // Connection options
  autoInstallSnap: true, // Auto-install snap if not present
  timeout: 30000, // Connection timeout (30 seconds)
  
  // Network configuration
  network: 'testnet', // 'mainnet' | 'testnet' | 'localnet'
  
  // UI preferences
  showInstallPrompt: true, // Show snap installation UI
  theme: 'auto' // 'light' | 'dark' | 'auto'
});
```

## 🔌 **Connection Management**

### **Automatic Snap Installation**

```typescript
async function connectWithSnapInstall() {
  try {
    const signer = new MetaMaskSigner({
      autoInstallSnap: true,
      showInstallPrompt: true
    });
    
    // This will automatically install the snap if needed
    await signer.connect();
    
    console.log('Snap installed and connected!');
  } catch (error) {
    if (error.code === 'SNAP_INSTALLATION_REJECTED') {
      console.log('User rejected snap installation');
    } else {
      console.error('Connection failed:', error);
    }
  }
}
```

### **Manual Snap Installation**

```typescript
async function manualSnapInstall() {
  // Request snap installation explicitly
  const snapId = 'npm:@tari-project/wallet-snap';
  
  await window.ethereum.request({
    method: 'wallet_requestSnaps',
    params: {
      [snapId]: {}
    }
  });
  
  // Now connect with the signer
  const signer = new MetaMaskSigner();
  await signer.connect();
}
```

### **Connection Status Checking**

```typescript
// Check if MetaMask is available
function isMetaMaskAvailable(): boolean {
  return typeof window.ethereum !== 'undefined';
}

// Check if Tari snap is installed
async function isSnapInstalled(): Promise<boolean> {
  try {
    const snaps = await window.ethereum.request({
      method: 'wallet_getSnaps'
    });
    
    return 'npm:@tari-project/wallet-snap' in snaps;
  } catch {
    return false;
  }
}

// Full connection check
async function checkConnection() {
  if (!isMetaMaskAvailable()) {
    throw new Error('MetaMask not available');
  }
  
  if (!(await isSnapInstalled())) {
    throw new Error('Tari snap not installed');
  }
  
  const signer = new MetaMaskSigner();
  return signer.isConnected();
}
```

## 💼 **Account Management**

### **Getting Accounts**

```typescript
// Get all available accounts
const accounts = await signer.getAccounts();
console.log('Available accounts:', accounts);

// Get default account
const defaultAccount = await signer.getDefaultAccount();
console.log('Default account:', defaultAccount.address);

// Get account balance
const balance = await signer.getBalance(defaultAccount);
console.log('Balance:', balance, 'Tari');
```

### **Account Information**

```typescript
// Get detailed account info
const accountInfo = await signer.getAccountInfo(defaultAccount);

console.log('Account Details:', {
  address: accountInfo.address,
  publicKey: accountInfo.publicKey,
  balance: accountInfo.balance,
  network: accountInfo.network
});
```

## 💸 **Transaction Operations**

### **Simple Transfer**

```typescript
import { TransactionBuilder } from '@tari-project/tarijs';

async function sendTransfer() {
  const defaultAccount = await signer.getDefaultAccount();
  
  // Build transaction
  const transaction = new TransactionBuilder()
    .fee(100)
    .callMethod(defaultAccount.address, 'transfer', {
      amount: 1000,
      destination: 'account_recipient_address_here'
    })
    .build();
  
  // Submit transaction (will prompt MetaMask for approval)
  const result = await signer.submitTransaction({ transaction });
  
  console.log('Transaction submitted:', result.transactionId);
  return result;
}
```

### **Transaction with User Confirmation**

```typescript
async function transferWithConfirmation() {
  try {
    // Show transaction preview to user
    const transactionPreview = {
      type: 'Transfer',
      amount: '1000 Tari',
      recipient: 'account_recipient...',
      fee: '100 Tari'
    };
    
    // User confirms in your UI
    const userConfirmed = await showTransactionModal(transactionPreview);
    if (!userConfirmed) return;
    
    // Build and submit transaction
    const transaction = new TransactionBuilder()
      .fee(100)
      .callMethod(account.address, 'transfer', {
        amount: 1000,
        destination: recipient
      })
      .build();
    
    // MetaMask will show its own confirmation
    const result = await signer.submitTransaction({ transaction });
    
    // Show success message
    showSuccessMessage(`Transaction sent: ${result.transactionId}`);
  } catch (error) {
    if (error.code === 'USER_REJECTED') {
      console.log('User rejected transaction');
    } else {
      console.error('Transaction failed:', error);
    }
  }
}
```

## 🔒 **Permission Management**

### **Request Permissions**

```typescript
// Request specific permissions
const permissions = await signer.requestPermissions([
  'accounts:read',
  'transactions:submit', 
  'substates:read'
]);

console.log('Granted permissions:', permissions);
```

### **Check Permissions**

```typescript
// Check current permissions
const currentPermissions = await signer.getPermissions();

if (currentPermissions.includes('transactions:submit')) {
  console.log('Can submit transactions');
}
```

## 🎨 **User Experience**

### **React Integration**

```tsx
import React, { useState, useEffect } from 'react';
import { MetaMaskSigner } from '@tari-project/metamask-signer';

export function MetaMaskConnector() {
  const [signer, setSigner] = useState<MetaMaskSigner | null>(null);
  const [isConnected, setIsConnected] = useState(false);
  const [isConnecting, setIsConnecting] = useState(false);

  const connectWallet = async () => {
    setIsConnecting(true);
    
    try {
      if (typeof window.ethereum === 'undefined') {
        alert('Please install MetaMask Flask to continue');
        window.open('https://metamask.io/flask/', '_blank');
        return;
      }

      const metaMaskSigner = new MetaMaskSigner({
        autoInstallSnap: true,
        showInstallPrompt: true
      });
      
      await metaMaskSigner.connect();
      
      setSigner(metaMaskSigner);
      setIsConnected(true);
    } catch (error) {
      console.error('Failed to connect:', error);
      alert('Failed to connect to MetaMask. Please try again.');
    } finally {
      setIsConnecting(false);
    }
  };

  const disconnect = async () => {
    if (signer) {
      await signer.disconnect();
      setSigner(null);
      setIsConnected(false);
    }
  };

  if (isConnected && signer) {
    return (
      <div className="wallet-connected">
        <p>✅ Connected to MetaMask</p>
        <button onClick={disconnect}>Disconnect</button>
      </div>
    );
  }

  return (
    <div className="wallet-connector">
      <h3>Connect Your Wallet</h3>
      <button 
        onClick={connectWallet} 
        disabled={isConnecting}
        className="metamask-connect-btn"
      >
        {isConnecting ? '🔄 Connecting...' : '🦊 Connect MetaMask'}
      </button>
      
      {typeof window.ethereum === 'undefined' && (
        <p className="install-prompt">
          <a href="https://metamask.io/flask/" target="_blank" rel="noopener">
            Install MetaMask Flask →
          </a>
        </p>
      )}
    </div>
  );
}
```

### **Error Handling**

```typescript
async function robustMetaMaskConnection() {
  try {
    await signer.connect();
  } catch (error) {
    switch (error.code) {
      case 'METAMASK_NOT_INSTALLED':
        showError('Please install MetaMask Flask', {
          action: 'Install',
          url: 'https://metamask.io/flask/'
        });
        break;
        
      case 'SNAP_INSTALLATION_REJECTED':
        showError('Tari snap installation was rejected. Please try again.');
        break;
        
      case 'USER_REJECTED':
        showError('Connection was rejected. Please accept to continue.');
        break;
        
      case 'SNAP_NOT_FOUND':
        showError('Tari snap not found. Please install it first.');
        break;
        
      default:
        showError(`Connection failed: ${error.message}`);
    }
  }
}
```

## 🔧 **Troubleshooting**

### **Common Issues**

#### ❌ **"Snap not found" error**
```typescript
// Solution: Ensure snap is installed
await window.ethereum.request({
  method: 'wallet_requestSnaps',
  params: {
    'npm:@tari-project/wallet-snap': {}
  }
});
```

#### ❌ **"MetaMask not detected"**
```typescript
// Solution: Check installation and provide guidance
if (typeof window.ethereum === 'undefined') {
  console.error('MetaMask not installed');
  // Guide user to install MetaMask Flask
}
```

#### ❌ **Connection timeout**
```typescript
// Solution: Increase timeout and add retry logic
const signer = new MetaMaskSigner({
  timeout: 60000, // 60 seconds
  retryAttempts: 3
});
```

### **Debug Mode**

```typescript
// Enable debug logging
const signer = new MetaMaskSigner({
  debug: true // Enables detailed logging
});

// Check snap status
const snapStatus = await signer.getSnapStatus();
console.log('Snap status:', snapStatus);
```

## 🚀 **Advanced Features**

### **Batch Transactions**

```typescript
// Submit multiple transactions as a batch
const batch = new TransactionBuilder()
  .fee(200) // Higher fee for batch
  .callMethod(account, 'transfer', { amount: 500, destination: addr1 })
  .callMethod(account, 'transfer', { amount: 300, destination: addr2 })
  .build();

const result = await signer.submitTransaction({ transaction: batch });
```

### **Confidential Transactions**

```typescript
// Create privacy-preserving transaction
const confidentialTx = new TransactionBuilder()
  .fee(150)
  .confidentialTransfer({
    amount: 1000,
    destination: recipientAddress,
    blindingFactor: generateBlindingFactor()
  })
  .build();

const result = await signer.submitTransaction({ 
  transaction: confidentialTx,
  isConfidential: true 
});
```

## 📚 **Best Practices**

### **Security**
- ✅ Always validate user input before transactions
- ✅ Use HTTPS in production environments
- ✅ Implement proper error boundaries
- ✅ Never store private keys in your application
- ✅ Validate snap authenticity before installation

### **User Experience**
- ✅ Show clear installation instructions
- ✅ Provide feedback during connection process
- ✅ Handle all error states gracefully
- ✅ Offer alternative wallet options
- ✅ Cache connection state appropriately

### **Performance**
- ✅ Only connect when needed
- ✅ Reuse signer instances
- ✅ Implement connection pooling
- ✅ Handle network switching
- ✅ Optimize transaction batching

## 🔗 **Related Documentation**

- **[Installation Guide](../installation)** — Set up your development environment
- **[Transaction Builder](../wallet/submit-transaction/transaction-builder/)** — Create complex transactions
- **[Provider vs Signer](../provider-vs-signer)** — Understand the architecture
- **[Troubleshooting](../troubleshooting)** — Common issues and solutions

## 💬 **Need Help?**

- **🐛 [Report issues](https://github.com/tari-project/tari.js/issues)** — Bug reports and feature requests
- **💬 [Join Discord](https://discord.gg/tari)** — Real-time community support  
- **📖 [API Reference](../api-reference)** — Complete method documentation
- **🦊 [MetaMask Docs](https://docs.metamask.io/snaps/)** — Official MetaMask snap documentation

---

**Ready to integrate MetaMask?** Start with our [Getting Started Tutorial](../guides/getting-started-tutorial) and add MetaMask support to your Tari application! 🚀
