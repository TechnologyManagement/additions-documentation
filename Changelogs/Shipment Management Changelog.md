
# SPM1.1.3.3
## A new goup is created when one already exists
<ul style="margin-top:0cm;">
 <li style="margin:0px 0px 0px -24px;color:rgb(0, 0, 0);font-family:&quot;Calibri&quot;,sans-serif;font-size:11pt;font-style:normal;font-weight:normal;"><span style="margin:0px;">Create a SO with no Requested Delivery Date – Grp No. = 59 and
     Shipment Date = 2<sup>nd</sup> Mar </span></li>
 <li style="margin:0px 0px 0px -24px;color:rgb(0, 0, 0);font-family:&quot;Calibri&quot;,sans-serif;font-size:11pt;font-style:normal;font-weight:normal;"><span style="margin:0px;">Create a SO and add a Requested Delivery Date of 2<sup>nd</sup> Mar
     <u>BEFORE</u> the lines – Grp No. = 59 and Shipment Date = 2<sup>nd</sup>
     Mar</span></li>
 <li style="margin:0px 0px 0px -24px;color:rgb(0, 0, 0);font-family:&quot;Calibri&quot;,sans-serif;font-size:11pt;font-style:normal;font-weight:normal;"><span style="margin:0px;">Create a SO and add a Requested Delivery Date of 2<sup>nd</sup> Mar
     <u>AFTER</u> the lines Mar – Grp No. = 60 and Shipment Date = 2<sup>nd</sup>
     Mar</span></li>
</ul>

## Respect customised calendar changes
The calendar changes to the location are not respected

# SPM1.1.3.2
## Event to Check Shipment Date Validity for Source Document
Provide an opportunity for a subscriber to indicate whether a given date is valid for a given source document.


## Support Customised Calendars
When scheduling source lines we currently support base calendar codes. We need to also support customised calendar codes e.g. when a Location uses a particular base calendar but has some customisation specific to that location.


## Source Header Shipment Mgt. Date
The Shipment Mgt. Date on the header of the source record is used as the starting date for scheduling records. This is causing more trouble than it is worth. We should use the shipment date on the source line instead.

SPM1.1.2
========

 No. of Picks not Updating 
----------------------------------------------

Creating a Pick does not update the field on the shipment management page


Only Single Warehouse Shipment Being Created
----------------------------------------------

Highlight a group line (indentation = 0) on the shipment management page and invoke the create warehouse shipments action. A warehouse shipment per source document should be created. Only a single header is being created.


Address Fields Not Populated
----------------------------------------------

The address fields are only updated when the ship-to code is not blank. Open the delivery area page and validate the destination type and destination no. and the address fields do not show the address.<div><br></div><div>Also, creating a delivery area line using the save delivery order page does not populate these fields.</div>


Date Used to Calculate Shipment Mgt. Group for Sales Line
----------------------------------------------

It appears that the Shipment Date from the sales header is being used to calculate the Shipment Mgt. Group for the sales line rather than the Shipment Date on the sales line.&nbsp;<div><br></div><div><div>&quot;Created a SO in Cronus, for 1 x 1000 for WHITE Location</div><div>On the lines the Shipment Date was the 10th, but the header was the 9th</div><div>Opened Shipment Management and couldn't see anything for the 10th, entered the 9th and this displayed the order.&nbsp;</div><div><br></div><div>This is incorrect, Shipment Mgt., uses the lines not the header as an order can have different shipment dates per line. See URL</div><div><br></div><div>Shipment Management Shipment Date should work at SO line level. &quot;</div><div><br></div><div><br></div></div>


Custom Sorting Order Calculation
----------------------------------------------

The Custom Sorting Order calculation that sorts by Delivery Order needs work. It doesn't take into account that there can be multiple warehouse activity lines for the same source line e.g. because the pick can have take and place lines or the take lines can be split into multiple lines to pick from multiple bins/lots etc.


Reverse Delivery Order Sorting for Picks
----------------------------------------------

Sometimes the warehouse will want to pick source lines in reverse delivery order so that the goods which are going to be delivered last can be loaded onto the vehicle first.<div><br></div><div>Add options to Shipment Management Setup and the pick creation routine to support this.</div>

SPM1.1.1
========

Validating Sales Line Shipment Date Shows Out of Schedule Warning
----------------------------------------------

Call the function to check whether the date is valid for the given delivery date.


Validating Ship-to Code Doesn't Recalculate Delivery Area Code
----------------------------------------------

OnValidate of the Ship-to Code call the function to calculate the delivery area code for the sales line again.


Validating Expected Receipt Date on Purchase Header Doesn't Update Shpt. Mgt. Group on Line
----------------------------------------------

Validating the expected receipt date on the header should also update the expected receipt date on the line and therefore influence the shipment management group that the line belongs to.


Save Delivery Order Dialog Improvements
----------------------------------------------

User should be able to see Address details on the Delivery Area lines.


Create Default Shipment Management Setup
----------------------------------------------

Create a default Shipment Management Setup record when the extension is installed, if it doesn't already exist.


Address Details on Delivery Area 
----------------------------------------------

User should be able to show Address and Address 2 fields that correspond to the destination type and no. that has been selected (if not blank) on the line.


Deliver to this Post Code - 0
----------------------------------------------

The Save Delivery Order dialog shows a value of 0 for &quot;Deliver to this Post Code&quot; when the page is opened. This should show blank.


Can Enter Destination No. and Ship-to Code with Blank Destination Type
----------------------------------------------

The user is able to enter data into the Destination No. and Ship-to Code when the Destination Type is blank. The user should only be able to populate these fields when the Destination Type is not blank and should validate the data.


Save Delivery Order from Customer/Vendor Card
----------------------------------------------

Add the Save Delivery Order action to the Customer and Vendor card pages. This will be useful when the transport planner wants to specify the order in which all destinations within a given Delivery Order will be visited (rather than relying on post code groups). The user will be able to create a new customer and then find a space in the Delivery Area for deliveries to this customer to be made.<div><br></div><div>The same action should also be added to the Ship-to Address and Order Address cards.</div>


Delivery Area Code for Line Level on Shipment Management Page
----------------------------------------------

We're showing three levels of grouping on the Shipment Management page: groups, drops and lines. The line level of grouping doesn't display anything in the first column (Delivery Area Code). This looks unusual - isn't obvious that there are some lines to see until the user scrolls to the right.


Inserting Delivery Area Lines
----------------------------------------------

Currently the Delivery Area lines are sorted by Line No. - which is required by the AutoSplitKey functionality to enable the user to enter a new line anywhere on the page.<div><br></div><div>As a result, if a user creates a new Delivery Area Line by using the Save Delivery Order action the line is added to the bottom of the subpage. The Delivery Order that is assigned to the line is correct, but it looks wrong. The user will expect the lines to be shown in the order that deliveries are going to be made to them.</div><div><br></div><div>The page should be sorted by the Delivery Order column instead, but that means that the (standard)&nbsp;<b>New</b>&nbsp;action to create a new line will not work, AutoSplitKey will not know what line no. to assign to the line. We should have a new action to pop a dialog page to allow the user to enter the details to create a new line instead.</div>


Move Delivery Area Lines Up and Down
----------------------------------------------

The current functionality is defining the next and previous lines to swap the current line with by their line numbers. The current line and the identified line are both renamed to have each other's line no.<div><br></div><div>This should be changed to find the next and previous records according to the delivery order i.e. if the current record has Delivery Order = 3 then it should be assigned Delivery Order = 2 and the existing Delivery Order = 2 record be given Delivery Order = 3.</div><div><br></div><div>Check that the page is sorted by Delivery Order before allowing the user to move up and down. If not, show an error asking them to resort the page before moving lines.</div>


Delivery Orders not Recalculated OnDelete of Delivery Area Line
----------------------------------------------

If a Delivery Area Line is deleted the Delivery Order values of the remaining lines should be recalculated to fill the gap that has been left.<div><br></div><div>Exposed with RecalcDeliveryOrderOnDeleteOfDeliveryAreaLine test in COD90001.</div>


Create Whse Shipment Actions Should Only Include Visible Lines
----------------------------------------------

There are a couple of actions on the Shipment Management page that create warehouse shipments for a given Shipment Mgt. Group. These actions create shipments for all source lines that belong to the highlighted group.<div><br></div><div>This should be changed to ensure that only lines which are visible on the shipment management page are included on the warehouse shipment.</div><div><br></div><div>This will be useful for customers who wish to define their own logic to determine whether a given source line should be included on the Shipment Management page or not.</div>


Allow Custom Logic for Inclusion of Source Lines on Shipment Management Page
----------------------------------------------

The product defines some logic as to whether a source line should be included on the Shipment Management page e.g. is the source header released, respecting reservations and the shipping advice of the source document.<div><br></div><div>We want to allow custom subscribers to override/enhance this logic.</div>


Remove Unnecessary Shipment Mgt. Fields from Posted Tables
----------------------------------------------

We have added Shipment Mgt. fields to posted sales tables for the sake of completeness, but which don't strictly need to be there. Given that on a large database, restoring this data takes a loooong time, we can remove these fields.

Updating Expected Receipt Date on Header/New line added
----------------------------------------------

<div>Updating the Expected Receipt Date on the Purchase Header, updates the date on the line. However this isn't reflected in the Shpt Mgt. Group No. This also needs updating</div><div><br></div><div>When a new document is created and a line is added, the expected receipt date is calculated by std NAV. But the Shpt Mgt Group No. is not updated. Calculate the Shpt Mgt Group No. when initially creating the line</div>

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
