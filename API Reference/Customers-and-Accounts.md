# Customers and Accounts

The following details explain how to submit debit and credit transactions to Actum.

### Merchant Object [consider renaming if integration partners would use this in a different way than a merchant]

The `merchant` object represents account information pertaining to the merchant that submits a debit or credit transaction.

| Parameter | Description | Type (& Max Length) | Req |
|---|---|---|---|
| `parent_id` | Parent ID of merchant | ALPHANUMERIC (8) | Y |
| `sub_id` | Sub ID of merchant | ALPHANUMERIC (8) | Y |
| `pmt_type` | Type of payment being submitted (check, or `chk`, is the default value) | -- | Y |
| `response_location` | The `man_trans.cgi` script will respond to this URL with response variables if passed in | Full path URL | N |

### Accounts

The `Accounts` object represents the bank account information associated with the customer being debited or credited.

| Parameter | Description | Type (& Max Length) | Req |
|---|---|---|---|
| `companyname` | The name of business being debited or credited (used when a business account is being debited/credited) | ALPHANUMERIC (64) | Y |
| `chk_acct` | The customer's checking account number | ALPHANUMERIC (32) | Y |
| `chk_aba` | The customer's ABA, or routing number. This field supports US. routing numbers. | ALPHANUMERIC (9) | Y |
| `acct_type` | Use one of the following values for this parameter: `C` for checking, or `S` for savings. This field is required for savings accounts only. | -- | O |
| `chk_fract` | Fractional number (Example: `123-45/1234`..... | ALPHANUMERIC (16) | N |
| `chk_number` | The customer's check number | NUMBER | N |
| `currency` | The type of currency being processed. Only U.S. currency is supported. The default value will reflect `US` if nothing is passed in. | -- | N |


