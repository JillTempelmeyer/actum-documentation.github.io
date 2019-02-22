# ACH Transaction Types

Now that you understand the ACH payment flow, this document will introduce you to the different transaction types that are available to your users, and provide an overview of how to get started with them.

## Table of Contents

* Introduction to Transaction Types
   * Debiting Transaction Types
   * Debiting and Crediting Transaction Types
   * Crediting Transaction Types
* Submission Deadlines

## Introduction to Transaction Types

There are two main categories into which all 8 transaction types fall, based on the movement of money: 

*	**Debiting** (pulling money from a Receiver’s account)
*	**Crediting** (depositing money into a Receiver’s account)  
 
### Debiting Transaction Types

1.	Debits
2.	Same-Day Debits
3.	Reinitiated Debits

### Debiting and Crediting Transaction Type

4.	Pre-Notifications or Pre-Notes

### Crediting Transaction Type:

5.	Credits
6.	Same-Day Credits
7.	Refunds
8.	Micro-Deposits 

Remember, transactions can be submitted to Actum through one of three methods:

* On the web through the Virtual Terminal
* Uploading transaction files via the shared SFTP
* Through a direct API integration. 

 
## Submission Deadlines

### Through the API

Most transactions must be submitted to Actum by **4:00 PM** (Central Time) in order to be included in that day’s batch file for processing.  Transactions that are submitted after that time will be processed the following day. Originators that need a later submission deadline (e.g., West Coast and/or online businesses) can sign up for Actum’s Late-Night Processing service, which pushes back the submission deadline from **4:00 PM to 9:00 PM** (Central). Both submission deadlines allow for transactions to take effect (i.e., be reflected in the Receiver’s bank account) the following banking day, usually at 9:00 AM (RDFI’s local time).

It’s important to note that Actum accepts transactions around the clock and will not decline a transaction for being late; it will simply be included in the next day’s batch.

There are two transaction types that have an earlier submission deadline: **Same-Day Debits** and **Same-Day Credits**.  These transactions need to be submitted to Actum by 11:00 AM (Central) in order to take effect by 4:00 PM (Central) later that day.  

### Through the SFTP

Transaction files submitted to us through the SFTP can take longer to process prior to being sent to the ODFI. Therefore, the submission deadlines are 30 minutes prior to the API cutoff times, as shown in the following details:

*	Regular transactions must be submitted by **3:30** PM (Central)
*	Late-Night transactions must be submitted by **8:30** PM (Central)
*	Same-Day transactions must be submitted by **10:30AM** (Central)

## Debits

ACH Debits are used to pull money from the Receiver’s account.  They are the most common transaction type.
Each Debit transaction must include the following data:

1.	Receiver’s Full Name (including the Company Name for B2B transactions)
2.	Receiver’s Bank Account and Routing Numbers
3.	Amount to be Debited
4.	IP Address of the client connecting to your server

In addition to having all the right data fields completed, transactions must not exceed certain **exposure limits**. These are maximums that are set based on historical and expected transaction activity, and are meant to limit the ACH Network’s exposure to the risk of excessive or fraudulent transactions.

Exposure limits are different for every Originator, and it’s the Originator’s job to stay within those limits.  Transactions that exceed an exposure limit will be declined by Actum automatically, which could cause unnecessary payment delays for the Originator.

**Note:**  To help your clients avoid such delays, Actum recommends that you help them keep track of their exposure limits, which are available through our Exposure Limits API.  Here, you can set up alerts for when a client is getting dangerously close to an exposure limit, so that he/she can proactively contact Actum to request a limit increase.  

There are five exposure limits:

*	Maximum dollar amount per transaction
*	Maximum dollar amount per day
*	Maximum dollar amount per month
*	Maximum transaction count per day
*	Maximum transaction count per month

## SAME-DAY DEBITS

Similar to regular ACH debits, **Same-Day Debits** have the same requirements, and are subject to the same exposure limits, as regular ACH Debits.  The only difference is the submission deadline (11:00 AM instead of 4:00 PM, Central) and the effective date (today instead of tomorrow morning).  They must also be designated for same-day processing in the API request or file.

The data requirements are spelled out in greater detail in our [Integration Guide](Link) and [File Import Specifications](Link).

**Note:**  Because Actum does not decline late transactions, and instead includes them in the next batch file, it may be helpful for your software solution to include an automated post-submission message letting your client know the submission was late and will therefore take effect on the following banking day instead of later that day.

## REINITIATED DEBITS

There are two kinds of **Reinitiated Debits**: 

* **Stopped Payment Retries**, which are used when a Debit transaction failed due to the following ACH Return Code:
  *	`R08` for Payment Stopped (the Receiver has placed a stop payment order on this debit Entry)
* **NSF Retries**, which are used to reinitiate previously failed Debit transactions resulting from one of two qualifying ACH Return Codes: 
  *	`R01` for Insufficient Funds (the Receiver’s available balance is not sufficient to cover the dollar value of the debit Entry), and
  * `R09` for Uncollected Funds (the Receiver has a sufficient ledger balance to satisfy the dollar value of the transaction, but the available balance is below the dollar value of the debit entry).
  
If a Debit transaction fails due to ACH Return Code `R08`, the Originator must receive separate authorization from the Receiver to reinitiate the Debit.  

For `R01` and `R09`, while a separate authorization is not required, it is considered a best practice for the Originator to follow up with their customer (the Receiver) to arrange a future date for the NSF Retry.

**Note:** An Originator is limited to 2 Reinitiated Debits and must submit them within 180 days of the initial Debit.  
NACHA requires that these transactions be identified as Reinitiated Debits rather than submitted as new transactions.  Any fees that are assessed by the Originator must also be clearly identified or explained at the time of the initial authorization.  Actum recommend using the following, or substantially similar, language:

* _“If your payment is returned unpaid, you authorize us to make a one-time electronic fund transfer from your account to collect a fee of [$ ];”_ or 
* _“If your payment is returned unpaid, you authorize us to make a one-time electronic fund transfer from your account to collect a fee. The fee will be determined [by/as follows]: [ ].”_ 

Actum recommends that you create an action (allowing your users to reinitiate a previous failed transaction) that turns on automatically, upon the receipt of a qualifying ACH Return Code.  The action should include fields where your users can enter a return fee and a future date for the Reinitiated Debit.  Your system should also disable the action based on the number of attempts (two maximum) and the time that has elapsed (180 days).  Actum's developer support team will offer guidance on how best to program this action in your software solution.

## Pre-Notes

**Pre-Notes** are zero-dollar transactions that are used to validate the Receiver’s bank account number.  They precede the authorized Debit or Credit transaction by three banking days.

An RDFI that receives a Pre-Note has three days to return it or to provide the ODFI with a **Notification of Change (NOC)**. 
NOCs are a type of return that the RDFI sends when they accept a transaction despite there being minor errors in the data. 

For example, if the RDFI was able to locate the Receiver’s account based on the submitted information, but the account number was slightly off, they would send an NOC (with the correct account number) to the ODFI. As part of our service, Actum will automatically apply those corrections in the subsequent debit transaction and in our database.

If the Pre-Note does not get returned by the end of Day 3, the account will have been validated, and the accompanying Debit or Credit Entry will go out automatically. Similarly, if the Pre-Note does get returned, then the Entry will not be processed and the Originator should follow up with the Receiver to get the correct information.

_While using Pre-Notes does delay fund settlement by three additional days, it’s considered a best practice. In fact, starting September of 2019, NACHA will require Originators to validate all newly authorized bank account numbers for web transactions using commercially reasonable methods.  And while there are some cool API-driven online bank account verification services to accomplish this (e.g., Plaid, Yodlee, Quovo, MicroBilt, etc.), Pre-Notes are the only full-coverage option available._

**Note:** Because a Pre-Note transaction must also include an authorized Debit or Credit, they are not submitted as two separate Entries, but rather, as one normal Entry with the authorized amount (subject to the same rules and exposure limits) and a special designation to be Pre-Noted.

The data requirements are spelled out in greater detail in our [Integration Guide](Link) and [File Import Specifications](Link).

## Credits

**ACH Credits** are used to deposit money into the Receiver’s account.  

The most important rule to understand here is that Credit transactions must be pre-funded.  Not all Originators will utilize ACH Credits, but the ones that do will understand that they must maintain a Credit Reserve Balance in their Actum account to fund their submitted Credit transactions.

The Credit Reserve Balance must be separately funded by one of several options: 

* Wires initiated by the Originator
Reverse wires authorized by the Originator and initiated by Actum
* ACH debits initiated by Actum from the Originator’s bank account
* Transfers from the Originator’s settled Debits before they are paid out.

In fact, Actum will automatically decline any Credit Entries submitted through the API that would cause the Credit Reserve Balance to go negative.  

The same information that’s required for an ACH Debit transaction is also required for an ACH Credit transaction (e.g., Receiver’s name, bank account details, transaction amount, etc.).  Unlike Debits however, Credits do not have the same exposure limits, unless the Originator requests them.  

**Note:**  Actum recommends that you use our [Credit Reserve API](Link) to display an up-to-date balance as a reference point for your clients. 

## Same-Day Credits

**Same-Day Credits** are submitted just like normal Credit Entries.  They are subject to the same Credit Reserve Balance requirement as normal Credits.  The only difference is the same-day designation and the earlier submission deadline.

How these transactions get designated as same-day Entries will be further spelled out in our [Integration Guide](Link) and [File Import Specifications](Link).

**Note:** Currently, NACHA limits the transaction size for Same-Day Credits to $25,000 per Entry.  Effective March 20, 2020, NACHA will increase the per-transaction dollar limit to $100,000.

## REFUNDS

**Refunds** are a type of Credit transaction back to the Receiver.  
So far, all the transaction types we’ve reviewed are submitted through a single API: Actum’s Submit API. Refunds, however, are submitted through a separate Refund API.

Because Refunds can only be issued against a previously settled Debit transaction, the required data are limited to that previous transaction’s unique IDs (generated by Actum) and the amount to be refunded.
Currently, our system does not allow for partial Refunds and the amount to be refunded must equal the amount of the initial Debit transaction.  Partial Refund functionality will be announced later this year.
Developer’s Note:  We recommend that you create a Refund action that gets enabled for each Debit or Same-Day Debit transaction upon settling.  All previous transactions should be stored and easily searchable so that if your users get a Refund request from the Receiver, they can process it with a click of a button.
(8) MICRO-DEPOSITS
Micro-Deposits, while not as common as they used to be, can be a useful verification tool for certain applications.  
It’s easy enough for someone to steal another person’s bank account information and use that to fraudulently pay the Originator for services.  To protect against that, some Originators will submit a Micro-Deposit transaction, which includes two separate amounts (usually a few pennies), and ask the Receiver to report back those amounts.  If the amounts match up, then the Originator has verified that the bank account is indeed the Receiver’s. 
Developer’s Note:  This is the one transaction type that is optional for our software partners to accommodate.  If you think it would be a useful addition to your software solution, let us know and we’ll help you build it out.
NEXT STEPS
Now that you understand each transaction type, you’re ready to delve into our Integration Guide (or File Import Specifications, depending on how you want to submit transactions to Actum).
Once you review those documents, we will go over the Reporting Requirements once a transaction gets submitted, including the various unique IDs that are generated by Actum that must be permanently associated with their respective transactions in your system.
Finally, we will go through our Actum Partner Certification Checklist together to make sure that your solution provides the smoothest, most delightful ACH payment processing experience to your clients.
