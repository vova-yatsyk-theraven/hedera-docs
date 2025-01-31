# CryptoTransfer

Transfer cryptocurrency from some accounts to other accounts. The accounts list can contain up to 10 accounts. The amounts list must be the same length as the accounts list. Each negative amount is withdrawn from the corresponding account \(a sender\), and each positive one is added to the corresponding account \(a receiver\). The amounts list must sum to zero. Each amount is a number of tinyBars \(there are 100,000,000 tinyBars in one Hbar\). If any sender account fails to have sufficient hbars to do the withdrawal, then the entire transaction fails, and none of those transfers occur, though the transaction fee is still charged. This transaction must be signed by the keys for all the sending accounts, and for any receiving accounts that have receiverSigRequired == true. The signatures are in the same order as the accounts, skipping those accounts that don't need a signature.

## CryptoTransferTransactionBody

| Field | Type | Description |
| :--- | :--- | :--- |
| transfers | [TransferList](../basic-types/transferlist.md) | Accounts and amounts to transfer |
| tokenTransfers | repeated TokenTransferList | The desired token unit balance adjustments; if any custom fees are assessed, the ledger will try to deduct them from the payer of this CryptoTransfer, resolving the transaction to `INSUFFICIENT_PAYER_BALANCE_FOR_CUSTOM_FEE` if this is not possible |

