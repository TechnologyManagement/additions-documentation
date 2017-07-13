SFDC1.2.5
=========

SFDC Task List
--------------

Add an SFDC Task List option to each of the Work Centre List/Card and Machine
Centre List/Card. This should open a list page showing prod. order routing
lines, built using the logic that build the prod. order selection list in SFDC.

Link Between Machine Centres and Operators
------------------------------------------

Add a new field to the SFDC Operator list, Machine Center No. validated to the
Machine Center table (set the caption for ENG to Machine Centre).

Add a new index on this field and a function to get the machine centre record
that corresponds to a given operator code.

Change COD9059231.InitItemJnlLine to use this function to find the correct
machine centre record (instead of the existing logic to get a machine centre
which has a No. the same as the operator code).

Link Between Machine Centres and Operators
------------------------------------------

Both the Machine Center card and the SFDC Operator list have fields to specify
the Unit Cost. These values need to be kept in sync when the Machine Center has
been linked to an SFDC Operator.

These costs need to synchronised:

OnValidate of SFDC Operator.Machine Center No. (when it is validated to a
non-blank value)

OnValidate of Machine Center.Unit Cost - when the Machine Center is linked to an
SFDC Operator

OnValidate of Work Center.Unit of Measure Code (synchronise all Machine Centers
that belong to this Work Center No. and are linked to SFDC Operators)

OnValidate of SFDC Operator.Unit Cost

OnValidate of SFDC Operator.Unit of Measure Code

Note: the Unit of Measure for the Machine Center unit cost is inherited from its
parent Work Center and the Unit of Measure for the SFDC Operator may be
different to that of the Machine/Work Center. If so, the Unit Cost will need to
be converted appropriately using CalendarManagement codeunit.

Optionally Show Prod. Order Routing Lines for Parent Work Center
----------------------------------------------------------------

See COD9095231.FilterProdOrderRoutingLineByMachineCenter for the existing logic
that builds the prod. order selection list.

Add a new field to SFDC Setup, Show Prod. Orders from Work Center (boolean). Add
a new field to the Machine Center card (SFDC tab), Show Prod. Orders from Work
Center (option - 'Default,Yes,No').

Change the logic in the above function to respect the combination of these
fields and decide whether to include the prod. order lines that are assigned to
the parent work center no.

Consider COD9059235. This codeunit has some logic targeted at PlannerOne
integration to determine whether a prod. order routing line can still
legitimately be marked to be included in the selection set.

Posting Negative Consumption Lines
----------------------------------

Make changes to SFDC Management codeunit to allow negative quantities to be
posted.

[Bug Fix]Duplicate routing comments
-----------------------------------

Routing Line comments are copied over Prod. Order Routing Line when lines are
create, so when getting Prod. Order Routing Line comments do not look at its
Routing Line comments.

SFDC1.2.4.1
===========

[Bug Fix]Show Ops with no Qty. to Start
---------------------------------------

COD9059231.FilterProdOrderForSelection SFDCSetup."Show No. of Exp. Operators" is
checked instead of "Show Ops with no Qty. to Start"

SFDC1.2.4
=========

Update HandlePortalResourceRequest Event Handler
------------------------------------------------

COD9059230 - Update OnHandlePortalResourceRequest EventSubscriber to remove
ResoureName length constraint

Add Item No. to Prod. Order selection page
------------------------------------------

SFDC1.2.3
=========

"Mandatory" Notes
-----------------

Prod. Order Routing Line Comments identified with a new field that indicates
they should be displayed to the user before they begin work on the prod. order.

Should be able to set at Routing Line and Prod. Order Routing Line level.

[Bug Fix]Device Data Lookups
----------------------------

The device data lookup event subscribers are using the existing logic for
filtering machine centres, operators and prod. order routing lines.

The user is likely to want to look this data up when they are first setting up
SFDC and there may not be any prod. orders ready to start in the system. Change
the logic as follows:

Machine Centres - show all machine centres which are ticked to be Included in
SFDC

Operators - show all opeprators

Prod. Order - existing logic is fine.

Linking Devices to Work Centres
-------------------------------

New Data Type to specify the Work Centre that a device is linked to. Centre
selection only shows the machine centres that exist under that work centre.

[Bug Fix]Lot No length error
----------------------------

update the lot no length to 20 from 10

Confirming Operators on Output
------------------------------

Add a new field to the SFDC tab of the Machine Centre card, Confirm Operator on
Output.

For Machine Centres were this option is enabled (and where more than one
operator has been assigned to the device) show a subform with the assigned
operators displayed as tiles and prompt the user to select an operator to link
with the output.

When (if) attaching operator dimensions to the ledger entries that are created
for the output, only attach the dimension value for this one operator - rather
than all operators that are assigned to the device.

Operator Notes
--------------

Remove code from COD9059231 that retrieves data from RecordLink records to pass
to the portal. We'll only pass header and line comment records instead.

Create a new table to populate with the note that an operator saves in the
portal. Just save the comment in a text250 field and throw an error if the user
writes a comment longer than that.

Also record the device, the prod. order line / prod. order routing line,
operators, date and time that were logged onto the device when the note was
posted.

Create a new page to show SFDC Comments that have been created. Also add an
action to Prod. Order & Prod. Order Routing pages to display a filtered list of
SFDC Comments for that prod. order / prod. order routing line.

Remove Output Lot Button If Not Final Routing Operation
-------------------------------------------------------

Don't show the Output Lot button unless the operation that is loaded on the
device is the final operation on the routing.

Support the assignment of an Operator to multiple Centers
---------------------------------------------------------

TAB9059243 - Extend the primary key to include Center No.

SFDC1.2.0.2
===========

[Bug Fix]SFDC Operator Center - Operator Code field length
----------------------------------------------------------

TAB9059245 Operator Code field length is 10, change it to 20.

SFDC1.2.2.1
===========

[Bug Fix]OnBeforeGetNextActivity blank parameters
-------------------------------------------------

COD9059233.OnBeforeGetNextActivity 

Pass PortalFwkApplicationDevice2 and PortalFrameworkActivityLog2

[Bug Fix]P1 Planned Ress Incompatible of Machine Center on a Work Center Routing Line.
--------------------------------------------------------------------------------------

if the Type is Work Center then the Planned Res. Type has to be Work Center as
well, if the Planned Res. Type is set to be Machine Center it doesn't check if
that belongs to the Work Center of the routing line.

COD9059235.GetRtngLinePlannedRessIncompatibleWithCenter()

Update the function so that when PlannedRessType is of Machine Center, check
that the Machine Center belong to the Work Center.

SFDC1.2.2
=========

Exp. No. of Operators (Setup)
-----------------------------

Split the Expected No. of Operators field into two - Expected No. of Operators
(Run) and Expected No. of Operators (Setup)

Show Ops with no Qty. to Start
------------------------------

New field on SFDC Setup table and page.

Changes the logic for calculating list of prod. order lines to ask the user to
select from depending on the value of this field.

SFDC1.2.2
=========

Exp. No. of Operators (Setup)
-----------------------------

Split the Expected No. of Operators field into two - Expected No. of Operators
(Run) and Expected No. of Operators (Setup)

SFDC1.2.1
=========

[Bug Fix]Reinstate OnBeforeGetNextActivity and OnAfterGetNextActivity publisher events
--------------------------------------------------------------------------------------

COD9059232 SFDC - Activity Mgt.

Reinstate the publisher events to support customization. 

[Bug Fix]Refactor PlannerOne Integration
----------------------------------------

Refactor COD9059235 subscriptions

Exclude prod. order routing lines on other devices from selection
-----------------------------------------------------------------

Filter out operations selected on other device from a the selection screen. 

[Bug Fix]Output decimals not supported
--------------------------------------

Output decimals is not supported.

Make By-Products optional
-------------------------

Add a field to SFDC setup

[Bug Fix]Lot No requirement check
---------------------------------

An item having an Item Tracking Code doesn't necessarily mean that it is lot
tracked. See Item Tracking Management.GetItemTrackingSettings (see COD22 for an
example of how this is used).

[Bug Fix]Default device status
------------------------------

Default device status to Ready. It's blank at the moment so the output screen is
loaded with a blank status which prevents the Back button from being displayed.

[Bug Fix]Missing Upgrade Instructions
-------------------------------------

Add missing upgrade instruction to the SFDC Management codeunit for the Prod.
Order Line table.

Remove the Back button from data selection screen for permanent data
--------------------------------------------------------------------

Remove the Back button from data selection screen if the previous datatypes are
permanent and has data.

[Bug Fix]Posting of device time when removing page operator
-----------------------------------------------------------

Device Time is posted alone with Operator when removing an operator

[Bug Fix]Operator Time from the portal is posted
------------------------------------------------

Operator time posted is different decimal places to the machine so they have
different time which is confusing when they have all been started/stopped
together.

Work out the duration from the SFDC Device Log to post the operator time,
instead of the raw time from the portal.

[Bug Fix]Calculate next Qty. Available to Start
-----------------------------------------------

Calculate Qty. Available to Start for the next manual flushing operation.

Having a different flushing method breaks the current logic.

[Bug Fix]Additional Run Time is posted when starting the device from startsubform
---------------------------------------------------------------------------------

Calling stop device to log the actual reason is causing the posting of device
run time.

Prevent the posting of runtime if the device is in Stopped status.

[Bug Fix]Stop Times are posted in second insead of the UOM
----------------------------------------------------------

Convent the Time in seconds before calling PostOutput

Change SFDC Page Instructive Text Control Add in
------------------------------------------------

Change the page to work with the new HTML editor control add-in.

Make Operator Time Posting Optional
-----------------------------------

Add a checkbox to SFDC Setup to determine whether extra output journal lines are
posted for the operator machine centres or not.

Lot Tracking Functionality
--------------------------

Need a couple of new columns on the components page:

Column to indicate whether lot tracking is required on each component line
(non-editable checkbox?)

Column to indicate the current Lot No. that has been selected to use.

The user should be able to highlight one of the component lines and click a new
button in the nav bar, Lot Info. This will open a separate page where the user
is able to either enter a lot number manually (or scan using a barcode scanner)
or select a lot no. from the list of available lots for the current item no.

This selection will determine the current lot no. for that component. All
consumption for that item that is posted from the portal should be taken from
that lot no. until it is changed by the user.

It is possible to make the Lot No. column in the components grid editable
directly. This would be a better UX if the user was going to scan the lot no.
They could click into the correct cell of the grid and scan the barcode.

Lot Tracking Functionality
--------------------------

Determine whether lot tracking is required for the finished item of the current
prod. order. If so, display a new "Lot No." tile in the middle section of the
output screen.

If some item tracking lines have already been assigned to the prod. order line
then show the first lot no. from the item tracking lines. Otherwise, if some
output has already been posted against the prod. order line then copy the lot
no. from the output capacity ledger entry.

A new button in the nav bar should allow the user to set the lot no. The user
should be able to enter (or scan) the lot no. manually. Also, if the Lot Nos.
field is set on the item card of the finished item for the current prod. order
then show an "Assign Lot" button in the nav bar. This will take the next number
from the defined No. Series and use that as the lot no.

This lot no. should be used to post all output entries from this device until
the user changes prod. order or changes the lot no. Save the lot no. as device
data which is to be cleared when the prod. order is changed.

SFDC Device Log - Add to Menu
-----------------------------

Add the Portal Framework activity log page to the menu as "SFDC Activity Log".
Filter the log to entries filtered by the Application ID set in SFDC Setup.

SFDC1.2.1
=========

[Bug Fix]Move Op to Machine Center
----------------------------------

Move operation to machine center is not being triggered in the portal framework
version.Subscribe to onafterstoredata

SFDC1.2.0.1
===========

[Bug Fix]Device time not posting
--------------------------------

9059230.OnGetRunningTime() subscriber not returning the run time.

Assign the return value to VAR RunTime

Preserve Prod. Order Routing Line When Moving Prod. Order to Machine Centre
---------------------------------------------------------------------------

See TAB5409.MachineCtrTransferfields

Before ProdOrderRoutingLine.No. is validated to the new Machine Centre take a
copy of the fields that are modified by this function. Validate the No. (which
will change all of these fields), then validate them back to their original
values.

SFDC1.2.1
=========

[Bug Fix]Qty. Available to Start
--------------------------------

The available to start calculation does not currently respect the flushing
method of the prod. order routing lines. Only routing lines which have a
flushing method of Manual should be included in this calculation.

The quantity should be set to:

The full expected quantity is the routing line is the first with manual flushing
on the prod. order

Otherwise, set to the quantity output from the previous manual flushing
operation.

SFDC1.2.0.1
===========

[Bug Fix]HTMLEditor control add-in has not been instantiated
------------------------------------------------------------

Occasionally errors on opening SFDC Page Instructive Text page.

This is because code gets to executed before the page has been rendered.

Posting Operator Time/Cost
--------------------------

When the item journal line is posted for the time of the machine create post
another item journal line for the time of the operator. 

Check that there is a machine centre with the same code as the operator, throw
an error if not.\*

Set the Type on the jnl. line to Machine Center

Validate the No. to the Operator Code

Validate the unit of measure code to that of the operator

Validate the Unit Cost to that of the operator

Validate the quantity to the time (in the UOM of the operator) that needs to be
posted - set as Setup, Run or Stop Time as appropriate.

\*Once this approach of posting extra cap. ledger entries to different machine
centres has been proven we can improve the process in future versions e.g.
automatically creating machine centres when operators are created and
synchronising changes between the two.

Please isolate these changes in their own changeset so that we can roll them
back easily if we decide not to follow this approach.

Linking Machine Centres & Operators
-----------------------------------

When building the list of data that the device operator is going to be asked to
select from respect the Operator Center table.

If the device has already been selected then filter the list of operators. Only
show operators which have:

No Operator Center records, or

Have an Operator Center record for the selected Machine Center, or

Have an Operator Center record for the Work Center which the selected Machine
Center belongs to, or

Have an Operator Center record for the Work Center Group which the selected
Machine Center belongs to (via its parent Work Center).

If the operator has been selected before the device, and the operator has at
least one Operator Center record, only show Machine Centers defined by the
Operator Center record (either directly or via its parent Work Center or parent
Work Center Group).
