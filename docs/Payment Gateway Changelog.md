PGY1.1.5.3
==========

[Bug Fix]Missing Permissions on Service Password table
------------------------------------------------------

Add permission to the Service Password table for the Payment Gateway Setup
table.

PGY1.1.6
========

Ability to take Payment from Cash Receipt Journal Page
------------------------------------------------------

Currently the Payment Gateway can be initiated from a sales order or a posted
sales invoice. Add the ability to initiate a payment gateway payment from the
Cash Receipt Journal.

The payment will need to be linked to a single gen. journal line. Functions will
be required to populate the payment information from the gen. journal line that
is passed. When the payment is posted the payment customer ledger entry no.
should be populated on the payment gateway log entry automatically.

Display Payment Gateway payment amount on Cash Receipt Journal
--------------------------------------------------------------

Add a flowfield to Gen. Journal Line to look up the payment amount from the log.

PGY1.1.5
========

[Bug Fix]RunModal Error When Suggesting Journal Lines with Multiple Payment Log Records
---------------------------------------------------------------------------------------

Set a single document no. for all lines on the journal. Determine that document
no. in the report and then pass the result to the codeunit that inserts the gen.
journal line.

[Bug Fix]Payment Gateway Log for Invoices Doesn't Show Log Entries
------------------------------------------------------------------

[Bug Fix]Applies-to ID Populated on Gen. Journal Line Even When There are No Applications
-----------------------------------------------------------------------------------------

PGY1.1.4
========

Support for Default Configuration
---------------------------------

Add a new action Get Default Configuration to the Payment Gateway Setup page.
Call a new function, GetDefaultConfiguration in the Payment Gateway Mgt.
codeunit.

Create a TempValueNameBuffer local variable (temporary, Name/Value Buffer
table). Populate the Name field with 'PGY' (the code for the Payment Gateway
addition).

Call COD52102425 (Get Additions Config) with the TempNameValueBuffer record.

PGY1.1.3
========

Replace UK with GB for Country/Region Code
------------------------------------------

SagePay requires that the ISO 2 digit or 3 digit code is used. This is GB, not
UK.

PGY1.1.2
========

[Bug Fix]Order Value not Retained from Payment Confirmation Page
----------------------------------------------------------------

PGY1.1
======

Payment Gateway Setup Changes for Hosting
-----------------------------------------

New fields are required on the Payment Gateway Setup page to support the hosted
version of the Payment Gateway

PGY1.0
======

Payment Gateway Setup Page
--------------------------

Create a new table, Payment Gateway Setup, ID = 9059250 with the following
fields:

Primary Key (code 10)

SQL Server (text 30)

Database Name (text 50)

Authentication (options, Windows Authentication, SQL Authentication)

User Name (text 50)

Password Key (GUID)

Create a new page, Payment Gateway Setup, ID = 9059251. Add the above fields
apart from the Primary Key and Password Key to a SQL Connection tab.

The password should not be stored in this table but in the Service Password
table (see the standard CRM integration page for an example of how this works).
The password should be entered into a variable on the page and saved into the
Service Password table.

The User Name and Password fields should only be enabled when the authentication
type is set to SQL Authentication.

Create a new function, GetConnectionString to return a SQL connection string
using the values entered in the above fields.

Support for PayPal & WorldPay
-----------------------------

Add new functions in COD905259 to process the additional payment types:

ProcessWorldPayPayment

ProcessPayPalPayment

Change the ProcessPayment function to accept an additional parameter of
PaymentType (option - SagePay,WorldPay,PayPal). Call the payment function
appropriate to the payment service that is being used.

The payment type should be determined by the Payment Method Code of the document
that has been submitted for payment.

Taking Payment from Sales Orders & Invoices
-------------------------------------------

Add new fields to the Sales Header record:

Payment Gateway Status (text 30) ID = 9059249

Payment Gateway Amount (decimal) ID =9059250

Add these fields to the Invoicing tab of the Sales Order and (unposted) Sales
Invoice pages.

Create a new function in the Payment Gateway Mgt. codeunit,
GetSalesOrderPaymentGatewayStatus. This function should:

Find the last Payment Gateway Log record with source details matching the sales
order that has been passed to it.

Return the Request Status and the Payment Amount (as VAR parameters) from the
record.

Subscribe to the OnAfterGetRecord event of the sales order and (unposted) sales
invoice pages.

Set the value of the Payment Gateway Status and Payment Gateway Amount fields in
Rec to the values returned from the above function (do not modify the record)

Payment Gateway Setup
---------------------

Add new fields to the Payment Method table:

Payment Gateway Type (option - (blank),SagePay) - other payment types will be
added in future.

Show Payment Page - boolean

Show Amount Confirmation - boolean

Pre-Authorization - boolean (ENG caption = Pre-Authorisation)

Wait for Reply - boolean
