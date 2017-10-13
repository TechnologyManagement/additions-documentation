
SPM1.1.0.2
========

Shipment Date field renamed
----------------------------------------------

SPM1.1.0.1
========

Incorrect Filter on Route Selection When Moving Lines
----------------------------------------------

The Shipment Date filter that is applied is &gt;[Shipment Date] rather than &gt;=[Shipment Date] meaning that routes which are due to leave on the same day cannot be selected.

Shipment Management Group 0 Does Not Exist
----------------------------------------------

<ul><li>Multiple Shipment Mgt. Groups exist for the same delivery area code / delivery schedule</li><li>Attempt to add a new sales line that would be added to one of those groups</li><li>&quot;Shipment Management Group 0 does not exist&quot; error is displayed.</li></ul>


SPM1.1
========

Include Purchase Orders to Update Existing documents
----------------------------------------------

The Update Existing Documents action on the Shipment Management page only caters for Sales Lines at the moment. Modify to update Purchase Line too.


Support for Additional Document Types
----------------------------------------------

Currently we only support sales order lines on the Shipment Management page to plan shipments of orders that are going to be made.<div><br></div><div>We need to expand this to include support for the following document types:</div><div><ul><li>Sales Return Orders</li><li>Purchase Orders</li><li>Purchase Return Orders</li></ul><div>Extend the existing logic for sales orders that populates the Shpt. Mgt. Post Code / Shpt. Mgt. Country/Region Code and related line-level fields to work with sales return orders.</div></div><div><br></div><div>Mirror the functionality on Purchase Header and Purchase Lines.</div><div><br></div><div>The intention is that the user will be able to manage collections as well as deliveries using Shipment Mgt. For now we will not attempt the differentiate between deliveries and collections (for example, we might say that collections should always go at the end of a Shpt. Mgt. Group - but that is out of scope for now).</div>

Recalc. after Moving Lines
----------------------------------------------

The result of the Move Lines action isn't apparent until the Shipment Management page has been recalculated.&nbsp;

Renaming Delivery Area Line error
----------------------------------------------

<p><br></p>


SPM1.0.7
========

Delivery Area Lines to Include Destination No.
----------------------------------------------

Eastbrook supply bathroom showrooms, which are often in the same part of town
and therefore often have the same post code. As this is the case, there is no
way for the Delivery Area lines to define a particular delivery order for
customers at that post code - although in real life there may be a particular
order that it is important to work in.

We will add new Destination Type and Destination No. columns to the Delivery
Area lines to allow the user to define the correct order in which to visit
different destinations at the same post code.

Shipment Mgt. Group Shipment Bin Behaviour
------------------------------------------

Optionally populate the Shipment Bin on new Shipment Mgt. Group records with the
Shipment Bin from the corresponding Location card.

Do not allow Warehouse Shipments to be created for a given Shipment Mgt. Group
until the Shipment Bin has been populated.

Printing Shipment Management Group Paperwork
--------------------------------------------

Currently we have the option to print a manifest report from the shipment
management page. This should be changed to "Print Documentation". We will have
some setup to determine which reports need to be printed.

Add a new action to the Shipment Management Setup page. This will display a list
page of reports that are to be printed when the Print Documentation action is
run.

The list page will allow the user to enter one or more lines of reports to be
printed.

Shipment Management Page Improvements
-------------------------------------

With the number of columns that are included the user has to scroll to the right
in order to see the destination details of each drop.

Include a code on in the Delivery Area Code column to indicate the destination.
This could be a concatenation of the destination no. and the post code e.g.
10000/WV3 0QH - the length of the delivery area code field might need to be
increased to ensure that there are no overflows (look at the max. length of
destination no. and post code +1).

Change the quantity (base) column to only display decimal places if there are
some to display.

[Bug Fix]Updating Ship-to Post Code on Sales Orders
---------------------------------------------------

Updating the Ship-to Post Code on a sales header ought to update the Post Code
on the corresponding Shipment Management Entry records.

As this doesn't update properly the Shipment Management page is incorrect the
next time the user looks at it

Optionally Highlight Auto-Placed Lines
--------------------------------------

Eastbrook want their transport planner to manually review the placement of
orders in the Shipment Mgt. Group that do not have a specific delivery area line
that matches the destination no.

To help them do this we will highlight those drops on the Shipment Management
page that do have a corresponding delivery area line.

This behaviour should be optional and not enabled by default.

Don't Delete Shipment Mgt. Group if Sales Shipment Lines Exist
--------------------------------------------------------------

Shipment Mgt. Group records are deleted when there are no longer any Sales Line
records existing for that group.

This means that the group is deleted from the Shipment Management page when all
of its whse. shipments/sales orders have been posted.

We want these groups to remain on the Shipment Management group so that the user
is able to print the documentation after all the source documents have been
posted.

Shipment Manifest Report to Run off Sales Shipment Lines
--------------------------------------------------------

Currently the manifest report runs off Shipment Management Entry records. Once
sales orders have been posted, these entries will no longer exist. Instead the
report should read Sales Shipment Line records that are related to the Shipment
Mgt. Group that is being printed.

SPM1.0.6
========

Initialise Shipment Management Fields for Existing Documents
------------------------------------------------------------

When the Shipment Management addition is first installed any existing documents
will not be included on the Shipment Management page.

We need a routine that can be run (probably from an action on the Shipment
Management Setup page) to initialise the Shipment Management fields of existing
documents.

[Bug Fix]Moving drop to a same route doesnt create a new instance of route
--------------------------------------------------------------------------

Using the 'Move Drop' action on Shpt Mgt. page should move a drop for a customer
to a same route, but increment the instance no.

Get the following error:

Need to delete the shpt entry first, and use a copy to create the new
one..with a higher instance 

Move Lines
----------

Moves sales lines between drops.

SPM1.0.5
========

Ability to Sort Pick Lines by Delivery Order
--------------------------------------------

It would be nice to have the ability to specify a new sorting order for the pick
lines. We want to optionally sort the pick lines by the delivery order of their
corresponding sales document in Shipment Management.

Assign Shipment Bin to Shipment Mgt. Group
------------------------------------------

Orders that are being picked to be included on a given Shipment Mgt. Group (aka
Route) are normally physically grouped together in the warehouse so that they
can be loaded onto the vehicle.

The user may want to group the pick lines into a specific bin in the warehouse
that represents a loading bay. The Bin Code on the warehouse shipment header
should be set to this bin which in turn will set the take lines on the warehouse
pick.

[Bug Fix]Reservations Mandatory
-------------------------------

The "Reservations Mandatory" option on Shipment Management setup dictates that
only orders which have been reserved should appear on the Shipment Management
page.

Currently any old reservation is being taken into account (via the Reserved
Quantity flowfield). This should be changed to only consider reservations to
ILEs.

[Bug Fix]Creating Consolidated Whse. Shipments Across Groups
------------------------------------------------------------

The Create Consolidate Whse. Shpt. action on the Shipment Management page allows
the user to select multiple entries and create a single warehouse shipment for
those entries. Works like a dream.

Except.

You shouldn't be able to create a consolidated warehouse shipment for entries
that belong to different shipment mgt. groups. Check that all the highlighted
lines belong to the same group before creating the shipment.

Populate Fields on Warehouse Shipment Header
--------------------------------------------

Add new fields to the Warehouse Shipment Header:

Delivery Area Code

Shipment Mgt. Group No.

When the warehouse shipment is created from the Shipment Management page
populate this fields.

Also populate the following standard fields with the fields from the
corresponding shipment mgt. group:

Shipping Agent

Shipping Agent Service

Show Sales Line Amount on Shipment Management
---------------------------------------------

Include a new Amount (LCY) column on the Shipment Management page. Take this
from the Outstanding Amount (LCY) of the sales line. Subtotal this field at each
level of grouping and as a total at the footer of the page.

Vehicles & Drivers
------------------

We want to be able to record which driver and vehicle is delivering a certain
route. Create two new tables to hold this master data, Shipment Mgt. Vehicle &
Shipment Mgt. Driver

The vehicle and driver details should be able to have default values pulled from
the Delivery Area / Delivery Area Schedule and should also be able to have their
values set from the Shipment Management page.

Rework how the Delivery Order is assigned
-----------------------------------------

Currently the delivery order on the Shipment Management Entry table is set
without taking into consideration order it appears in in Delivery Areas set up.

SPM1.0.5
========

Default Pick Sorting Order
--------------------------

Support the ability to specific a default sorting order for the lines on picks
as they are created.

SPM1.0.4.1
==========

[Bug Fix]Attempt to Modify Line Before it Exists
------------------------------------------------

Shipment Management performs several calculations when sales lines are inserted
or their fields validated before MODIFYing the line. Clearly, the MODIFY should
not be called before the record actually exists in the database.

The Line No. = 0 check is normally sufficient to catch this, but sometimes the
Line No. might be populated even before the line exists. Add a check to the
Shipment Management functions that are called from subscribers to avoid the
MODIFY in this case.

SPM1.0.4
========

Option to Create a Consolidated Whse. Shipment
----------------------------------------------

Add a new action to the Shipment Management page.

Create supporting functionality in Shipment Mgt. Buffer codeunit to create
single warehouse shipment header and call get source documents with each of the
source documents to add their item lines to that shipment header.

[Bug Fix]Customer.Delivery Area Code not Respected
--------------------------------------------------

If the Delivery Area Code on the customer card has been populated, then sales
lines for that (sell-to) customer should inherit it and not attempt to calculate
the delivery area from the post code and country/region code.

If a delivery area code has been set on the Ship-to Address (if one exists) then
that should take precedence.

[Bug Fix]FilterDeliveryAreaLineForPostCode
------------------------------------------

If FilterDeliveryAreaLineForPostCode is called with a blank post code it should
error without inserting any TempDeliveryAreaLine records.

[Bug Fix]Delivery Area Calc. with Blank Post Code
-------------------------------------------------

Error attempting to split the post code into elements when it is blank.
COD52102435.GetDeliveryAreaForPostCodeCountryRegionCodeAndLocation should not
attempt to calculate a delivery area code if the post code is blank, just assign
the default delivery area code from Shipment Management Setup.

SPM1.0.3
========

Shipment Management Manifest
----------------------------

Create a manifest report which is available from the Shipment Management page.
See MW for design.

[Bug Fix]Save Delivery Order - Check for delivery area line with postcode from and to
-------------------------------------------------------------------------------------

COD52102435.CheckPostCodeExistsInDeliveryArea() - check for a deliveryarealine
with the postcode as both From and To

SPM1.0.2
========

Multiple Instances of Shipment Management Groups
------------------------------------------------

A Shipment Management Group is currently defined as each unique combination of:

Delivery Area Code

Delivery Schedule ID

Location Code

Shipping Agent Code

Shipping Agent Service Code

Shipment Date

although this isn't the primary key of the table, see
COD52102436.ShipmentMgtGroupCheckExist.

We need to support multiple groups having the same combination of those fields
e.g. if a group contains more drops that can be fulfilled by a single vehicle
the transport planner might want to split the route into two. (Note, Shipment
Management Group is also referred to as a "route").

Add an "Instance No." integer field to the Shipment Management Group table. This
field will distinguish between groups that otherwise have the same combination
of fields. When a group is inserted (COD52102436.ShipmentMgtGroupInsert), if
there is no other group with the same combination of these fields then Instance
No. should be set to 1, if there is already one or more groups with the same
combination then set the Instance No. to be 1 greater. Add the Instance No.
field to the Shipment Management page to show the user the difference between
the two groups.

When sales lines are scheduled (CalcScheduleForSalesLine) there may be more than
one group that the sales line could be added to. Decide which group to add the
sales line to as follows:

Find the shipment management group(s) that the sales line could be assigned to.

If there are no valid groups then create a new one

If there is a single valid group, add the sales line to that group

If there are multiple valid groups:

Find the first group that has already been assigned to another sales line on the
same source document

Otherwise, find the first group that has already been assigned to another sales
line where the sales header matches the following fields from the sales header
of the sales line that is being schedule

Sell-to Customer No.

Ship-to Code

Shipment Mgt. Post Code

Otherwise assign the sales line to the shipment mgt. group with the highest
instance no. (which assumes that if multiple instances of the same group exist
it is because everything except the last is considered to be full).

Document Counts on Shipment Management Page
-------------------------------------------

Add a couple of columns to the Shipment Management page, only to be populated at
the top level of grouping:

No. of Shipments - the count of warehouse shipments that exist for this group

No. of Picks - the count of picks that exist for this group - note that this
could be either warehouse picks or inventory picks.

This will be useful for the user when they are registering picks and posting
shipments. it is difficult to trap posting errors and present them back to the
user - unless they use the "Stop and show first error" policy in Warehouse
Setup. These counts will be a visual indicator that there are still documents
outstanding for this group.

Ability to Move Drops Between Routes
------------------------------------

Add a new action to the Shipment Management page to allow the user to move a
drop between different routes. Create an action, Move Drop.

This action should open a page with the following options:

Existing Route (checkbox)\*\*

Route - allows the user to select from a Shipment Management Group (filtered to
the same location) - only enabled when Existing Route is selected

New Route (checkbox)\*\*

Delivery Area\* - only enabled when the New Route option is selected

Shipping Agent\* - only enabled when the New Route option is selected

Shipping Agent Service\* - only enabled when the New Route option is selected

Shipment Date\* - only enabled when the New Route option is selected

\*these fields allow the user to specify the field value for new Shipment
Management Group that will be created. The Location Code for the new group
should match the Location Code of the group that the drop is being moved from.

\*\*these options are exclusive. Existing Route should be selected by default.
If New Route is selected then deselect the Existing Route option. 

Updating Delivery Area Definition From Shipment Mgt. Entries That are Out of Delivery Order
-------------------------------------------------------------------------------------------

The Delivery Area is built up of post code areas in a specified order. When
drops are added to the Shipment Management page they are added to the groups in
the same order as the delivery area lines.

There might be a reason to visit certain addresses out of the usual delivery
order e.g. if a customer insists that they must be the first delivery of the
day.

Add an action to the Shipment Management page, Save Delivery Order. This action
will open a new page with the following controls:

Delivery Area Code (non-editable)

Post Code - copy from the post code of the current record, but this can be
edited by the user

"Deliver to this Post Code" (option, First,Last,Before Selected,After Selected)

Subform showing the current delivery area lines relating to the delivery area

If the user clicks OK on the page, create a new delivery area line in the
corresponding delivery area. The from post code and to post code should both be
populated with the post code entered on the page (throw an error if the post
code is blank).

Set the Delivery Order value appropriately depending on the "Deliver to this
Post Code" option ("selected" refers to the line that was highlighted on the
delivery area lines subform when the user clicked OK).

Shipment Mgt. Fields on Sales Subforms
--------------------------------------

Add the following fields to the sales order subform and sales return order
subform:

Delivery Area Code

Shipment Mgt. Group No.

SPM1.0.1
========

Create Picks from Shipment Management Page
------------------------------------------

Add a new action to the Shipment Management page to allow users to create picks
for the selected order(s). This action should create warehouse picks or
inventory picks as appropriate according to the setup of the current location
code. If no location code is selected then throw an error asking the user to
select one.

Disable Create Warehouse Shipments Action when Not Appropriate
--------------------------------------------------------------

If the currently selected location does not support the creation of warehouse
shipments then the "Create Warehouse Shipments" action should be disabled - as a
visual indicator that the user does not need to use it.

Register Picks Action
---------------------

Add a new action to the Shipment Management page to register picks that exist
for the selected line(s). Depending on the setup of the location these may be
either inventory picks or warehouse picks.

Post Warehouse Shipments Action
-------------------------------

Add a new action to the Shipment Management page to Post Warehouse Shipments.
This action should determine the warehouse shipments that exist for the selected
line(s) and post them. This action should be disabled if the currently selected
location does not support the use of warehouse shipments.

Shipping Tab Fields on Warehouse Shipment
-----------------------------------------

The Shipping tab of the Warehouse Shipment page has fields which get written
back to the source documents that are on the warehouse shipment:

Shipment Date

Shipping Agent Code

Shipping Agent Service Code

Shipment Method Code

See InitSourceDocumentLines and InitSourceDocumentHeader in COD5763.

The first three of these fields determine which Shipment Management Group each
sales line falls into. It is possible that a Shipping Agent / Shipping Agent
Service Code will be assigned when the sales order is created but the warehouse
staff will decide that is needs to go onto another shipping agent / shipping
agent service and update the fields on the warehouse shipment. It is also
possible that the warehouse staff decide that the shipment date needs to change.

When the shipping fields are updated on the warehouse shipment the shipment
management group of the source lines that are included on the shipment should be
recalculated - this will give instant visibility on the Shipment Management
screen of which sales orders and lines will go on each shipment management
group.

Shipment Management Group keys
------------------------------

The primary key of this table is Entry No.

We need to have one or more additional key(s) to search this table, otherwise as
this table grows performance is going to stink. Check the filters and FINDSETs
that are used with this table and create appropriate keys e.g.
ShipmentMgtGroupCheckExist in COD52102436.

NewDeliveryAreaLineIsCloser
---------------------------

NewDeliveryAreaLineIsCloser - Exit with false if both From and To postcodes are
same.

[Bug Fix]Assigning Shipment Mgt. Group No.
------------------------------------------

Rollback workitem 1956, TAB52102441.GetNextEntryNo

SPM1.0.0.4
==========

[Bug Fix]Shipment Management Factbox - Statistics
-------------------------------------------------

TAB52102441 - Shipment Management Group

Exit GetVolume, GetNetWeight & GetGrossWeight if the entry no is 0, else exit
with tallied total.

SPM1.0.0.3
==========

[Bug Fix]Record does not exist error after Ship & Invoice
---------------------------------------------------------

Sales Header does not exist error after posting.

Put a IF around SalesHeader.GET on "Shipment Management Buffer".GetStatus

SPM1.0.0.2
==========

[Bug Fix]Missing Upgrade Instructions
-------------------------------------

110 (Sales Shipment Header)

111 (Sales Shipment Line)

112 (Sales Invoice Header),

113 (Sales Invoice Line)

114 (Sales Cr.Memo Header)

115 (Sales Cr.Memo Line)

6660 (Return Receipt Header)

6661 (Return Receipt Line)

SPM1.0.0.1
==========

[Bug Fix]Creating Whse. Shipments from Ship Mgt. Group on Drop Line
-------------------------------------------------------------------

[Bug Fix]Assigning Shipment Mgt. Group No.
------------------------------------------

It is possible for the Sales Line table to have records with a Shipment Mgt.
Group No. that no longer exists. This could result in new sales lines being
assigned a group no. which matches the group no. of these orphaned records.

Make sure that that doesn't happen.
