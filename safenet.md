# Safenet

Instant cross-chain transactions without bridging delays while maintaining self-custody.

## Core Features

- Execute transactions on any chain instantly
- Single unified balance across chains
- Self-custody with security guarantees
- Economic incentives ensure safe settlement

## How It Works

```mermaid
flowchart TD
    A[User Funds] -->|1. Lock| B[Original Chain]
    B -->|2. Secure| C[Safenet Network]
    D[Liquidity Provider] -->|3. Front Funds| E[Target Chain]
    C -->|4. Execute| E
    E -->|5. Settle| B

    style A fill:#e6f3ff
    style B fill:#fff2e6
    style C fill:#e6ffe6
    style D fill:#ffe6e6
    style E fill:#e6e6ff
```

## Key Components

1. **Network Actors**
   - Processors: Execute transactions
   - Validators: Monitor security
   - Liquidity Providers: Enable instant spending

2. **Security**
   - Optimistic validity proofs
   - Challenge system for invalid transactions
   - Economic incentives for honest behavior

## Current Status
- Documentation: [Safe Docs](https://docs.safe.global)
