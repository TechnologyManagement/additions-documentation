EC5.2.6
=======

Web Order Status
----------------

Change COD52102153.GetOrderStatus to return a status of Processing if the
inbound document has been accepted but the sales order / sales return order has
not been invoiced.

Web Order Status
----------------

Throw a new event from COD52102153.GetOrderStatus and 52102155.GetOrderStatus to
allow for custom subscribers to handle the order status calculation.

Inconsistent UOMs on Sales Orders
---------------------------------

Change the order acceptance routine to make sure that orders are created in base
UOM, rather than the sales UOM.

This bug might belong in IBD rather than EC, I'm not sure without investigating.

Category display order
----------------------

Support the ability to set display order of categories 

EC5.2.5
=======

[Bug Fix]Non-Existent Item in Pending Inbound Documents Causes Data Trans. to Fail
----------------------------------------------------------------------------------

Pending inbound document included a line for an item number that doesn't exist

Caused an error retrieving the details for that inbound document

Caused the whole data trans service to fail and show that there are no documents
to display in the My Account portal.

Can this be made more resilient?

[Bug Fix]Including Inbound Documents in Customer Transactions Service
---------------------------------------------------------------------

A temporary pair of sales header and sales line records are populated to return
to the web site when requesting transactions to display in MyAccount. Pending
Inbound Documents are (temporarily) accepted to create the sales header record.

This is causing a problem when bespoke (or Foodware) is present in the database
that is firing validation logic that is expecting the sales header and line
records to be real (not temporary) records.

Image Upload by Web Item No.
----------------------------

Change the image upload report to have a new option of "Web Item" distinct from
the existing Item option. Implement this option in the tables/codeunits that
this report calls through. Match the images to the correct items by their web
item numbers.

Need to give some thought to the Site Code. The user is prompted to enter the
site code on the report request page, but they could have an image for a web
item no. that is for a different site code to the one specified. Don't prompt
the user for the site code if they are using web item nos.? Throw an error or
ignore images that are for a different site? The first of those two is probably
preferable.

Need to make sure that the user also has some sensible way of seeing the web
item no. for a given item actually is. Is this column on the Web Site Items
page? If not, add it (uneditable).

EC5.2.4.1
=========

[Bug Fix]Display History Period
-------------------------------

This setting should determine the period of transactions that are returned to
the website (see Customer Trans. xmlport). If the dateformula does not begin
with '-' (minus sign) then put one at the start so the dateformula is subtracted
from today's date not added.

[Bug Fix]Add Sell-to Customer No. to TempSalesHeader When Calculating Sales Prices
----------------------------------------------------------------------------------

We've come across a couple of instances where some bespoke is trying to retrieve
the customer no. from the Sales Header.Sell-to Customer No. and is falling over
because we're not populating it.

Populate that field in the temporary sales header that is used to calculate the
sales prices in Sales Price Calc. Mgt.

EC5.2.4
=======

[Bug Fix]Unit Price Calculation in Sales UOM with Different Qty. per Unit of Measure
------------------------------------------------------------------------------------

CalcByUOM used incorrectly in eCommerce Mgt.

EC5.2.3.3
=========

[Bug Fix]Item Quantity Breaks/Prices are Calculated from the Base UOM not the Selling UOM
-----------------------------------------------------------------------------------------

Item quantity breaks should use the Selling UOM from the item rather than its
Base UOM. This is the UOM that will be used by standard functionality when an
order line is created.

[Bug Fix]Item Quantity Breaks/Prices are Calculated from the Base UOM not the Selling UOM
-----------------------------------------------------------------------------------------

Item quantity breaks should use the Selling UOM from the item rather than its
Base UOM. This is the UOM that will be used by standard functionality when an
order line is created.

EC5.2.3.2
=========

[Bug Fix]Remove Item.Last DateTime Modified
-------------------------------------------

XML52102154 - use Web Site Item.Last DataTime Modified for filtering blocked
items.

Delete Item.Last DateTime Modified and remove references to the field.

EC5.2.3.1
=========

[Bug Fix]Find Best Web Price customization only fired out per Min Qty
---------------------------------------------------------------------

COD52102149.FindBestWebPrices:

WebTradingPriceBuf is sorted by Minimum Quantity therefore calling
the customization from within the group will not permit customization for each
item.

Loop through WebTradingPriceBuf at the end of the function to allow
customization per item.

EC5.2.3
=======

[Bug Fix]Find Best Web Price customization
------------------------------------------

WebTradingPriceBuf is sorted by Minimum Quantity, if a customization changes the
min qty it could potentially cause issues.

Insert the best prices onto a new WebTradingPriceBuf record and perform the
customization, copy the new WebTradingPriceBuf into the original at the end of
the function.

[Bug Fix]Changing Item Attributes Doesn't Update the Last Date Modified
-----------------------------------------------------------------------

When attribute entries are inserted/modified/deleted the related items (or items
which belong to the related categories) should be updated with the current
datetime as their Last DateTime Modified.

This works using events in 2016+. There is nothing to call the appropriate
functions in eCommerce Mgt. codeunit in 2015. Add a call to
eCommerceMgt.AttributeEntryOn... functions from the OnDatabase... in codeunit 1.

The same bug is true of associated items. Adding or removing as associated item
should update the related item, but there is nothing to call the handling
functions in ecommerce Mgt.

Version: 2015

Web Trading Item Changes Includes Non-Web Site Items
----------------------------------------------------

Add some code to the items\_blocked dataitem of the xmlport. For each item
record check if there is a corresponding Web Site Item record. If not, skip the
record and do not pass it back to the website.

Remove obsolete fields from Web Site Card/Table
-----------------------------------------------

The following fields are no longer required in the web site table. Remove them
and references to them on pages and xmlports (leave the nodes in the xmlports
but pass a blank value - otherwise the website will fail to read the web
service).

Default VAT Rate

Default Location Code

Default Shipping Agent

Default Shipping Service

Default VAT Prod. Posting Group

Sage Journal Template Name

Sage Journal Batch Name

Sage Receipting Bank Acct

Activate Sage Pay

Sage Pay Vendor

Sage Pay Live URL

Sage Pay Test URL

Description on Web Item Specification Detail
--------------------------------------------

The text displayed in the Web Item Specification Detail is concatenated without
spaces. Add spaces between the description fields in the GetTextContent
function.

Modify XML52102154 (Web Trading Item changes) to also add spaces between the Web
Item Specification description

EC5.2.2
=======

Customization Events for Return Orders
--------------------------------------

We already have some customization events for orders and now require the same
functionality for return orders.

AfterImportReturnOrder (to be called from Web Return Order Mgt.)

AfterWebReturnOrderAccept (to be called from Web Return - Accept).

FindBestWebPrice Customization Event
------------------------------------

We have a requirement to report some quantity breaks for prices as 1,000 times
the quantity that is stored in NAV (because they sell some items in eaches but
enter them into NAV as if they are thousands i.e. customer orders 1,000  and the
sales order is entered as a quantity of 1).

If there are quantity breaks at 1 and 3 these need to be reported to the web
site as breaks at 1,000 and 3,000.

Add a new customization event in COD52102149.FindBestWebPrices. Pass the
following to the customization:

Site Code,

Item No.

Sales Type

Sales Code

Currency Code

Variant Code

Unit of Measure Code

VAR Minimum Quantity

VAR Unit Price

If the customization returns a minimum quantity or unit price that is different
to the values already in the WebTradingPriceBuf record, update the record with
those values.

EC5.2.1.1
=========

[Bug Fix]Validating Category Entry No. on Web Item Category
-----------------------------------------------------------

Few bugs to do with selecting/changing the web site categories that an item is
assigned to.

EC5.2.1
=======

Customization Events COD52102153
--------------------------------

Call out to eCommerce Customization Mgt. from COD52102153 at the end of
ImportOrder (called AfterImportOrder) to give the opportunity for bespoke code
to be executed once an order has been imported. This is pretty redundant in 2016
as there is already an event thrown by the inbound document codeunit, but this
is useful for EC in 2015. 

Populate Web Order field on Accepting Web Order
-----------------------------------------------

COD52102156 (Web Order - Accept) is responsible for creating sales orders from
inbound documents. The Once the order has been completed the Web Order field
should be set to true.

Once the Inbound Document-Accept Sales codeunit has been called the Inbound
Document Header.No. field should contain the number of the Sales Header record
that was created.

AfterWebOrderAccept Customization
---------------------------------

Add a call to the eCommerce Customization Mgt. codeunit from COD52102156 after
the order has been processed (and the Web Order field been updated as per
\#2119), AfterWebOrderAccept.

This will give the opportunity for customisation to be called after the order
has been accepted e.g. populating bespoke fields on the Sales Header record.

EC5.2.0
=======

Record Web Image Deletions
--------------------------

COD52102144.get\_changed\_images retrieves details of image records that have
changed since a given datetime. This service includes details of images that
have been deleted since the last date time (the Deleted boolean should be set to
true).

In practice this is not working because any web image records that have been
deleted are actually deleted, so they will never be in the database to be
included in the xmlport.

Add code to Web Image.OnDelete to populate a record in the Web Deleted Record
table:

Entry No. (automatically populated OnInsert)

Table ID = table id of the Web Image table

No. = Web Image.Entry No. (formatted to a code)

DateTime Deleted = current date time.

Change XML52102157 to export data from a temporary table. This table should be
populated from

Web Image records that have been changed since the datetme passed into the
changes\_since\_datetime parameter.

Web Deleted Record records where the DateTime Deleted is more recent than the
datetime passed into the changes\_since\_datetime parameter.

Evaluate the No. of the Web Deleted Record into the Web Image.ID field.

This should give the website the details that it needs to remove the images. The
website will also make a call to purge the deleted images
(purge\_deleted\_images). This won't do anything because the images have already
been deleted, but that's ok.

EC5.1.9
=======

[Bug Fix]Web Site Item Synchronisation
--------------------------------------

The Web Site Item table has some functions to indicate whether each record has
been synchronised with the web site. A record is created in the Web Record to
Sync. table to indicate that the record is out sync between the two systems.

XML52102154 is called by the website to retrieve the latest details from the web
site item table. The SetSynchronized function is called OnAfterGetRecord of
items\_current to indicate that the record has been read. The same function
should also be called from the other Web Site Item dataitems
(items\_never\_show, items\_before\_start\_date & items\_after\_end\_date).
Without this function call web site items that fall outside the filters of
item\_current are never being updated as having been synchronised.

[Bug Fix]Web Deleted Record.INSERT(TRUE)
----------------------------------------

Web Deleted Record has a primary key of Entry No. which is populated by the
OnInsert trigger. Ensure that wherever records are inserted into this table that
INSERT(TRUE) is used.

EC5.1.7
=======

Assigning Web Item No.
----------------------

Move the assignment of Item.Web Item No. from Item - OnInsert to the creation of
the Web Site Item no. instead.

EC5.1.6.1
=========

[Bug Fix]Remove Web Item Specification Card Action
--------------------------------------------------

Remove the web item specification card action from the web item specification
list.

EC5.1.6
=======

Retrieving Customer Transactions from Web User ID
-------------------------------------------------

Some customers may have a unique account on the website but not have a
corresponding customer account in NAV. This may be because they are a new
customer and the account has not been created in NAV yet (it is created when the
first pending inbound document is accepted) or because eCommerce is not
configured to create customers (all sales go through a cash sales account).

The MyAccount section of the website allows users to view their transactions
(pending orders, posted invoices, credits, shipments etc.) These transactions
are not shown if the user does not have a customer account in NAV.

We ought to have enough information from the Web User ID (unique identifier
given to the account by the website) to build this transaction history without
the customer no.

Item Availability - Account for Qty. on Pending Inbound Documents
-----------------------------------------------------------------

COD52102149.CalcItemAvailability handles the calculation of available inventory
using the AvailableManagement codeunit.

AvailableManagement.ExpectedQtyOnHand takes an ExtraNetNeed parameter to reduce
the available figure by.

Create a new function in eCommerce Mgt. to calculate the quantity (base) of the
item on Inbound Document Line records where the Inbound Document Status is
Pending.

The Quantity (Base) field will probably not be populated when the order is still
pending. Calculate the quantity (base) from the Quantity and the Unit of Measure
Code (if the Unit of Measure Code field is not populated take the Sales Unit of
Measure from the Item card instead).

GetItemPrices Behaviour for Items that Cannot be Found
------------------------------------------------------

Currently if a web item no. is passed to COD52102149.GetItemPrices for which a
corresponding item record cannot be found the function is EXITed without any
return value.

This means that if one item doesn't exist in a call for the prices of ten items
nothing is returned.

Change the function to skip the SalesPriceBuf record is an item cannot be found
rather than exiting.

Return Order Control for Invoices
---------------------------------

Add a new tag return\_created at the bottom of the customer\_transaction group.
Take the value of this node from the Sales Header.Ship field. (the Sales Header
table is temporary and Ship is a convenient boolean field to use).

Make a change to the CreateSalesInfoFromPostedSalesInvoiceInfo function to this
field to true if a return order has been created from the current invoice. You
will need to check:

Pending or Rejected Inbound Document records

The records will have a corresponding Additional Data record with a Field Name
of 'InvoiceNo' and a Field Value matching the invoice no.

Sales Return Orders

The Sales Header record will have an Applies-to Doc. Type of Invoice and an
Applies-to Doc. No. matching the invoice no.

Posted Sales Cr. Memos.

The Applies-to Doc. Type and Doc. No. fields will be populated with matching
information.

If any of the above records can be found for a given sales invoice, populate the
Ship field of the temporary Sales Header record in the xmlport with true. This
will indicate to the website that a return order has been created from the
invoice and that the user should not be permitted to create another.

[Bug Fix]Email Address Not Populated on Contacts Created by UserRegistration function
-------------------------------------------------------------------------------------

COD52102149.UserRegistration missing the necessary code.

EC5.1.4
=======

[Bug Fix]Sales Invoice / Cr. Memo Report Not Filtered When Requested by Site
----------------------------------------------------------------------------

PDF that is downloaded from the website contains all invoices or credit memos
instead of the single document that has been requested to the web service.

[Bug Fix]Web Image ID Calculation
---------------------------------

When a new web image record is added sometimes an existing ID is being assigned.
The ID column should be unique.

[Bug Fix]Web Site Items List, No. of Attributes Column
------------------------------------------------------

The No. of Attributes flowifleld in the table is actually calculating the no. of
associated items. Change the caption of the field to match. Add a new column
that shows the result of a new function in the Web Site Item table that will
calculate the no. of attributes that correspond to the item.

EC5.1.2
=======

GetItemsHidden
--------------

Using the existing capability to hide items when the pricing and availability is
calculated, this service determines which item(s) should be hidden, if any, when
a page of items is loaded.

Batching this logic at the page level provides better performance than calling
item by item.

EC5.1.3
=======

Statement Request Page
----------------------

Support to configure the options on the request page of the statement report
that is printed when requested by the website.

Contact/Customer Creation Process
---------------------------------

Create a new public function in the Web Trading Services codeunit,
UserRegistration to accept the following parameters:

Email Address - (used as username)

Title

First Name

Last Name

Telephone

Mobile

Company Name

Address - (lines 1-3)

City

County

Country

PostCode

WebUserID

Return Code20 from the function.

Pass the same parameters to a new function of the same name in the eCommerce
Mgt. codeunit.

Create a new company contact using the name and address details passed to the
function.

Populate the Account No. field in the Contact table with the next available
number from the Customer No. Series identified in the Sales & Receivables Setup
table.

Populate the Web User ID field with the value of the WebUserID parameter.

Create a new person contact associated with the new company contact with the
Title, First Name and Last Name details passed to the function

Return the value of the Account No. field (populated above) from the function.

Customer Creation - Web Order - Accept
--------------------------------------

Add a new field to the Web Site and eCommerce Setup tables, Customer Template.
The field should have the same ID in both tables. Add the field to the Web Site
Card and eCommerce Setup tables.

The field should be validated against the Customer Template table.

Create a new function in the eCommerce Setup table to retrieve the value of this
field from the Web Site table, or if that is blank, from the eCommerce Setup
table (see the existing functions for an example).

Remove AddVat Function from Web Trading Services
------------------------------------------------

This function is not required and should be removed.

EC5.1.2
=======

Routine to Export/Import eCommerce Data
---------------------------------------

Support for exporting the eCommerce configuration from one company and importing
into another.

[Bug Fix]Error creating records on Front Page Items Page
--------------------------------------------------------

Record already exists error when creating new records on this page.

Remove AutoIncrement Property
-----------------------------

AutoIncrement is used on the following tables:

Stock Availability Line

Web Image

Web Deleted Record

Web Image Record

Remove the AutoIncrement property where it is used and add code to set the value
of those fields instead. Add a key on those fields to ensure good performance.

EC5.1.1
=======

Sell-to Contact No. on Inbound Document Header
----------------------------------------------

COD52102156 has some code to find the Contact record that corresponds to the
data passed from the website and to create a new contact if one cannot be found.

In either case, the Sell-to Contact No. of the Inbound Document Header should be
updated with the no. of the contact that has been found/created. Add some code
to the OnBeforeAcceptInboundDocument function to do this.

Hide Items on Website Option on Stock Availability Lines
--------------------------------------------------------

A new field in the dataset that is returned with pricing and availability web
service calls can indicate to the website that the item should be hidden.

This field is never populated as standard, but can be populated with a bespoke
subscription to an event if the customer has some custom logic for hiding
certain items in given situations.

EC5.1.0
=======

Data\_Customer\_Trans to Include Pending Inbound Documents
----------------------------------------------------------

Pending Inbound Documents are now included in the customer transactions that are
displayed in MyAccount. Previously only documents that had been accepted, and
therefore had created Sales Order / Posted Sales Invoice records were included.

Web Trading Changed Images, Include Filename
--------------------------------------------

Include a new node in XMl52102157 nav\_image\_file\_extension.

This node will be based on a variable which takes its value as the file
extension from the Original Filename field of the Web Image table.

Set MaxOccurs to Once for this node.

Web Trading Changed Images
--------------------------

Add a new node beneath the nav\_table\_id node, nav\_table\_name. This node will
take its value from a variable rather than a field. The variable should have the
value of the table name (not caption) corresponding to the table identified by
the Web Image.Table ID field. Set the MaxOccurs of this variable to Once.

EC5.0.9
=======

[Bug Fix]Duplicate Prices Returned for Customer/Item Price Call
---------------------------------------------------------------

The web site calls to NAV when item pages are loaded to retrieve the correct
quantity breaks, price and stock level for the item. Under some circumstances a
duplicate price is being returned.

[Bug Fix]XML52102166 Preventing Web Service WSDL from Loading
-------------------------------------------------------------

Additional Charges
------------------

The ability to add “Additional Charges” to a website order before it is
confirmed and paid on the site. For example, the customer may want to charge a
handling fee or a minimum order charge in certain situations.

No charges are added by default, but integration events in eCommerce
Customization Mgt. codeunit allow this flexibility.

Customer Images
---------------

Support for associating images with customers e.g. customer logos and passing to
the site. The site can then display these images in the MyAccount area of the
site.

Don't Pass All Levels of Web Site Categories in Item Changes xmlport
--------------------------------------------------------------------

Currently the Web Trading Item Changes xmlport includes the web site categories
for each item along with all their parent categories. This is no longer required
by the website.

EC5.0.8
=======

Process Invoice Payments
------------------------

Support for taking payments for posted transactions from the MyAccount area of
the site. The website user can see their list of outstanding invoices, tick the
one(s) that they want to pay and be directed to the payment provider for the
payment to be taken.

EC5.0.7
=======

PrintReport Web Service
-----------------------

Support for printing invoices, credit memos and statements from the website. A
request is made to the NAV web service to print the appropriate NAV report, save
it as PDF and return the document to the website to be downloaded by the website
user.

Payment Information on Customer Transaction xmlport
---------------------------------------------------

NAV now provides payment information about the customer transactions that are
returned to the site to be shown on the MyAccount area of the site.

Querying Customer Transactions
------------------------------

The website user can now query transactions from the MyAccount area of the site.
They view their list of transactions, view the details of a given invoice and
can flag it as being queried, with a free type message.

GetReturnReasons Web Service
----------------------------

Support for passing valid return reason codes to the website. These are used to
process invoice returns from the MyAccount area of the site.
