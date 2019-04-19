# ACH Transaction Types

ACH transaction types are identified as the following terms:


**Check Pre-Note:**  This means that we are sending a Pre-Notification of the pending debit entry to allow the RDFI (consumer's bank) the opportunity to return (Check Return) or correct (Notice of Change (NOC)) the item. The pre-notification process involves validation of the ABA and bank account number only.

**Check Pre-Auth:** This is when we request the funds from the bank (initiate the ACH debit entry).

**Same-Day Debit:**  A Check Pre-Auth transaction that will debit the consumer’s account the same day.

**Same-Day Credit:**  A Check Refund transaction that will deposit into the consumer’s account the same day.

**NSF Retry:**  Per NACHA regulations a merchant can retry a debit that has been returned due to Insufficient Funds up to 2 times.  

**NSF Fee:**  A return fee applied to a debit that is returned due to Insufficient Funds.

**Check Settlement:** This indicates that the Check Pre-Auth has not been returned by the RDFI (consumer's bank) prior to the specified time period indicated in the Service Agreement the merchant and Actum Processing entered into.

**Check Return:** This means that the RDFI (consumer’s bank) has returned the item prior to the specified time period defining a Check Settlement (Settled Funds) indicated in the Actum Processing Service Agreement.

**Check Late Return:** This means that a Return was received by Actum Processing after a Check Settlement.

**Check Refund:** We have refunded this money back to the consumer.

**Notice of Change (NOC):**  This means a response from the RDFI that Actum Processing needs to make a change to the ABA or bank account data in order to properly process the entry.

