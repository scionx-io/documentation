# Scionx Technical Documentation (C4 Model)

## Table of Contents
1. [System Context (Level 1)](#1-system-context)
2. [Container Level (Level 2)](#2-container-level)
3. [Component Level (Level 3)](#3-component-level)
4. [Code Level (Level 4)](#4-code-level)
5. [Supplementary Documentation](#5-supplementary-documentation)

## 1. System Context

### 1.1 Overview
```mermaid
C4Context
    title System Context - Scionx Payment Platform

    Person(merchant, "Merchant", "Business accepting crypto payments")
    Person(customer, "Customer", "End user making payments")
    Person(admin, "Platform Admin", "Scionx administrator")
    
    System(scionx, "Scionx Platform", "Crypto payment processing and settlement system")
    
    System_Ext(blockchain, "Blockchain Networks", "Multiple chains (ETH, etc)")
    System_Ext(dex, "DEX Providers", "Uniswap, etc")
    System_Ext(gnosis, "Gnosis Safe", "Multi-sig wallet system")
    System_Ext(monitoring, "Monitoring", "Platform monitoring")
    
    Rel(merchant, scionx, "Manages payments & settings")
    Rel(customer, scionx, "Makes payments")
    Rel(admin, scionx, "Administers platform")
    
    Rel(scionx, blockchain, "Executes transactions")
    Rel(scionx, dex, "Token swaps")
    Rel(scionx, gnosis, "Wallet management")
    Rel(scionx, monitoring, "System metrics")
```

### 1.2 Key Relationships
- **Merchant-Platform**: Payment management, settings configuration
- **Customer-Platform**: Payment execution, transaction monitoring
- **Platform-Blockchain**: Transaction execution, event monitoring
- **Platform-DEX**: Token swap operations
- **Platform-Gnosis**: Multi-signature wallet operations

## 2. Container Level

### 2.1 Container Overview
```mermaid
C4Container
    title Container View - Scionx Platform

    Container_Boundary(scionx, "Scionx Platform") {
        Container(web_ui, "Web Interface", "React/Vite", "Merchant dashboard")
        Container(mobile_ui, "Mobile Interface", "React Native", "Customer payments")
        Container(api_gateway, "API Gateway", "Rails 8.0", "API orchestration")
        Container(payment_engine, "Payment Engine", "Ruby", "Core payment logic")
        Container(blockchain_service, "Blockchain Service", "Node.js", "Chain interactions")
        ContainerDb(postgres_db, "Database", "PostgreSQL", "Primary data store")
        ContainerDb(redis_cache, "Cache", "Redis", "Caching layer")
        Container(worker_service, "Worker Service", "Sidekiq", "Async processing")
    }

    Rel(web_ui, api_gateway, "HTTPS/JSON")
    Rel(mobile_ui, api_gateway, "HTTPS/JSON")
    Rel(api_gateway, payment_engine, "gRPC")
    Rel(payment_engine, blockchain_service, "Internal API")
    Rel(api_gateway, postgres_db, "SQL")
    Rel(api_gateway, redis_cache, "Redis Protocol")
    Rel(payment_engine, worker_service, "Message Queue")
```

### 2.2 Container Responsibilities
1. **Web Interface**
   - Merchant dashboard
   - Payment management
   - Analytics views

2. **Mobile Interface**
   - Customer payment flow
   - Transaction status
   - Payment history

3. **API Gateway**
   - Request routing
   - Authentication
   - Rate limiting
   - API versioning

4. **Payment Engine**
   - Transaction processing
   - Fee calculation
   - Currency conversion
   - Compliance checks

## 3. Component Level

### 3.1 Payment Engine Components
```mermaid
C4Component
    title Component View - Payment Engine

    Container_Boundary(payment_engine, "Payment Engine") {
        Component(payment_manager, "Payment Manager", "Ruby", "Payment flow orchestration")
        Component(fee_service, "Fee Service", "Ruby", "Fee calculation")
        Component(tx_validator, "Transaction Validator", "Ruby", "Validation logic")
        Component(conversion, "Conversion Service", "Ruby", "Currency conversion")
        Component(risk_engine, "Risk Engine", "Ruby", "Risk assessment")
    }

    ContainerDb(state_store, "State Store", "Redis", "Transaction state")
    
    Rel(payment_manager, fee_service, "Calculates fees")
    Rel(payment_manager, tx_validator, "Validates tx")
    Rel(payment_manager, conversion, "Converts amounts")
    Rel(payment_manager, risk_engine, "Assesses risk")
    Rel(payment_manager, state_store, "Manages state")
```

### 3.2 Blockchain Service Components
```mermaid
C4Component
    title Component View - Blockchain Service

    Container_Boundary(blockchain_service, "Blockchain Service") {
        Component(tx_manager, "Transaction Manager", "Node.js", "Transaction handling")
        Component(event_listener, "Event Listener", "Node.js", "Chain monitoring")
        Component(wallet_manager, "Wallet Manager", "Node.js", "Key management")
        Component(gas_service, "Gas Service", "Node.js", "Gas estimation")
    }

    Rel(tx_manager, event_listener, "Monitors")
    Rel(tx_manager, wallet_manager, "Signs tx")
    Rel(tx_manager, gas_service, "Gets gas price")
```

## 4. Code Level

### 4.1 Smart Contract Architecture
```solidity
// Key interfaces and structures
interface IPaymentProcessor {
    struct PaymentIntent {
        address sender;
        address recipient;
        uint256 amount;
        uint256 fee;
        bytes32 id;
    }
    
    function processPayment(PaymentIntent calldata intent) external;
    function refundPayment(bytes32 paymentId) external;
}
```

### 4.2 API Endpoints Structure
```ruby
# Payment Engine API
module API::V1
  class Payments < Grape::API
    resource :payments do
      post do
        # Payment creation logic
      end
      
      get ':id' do
        # Payment retrieval logic
      end
    end
  end
end
```

## 5. Supplementary Documentation

### 5.1 Data Flows
```mermaid
sequenceDiagram
    participant Customer
    participant API
    participant PaymentEngine
    participant Blockchain
    
    Customer->>API: Initiate Payment
    API->>PaymentEngine: Create Intent
    PaymentEngine->>Blockchain: Submit Transaction
    Blockchain-->>PaymentEngine: Confirm
    PaymentEngine-->>API: Update Status
    API-->>Customer: Complete
```

### 5.2 Deployment Architecture
```mermaid
C4Deployment
    title Deployment View - Production Environment

    Deployment_Node(aws, "AWS Cloud") {
        Deployment_Node(eks, "EKS Cluster") {
            Container(api_pods, "API Pods", "Rails, Node.js")
            Container(worker_pods, "Worker Pods", "Sidekiq")
        }
        Deployment_Node(rds, "RDS") {
            ContainerDb(db, "PostgreSQL", "Main Database")
        }
        Deployment_Node(elasticache, "ElastiCache") {
            ContainerDb(cache, "Redis", "Cache Layer")
        }
    }
```

### 5.3 Security Architecture
- Authentication flows
- Authorization matrix
- Encryption standards
- Key management
- Audit logging

### 5.4 Monitoring & Operations
- Health checks
- Metrics collection
- Alert thresholds
- Backup procedures
- Disaster recovery
