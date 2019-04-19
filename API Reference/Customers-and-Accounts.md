# Customers and Accounts

The following details explain how to submit debit and credit transactions to Actum.

## Merchant Object [consider renaming]

The `merchant` object represents account information pertaining to the merchant that submits a debit or credit transaction.

| Parameter | Description | Type (& Max Length) | Req |
|---|---|---|---|
| `parent_id` | Parent ID of merchant | ALPHANUMERIC (8) | Y |
| `sub_id` | Sub ID of merchant | ALPHANUMERIC (8) | Y |
| `pmt_type` | Type of payment being submitted (check, or `chk`, is the default value) | -- | Y |
