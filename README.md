# Stacks Token Streaming Protocol

A decentralized token streaming protocol built on Stacks using Clarity smart contracts. This project enables continuous, time-based token transfers similar to Superfluid, allowing for streaming payments, subscriptions, and automated token distribution.

## 🚀 Features

- **Continuous Token Streaming**: Stream tokens over time with configurable payment rates
- **Block-based Payments**: Payments are calculated per block for precise timing
- **Flexible Timeframes**: Set custom start and stop blocks for stream duration
- **Refuel Capability**: Add more tokens to active streams
- **Secure Withdrawals**: Recipients can withdraw earned tokens at any time
- **Refund Mechanism**: Senders can recover unused tokens after stream completion
- **Signature-based Updates**: Update stream parameters with cryptographic signatures
- **Real-time Balance Tracking**: Check balances for any party involved in streams

## 📋 Prerequisites

- [Clarinet](https://docs.hiro.so/clarinet/) - Stacks development environment
- [Node.js](https://nodejs.org/) (v16 or higher)
- [npm](https://www.npmjs.com/) or [yarn](https://yarnpkg.com/)

## 🛠️ Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd stacks-token-streaming
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Start the local development environment**
   ```bash
   clarinet start
   ```

## 🧪 Testing

Run the test suite to verify the smart contract functionality:

```bash
# Run all tests
npm test

# Run tests with coverage report
npm run test:report

# Watch mode for development
npm run test:watch
```

## 📖 Usage

### Creating a Stream

To create a new token stream, call the `stream-to` function:

```typescript
const result = simnet.callPublicFn(
  "stream",
  "stream-to",
  [
    Cl.principal(recipient),           // Recipient address
    Cl.uint(1000),                     // Initial balance (in microSTX)
    Cl.tuple({                         // Timeframe
      "start-block": Cl.uint(100),
      "stop-block": Cl.uint(200)
    }),
    Cl.uint(10)                        // Payment per block
  ],
  sender
);
```

### Refueling a Stream

Add more tokens to an existing stream:

```typescript
const result = simnet.callPublicFn(
  "stream",
  "refuel",
  [
    Cl.uint(0),                        // Stream ID
    Cl.uint(500)                       // Amount to add
  ],
  sender
);
```

### Withdrawing Tokens

Recipients can withdraw their earned tokens:

```typescript
const result = simnet.callPublicFn(
  "stream",
  "withdraw",
  [Cl.uint(0)],                        // Stream ID
  recipient
);
```

### Checking Balances

Query the current balance for any party:

```typescript
const balance = simnet.callReadOnlyFn(
  "stream",
  "balance-of",
  [
    Cl.uint(0),                        // Stream ID
    Cl.principal(recipient)            // Address to check
  ],
  sender
);
```

### Updating Stream Parameters

Update payment rate and timeframe with cryptographic signatures:

```typescript
// Create signature for the update
const hash = simnet.callReadOnlyFn(
  "stream",
  "hash-stream",
  [
    Cl.uint(0),                        // Stream ID
    Cl.uint(15),                       // New payment per block
    Cl.tuple({                         // New timeframe
      "start-block": Cl.uint(100),
      "stop-block": Cl.uint(250)
    })
  ],
  sender
);

const signature = signMessageHashRsv(hash.value, senderPrivateKey);

const result = simnet.callPublicFn(
  "stream",
  "update-details",
  [
    Cl.uint(0),                        // Stream ID
    Cl.uint(15),                       // New payment per block
    Cl.tuple({                         // New timeframe
      "start-block": Cl.uint(100),
      "stop-block": Cl.uint(250)
    }),
    Cl.principal(recipient),           // Signer
    Cl.buffer(signature)               // Signature
  ],
  sender
);
```

## 🏗️ Smart Contract Functions
contract identifier :STGK3TPN8WBGTM4G04798T306DM4X3JYHPS3H8KA.stream
contract screenshot :

### Public Functions

- `stream-to`: Create a new token stream
- `refuel`: Add more tokens to an existing stream
- `withdraw`: Withdraw earned tokens (recipients only)
- `refund`: Recover unused tokens after stream completion (senders only)
- `update-details`: Update stream parameters with signatures

### Read-Only Functions

- `balance-of`: Check current balance for any party
- `calculate-block-delta`: Calculate active blocks for a stream
- `hash-stream`: Generate hash for signature verification
- `validate-signature`: Verify cryptographic signatures

## 🔧 Configuration

The project uses Clarinet for development and testing. Key configuration files:

- `Clarinet.toml`: Project configuration and contract settings
- `settings/Devnet.toml`: Development network settings
- `deployments/default.simnet-plan.yaml`: Deployment configuration

## 🚀 Deployment

### Local Development
```bash
clarinet start
```

### Testnet Deployment
```bash
clarinet deploy --network testnet
```

### Mainnet Deployment
```bash
clarinet deploy --network mainnet
```

## 📁 Project Structure

```
stacks-token-streaming/
├── contracts/
│   └── stream.clar          # Main smart contract
├── tests/
│   └── stream.test.ts       # Test suite
├── deployments/
│   └── default.simnet-plan.yaml
├── settings/
│   └── Devnet.toml         # Development configuration
├── Clarinet.toml           # Project configuration
├── package.json            # Node.js dependencies
└── README.md              # This file
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the ISC License.

## 🙏 Acknowledgments

- Built as part of the "Introduction to Stacks" course for the "Stacks Developer Degree" on LearnWeb3
- Inspired by [Superfluid](https://www.superfluid.finance/) protocol
- Built with [Clarinet](https://docs.hiro.so/clarinet/) and [Clarity](https://docs.stacks.co/write-smart-contracts/overview)

## 🔗 Links

- [Stacks Documentation](https://docs.stacks.co/)
- [Clarity Language Reference](https://docs.stacks.co/write-smart-contracts/overview)
- [Clarinet Documentation](https://docs.hiro.so/clarinet/)
- [LearnWeb3](https://learnweb3.io/)

## 📞 Support

If you have any questions or need help, please open an issue on GitHub or reach out to the community.
