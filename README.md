# Solana DEX Smart Trading Contract

This is a smart contract project based on the Solana blockchain, enabling automated trading with the decentralized exchanges (DEX) Pump.fun and Raydium. The contract allows users to execute token buy and sell operations on these DEXs and provides smart routing functionality to automatically select the DEX where the token resides for trading. Additionally, the project integrates MEV protection to effectively prevent front-running attacks on transactions.

## Features

- **Multi-DEX Support**: Supports trading on Pump.fun and Raydium
- **Buy Functionality**: Users can purchase specific tokens with SOL
- **Sell Functionality**: Users can sell tokens to obtain SOL
- **Smart Routing**: Automatically selects the DEX where the token resides for trading
- **Slippage Control**: Sets slippage parameters to ensure that transactions are executed within the expected price range
- **MEV Protection**: Prevents front-running attacks with a commit-reveal scheme
- **Batch Trading**: Supports the creation and execution of multiple trade commitments, improving trading efficiency
- **High-Frequency Trading Optimization**: Includes specific performance optimizations for high-frequency trading scenarios

## Technical Architecture

- **Development Framework**: Built using the Anchor framework for Solana smart contract development
- **Programming Languages**: Rust (smart contract), TypeScript (client and tests)
- **Blockchain**: Solana
- **Interaction Protocols**: Utilizes CPI calls to interact with Raydium and Pump.fun protocols

## Installation and Setup

### Prerequisites

- Install [Rust](https://www.rust-lang.org/tools/install)
- Install [Solana CLI](https://docs.solana.com/cli/install-solana-cli-tools)
- Install [Anchor](https://project-serum.github.io/anchor/getting-started/installation.html)
- Install [Node.js](https://nodejs.org/) and [Yarn](https://yarnpkg.com/)

### Install Dependencies

```bash
# Install Rust dependencies
cargo build

# Install Node.js dependencies
yarn install
```

## Building and Testing

### Build Contracts

```bash
anchor build
```

### Run Tests

```bash
anchor test
```

## Deployment

### Deploy to Local Network

```bash
anchor localnet
```

### Deploy to Devnet

```bash
anchor deploy --provider.cluster devnet
```

### Deploy to Mainnet

```bash
anchor deploy --provider.cluster mainnet
```

## Usage Guide

### Initialize DEX Account

```typescript
await program.methods
  .initialize()
  .accounts({
    authority: wallet.publicKey,
    dexAccount: dexAccount,
    systemProgram: SystemProgram.programId,
  })
  .rpc();
```

### Buy Tokens on Pump.fun

```typescript
await program.methods
  .pumpBuy(
    new BN(amountIn), // Amount of SOL
    new BN(minAmountOut) // Minimum amount of tokens to receive
  )
  .accounts({
    // Account parameters
  })
  .rpc();
```

### Buy Tokens on Raydium

```typescript
await program.methods
  .raydiumBuy(
    new BN(amountIn), // Amount of SOL
    new BN(minAmountOut) // Minimum amount of tokens to receive
  )
  .accounts({
    // Account parameters
  })
  .rpc();
```

### Trade Using Smart Routing

```typescript
await program.methods
  .smartTrade(
    tokenMint,
    new BN(amountIn), // Input amount
    new BN(minAmountOut), // Minimum output amount
    isBuy // true for buy, false for sell
  )
  .accounts({
    // Account parameters
  })
  .rpc();
```

### Use MEV Protection

#### Create Trade Commitment

```typescript
const nonce = generateRandomNonce();
const commitmentHash = calculateCommitmentHash(
  tokenMint, 
  amountIn, 
  minAmountOut, 
  isBuy, 
  nonce
);

await program.methods
  .createTradeCommitment(
    commitmentHash,
    new BN(slotDelay)
  )
  .accounts({
    // Account parameters
  })
  .rpc();
```

#### Execute Trade Commitment

```typescript
await program.methods
  .executeCommittedTrade(
    tokenMint,
    new BN(amountIn),
    new BN(minAmountOut),
    isBuy,
    nonce
  )
  .accounts({
    // Account parameters
  })
  .rpc();
```

#### Batch Execute Trade Commitments

```typescript
await program.methods
  .batchExecuteCommitments(
    commitmentIds
  )
  .accounts({
    // Account parameters
  })
  .rpc();
```

## Performance Optimizations

This project includes various optimizations for high-frequency trading scenarios:

- **Reduced Account Loading Time**: Optimized logic for loading pool states
- **Batch Trading Functionality**: Supports executing multiple operations in a single transaction
- **Secure Arithmetic Operations**: Ensures calculations are safe from overflow errors
- **Minimized Storage Operations**: Reduces on-chain storage operations to lower gas costs
- **Precomputed Constants**: Avoids repeated calculations to improve efficiency

## Security Enhancements

- **Re-entrancy Protection**: Implements a re-entrancy lock mechanism
- **Price Impact Check**: Prevents market manipulation and slippage attacks
- **MEV Protection**: Uses a commit-reveal scheme to prevent front-running attacks
- **Detailed Error Handling**: Provides more specific error types and messages

## Considerations

- **Security**: Ensure you understand the risks before trading with the contract.
- **Slippage Settings**: Set slippage parameters appropriately based on market volatility.
- **Trading Rules**: Follow the trading rules of Pump.fun and Raydium.
- **MEV Protection**: When using the commit-reveal scheme, wait for sufficient block confirmations.

