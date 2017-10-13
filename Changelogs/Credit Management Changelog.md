CH2.0.9
=========

Add Two Columns to Credit Held List
----------------------------------------------------------------

Two columns added to the Credit Held List Page:<br><p>Order Date</p><p>Amount Including VAT</p><p><br></p><p>Both these fields are on the table (Sales Header 36) , just not showing on the page</p>

SQL Error when trying to filter too many Customer records.
----------------------------------------------------------------

<div>Credit Controller filter on Credit Control screen builds a text filter of customers which is too long.&nbsp;</div><div>Mark the customer records which have been found and build the page by this instead</div>

CreditSafe in menu
----------------------------------------------------------------

<p>Departments -&gt; Administration -&gt; Application Setup -&gt; CreditSafe</p><p>Creditsafe is still appearing on the menu.</p><p>Takes you to credit management but no longer available</p>

CH2.0.8
=========

Remove CreditSafe functionality
----------------------------------------------------------------

Removal of the CreditSafe functionality

CH2.0.7
=========

Remove Credit Mgt. fast tab from Sales Documents
----------------------------------------------------------------

As in the Credit Mgt. app the Credit Mgt. fast tab should be removed to save vertical space and the Credit Mgt. Status field be moved to the General tab. The user should be able to access the other credit mgt. fields from an action on the ribbon.

CH2.0.6.1
=========

[Bug Fix]Calc overdue balance based on TODAY instead of Doc Date
----------------------------------------------------------------

Calculate the overdue balance for a customer based on today instead of the date
of the document

CH2.0.6
=======

[Bug Fix]Permission to Modify CLE from Interaction
--------------------------------------------------

PAG52102295 OnModifyRecord errors with no permission to modify Cust. Ledger
Entry.

Add modify permission to Cr. Control Interact. Subpage

Add Credit Mgt Status Field to Sales List
-----------------------------------------

Add the Credit Management Status field to the Sales List page (hidden by
default).

CH2.0.5
=======

Routine to Update Blocked Status of Customers
---------------------------------------------

Eastbrook have a bespoke routine that is run on their job queue to run through
the customers and update their Blocked flag according to the credit status of
their account e.g. automatically set customers to blocked when they have an
overdue balance or are over their credit limit; automatically set blocked to
blank if customers have paid invoices and now have a clear account.

Exclude Payment Methods from Credit Hold
----------------------------------------

The customer may want the ensure that certain sales orders are not credit held
if they have a certain payment method code. For example, if a customer is paying
by credit card it may not be necessary for the order to be approved by the
credit control team.

[Bug Fix]Copy Document Mgt. Publishers
--------------------------------------

In 2016 Copy Document Mgt. codeunit doesn't have the following publishers. Copy
them from 2017.

OnBeforeCopySalesDocument

OnBeforeCopyPurchaseDocument

OnAfterCopySalesDocument

OnAfterCopyPurchaseDocument

CH2.0.4.1
=========

[Bug Fix]Installing extension - Error on modifying data
-------------------------------------------------------

Installing the extension to 2017, cannot restore data to the Sales Shipment
Header table due to Modify permissions. Use the NAVAPP.RESTOREARCHIVEDATA to
restore the data OnNavAppUpgradePerCompany 

Permissions added to COD52102285 in versions 2016 and 2017

[Bug Fix]Copy Document Doesn't Clear CH Fields
----------------------------------------------

Credit Mgt. has several fields that are populated when the sales document is
held and released. These fields should be cleared when a new document is created
with the Copy Document functionality.

Without this, it is possible that a newly copied document can be credit released
automatically without being seen by the credit control team.

CH2.0.4
=======

[Bug Fix]Customer Statement
---------------------------

The customer details on the header of the report are indented into the center
instead of on the right hand side. The layout moves as the textboxes are not
touching one another in the header.

Overdue Balance on Credit Control Page
--------------------------------------

Add the overdue balance columns to the Credit Control page.

CH2.0.3
=======

[Bug Fix]Do Not Allow Release of Zero Value Orders
--------------------------------------------------

When the Order total in 0 and the Amount Credit Released is also 0..users are
able to release orders regardless of whether they have gone over their credit
limit with other orders.

Modify function CalcCrediHold in COD52102285 to check Amount Including VAT is
not 0 before checking it against the Amount Credit Released (LCY)

CH2.0.2
=======

[Bug Fix]Make Phone Call from To-Do
-----------------------------------

The Make Phone Call action on the To-Do card and list pages are still passing a
temporary record to the Interaction Page.

Change this to pass a real record

CreditSafe Automated Report Retrieval
-------------------------------------

We need some functionality to allow NAV to retrieve CreditSafe reports for
customer accounts periodically.

Add some new fields to the Credit Management Setup page

CreditSafe Auto Report (boolean) - enable/disable the other fields on the page
based on the value of this field.

CreditSafe Auto Report Frequency (dateformula).

CreditSafe Auto Report Selection (option - All Customers,Selected Customers)

Skip Customers with no Balance (boolean)

Add a new field to the Credit Management tab on the customer card

Include in CreditSafe Auto Report (boolean)

On validate of this field - if the value has been changed from true to false and
the "CreditSafe Auto Report" field is set to true and the "CreditSafe Auto
Report Selection" field is set to All Customers, pop a message "This customer
will be included in CreditSafe auto reporting as CreditSafe Auto Report
Selection is set to All Customers".

Create a new codeunit that can be run from the job queue that will identify the
customer account to retrieve reports for:

If CreditSafe Auto Report is set to false exit without doing anything.

Test that the CreditSafe fields are populated in Credit Management Setup (API
key, URL etc.)

If CreditSafe Auto Report is set to 'Selected Customers' filter the customer
table to records where "Include in CreditSafe Auto Report" is true.

Skip Customers with no Balance is true, filter the customer table to Balance \>
0

Filter the customer table to records where the Company Reg. No. is not blank and
the Country/Region Code is not blank.

For each customer find the date of the last report that was generated. If the
time period since the last report is greater than CreditSafe Report Frequency
then request a new report. (date retrieved + frequency \< Workdate...then
retrieve new report)

If the credit rating on the new report is lower than the previous report than
create a notification to the credit controller of the customer account.

If there is not credit controller then send the notification to the Default
Credit Controller instead.

If there is no previous report for the customer then retrieve one and send a
notification to the credit controller (or default credit controller) that the
first credit report has been retrieved for the customer.

Default Credit Controller
-------------------------

Create a new field on the Credit Management Setup page, Default Credit
Controller, validated to the Credit Controller table.

This credit controller will be used by CreditSafe as the responsible user for
accounts that don't have a credit controller assigned.

Credit Management Status field on Sales Documents
-------------------------------------------------

Remove the "Credit Held" option from status field of the Sales Header table.

Add a new Credit Management Status field.

CH2.0.1
=======

Rework Calculation of Credit Control page to use Query
------------------------------------------------------

Rework the calculation of this page as far as possible into a query so that it
can be executed in one trip to the server.

[Bug Fix]Credit Controller Code filter on Credit Control screen
---------------------------------------------------------------

If an invalid credit controller code is used to filter on, all the records
remain on the screen. If there is no customer linked to the credit controller
code, the credit control screen should show no entries (right now it shows them
all)

Modify the InitFilteredEntries function to DELETEALL entries when the
CustomerFilter is blank.

Add Make Phone Call Action to Customer List/Card
------------------------------------------------

Add the Make Phone Call action from the Credit Control screen to the Customer
List and Customer Card pages.

Interactions Factbox
--------------------

Add factbox with details of credit control interactions to:

Credit Control page

Customer List page.

Customer Card page

Filters on Credit Control Page
------------------------------

Need to be able to filter on different columns on the Credit Control page e.g.
Horizon want to be able to filter on the Balance fields and the No. of To-dos
column.

Changing the page type to List (rather than ListPlus) would allow filters to be
applied to any field, but would make the controls above the list (show LCY,
Credit Controller) uneditable. Need to investigate if there a good way to have
the best of both worlds. Ideally not adding additional controls next to Credit
Controller to apply filters to other fields - ideally the user should be able to
filter on any of the columns on the page.

[Bug Fix]Credit Control Interaction notes bug
---------------------------------------------

When entering a note in the Credit Control Interaction without any CLE selected,
in some cases we get the error message:

'The Credit Control Temp Blob already exists. Identification fields and values:
Primary Key ='\*\*\*\*''

Make changes to the function BuildCLERecFromSelectedEntries to set the CLE
filter text to 0 when the EntryCount = 0

[Bug Fix]Make Phone Call action from CLE list
---------------------------------------------

ERROR: A transaction must be started before changes can be made to the database

Use real table when opening page, and modify the OnClosePage function on
PAG2102294 to longer create a new interaction

CH2.0.0
=======

Toggle Enabled Status of Actions on Credit Held List page
---------------------------------------------------------

Two actions, Held Documents and Released Documents allow the user to switch the
records that are shown on the page between held documents and released
documents.

Toggle whether these actions are enabled depending on the state of the screen
i.e. when held documents are visible the Held Documents action should be
disabled; when released documents are visible Released Documents should be
disable.

Credit Controller Profile & Role Centre
---------------------------------------

A new profile should be included in the default configuration for Credit Mgt.
This profile should have a new role centre page which includes the following:

Total Debt (LCY) - the total amount (in LCY) owed by customers

Total Debt Due (LCY) - the total amount (in LCY) which has a due date of the
WORKDATE or earlier.

Total Promised (LCY) - the total amount (in LCY) which has been promised (only
on Open customer ledger entries, do not include promised amounts on entries
which have been closed).

% Due Promised - the percentage of the debt that is due that has been promised.

Calculate the total promised amount (LCY) on ledger entries which have a due
date of the WORKDATE or earlier.

Divide that amount by the Total Debt Due (LCY) \* 100 to give a percentage.

Received MTD (LCY) - the sum in LCY of payment type customer ledger entries that
has been posted in this month.

A separate group on the role centre should show these figures:

Customers Overdue - the number of customers that have an overdue customer ledger
entry

Customer Blocked - the number of customers where the Blocked field is anything
other than blank

Credit Held Documents - the number of sales documents which have a status of
Credit Held.

Open To-dos - the number of to-dos that are Open

Overdue To-dos - the number of to-dos that have a Due Date of the WORKDATE or
earlier.

CH1.6.4.1
=========

[Bug Fix]Typo on action caption
-------------------------------

The action Default Configuration is spelt incorrectly.

CH1.6.4
=======

Ability to Delete Credit Control Interactions
---------------------------------------------

Credit Control Interactions cannot be deleted currently, the user might need to
be able to.

Customer Payment Code
---------------------

Add customers Payment Term Code to the Credit Control list page and the Credit
Control Interaction page

Remaining Amount total on Interaction
-------------------------------------

Remaining Amt and Remaining Amt (LCY) fields on the Credit Control Interaction
to show the total of the selected customer ledger entries of that interaction.
Dynamically changes when the CLE is selected/unselected

[Bug Fix]Populate To-Do Entry notes
-----------------------------------

The To-Do Entry notes are not being populated from the Interaction.

Set notes when the interaction is closed, or when the note is changed

CH1.6.3
=======

CreditSafe Error Handling
-------------------------

If an error occurs calling the creditsafe web services the only error the user
sees is a failure to load an xml document (because it is blank). Change this to
show the error message that was returned from the web service (will help to
identify whether the failure was due to not being able to authenticate, not
being able to connect or something else).

When requesting a creditsafe report we should test that the country/region code
has been populated (as without it the FindCompanies call will not work).

CreditSafe Default Configuration
--------------------------------

Include some default configuration for the CreditSafe integration in the config
that is retrieved from the Credit Management Setup page. The data should be as
follows:

CreditSafe
URL: https://webservices.creditsafe.com/GlobalData/1.3/MainServiceBasic.svc

Report Type: Full

Language Code: EN

Credit Control
--------------

A new Credit Control page that shows the aged debt of customers to allow credit
controllers to prioritise customers that they want to contact for debt
collection.

CH1.6.2
=======

Credit Control To-dos
---------------------

This entity will represent work (normally a phone call) that needs to be
completed by the credit control team. This is opposed to Credit Control
Interactions which record work that has been completed.

CreditSafe Integration
----------------------

Integration with CreditSafe external service.

This service has an API to retrieve financial information about companies -
usually required to determine the credit worthiness of those companies.

Separating Credit Release and Sales Release Functions
-----------------------------------------------------

Create a new field in Sales & Receivables Setup, Release Doc. on Credit Release

Create a new option for the Status field of the Sales Header table, Credit
Released.

Change the behaviour of the Release action on the Credit Held List page. This
action should call COD52102285.Release to perform the existing checks that the
user has sufficient privileges to release the document. The status of the
document should be changed to Credit Released.

If the Release Doc. on Credit Release field on the Sales Setup page is true,
pass the record to the Release Sales Document codeunit to be released by the
standard functionality.

Note that the Credit Management Release function is also called by an event in
the standard release codeunit, be careful not to end up in an infinite loop.

[Bug Fix]Missing Upgrade Instructions
-------------------------------------

[Bug Fix]Change Aging Band on TM Statement Report
-------------------------------------------------

Currently the TM Statement report has the following buckets in the aging bands:

30 days

60 days

90 days

120+

Any customer ledger entries that are not due seem to go into the 30 days bucket,
which is not correct.

Create a new "Not Due" bucket to be displayed first and followed by the existing
buckets. Any cust. ledger entries with a due date before the start date of the
30 days bucket should go into the Not Due bucket.
