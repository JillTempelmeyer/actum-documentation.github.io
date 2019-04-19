# Customers and Accounts

The following details explain how to submit debit and credit transactions to Actum.

### Merchant Object [consider renaming if integration partners would use this in a different way than a merchant]

The `merchant` object represents account information pertaining to the merchant that submits a debit or credit transaction.

| Parameter | Description | Type | Req |
|---|---|---|---|
| `parent_id` | Parent ID of merchant [max Length = 8 ] | ALPHANUMERIC | Y |
| `sub_id` | Sub ID of merchant [max Length = 8 ] | ALPHANUMERIC | Y |
| `pmt_type` | Type of payment being submitted (check, or `chk`, is the default value) | -- | Y |
| `response_location` | The `man_trans.cgi` script will respond to this URL with response variables if passed in | Full path URL | N |

### Accounts

The `Accounts` object represents the bank account information associated with the customer being debited or credited.

| Parameter | Description | Type | Req |
|---|---|---|---|
| `companyname` | The name of business being debited or credited (used when a business account is being debited/credited) [max Length = 64] | ALPHANUMERIC | Y |
| `chk_acct` | The customer's checking account number. [max Length = 32] | ALPHANUMERIC | Y |
| `chk_aba` | The customer's ABA, or routing number. This field supports US. routing numbers. [max Length = 9]| ALPHANUMERIC | Y |
| `acct_type` | Use one of the following values for this parameter: `C` for checking, or `S` for savings. This field is required for savings accounts only. | -- | O |
| `chk_fract` | Fractional number (Example: `123-45/1234`..... [max length = 16] | ALPHANUMERIC | N |
| `chk_number` | The customer's check number | NUMBER | N |
| `currency` | The type of currency being processed. Only U.S. currency is supported. The default value will reflect `US` if nothing is passed in. | -- | N |

### Consumers

The `Consumers` object represents the Know Your Customer (KYC) details associated with the customer, or consumer being debited or credited.

| Parameter | Description | Type | Req |
|---|---|---|---|
| `custmane` | The consumer's full name (first and last names) [max length = 64] | ALPHANUMERIC | Y |
| `custemail` | The consumer's email address. 
