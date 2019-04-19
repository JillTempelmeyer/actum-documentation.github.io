# Submitting a Transaction

The following tables outline the required and optional parfields to submit a debit/credit to the Actum Processing system.

It may be helpful to familiarize yourself with the following documents:

* [ACH Transaction Types](https://github.com/JillTempelmeyer/jilltempelmeyer.github.io/blob/master/ACH%20transaction%20Types.md)
* [ACH Transaction Lifestyle](https://github.com/JillTempelmeyer/jilltempelmeyer.github.io/blob/master/API%20Reference/ACH%20transaction%20Lifestyle.md)

### Account Object

The `Accounts` object represents the bank account information associated with the customer being debited or credited.

| Parameter | Description | Type | Req |
|---|---|---|---|
| `companyname` | The name of business being debited or credited (used when a business account is being debited/credited). [max Length = 64] | ALPHANUMERIC | Y |
| `chk_acct` | The customer's checking account number. [max Length = 32] | ALPHANUMERIC | Y |
| `chk_aba` | The customer's ABA, or routing number. This field supports US. routing numbers. [max Length = 9]| ALPHANUMERIC | Y |
| `acct_type` | Use one of the following values for this parameter: `C` for checking, or `S` for savings. This field is required for savings accounts only. | -- | N |
| `chk_fract` | Fractional number (Example: `123-45/1234`). [max length = 16] | ALPHANUMERIC | N |
| `chk_number` | The customer's check number. | NUMBER | N |
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


### Billing Information

Information about setting up recurring payments (and can expand on how to manage these here, also)

| Parameter | Description | Type | Req |
|---|---|---|---|
| `initial_amount` | The initial amount of a bill payment. The field should be specified in XX.XX format, such as `49.95` for the amount. | -- | Y |
| `recur_amount` | The recurring Amount not required for one-time billing. Default to `initial_amount` if no value is defined. | -- | N |
| `billing_cycle` | A numerical value that corresponds with the frequency with which customer payments will be made. Input values include the following: One-Time Billing = -1, Weekly = 1, Monthly = 2, Bi-Monthly = 3, Quarterly = 4, Semi-Annually = 5, Annually = 6, Bi-Weekly = 7, Business-Daily = 8 | NUMBER | Y |
| `days_til_recur` | The number of days remaining until `recur_amount` is billed | NUMBER | N |
| `max_num_billing` | Maximum number of times consumer will be billed.  Default to “-1” (perpetual billing) if no value is defined. | NUMBER | N |
