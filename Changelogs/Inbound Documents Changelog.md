IBD1.3.1
========

Support for Archived Versions of Inbound Documents
--------------------------------------------------

Create functionality similar to sales header archive. Automatically create an
archived version of an inbound document after it has been mapped (and after an
custom subscribers have had their hands on it).

Inbound Document Tracking
-------------------------

Create a new Inbound Document Tracking table linked to the Inbound Document Line
table which records the following information:

Lot No.

Serial No.

Quantity

Unit of Measure Code

Quantity (Base)

Expiration Date

IBD1.2.9.1
==========

[Bug Fix]Missing Upgrade Instructions
-------------------------------------

Could not upgrade the NAV extension 'Inbound Documents' due to the following
error: 'The following tables contain data but cannot be upgraded because the
relevant upgrade instructions are missing:

36 (Sales Header), 

38 (Purchase Header), 

110 (Sales Shipment Header), 

112 (Sales Invoice Header), 

114 (Sales Cr.Memo Header), 

120 (Purch. Rcpt. Header), 

122 (Purch. Inv. Header), 

124 (Purch. Cr. Memo Hdr.), 

6650 (Return Shipment Header), 

6660 (Return Receipt Header)

IBD1.3.0
========

Hide Default Config Action when Extension
-----------------------------------------

Hide the Default Configuration action on the Document Types page. Create a
function to determine whether the addition has been installed as an extension.

IBD1.2.9
========

Add Factboxes to Inbound Document Pages
---------------------------------------

Add the following factboxes:

Customer Statistics Factbox

Vendor Statistics Factbox

to the following pages:

Pending Inbound Document List & Card

Accepting Inbound Document List & Card

Rejected Inbound Document List & Card

The link to each page is from the Sell-to/Buy-from Account No. field.

IBD1.2.8
========

[Bug Fix]Extend Supplier ID length
----------------------------------

Extend the length to 20 characters

Consolidating Inbound Documents
-------------------------------

The user might need to be able to consolidate several inbound documents into a
single document i.e. copy the line details from one inbound document and insert
them onto a different inbound document.

The user will highlight one or more records on the pending inbound document list
page and choose a new action, Consolidate Documents.  The system should check
that the selection is valid, i.e.

All of the selected documents have the same inbound document type code

All of the selected documents have the same sell-to/buy-from account no.

All of the selected documents have the same bill-to/pay-to account no.

All of the selected documents have the same Document Type

The system should show a new list page only containing the records that have
been selected and show a message "Select the document to consolidate the lines
to." Show this message as a caption at the top of the list page rather than a
modal message box.

If the user selects a document (LookupOK) then the system should:

Throw an event OnBeforeConsolidateInboundDocuments with a var parameter of the
records that are included in the selection and a second parameter of the record
that the documents are being consolidated to

Copy inbound document line (incl. additional data) records from the selected
documents (apart from the document selected as the target) and insert them into
the target document.

Delete the selected inbound document records (apart from the record chosen as
the target).

Throw an event OnAfterConsolidateInboundDocuments with the same parameters as
the OnBefore event.

If the user does not select a document (LookupOK = false) then the system should
fail with a silent error (ERROR(''))

IBD1.2.7
========

Support for Default Configuraiton
---------------------------------

Add default configuration action to Inbound Document Types page.

IBD1.2.6
========

Optionally Release Documents on Acceptance
------------------------------------------

Add a new column to the Inbound Document Types list, Release on Accept (bool).
If this field is true then call the appropriate release codeunit after creating
the sales/purchase document.

IBD1.2.5
========

Changes to support EDI
----------------------

Changes to the Inbound Document-Accept Sales & Inbound Document-Accept Purch.

to support changes to existing Sales & Purchase header and lines.

to allow events to subscribe to the field assignments of header and line
acceptance (custom accept)

to allow insertion of comment Lines (removed the
InboundDocumentLine.TESTFIELD("No."))

Changes to the Map Data Exch. to Inbound Doc to set the Entry No. of the Data
Exch. to a new field on the Inbound Document Header, "Data Exch. Entry No.".

Added fields to the Inbound Document Type, Inbound Document Header & Inbound
Document Line tables and pages to support EDI

IBD1.2.4
========

Inbound Document Cues
---------------------

Create a new Inbound Document Activities page to show cue groups that take their
count from the Inbound Documents tables.

The cues should be configurable so that the user can set up different cues for
data that they are interested in e.g. one user might be interested in pending
web orders, another user might be interested in pending EDI orders for specific
customers.

We will have a cue setup table that allows these cues to be configured (with a
label to display) and the page to drilldown to.

IBD1.2.3.1
==========

[Bug Fix]DateTime Processed Field Not Populating
------------------------------------------------

This field ought to be populated with the datetime that it was accepted or
rejected. Note that if the document encounters an error and cannot be accepted
or rejected this field should not be updated.

IBD1.2.2
========

[Bug Fix]Inbound Document Line Field Values Carried from Previous Line
----------------------------------------------------------------------

COD9030274 - Map Data Exch. to Inbound Doc.

InsertEmptyInboundDocumentLine()

This function identifies the next LineNo to use with
InboundDocumentLine.FINDLAST. The same record is used to insert the new Inbound
Document Line record without INITing the record in between. This will result in
field values from the previous record that are not overwritten by the new line
being carried over.

IBD1.2.1
========

[Bug Fix]2016 - Pending Inbound Document List Page Actions - write transactions error
-------------------------------------------------------------------------------------

Accept & Reject.

Write transactions error when processing multiple documents,  COMMIT is
required.

IBD1.2.0
========

Generic Sales Accept Codeunit
-----------------------------

Change COD9030270 to set/validate the following fields:

Validate Ship-to Code if it isn't blank. Continue to assign the values of the
other ship-to address fields if they are not blank.

IBD1.1.9
========

Return Inbound Document No. from HandleData
-------------------------------------------

Investigate methods to determine the Inbound Document No(s) that have been
created from the data that has been passed to the inbound document type record.

Read the last no. in the table before inserting any, locking the table then
comparing with the last no. in the table afterwards?

IBD1.1.8
========

Populate Inbound Document Type on Inbound Document Header record
----------------------------------------------------------------

Set the SingleInstance property of COD9030275 to Yes.

Create a new global variable in the codeunit, InboundDocumentTypeCode. Set the
value of this global variable when the HandleData function is called. Set the
value of the variable to blank again at the end of the HandleData function.

Create a new function, GetInboundDocumentTypeCode to return the value of the
InboundDocumentTypeCode variable.

Add code in COD9030274.ProcessDataExchMapping. When a new Inbound Document
Header record is created, call GetInboundDocumentTypeCode and set the value of
the Inbound Document Type field.

[Bug Fix]Error is thrown when Inbound Document Type only has a Data Exchange Definition
---------------------------------------------------------------------------------------

If the Inbound Document Type does not have a Data Handling Codeunit or Data
Handling Xmlport defined an error message is thrown.

This is correct in 2015, but not in 2016. The error message should only be
thrown in 2016 if the Data Exchange Definition is also blank.

HandleData with TempBlob
------------------------

Inside the case statement of HandleData in COD9030275, DataVariant.ISRECORD,
test whether the DataVariant variant contains a TempBlob record. If it does,
make the Blob field of the InboundDocumenType record equal to the Blob field of
the TempBlob record.

If the variant contains a record of any other table, throw the following error:

"HandleData in the Inbound Document Data Handler codeunit should be called with
a TempBlob record".

IBD1.1.7
========

External System ID - Sales Header & Purchase Header
---------------------------------------------------

Add a new field, ID = 9030269 External System ID to sales header, purchase
header and all the corresponding posted and archive header tables.

This field should be available on list and card pages of those documents. Make
the field 'Additional' importance on the card pages.

Add code to COD9030270 and COD9030272 to set the value of the External System ID
field on the sales header/ purchase header table from its value in the Inbound
Document Header record.

IBD1.1.6
========

[Bug Fix]Return Reason Code Validation
--------------------------------------

The sales acceptance routine is not validating the return reason code on the
inbound document line. This is allowing return orders to be created with invalid
return reason codes.

Change the sales acceptance codeunit to validate that the return reason code (if
not blank) is valid. Throw an error if not.

Error Handling when Accepting a Single Document
-----------------------------------------------

Make changes to PAG9030270 and PAG9030271. If a single record is being accepted
or rejected and the corresponding tryfunction returns false then throw an error
with the result of GETLASTERRORTEXT.

External System ID
------------------

Include the External System ID field on the inbound document lists (pending,
accepted, rejected).
