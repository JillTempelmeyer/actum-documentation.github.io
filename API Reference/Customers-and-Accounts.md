# Customers and Accounts

The following details explain how to submit debit and credit transactions to Actum.

### Merchant Object  [consider renaming if integration partners would use this in a different way than a merchant]

The `merchant` object represents account information pertaining to the merchant that submits a debit or credit transaction.

| Parameter | Description | Type | Req |
|---|---|---|---|
| `parent_id` | Parent ID of merchant [max Length = 8 ] | ALPHANUMERIC | Y |
| `sub_id` | Sub ID of merchant [max Length = 8 ] | ALPHANUMERIC | Y |
| `pmt_type` | Type of payment being submitted (check, or `chk`, is the default value) | -- | N |
| `response_location` | The `man_trans.cgi` script will respond to this URL with response variables if passed in | Full path URL | N |

### Account Object

The `Accounts` object represents the bank account information associated with the customer being debited or credited.

| Parameter | Description | Type | Req |
|---|---|---|---|
| `companyname` | The name of business being debited or credited (used when a business account is being debited/credited) [max Length = 64] | ALPHANUMERIC | Y |
| `chk_acct` | The customer's checking account number. [max Length = 32] | ALPHANUMERIC | Y |
| `chk_aba` | The customer's ABA, or routing number. This field supports US. routing numbers. [max Length = 9]| ALPHANUMERIC | Y |
| `acct_type` | Use one of the following values for this parameter: `C` for checking, or `S` for savings. This field is required for savings accounts only. | -- | N |
| `chk_fract` | Fractional number (Example: `123-45/1234`..... [max length = 16] | ALPHANUMERIC | N |
| `chk_number` | The customer's check number | NUMBER | N |
| `currency` | The type of currency being processed. Only U.S. currency is supported. The default value will reflect `US` if nothing is passed in. | -- | N |

### Consumer Object

The `Consumer` object represents the Know Your Customer (KYC) details associated with the customer, or consumer, being debited or credited.

| Parameter | Description | Type | Req |
|---|---|---|---|
| `custmane` | The consumer's full name (first and last names) [max length = 64] | ALPHANUMERIC | Y |
| `custemail` | The consumer's email address. This field may be required, based on merchant settings. [max length = 64] | ALPHANUMERIC | C |
| ` custaddress1` | The first line of a consumer's street address. This field may be required, based on merchant settings. [max length = 64] | ALPHANUMERIC | C |
| `custaddress2` | The second line of a consumer's street address. [max length = 64] | ALPHANUMERIC | N |
| `custcity` | The city in which the consumer resides. This field may be required, based on merchant settings. [max length = 32] | ALPHANUMERIC | C |
| `custstate` | The state in which the consumer resides. This field may be required, based on merchant settings. [max length = 32] | ALPHANUMERIC | C |
| `custzip` | The consumer's zip code. This field may be required, based on merchant settings. [max length = 16] | ALPHANUMERIC | C |
| `custphone` | The consmer's phone number. This field may be required, based on merchant settings. [max length = 16] | ALPHANUMERIC | C
| `shipaddress1` | The first line of a consumer's shipping address. [max length = 64] | ALPHANUMERIC | N |
| `shipaddress2` | The second line of a consumer's shipping address. [max length = 64] | ALPHANUMERIC | N | 
| `shipcity` | The city included in the consumer's shipping address. [max length = 32] | ALPHANUMERIC | N | 
| `shipstate` | The state included in the consumer's shipping address. [max length = 32] | ALPHANUMERIC | N |
| `shipstate` | The zip code included in the consumer's shipping address. [max length = 16] | ALPHANUMERIC | N |
| `custssn` | The social security details of the consumer. This field may be expressed as the last four digits, or the full social security number. This field may be required, based on merchant settings. [max length = 16] | -- | C |
| `birth_date` | The consumer's birth date, expressed in `MMDDYYYY` format. This field may be required, based on merchant settings. | -- | C | 
