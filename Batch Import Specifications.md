# Batch Import Specifications

### ACH Transaction File Format

The name of the file will need to begin with “ACH_YOURPARENTID_”. The ParentID will be unique to your account and will be assigned by a merchant support specialist upon integration. The file must be a plain text, ASCII file, and all data items may contain only normal ASCII characters (ASCII 32-126). ASCII control characters (ASCII 0-31), Upper-ASCII characters (ASCII 127-255) or Unicode characters are not allowed. This means foreign language (i.e. non-English) characters are not allowed.
Each transaction must be on its own line after the header record - one line per transaction and one transaction per line - after the header line. The field values must be comma-delimited, and all values must be surrounded by double quotes.
Header Line (Line 1 in the file)

The first line of the file is a header line. This line specifies the fields and the order of the fields in every other line in the file:

"ParentID","SubID","PmtType","TransactionType","CustName","CustPhone","CustEmail","CustAddress1","Cus tAddress2","CustCity","CustState","CustZip","ShipAddress1","ShipAddress2","ShipCity","ShipState","ShipZip"," AccountType","ABANumber","AccountNumber","MerOrderNumber","Currency","InitialAmount","BillingCycle"
,"RecurAmount","DaysTilRecur","MaxNumBillings","FreeSignUp","ProfileID","PrevHistoryID","CheckNumber"," Username","Password","NextBillingDate"

### Field Descriptions:

1.	ParentID (required): This field must contain a ParentID string assigned by Merchant Support to the merchant during the initial set up.

2.	SubID (required): This field must contain a SubID string assigned by Merchant Support to the merchant during the initial set up.

3.	PmtType (required): This field must contain the following value – “CHK”.

4.	TransactionType (required): This field must contain one of the following values:
•	“D” = Debit
•	“C” = Credit
•	“R” = Refund
•	“SD” = Same-day Debit
•	“SC” = Same-day Credit
•	“DN” = Pre-Note
•	“DY” = NSF Retry (see note on “InitialAmount”)

5.	CustName (required): This field must contain the consumer’s or company’s name.

6.	CustPhone (depends on SubID setting – default is required): This field may contain the consumer’s or company’s phone number.
 
7.	CustEmail (depends on SubID setting – default is optional): This field may contain the consumer’s or company’s email address.

8.	CustAddress1 (depends on SubID setting – default is required): This field must contain the consumer’s or company’s address.

9.	CustAddress2 (optional): This field may contain additional address information if it is present, but it is
generally better to leave this blank and put all relevant information in “CustAdress1”.

10.	CustCity (depends on SubID setting – default is required): This field must contain city of residence.

11.	CustState (depends on SubID setting – default is required): This field must contain the state of residence.

12.	CustZip (depends on SubID setting – default is required): This field must contain the zip code of residence.

13.	ShipAddress1 (optional): This field may contain the shipping address if it is different from the billing address.

14.	ShipAddress2 (optional): This field may contain an additional line of the shipping address if it is different from the billing address.

15.	ShipCity (optional): This field may contain the shipping city if it different from the billing city.

16.	ShipState (optional): This field may contain the shipping state if different from the billing state.

17.	ShipZip (optional): This field may contain the ship zip code if different from the billing zip code.

18.	AccountType (required): This field must contain one of the following values –
•	“C” = Checking Account
•	“S” = Savings Account

19.	ABANumber (required): This field must contain the ABA Routing number of the bank account.

20.	AccountNumber (required): This field must contain the bank account number.

21.	MerOrderNumber (optional): This field is usually left blank for batch transaction imports, but it may be used for any extra information regarding the transaction that the merchant may wish to keep track of. Actum will store this information to the database and it will be sent back to the merchant through the daily transaction file.
 
22.	Currency (required): This field must contain the value of “US” – U.S. Dollars.

23.	InitialAmount (required – See note on recurring): This field must contain the amount of the transaction to be processed. This amount must be entered as a decimal amount, for example “29.20” and NOT “29.2”. A value of “29.2” in the field will fail to be imported.

To submit an NSF Fee along with the retry, add a second amount to the “InitialAmount” value delimited with a colon; e.g. to do a 19.95 retry transaction along with a 10.00 NSF Fee, submit “19.95:10.00” for “InitialAmount”.

24.	BillingCycle (required): This field must contain one of the following values:
•	“-1” = One-Time Billing (i.e. no recurring transactions)
•	“1” = Weekly
•	“2” = Monthly
•	“3” = Bi=Monthly (once every 2 months)
•	“4” = Quarterly
•	“5” = Semi-Annually (once every 6 months)
•	“6” = Annually
•	“7” = Bi-Weekly (once every 2 weeks)
•	“8” = Business-Daily

25.	RecurAmount (required if “BillingCycle” indicates periodic billing; optional otherwise – See note on Recurring)
If the “BillingCycle” indicates a periodic billing and the periodic rebilling amount is different from the “InitialAmount”, enter that rebilling amount here. Like the “InitialAmount”, this amount must be
entered as a decimal amount, for example “29.20” and NOT “29.2”. A value of “29.2” in this field will
fail to be imported.

If the “BillingCycle” indicates a One-Time Billing, then this should be left blank, i.e. a double-quoted empty string (“”).

26.	DaysTilRecur (optional): If the “BillingCycle” indicates a periodic billing, then put the number of days after the initial transaction is billed that the first recurring transaction should be billed. If the “BilllingCycle” indicates a One-Time Billing, then this should be left blank, i.e. a double-quoted empty string (“”).

27.	MaxNumBillings (optional): If the “BillingCycle” indicate a One-Time Billing then this should be left blank, i.e. a double-quoted empty string (“”). If the recurring billing is to be continued until canceled, this field should be set to “-1”. If the recurring billing should be for a fixed number of payments then automatically stop, this field should contain the appropriate integer (e.g. “4”)

28.	FreeSignUp (optional): If the “BillingCycle” indicates a periodic billing, then this field should contain a
“1” if the “InitialAmount” is free (i.e. 0.00). Otherwise this field should contain a “0”. If the
“BillingCycle” indicates a One-Time Billing, then this should be left blank, i.e. a double-quoted empty string (“”).
 
29.	ProfileID (optional): This is an option to the itemized billing information fields (InitialAmount, BillingCycle, RecurAmount, DaysTilRecur, MaxNumBillings, FreeSignUp). This option requires a billing profile to be set up by Actum Merchant Support and the ID for that profile given to the merchant. If this value is specified the itemized billing information fields may be left blank, i.e. a double-quoted empty string. ("")

30.	PrevHistoryID (optional): Leave this field blank, i.e. a double-quoted empty string. (“”)

31.	CheckNumber (optional): If you have the check number of the check, put in this field. Otherwise, you may leave this field blank, i.e. a double-quoted empty string. (“”)

32.	Username (optional): If there is a username associated with this record, put it in this field. Otherwise, you may leave this field blank, i.e. a double-quoted empty string. (“”)

33.	Password (optional): If there is a username associated with this record, put it in the field. Otherwise, you may leave this field blank, i.e. a double-quoted empty string. (“”)

34.	NextBillingDate (required if billing date is in the future. See note on recurring.): In this field, if the record is to be billed on a future date, put this date in this field. Format of field is “MM/DD/YYYY”. Otherwise, you may leave this field blank, i.e. a double-quoted empty string. (“”)

**Recurring Note:** If importing recurring transactions that will be billed on a future date, please set the “InitialAmount” field to
“0.00”. “RecurAmount” and “NextBillingDate” will be required.

### Examples

The below is some sample data to illustrate what the file should look like.
•	Record 1 is an initial one-time transaction.
•	Record 2 is a monthly recurring transaction with an initial billing of $29.90 which will recur at $39.90 14 days after the initial billing.
•	Record 3 is a monthly recurring transaction that will bill $39.90 on 12/12/2012.

#### Record 1

"ParentID","SubID","PmtType","TransactionType","CustName","CustPhone","CustEmail","CustAddress1","Cus tAddress2","CustCity","CustState","CustZip","ShipAddress1","ShipAddress2","ShipCity","ShipState","ShipZip"," AccountType","ABANumber","AccountNumber","MerOrderNumber","Currency","InitialAmount","BillingCycle"
,"RecurAmount","DaysTilRecur","MaxNumBillings","FreeSignUp","ProfileID","PrevHistoryID","CheckNumber"," Username","Password","NextBillingDate"
"ACTUM01","TEST01","CHK","C","Joe Q Public","512-555-1234","joe@null.com","100 Main St.","CustAddress2","Austin","TX","00893","","","","","","S","999999999","13371337","","US","29.90","- 1","","","","","","",""

#### Record 2

"ParentID","SubID","PmtType","TransactionType","CustName","CustPhone","CustEmail","CustAddress1","Cus tAddress2","CustCity","CustState","CustZip","ShipAddress1","ShipAddress2","ShipCity","ShipState","ShipZip"," AccountType","ABANumber","AccountNumber","MerOrderNumber","Currency","InitialAmount","BillingCycle"
,"RecurAmount","DaysTilRecur","MaxNumBillings","FreeSignUp","ProfileID","PrevHistoryID","CheckNumber"," Username","Password","NextBillingDate"
"ACTUM01","TEST02","CHK","C","Bob Yakuza","512-555-0893","bob@null.com","893 Ginza","CustAddress2","Austin","TX","00893","","","","","","C","999999999","483219
3828","","US","29.90","2","39.90","14","-1","","","","","","",""

#### Record 3

"ParentID","SubID","PmtType","TransactionType","CustName","CustPhone","CustEmail","CustAddress1","Cus tAddress2","CustCity","CustState","CustZip","ShipAddress1","ShipAddress2","ShipCity","ShipState","ShipZip"," AccountType","ABANumber","AccountNumber","MerOrderNumber","Currency","InitialAmount","BillingCycle"
,"RecurAmount","DaysTilRecur","MaxNumBillings","FreeSignUp","ProfileID","PrevHistoryID","CheckNumber"," Username","Password","NextBillingDate"
"ACTUM01","TEST03","CHK","C","Bob Yakuza","512-555-0893","bob@null.com","893 Ginza","CustAddress2","Austin","TX","00893","","","","","","C","999999999","483219 3828","","US","0.00
","2","39.90","","-1","","","","","","","12/12/2012"

### Decline Codes

Please refer to the following table: [Decline Codes](https://github.com/JillTempelmeyer/jilltempelmeyer.github.io/blob/master/API%20Reference/Submitting%20a%20Transaction.md#decline-codes)
