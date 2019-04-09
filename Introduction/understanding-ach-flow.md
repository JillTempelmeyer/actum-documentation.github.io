# Understanding the ACH Payment Flow

Before embedding software or web-based solution to facilitate Automated Clearing House (ACH) payments, you'll want to make sure that you understand how the ACH payment flow works.  The rules and processes around ACH will impact the features you build and how you communicate the status of a transaction. 

This document will briefly introduce you to the ACH payment flow and how it relates to Actum's ACH processing procedures and policies.

## Table of Contents

* [Introduction](#introduction)
* [How ACH Works](#how-ach-works)
* [ACH Returns](#ach-returns)
* [Batch Processing](#batch-processing)
* [How Actum Transactions Work](#how-actum-transactions-work)
* [Late Returns](#late-returns)
* [Using Actum outto Facilitate ACH Payments](#using-actum-to-facilitate-ach-payments)
* [Actum's API](#actums-api)

## Introduction

Before building out a software or web-based solution to facilitate Automated Clearing House (ACH) payments, it is first important to understand how the ACH payment flow works.  The rules and processes around ACH will impact the features you build and how you communicate the status of a transaction. 


The ACH Network is managed by **NACHA** and is made up of banks and credit unions, called depository financial institutions.  Only depository financial institutions can transmit transaction data to the Federal Reserve (or the Electronic Payments Network, another ACH Operator), and they do so in batches throughout the day.  

**Note:** _NACHA is the governing body that establishes and enforces the processes and rules for ACH transactions._

## How ACH Works

In the world of ACH Processing, there’s **Originating** and there’s **Receiving**.  

For the purposes of understanding the ACH payment flow, it can be helpful to think of these actions as describing the movement of transaction data (and not the actual money).  Whether a merchant collects or sends funds from a customer, the merchant's business is the **Originator**, and its customer the Receiver.  

_The Originator is an individual or entity that has received authorization from the Receiver to initiate a direct payment (ACH debit) or direct deposit (ACH credit) over the ACH Network._

Not all originating merchants have ACH originating capability with their depository financial institutions.  In fact, most businesses require an intermediary with an established relationship at an **Originating Depository Financial Institution (ODFI)** in order to originate transactions. In this case, that intermediary is Actum Processing. 

With an intermediary involved, the Originator will submit a transaction to Actum, which will then send that data to its ODFI, and transmit that data to the Federal Reserve or other ACH Operator.  

As an example, let’s say that the customer’s bank account is at Wells Fargo.  In this case, Wells Fargo would be the Receiving Depository Financial Institution (RDFI).  If this transaction were an ACH debit for $100, Wells Fargo would see its ledger at the Federal Reserve decrease by $100, automatically, while the ODFI would see an increase by $100 (if this were an ACH credit, Wells Fargo would see its ledger increase, while the ODFI would see a decrease).  This increase or decrease should be further reflected in the individual accounts at the different banks.   

## ACH Returns

In most cases, an RDFI has two business days to object to a transaction and take back the funds from the ODFI.  This objection comes in the form of an ACH Return Code.  
For example, if the customer’s account had insufficient funds or simply didn’t exist, Wells Fargo would send the ACH Return Codes `R01` or `R03`, respectively, to Actum’s ODFI and the transaction would be reversed, leading to a failed transaction.  There are many reasons why a transaction might be returned, and sometimes customers have up to 60 days to dispute a charge, which could reverse a transaction that had already settled.

**Note:**  When there’s a return due to insufficient funds, the business that received the return can follow up with the customer and resubmit the transaction. However,it must not be submitted as a new transaction. Per NACHA rules, the transaction must be submitted as a `Retry PYMT`, which may include the addition of a retry fee in order for the merchant to recoup the return fee that Actum passes on from its ODFI (such as a bounced check fee).  

Therefore, your software (or web) solution should account for this requirement.

Refer to this list of [85 ACH Return Codes](Placeholder for Document), so that your software can translate them in a way that allows the user to quickly take action, such as contacting the customer).

## Batch Processing

As you can see, ACH doesn’t happen in real-time. While Actum helps streamline ACH processing as much as possible, it is not a fully-automated process.

Not only is the process somewhat manual because the RDFI can return a transaction, even after the money technically “changed hands” between banks, but also because ODFIs use a batch processing system.  

**Batch processing** occurs when an ODFI waits to aggregate all incoming transaction data into a single batch file before transmitting it to the Federal Reserve at regular intervals throughout the day.  Similarly, ACH Return Codes are also batched before being transmitted from the ODFI to Actum at regular intervals throughout the day.

Actum operates by a batch processing system.  This can be helpful to understand, as there is typically a buffer zone between when the Originator submits their transaction data and when Actum sends it to the ODFI. As a result, the Originator can edit or revoke a recently submitted transaction.  It also means that the status of a transaction can change only at predetermined times throughout the day.

## How Actum Transactions Work

A transaction that you submit to Actum can get accepted or declined. However, being accepted by Actum doesn not mean that the transaction cleared. Rather, it means that Actum's system automatically approved it for transmission to an Actum ODFI.

**Note:** If a transaction submitted through Actum's API gets declined, a response code that details the reason for the decline will be generated automatically.

Once a transaction is accepted by Actum, its status will remain `pending` until one of two things happens:

1. the transaction settles (a predetermined time period, or settlement period, has elapsed), or
2. the transaction fails (an ACH Return Code is received during the settlement period).

All accepted transactions will settle, as long as an ACH Return Code is not received by the end of its settlement period.  
If the transaction settles, it will do so at 2:00PM (Central Time) on the last day of its settlement period. The status will change from `pending` to `settled` at 2:00PM, as a result.  The Originator can then expect to receive their associated funds on the following business day, depending on how their account is setup.

If an ACH Return Code is received before the end of the settlement period, the transaction will fail. In this case, the failure may not be reflected in its status immediately.  Because of the batch processing system, it can take a few hours between when an RDFI returns a transaction, notifies the ODFI, and when the ODFI notifies Actum, and in turn the Originator.

Actum updates its files twice a day, once at 6:00AM (which would include any returns from the past 24 hours) and again at 4:00PM (which would include any other returns that might have come in until 3:00PM that day). Ultimately, for failed transactions, the status can only change either at 6:00AM or 4:00PM.

These times are important to note, because Originators want to know as soon as the information is available, if a transaction failed, so that they can act on it (e.g., follow up with the customer, suspend delivery of service, etc.).  

**Note:**  To meet the evolving expectations in the software industry, we are currently developing webhooks to push status changes to you as they happen.  Until then, we can only provide statuses on-demand through an API call.  Traditionally, it has been up to the Originator (or the software partner facilitating payments for the Originator) to develop their own status update protocols on their platform based on:

* Their own settlement date calculations, and
* Parsing through their clients’ transaction history files (provided through an SFTP on a daily or twice daily basis) to match return line items with their respective transactions using their Reference Key IDs.  

## Late Returns

What happens when an ACH Return Code comes in after the settlement period ends?  

First, Actum will advocate on your behalf to dishonor the return, if applicable.  If the return is legitimate, however, then it will be considered a Late Return and deducted from that day’s settlement calculation. 

As an example, if you had 10 transactions of $20 each, scheduled to settle today, and there were no returns by 2:00PM, the full $200 would settle and be paid out to the Originator.  If a legitimate return for one of those transactions is received later that day or tomorrow, and you have another $200 scheduled to settle tomorrow, only $180 would end up settling tomorrow ($200 settlement LESS the $20 late return).

## Using Actum to Facilitate ACH Payments

There are three main ways to interact with Actum:

1.	On the web through our Virtual Terminal
2.	Via SFTP to upload transaction files and download history files
3.	Through a direct API integration

Many of Actum's software partners and web merchants use a combination of the last two methods, by pushing transactions to Actum in real-time through an API call, then pulling transaction history files from us at scheduled intervals (6:00AM and 4:00PM) through a shared SFTP.

Some merchants use payment management software that is not directly integrated with Actum. Those merchants will often use the SFTP method to upload transaction files, or the Virtual Terminal to manually enter transactions and access their reports. 

## Actum's API

The following actions are available through our APIs:

* Submit transactions
* Validate a customer’s bank account
* Refund a transaction
* Edit or revoke a recently submitted transaction before it gets sent to the ODFI
* Cancel or update a recurring transaction

You can reference our APIs in our [Integration Guide](Placeholder for link to integration guide).

There are several transaction types that your software (or web) solution should accommodate:
*	Debits
*	Credits
*	Same-day Debits
*	Same-day Credits
*	Refunds
* Pre-Notes
* NSF Retries
