# Payment Flow Documentation

## State Diagram
```mermaid
stateDiagram-v2
    [*] --> PaymentInitiated
    PaymentInitiated --> WaitingForPayment: Display QR/Link
    WaitingForPayment --> ProcessingPayment: Customer Pays
    ProcessingPayment --> TokenSwap: If different token
    ProcessingPayment --> AEDeTransfer: If AEDe
    TokenSwap --> AEDeTransfer: After swap
    AEDeTransfer --> SettlementPending: Transfer to merchant
    SettlementPending --> PaymentComplete: Success
    SettlementPending --> PaymentFailed: Error
```

## State Descriptions

### Initial States
- **PaymentInitiated**: Merchant creates checkout request
- **WaitingForPayment**: System displays payment QR/link to customer

### Processing States
- **ProcessingPayment**: Validates incoming payment
- **TokenSwap**: Converts other tokens to AEDe if needed
- **AEDeTransfer**: Handles AEDe token transfer
- **SettlementPending**: Finalizes merchant settlement

### Final States
- **PaymentComplete**: Transaction successful
- **PaymentFailed**: Transaction failed, requires investigation

## State Transitions
1. Create checkout -> Generate payment link
2. Customer payment -> Process and validate
3. Token conversion -> Handle if needed
4. Settlement -> Transfer to merchant
5. Confirmation -> Update final status
