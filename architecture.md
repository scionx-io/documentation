# System Architecture

```mermaid
graph TB
    subgraph Background["Background Processing"]
        Triggers["Triggers"]
        BgJobs["Background Jobs"]
        SolidQueue["Solid Queue"]
        WebhookSystem["Webhook System"]
    end

    subgraph ClientInterfaces["Client Interfaces"]
        Dashboard["Dashboard App<br>(app.scionx.io)"]
        POS["Point of Sale<br>(pos.scionx.io)"]
        API["API<br>(api.scionx.io)"]
        PaymentInterface["Payment Interface<br>(pay.scionx.io)"]
    end

    subgraph CoreProcessing["Core Processing"]
        Checkout["Checkout System"]
        ChargeProcessing["Charge Processing"]
        TransactionHandler["Transaction Handler"]
    end

    subgraph BlockchainIntegration["Blockchain Integration"]
        TransactionSync["Transaction Sync"]
        SmartContract["Smart Contract"]
        WalletBalance["Wallet Balance"]
    end

    subgraph DataStorage["Data Storage"]
        Database[(Database)]
        SolidCache[(Solid Cache)]
    end

    %% Client Interface Connections
    Dashboard -- "Creates" --> Checkout
    POS -- "Initiates" --> Checkout
    API -- "REST API" --> CoreProcessing
    PaymentInterface -- "Processes" --> ChargeProcessing

    %% Background Processing Connections
    Triggers --> BgJobs
    BgJobs -- "Notifies" --> WebhookSystem
    WebhookSystem -- "Notifies" --> ClientInterfaces
    BgJobs --> SolidQueue
    SolidQueue -- "Queues" --> ClientInterfaces
    SolidQueue -- "Executes" --> BlockchainIntegration

    %% Core Processing Connections
    Checkout -- "Stores" --> Database
    ChargeProcessing -- "Creates" --> TransactionHandler
    TransactionHandler -- "Stores" --> Database
    TransactionHandler -- "Initiates" --> TransactionSync

    %% Blockchain Integration Connections
    TransactionSync -- "Updates" --> SmartContract
    TransactionSync -- "Updates" --> WalletBalance
    SmartContract -- "Status" --> TransactionSync
    WalletBalance -- "Checks" --> TransactionSync
    WalletBalance -- "Caches" --> SolidCache
```

## System Components

### Client Interfaces
- Dashboard App (app.scionx.io)
- Point of Sale (pos.scionx.io)
- API (api.scionx.io)
- Payment Interface (pay.scionx.io)

### Core Processing
- Checkout System
- Charge Processing
- Transaction Handler

### Background Processing
- Triggers
- Background Jobs
- Solid Queue
- Webhook System

### Blockchain Integration
- Transaction Sync
- Smart Contract
- Wallet Balance

### Data Storage
- Database
- Solid Cache
