
LC3.7.2.1
=========

Generating Expected Cost Invoices When Handheld Installed
--------------------------------------------------

The expected cost worksheet gives the user the ability to generate a purchase invoice for the expected cost of receipts (Duty, Freight) and automatically populates the Item Charge Assignment behind the purchase line.<div><br></div><div>Landed Costs uses the Expected Costs Received table to create the Item Charge Assignment with the correct Receipt No. and Receipt Line No.</div><div><br></div><div>Trouble is, Handheld changes the Line No. that Purch. Rcpt. Lines have so the lines that the Expected Costs Received table point to lines that don't actually exist, so the purchase invoice fails to post.</div>

Disallow Deletion of Posted Purchase Receipts for Expected Item Costs
--------------------------------------------------

Standard NAV allows posted purchase receipts to be deleted. This presents a problem for Landed Costs if there are outstanding Expected Costs Received records. Without the receipt lines you cannot post a purchase invoice with item assignment.<div><br></div><div>This is the functionality that Landed Costs uses to reverse the accrued expected costs and reverse the expected cost value entries. Without the purchase receipt there is no mechanism to update these records.</div>

LC3.7.2
=========

Copy Document Functionality
--------------------------------------------------

Landed Costs should tie into the copy document functionality.<div><br></div><div>When a purchase order is copied and there are Expected Costs associated with the original document&nbsp;a confirmation dialog&nbsp;should ask&nbsp;the user whether they would like to just copy the expected costs from the original line&nbsp;or to copy and update the costs for the new purchase lines. </div>

LC3.7.1.1
=========

Duty Calculation Doesn't Update Default Cost (LCY)
--------------------------------------------------

Default Cost

Default Cost (LCY)

Default Cost (Base)

Default Cost (Base) (LCY)

these four fields ought to be updated when the duty is calculated - but only
three of them are.

LC3.7.1
=======

Rename Duty and CCT fields
--------------------------

See https://www.yammer.com/tecman.co.uk/\#/Threads/show?threadId=877555481 for a
discussion of the problem.

Go through LC tables and rename "Duty" to "Is Duty" and "CCT" to "Is CCT". This
offends my sense of naming convention but will avoid the RapidStart problem
described.

LC3.7.0.1
=========

[Bug Fix]Prevent Revaluation of Items with Outstanding Expected Document Costs
------------------------------------------------------------------------------

We need to prevent items from being revalued if they have outstanding expected
document costs against ILEs for that item - because the revaluation entry that
is created in this scenario will be wrong.

REP5899 (Calculate Inventory Value) can be run in a couple of ways, by item or
by item ledger entry. The report needs to be changed to skip records which have
outstanding (uninvoiced) expected document received records.

If running per item skip the item if there are any expected cost received
records for the item that have not been posted (the posted invoice fields are
blank).

If running per ILE, skip item ledger entries that have any uninvoiced expected
cost received records.

Support for Item Tracking
-------------------------

LC3.7.0
=======

[Bug Fix]OnModify Not Handled Correctly
---------------------------------------

See COD52102268, the values for xRec and Rec are the same by the time the
function is called. We need to be able to retrieve the correct values for xRec
so that the function to update the expected document costs is called when
necessary.

Insert a purchase line without specifying the item no.

Validate the No. to an item no. that has expected item costs attached

Modify the purchase line

Expected document costs will not exist for the record

When the line is modified we should recognise that the No. field is different
and update the expected document costs accordingly.

LC3.6.0.1
=========

[Bug Fix]Incorrect Revaluation Calculation
------------------------------------------

The revaluation calculation is not taking expected costs into account
(REP5899.InsertItemJnlLine).

Imagine:

Item A has a base unit cost of 10 and freight cost of 2.

Std Cost is rolled up for item A to calculate a std cost of 12.

Item A is received and invoiced, quantity 2

Expected item cost of 20

Expected freight cost of 4

Expected freight cost for item A is changed from 2 to 3

Std cost is rolled up to 13

Revaluation journal is calculated for item A

An extra value entry is created against the goods for 1 (the difference between
the standard cost at the time of the receipt and the new standard cost).

This is incorrect, a new freight cost should not force a revaluation of the
goods.

Change REP5899 InsertItemJnlLine (line 442). If expected costs exist against the
item, validate the new unit cost with the Base Unit Cost of the item, otherwise
use the existing logic. If the base unit cost is not different to the unit cost
of the item ledger entry then do not insert an item journal line.

(Don't worry about making changes to the report itself, Landed Costs cannot be
packaged as an extension anyway).

LC3.6.0
=======

Making Container No. Mandatory (Optionally)
-------------------------------------------

Currently the Container No. field on the Purchase Line table is mandatory (you
cannot receive the purchase order lines without one). This might not be relevant
to all customers  - add a setup table and page with an option to determine
whether Container No. should be mandatory or not.

LC3.5.9.2
=========

[Bug Fix]Item Charge Assignment Calculation
-------------------------------------------

Calculate the item charge assignments to 5 decimal places (as per standard NAV).

LC3.5.9.1
=========

[Bug Fix]Expected Cost Calculation Incorrect
--------------------------------------------

When the actual item charge amount is posted via an item charge assignment the
expected cost is not being reversed.

Invoiced Amount Column on Expected Cost Worksheet
-------------------------------------------------

[Bug Fix]Expected Document Costs Should Not be Editable
-------------------------------------------------------

LC3.5.8
=======

Writing-off Expected Costs
--------------------------

Currently there is no way to write-off expected costs that you are not going to
get invoiced for.

For example, you expect to receive an invoice for some freight costs for certain
receipts. The expected costs are posted when you received the goods and the
lines appear on the expected cost worksheet. If after a few months you decide
that you are not going to receive an invoice for that freight, there is no way
to record that you are writing the expected cost off and preventing the lines
from re-appearing on the worksheet.

The user should be able to create a zero value purchase invoice for identified
lines on the expected cost worksheet and create an item charge assignment of
zero value to the relevant purchase receipt lines. This should reverse the
expected cost and (in the case of standard costed items) post a variance for the
invoice value that was expected.

See the attached objects with some proof of concept changes.

LC3.5.7
=======

Cross Currency Item Charge Assignment
-------------------------------------

LC3.5.6
=======

Assign Invoice Amount Action
----------------------------

Create a new action, Assign Invoice Amount on the Expected Cost Worksheet page.
This action should call a new function of the same name in COD52102265. Pass the
Invoice Total that has been entered on the worksheet and the Expected Cost
Worksheet Line record (VAR)

Test that the invoice value that has been passed is \>0

Sum the "Cost Amount Expected (LCY)" of the worksheet lines within the current
filters

Loop through the worksheet lines and for each line validate the Invoiced Amount
field with (cost amount expected (of current line) / cost amount expected (sum
of all lines)) \* worksheet invoice total

When the Invoiced Amount field has been updated for all lines, check that the
sum of those lines equals the Invoice Total entered on the worksheet.

Correct for any rounding errors on the final line of the worksheet so that the
sum of the lines does match the worksheet value.

Calculate Charge Options
------------------------

Create a new processing only report that will allows the user to enter filters
on the Expected Cost Received table. The following fields should be presented as
default filters.

Document Type

Document No.

Container No.

Item Charge No.

Currency Code

Vendor No.

Run the report modally from the CalculateBatch function of COD52102265. If the
user clicks OK on the report copy the filters from the Expected Cost Received
record to the ExpectedCostReceived record that is used in the function. The
existing filers should still be applied on the Open, Worksheet Name and Posted
Invoice No. fields.

Remove the code that applies filters to the Item Charge No. and Vendor No.
fields.

LC3.5.4
=======

Allow Multiple Invoices for the Same Expected Cost Received Record
------------------------------------------------------------------

Remove the code from Value Entry OnAfterInsert (in COD22 in the 2015 version, in
the Expected Cost Mgt. codeunit in the 2016 version) which throws an error under
these circumstances.

Set DecimalPlaces Property on LCY fields in Expected Cost tables
----------------------------------------------------------------

The amount fields in the expected cost tables should have the DecimalPlaces
property set correctly.

Carry Container No. field from Purchase Line through to the Purch. Rcpt. Line table
-----------------------------------------------------------------------------------

Add the Container No. field to the Purch. Rcpt. Line table.

Add the field to PAG5806.

Received Expected Costs Appearing on Expected Cost Worksheet After Completely Invoicing
---------------------------------------------------------------------------------------

Add some code to COD52102265 to filter the Expected Cost Received records that
are used to create worksheet lines. Only lines where the Posted Invoice
No. field is blank should be used.

LC3.5.3
=======

Disallow an Expected Cost Received record from being invoiced more than once
----------------------------------------------------------------------------

COD52102268 OnAfterInsertValueEntry, if the Value Entry has a Document No. that
is different to the Expected Cost Received.Posted Invoice No. (and that field is
not blank) then throw the following error message:

"Expected costs for %1 on %2 %3 %4 %5 have already been posted on invoice no.
%6" where:

%1 is the item charge no.

%2 is the field caption for "Receipt No."

%3 is the "Receipt No."

%4 is the field caption for "Receipt Line No."

%5 is the "Receipt Line No."

%6 is the "Posted Invoice No."

Update Expected Document Costs on Invoicing
-------------------------------------------

Create a new field in Expected Document Cost, "Actual Cost Posted (LCY)" (field
\#17).

Create a fields in Expected Cost Received, "Invoice Amount (LCY)" (field \#31)

Add code to COD52102268 OnAfterInsertValueEntry (this code is in COD22 itself in
the 2015 version) to:

Update the Invoiced Amount LCY field on the Expected Cost Received record that
has been retrieved.

Find the corresponding Expected Document Cost record:

Transaction Type = Payables

Document Type = Expected Cost Received.Document Type

Document No. = Expected Cost Received.Document No.

Document Line No. = Expected Cost Received.Document Line No.

Item Charge No. = Expected Cost Received Document.Item Charge No.

If an Expected Document Cost record can be found, update the Actual Cost Posted
(LCY) field with the value of the value entry that has just been created.

(Leave the existing code to also update the Open, Posted Invoice No. and Posted
Invoice Line No. fields).
