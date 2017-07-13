# IPE1.0.3
## Calc. for Additional Fields
We require the ability to use the 'engine' of this addition to calculate the new value of other fields that may have been added by project bespoke.<div><br></div><div>From the item planning setup page the user should be able to populate rows in a subpage of additional fields from the item/SKU table that are to be calculated in addition to the standard planning parameters. The user should be able to define the formula to calculate the value of these fields using the same placeholders.</div><div><br></div><div>These new fields should be shown on the Item Plan. Modifier Worksheet with their old value and the calculated new value.</div>

# IPE1.0.2.1
## Add Description Field to Item Plan. Modifier Worksheet
<span style="font-size:12px;">Add description column to worksheet, to be populated with the description from the item card.</span>

# IPE1.0.2
## [Bug Fix]Exclude Drop Shipment ILEs from Demand Calculation
Drop Shipments sales never come in to the warehouse so should not be included in the calculation of demand that the warehouse needs to handle.

## [Bug Fix]Decimal Error When Calculating Volatility
Related to No. of period. See workitem&nbsp;2893.

## [Bug Fix]No. of Periods Calculation
Example;<div><br></div><div>01/03/17 - 30/06/17 calculates as 3 periods instead of 4.</div><div><br></div><div>TAB9030589.CalcNoOfPeriods.. calculate the no of period is between the Period Starts.&nbsp;</div>

## Custom Calculations for Planning Modifiers
Add some new fields to the Item Planning Enhancement Setup page:<div><ul><li>Safety Stock Calculation</li><li>Reorder Point Calculation</li><li>Reorder Quantity Calculation</li></ul><div>These fields will allow the user to enter an arithmetic expression to calculate each of the planning modifiers. The user should be able to use variables in the calculation as follows:</div></div><div><ul><li>%1 = Avg. Demand / Period</li><li>%2 = Avg. Demand / Day</li><li>%3 = Avg. Demand / Week</li><li>%4 = Avg. Demand / Month</li><li>%5 = Avg. Demand / Quarter</li><li>%6 = Avg. Demand / Year</li><li>%7 = Lead Time (Periods)</li><li>%8 = Lead Time (Days)</li><li>%9 = Lead Time (Weeks)</li><li>%10 = Lead Time (Months)</li><li>%11 = Lead Time (Quarters)</li><li>%12 = Lead Time (Years)</li><li>%13 = New Safety Stock Quantity</li><li>%14 = New Reorder Point</li></ul><div>For example the user should be able to enter something like,</div></div><div><br></div><div>Safety Stock Calculation = %4</div><div>Reorder Point Calculation = %1 * %7 + %13</div><div>Reorder Quantity Calculation = %14</div><div><br></div><div>The worksheet should use the formulae to calculate the new planning modifiers.</div><div><br></div>

## Respect Lead Time Calculation on Vendor Catalog
When calculating the lead time calculation for the worksheet:<div><ul><li>If SKUs are being used and a vendor no. is specified on the SKU, if there is an Item Vendor record with a non-blank lead time calculation use that value.</li><ul><li>If the lead time calculation is blank take the lead time calculation from the SKU card</li><li>If that is blank take the lead time calculation from the item card</li><li>If the lead time calculation is still blank, or less than the min. lead time calculation from the setup page - use the value from the setup page instead (provided that is not also blank)</li></ul><li>If SKUs are not being used then if there is a vendor no. specified on the item card, if there is an Item Vendor record with a non-blank lead time calculation use that value.</li><ul><li>If that lead time is blank then take the lead time calculation from the item card</li><li>If the lead time is still blank, or less than the min. lead time calculation from the setup page - use the value from the setup page instead (provided that isn't also blank).</li></ul></ul></div>

# IPE1.0.1
## [Bug Fix]Worksheet recalculation increments the existing line value
Delete existing Item Plan. Mod. Wksht. Line and Item Plan. Mod. Wksht. Demand records for the item&nbsp;on calculation.

## Add Item Planning FactBox to the worksheet
Add Item Planning FactBox to Item Plan. Mod. Worksheet page

## [Bug Fix]Obsolete Item Plan. Mod. Wksht. Demand records left behind
<div>Move the deletion of associated Item Plan. Mod. Wksht. Demand record of Plan. Mod. Wksht. Line from the Insert trigger to the Delete trigger</div><div><br></div><div>REP9030589.AnalyzeItemPlanModWkshLine() -&nbsp;delete Item Plan. Mod. Wksht. Line with trigger set true.</div><div>ItemPlanModWkshtLine.DELETE(TRUE);</div><div><br></div><div><b>ERROR</b> - Value was either too large or too small for a Decimal.</div><div>TAB9030590.CalcVolatlity&nbsp;- obsolete demand results in a negative StdDev which then attempted&nbsp;to calculate a POWER to 0.5 throws an error.<br></div>

## [Bug Fix]Worksheet ILE drilldown filter
Filter ILE &quot;Entry Type&quot; to&nbsp;&quot;Assembly Consumption&quot;,Consumption,Sales,Transfer

## Start and End Date Validation
Check the Start and End date in REP9030589

