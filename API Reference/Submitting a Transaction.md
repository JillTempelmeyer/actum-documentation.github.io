# Submitting a Transaction

The following information outlines the required and optional parfields for submitring a debit or credit transaction to Actum.

It may be helpful to familiarize yourself with the following resources, prior to submitting a transction.

* [ACH Transaction Types](https://github.com/JillTempelmeyer/jilltempelmeyer.github.io/blob/master/API%20Reference/ACH%20transaction%20Types.md)
* [ACH Transaction Lifestyle](https://github.com/JillTempelmeyer/jilltempelmeyer.github.io/blob/master/API%20Reference/ACH%20transaction%20Lifestyle.md)

### Account Object

The `Accounts` object represents the bank account information associated with the customer being debited or credited.

| Parameter | Description | Type | Req |
|---|---|---|---|
| `companyname` | The name of business being debited or credited (used when a business account is being debited/credited). [max Length = 64] | ALPHANUMERIC | Y |
| `chk_acct` | The customer's checking account number. [max length = 32] | ALPHANUMERIC | Y |
| `chk_aba` | The customer's ABA, or routing number. This field supports US. routing numbers. [max length = 9]| ALPHANUMERIC | Y |
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

### Miscellaneous Information

... can we call this an object? Can we integrate this info. into the other sections?

| Parameter | Description | Type | Req |
|---|---|---|---|
| `ip_forward` | The IP address of the client connecting to your server. Max length = 32 | VARCHAR2 | Y |
| `futureinitial` | This field is required for submitting a transaction to originate on a future date. | DATE MM/DD/YYYY | N |
| `merordernumber` | The merchant defined order number.  This field will be saved to the database Max length = 512 | VARCHAR2 | N |
| `action_code` | P = Process (default) | -- | N |
| `creditflag` | Set `creditflag=1` to issue a credit.  **Note:** This is not for refunds, but rather to issue a credit for a transaction that was not initially debited. | NUMBER | N |
| `trans_modifier` | S = Same-day transaction, N = Pre-note transaction, Y = NSF retry | N |
| `retry_fee_amt` | The return fee you'll want to bill on an NSF Retry.  Must be separate than the `initial_amount`. | XX.XX ex. 10.00 | N |

# Payment Retries and Refunds

### Payment Retries

| Parameter | Description | Type | Req |
|---|---|---|---|
| `trans_modifier` | S = Same-day transaction, N = Pre-note transaction, Y = NSF retry | N |
| `retry_fee_amt` | The return fee you'll want to bill on an NSF Retry.  Must be separate than the `initial_amount`. | XX.XX ex. 10.00 | N |

### Payment Refunds

Refunds can only be submitted against the transaction status of `Check Settlement`.

| Parameter | Description | Type | Req |
|---|---|---|---|
| `username` | The merchant's Actum portal username. Max length = 16 | ALPHANUMERIC | Y |
| `password` | The merchant's Actum portal password. Max length = 16 | ALPHANUMERIC | Y |
| `syspass` | The merchant's system password assigned by Actum. Max length = 16 | ALPHANUMERIC | Y |
| `action_code` | You will want to enter `action_code=R` to tell the script we are refunding the transaction. Max length = 16 | ALPHANUMERIC | R |
| `prev_history_id` | The `History_ID` of the transaction you want to refund. | NUMBER | If `order_id` is not provided |
| `order_id` |The order ID of the transaction you want to refund. Max length = 32 | ALPHANUMERIC | Y |
| `initial_amount` | The initial amount of the bill.  Partial refunds are not accepted. | XX.XX ex. 49.95 | Y |
 
# Revoking Transactions

Revoking a transaction will prevent the transaction from being originated to the bank for processing. Standard debits and credits can be revoked until 4pm CT the same day of submission.  Same Day Debits / Credits can be revoked until 11am CT.  If you have the ability to originate transactions using late-night processing, you will have until 9pm CT to revoke.

The response may contain the following parameters:
*	`status=success`
*	`status=Error` 
* `error=Order Number Not Found` (transaction has already originated)

(note that a lot of info. in this section is repetitive)

| Parameter | Description | Type | Req |
|---|---|---|---|
| `username` | The merchant's Actum portal username. Max length = 16 | ALPHANUMERIC | Y |
| `password` | The merchant's Actum portal password. Max length = 16 | ALPHANUMERIC | Y |
| `syspass` | The merchant's system password assigned by Actum. Max length = 16 | ALPHANUMERIC | Y |
| `action_code` | You will want to enter `action_code=K` to tell the script we are revoking the transaction. Max length = 16 | ALPHANUMERIC | Y |
| `prev_history_id` | The `History_ID` of the transaction you want to revoke. | NUMBER | If `order_id` is not provided |
| `order_id` |The order ID of the transaction you want to revoke. Max length = 32 | ALPHANUMERIC | Y |


# Recurring Transactions

### Canceling Recurring Transactions

In order to cancel recurring transactions, the following parameters are required.  Please note that “canceling” is not the same as “revoking” a transaction.

The response may contain the following parameters:

*	`status=success lastdateactive=01/01/2019` (if order is active)
* `status=Error error=Order Inactive!` (if order is already canceled)

When canceling recurring transactions, please note the transaction that is scheduled to originate the same day as the cancel request will still originate to the bank.


| Parameter | Description | Type | Req |
|---|---|---|---|
| `username` | The merchant's Actum portal username. Max length = 16 | ALPHANUMERIC | Y |
| `password` | The merchant's Actum portal password. Max length = 16 | ALPHANUMERIC | Y |
| `syspass` | The merchant's system password assigned by Actum. Max length = 16 | ALPHANUMERIC | Y |
| `action_code` | You will want to enter `action_code=C` to tell the script that we are canceling further recurring transactions. Max length = 16 | ALPHANUMERIC | Y |
| `prev_history_id` | The `History_ID` of the transaction you would like to cancel. | NUMBER | If `order_id` is not provided |
| `order_id` |The order ID of the transaction you would like to cancel. Max length = 32 | ALPHANUMERIC | Y |
| `canceltype` | `canceltype=1` | NUMBER | Y |
| `response_location` | The URL where you would like to store the response. | Full path URL | N |


### Editing Existing Transactions

In order to edit an existing transaction that is scheduled to originate the same day, the following parameters are needed in the request.  Please note that if you change any of the consumer information, it will be updated, as well.

| Parameter | Description | Type | Req |
|---|---|---|---|
| `username` | The merchant's Actum portal username. Max length = 16 | ALPHANUMERIC | Y |
| `password` | The merchant's Actum portal password. Max length = 16 | ALPHANUMERIC | Y |
| `syspass` | The merchant's system password assigned by Actum. Max length = 16 | ALPHANUMERIC | Y |
| `action_code` | You will want to enter `action_code=D` to indicate that this transaction is being updated. Max length = 16 | ALPHANUMERIC | Y |
| `order_id` | The order ID you would like to update. | NUMBER | Y |
| `recur_amount` | The new billing amount for the consumer. | XX.XX ex. 49.95 | Y |
| `billing_cycle` | A numerical value that corresponds with the frequency with which customer payments will be made. Input values include the following: One-Time Billing = -1, Weekly = 1, Monthly = 2, Bi-Monthly = 3, Quarterly = 4, Semi-Annually = 5, Annually = 6, Bi-Weekly = 7, Business-Daily = 8 | NUMBER | Y |   
| `max_num_billing` | The maximum number of times that a consumer will be billed (-1 is for perpetual billing). | NUMBER | Y |
| `next_bill_date` | The date of the next billing. | DATE MM/DD/YYYY | R |

### Updating Recurring Transactions

To update the billing amount, billing cycle, maximum number of billings, or the next billing date, the following parameters are required.  If any of the parameters are left blank, the adjustments will not take place.  Please note that this request will apply to the transaction scheduled for the following business day.

The response may contain the following parameters:
*	`status=success`
*	`status=error` (with reason for error)

| Parameter | Description | Type | Req |
|---|---|---|---|
| `username` | The merchant's Actum portal username. Max length = 16 | ALPHANUMERIC | Y |
| `password` | The merchant's Actum portal password. Max length = 16 | ALPHANUMERIC | Y |
| `syspass` | The merchant's system password assigned by Actum. Max length = 16 | ALPHANUMERIC | Y |
| `action_code` | You will want to enter `action_code=D` to indicate that this transaction is being updated. Max length = 16 | ALPHANUMERIC | Y |
| `order_id` | The order ID you would like to update. | NUMBER | Y |
| `recur_amount` | The new billing amount for the consumer. | XX.XX ex. 49.95 | Y |
| `billing_cycle` | A numerical value that corresponds with the frequency with which customer payments will be made. Input values include the following: One-Time Billing = -1, Weekly = 1, Monthly = 2, Bi-Monthly = 3, Quarterly = 4, Semi-Annually = 5, Annually = 6, Bi-Weekly = 7, Business-Daily = 8 | NUMBER | Y |   
| `max_num_billing` | The maximum number of times that a consumer will be billed (-1 is for perpetual billing). | NUMBER | Y |
| `next_bill_date` | The date of the next billing. | DATE MM/DD/YYYY | R |


# Transaction Status

To check the transaction status, the following parameters are required.

The response may contain the following parameters:
* `curr_bill_status`:  PreAuth, Settled, Returned, Declined
*	`refund_status` (if applicable):  Accepted, Pending, Declined 
*	`join_date` 
*	`recurstatus` (only for type=extended):  Active, Inactive   
*	`billing_cycle` (only for type=extended)   
*	`last_billing_date`  (only for type=extended)   
*	`next_billing_date`  (only for type=extended)
*	`error`

| Parameter | Description | Type | Req |
|---|---|---|---|
| `username` | The merchant's Actum portal username. Max length = 16 | ALPHANUMERIC | Y |
| `password` | The merchant's Actum portal password. Max length = 16 | ALPHANUMERIC | Y |
| `action_code` | You will want to enter `action_code=A` to tell the script that you are cancelling further recurring transactions. Max length = 16 | ALPHANUMERIC | Y |
| `prev_history_id` | The `History_ID` from the transaction that you would like to check the status on. | Max length = 32 | VARCHAR | If `order_id` is not provided |
| `order_id` |The order ID of the transaction you would like to check the status on. Max length = 32 | ALPHANUMERIC | Y |
| `type` | The transaction type can be either `basic` or `extended` (if neither are provided, then `default=basic`) | -- | N
