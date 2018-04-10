DTR2.2.5.2
==========

[Bug Fix]Event subscription deleted on collecting existing record
-----------------------------------------------------------------

Performing collect existing record on a filter collect type deletes event
subscription for each record in the view, resulting in one event subscription
for the last record in the view.

DTR2.2.5.1
==========

[Bug Fix]Volume test - record already exist error
-------------------------------------------------

Record already exist error under volume test by HA, event triggered collection
fails.

Introduce table locking during collection. 

[Bug Fix]Attempt to calculate old values on Calculated Fields
-------------------------------------------------------------

Do not calculate old values on flowfields or calculated fields.

DTR2.2.5
========

Deleting Processed Event Data
-----------------------------

Add a new dateformula field to the Data Publisher Subscriptions subpage, Retain
Data For. This will allow the user to define the period that subscription data
will be retained for before it is deleted.

Add a new datetime field to the Data Event Subscription table to record when
those event subscriptions have been processed, DateTime Processed. Note the
existing datetime field on this page is the datetime that the event was raised,
not when it was processed.

Add a new codeunit to delete processed event subscriptions and the related event
data records. Call this functionality after the data is published. Using the
above dateformula field to determine which records can be deleted.

Handle Duplicate Records
------------------------

Add a new function to COD9030170 to determine whether there is an existing Data
Publisher Subscription (and related Data Publisher Event Data) record with the
same primary key as the RecRef whose event is being handled - only search for
records that are for the same data collector and publisher and which has not
already been published. Delete those records if they are found.

Call this function from InsertEventSubscription to ensure that more than one
cannot be waiting to be published with the same primary key.

Calculated Field Data Types
---------------------------

Calculated Fields can be based on a min/max calculation (like flowfields).
Currently the calculation only supports numeric fields.

This should be changed to support date, time, datetime and alpha data types.

DTR2.2.2
========

Ability to Add Calculated Fields on Filter View
-----------------------------------------------

The "Record View" allows the user to define filters against a Data Collector to
specify that only a subset of the records in that table should be collected.

Currently the record view can only include fields in the table (as it uses the
filter page functionality to generate the view).

It would be useful it is was possible to include filters on calculated fields
i.e. fields which take their values from another table.

[Bug Fix]File Share Data Publisher Doesn't Mark the Events as Processed
-----------------------------------------------------------------------

Mark the Data Event Subscription records as Processed when the dataset is
published so that they are not included in future publishes.

Rework Data Consumer to Avoid TryFuction
----------------------------------------

COD9030174 uses several TryFunctions. The codeunit needs to be reworked to avoid
the use of TryFunctions because from NAV 2017 you cannot make write transactions
in TryFunctions be default.

Change TryFunctions to IF CODEUNIT.RUN calls. Move the
ProcessDataConsumerRecordx functions to a new codeunit. Call that codeunit with
a Data Consumer Record record. If the CODEUNIT.RUN call fails then record the
processing error.

COMMIT each Data Consumer Record within the REPEAT UNTIL loop as you go.

Include Calculated Fields in Data Event Data
--------------------------------------------

COD9030170.InsertEventSubscriptionData excludes included fields that have a
Field No. of 0. Calculated fields have Field No. 0 so do not end up creating
Data Event Data records.

Take care with publishers of type File Share. Included calculated fields in the
Data Event Data should not cause errors when attempting to consume that event
data in another NAV company.

Make Record View not Mandatory for Filter Collector Type Data Collectors
------------------------------------------------------------------------

Remove the TESTFIELD that is causing the error. Only attempt to set the view of
the source record if a Record View has actually been set.

DTR2.2.1.1
==========

Data Type on Data Collector Columns
-----------------------------------

The user needs to have the ability to set the data type against fields that are
included in data collectors. This is for the benefit of PowerBI when the dataset
is created and data is published to it.

[Bug Fix]Duplicate fields in PowerBI
------------------------------------

When creating a relationship between 2 collectors, the collecting of the data
works fine in NAV. But when publishing the data all the fields are grouped
together for both tables under one another.

Hidden property on Data Collector
---------------------------------

Add hidden boolean onto TAB9030170 and PAG9030171

In COD9030171 pass in the boolean as part of the SchemaField constructor

[Bug Fix]Service Password Permissions
-------------------------------------

Give TAB9030171 permission to Service Password table

DTR2.2.1
========

Allow Data Collectors to Use Filters
------------------------------------

Currently the table filters are only used for a Collector Type of filter i.e.
collecting all records from a given table which fall within the filters.

It would be nice to combine the filters with the event Collector Type and
collect records based on events, but only if they match the filters that have
been set against the collector.

DTR2.2.0.1
==========

Format/Summarize By field on Data Collector
-------------------------------------------

A new field on the Data Collector fields to specify the format of the field i.e.
dd/mm/yyyy

New field 'Summarize By' with the following set options (case sensitive):

default

none

sum

min

max

count

average

distinctCount

default is the default value for the summarize by field

Add these fields to the Data Collector Fields page

Pass this format information to the JSON using the SchemaField with format
and summarizeBy constructor

[Bug Fix]Object No. Relationship Should be to AllObj Not Object
---------------------------------------------------------------

Relationship to Object means you cannot select tables that belong to other
extensions.

DTR2.1.0
========

Ability to Create "Measures" on Data Collectors
-----------------------------------------------

“Measures” are expressions that PowerBI can use to further manipulate data that
has been published using Data Replication. This field doesn’t have any business
logic implications it is just included in the data that is published to PowerBI.

Defining Relationships Between DataSet Tables
---------------------------------------------

Add a new action to the Subscriptions tab of the Data Publisher Card,
Relationships. This action should open a new list page to define the
relationship between the current subscription and other subscriptions.

The page should include:

From Data Collector Code (populated with the selected data collector code - by
the filters applied from the action, not visible by default)

From Field Name - validated to the DataSet Field Name of fields that are
included in the From Data Collector Code.

To Data Collector Code - validated to a subscription that is included in the
Data Publisher (other than the From Data Collector Code)

To Field Name - validated to the DataSet Field Name of fields that are included
in the To Data Collector Code.

Data Consumers
--------------

Ability to consume xml files that have been created by a Data Publisher of type
File Share. Optionally push this data through a Configuration Package for
further processing/validation.

File Share Data Publisher
-------------------------

Support for publishing data that has been collected via a Data Collector to xml
files in a file share by means of Data Publisher of type ‘File Share’.

Rename 'Data Consumer' to 'Data Publisher'
------------------------------------------

New functionality is required to support bringing xml files into NAV and
replicating changes from another NAV database / company.

It makes sense to name this concept 'consuming' data - therefore the related
entity in NAV will be a 'Data Consumer'. Rename the existing objects called
'Data Consumer' to 'Data Publisher' - this area of functionality is more
concerned with publishing the data for another application to consume than
actually consuming it.

Max. No. of Fields in DataSet Table
-----------------------------------

Create a new function in TAB9030169, GetMaxNoOfFields which doesn't take any
parameters and returns an integer. Hard-code to return value to 75 for now.

When the Include field is validated for a Data Collector Field record, check how
many fields are already included. If the maximum no. of fields (determined by
the above function) is already included, throw an error message:

"The maximum no. of fields (%1) has already been included in this %2." where %1
is the maximum no. of fields and %2 is the table caption of the data collector
table.

Also change the UpdateSelected function of PAG9030171 to prevent more than the
maximum no. of fields from being set to be included. If the user selection will
include more than the maximum no. of fields, do not throw an error, but Include
fields up to the field limit and do not include the rest. In that case, show a
message:

"This %1 can include a maximum of %2 fields. As your selection included more
than %2 fields only the first %2 have been included." where %1 is the table
caption for the data collector table and %2 is the maximum no. of fields.

Do Not Allow Primary Key Fields to Not Be Included
--------------------------------------------------

Change UpdateSelected in PAG9030171. Do not set the Include field to false for
primary key fields.

Flowfield Calculation
---------------------

Add code to COD9030170 to identify when a field being used as the source of a
dataset field is a flowfield. Calculate the value of the flowfield when the
event data is inserted.

Data Collector Fields
---------------------

When a new line is inserted on the Data Collector Fields subpage, it should
automatically be ticked as a Calculated Field. Do not allow the user to untick
this box.

Equally, users should only be able to delete calculated fields. Prevent them
from deleting any other records.
