# Customers and Accounts

The following details explain how to submit debit and credit transactions to Actum.

### Merchant Object  [consider renaming if integration partners would use this in a different way than a merchant]

The `merchant` object represents account information pertaining to the merchant that submits a debit or credit transaction.

| Parameter | Description | Type | Req |
|---|---|---|---|
| `parent_id` | Parent ID of merchant. [max Length = 8] | ALPHANUMERIC | Y |
| `sub_id` | Sub ID of merchant. [max Length = 8] | ALPHANUMERIC | Y |
| `pmt_type` | Type of payment being submitted (check, or `chk`, is the default value). | -- | N |
| `response_location` | The `man_trans.cgi` script will respond to this URL with response variables if passed in. | Full path URL | N |
