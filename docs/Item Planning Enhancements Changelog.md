IPE1.0.3
========

Calc. for Additional Fields
---------------------------

We require the ability to use the 'engine' of this addition to calculate the new
value of other fields that may have been added by project bespoke.

From the item planning setup page the user should be able to populate rows in a
subpage of additional fields from the item/SKU table that are to be calculated
in addition to the standard planning parameters. The user should be able to
define the formula to calculate the value of these fields using the same
placeholders.

These new fields should be shown on the Item Plan. Modifier Worksheet with their
old value and the calculated new value.

IPE1.0.2.1
==========

Add Description Field to Item Plan. Modifier Worksheet
------------------------------------------------------

Add description column to worksheet, to be populated with the description from
the item card.

IPE1.0.2
========

[Bug Fix]Exclude Drop Shipment ILEs from Demand Calculation
-----------------------------------------------------------

Drop Shipments sales never come in to the warehouse so should not be included in
the calculation of demand that the warehouse needs to handle.

[Bug Fix]Decimal Error When Calculating Volatility
--------------------------------------------------

Related to No. of period. See workitem 2893.

[Bug Fix]No. of Periods Calculation
-----------------------------------

Example;

01/03/17 - 30/06/17 calculates as 3 periods instead of 4.

TAB9030589.CalcNoOfPeriods.. calculate the no of period is between the Period
Starts. 

Custom Calculations for Planning Modifiers
------------------------------------------

Add some new fields to the Item Planning Enhancement Setup page:

Safety Stock Calculation

Reorder Point Calculation

Reorder Quantity Calculation

These fields will allow the user to enter an arithmetic expression to calculate
each of the planning modifiers. The user should be able to use variables in the
calculation as follows:

%1 = Avg. Demand / Period

%2 = Avg. Demand / Day

%3 = Avg. Demand / Week

%4 = Avg. Demand / Month

%5 = Avg. Demand / Quarter

%6 = Avg. Demand / Year

%7 = Lead Time (Periods)

%8 = Lead Time (Days)

%9 = Lead Time (Weeks)

%10 = Lead Time (Months)

%11 = Lead Time (Quarters)

%12 = Lead Time (Years)

%13 = New Safety Stock Quantity

%14 = New Reorder Point

For example the user should be able to enter something like,

Safety Stock Calculation = %4

Reorder Point Calculation = %1 \* %7 + %13

Reorder Quantity Calculation = %14

The worksheet should use the formulae to calculate the new planning modifiers.

Respect Lead Time Calculation on Vendor Catalog
-----------------------------------------------

When calculating the lead time calculation for the worksheet:

If SKUs are being used and a vendor no. is specified on the SKU, if there is an
Item Vendor record with a non-blank lead time calculation use that value.

If the lead time calculation is blank take the lead time calculation from the
SKU card

If that is blank take the lead time calculation from the item card

If the lead time calculation is still blank, or less than the min. lead time
calculation from the setup page - use the value from the setup page instead
(provided that is not also blank)

If SKUs are not being used then if there is a vendor no. specified on the item
card, if there is an Item Vendor record with a non-blank lead time calculation
use that value.

If that lead time is blank then take the lead time calculation from the item
card

If the lead time is still blank, or less than the min. lead time calculation
from the setup page - use the value from the setup page instead (provided that
isn't also blank).

IPE1.0.1
========

[Bug Fix]Worksheet recalculation increments the existing line value
-------------------------------------------------------------------

Delete existing Item Plan. Mod. Wksht. Line and Item Plan. Mod. Wksht. Demand
records for the item on calculation.

Add Item Planning FactBox to the worksheet
------------------------------------------

Add Item Planning FactBox to Item Plan. Mod. Worksheet page

[Bug Fix]Obsolete Item Plan. Mod. Wksht. Demand records left behind
-------------------------------------------------------------------

Move the deletion of associated Item Plan. Mod. Wksht. Demand record of Plan.
Mod. Wksht. Line from the Insert trigger to the Delete trigger

REP9030589.AnalyzeItemPlanModWkshLine() - delete Item Plan. Mod. Wksht. Line
with trigger set true.

ItemPlanModWkshtLine.DELETE(TRUE);

ERROR - Value was either too large or too small for a Decimal.

TAB9030590.CalcVolatlity - obsolete demand results in a negative StdDev which
then attempted to calculate a POWER to 0.5 throws an error.

[Bug Fix]Worksheet ILE drilldown filter
---------------------------------------

Filter ILE "Entry Type" to "Assembly Consumption",Consumption,Sales,Transfer

Start and End Date Validation
-----------------------------

Check the Start and End date in REP9030589
