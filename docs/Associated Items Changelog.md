ASI1.1.6
========

Suggest Item Associations
-------------------------

New functionality to suggest item associations. Analyses the frequency with
which items are sold together and if it exceeds a given threshold, suggests that
an item association should be created between the two items.

This functionality can be run automatically in the background when the item is
included on a sales order or manually/offloading to the job queue for later
review.

ASI1.1.5.1
==========

Notification to Prompt User that there are Associated Items for the Given Item
------------------------------------------------------------------------------

Subscribe to the OnInsertRecordEvent of the Sales Order Subform, check the
document type, check if associated item record(s) exist. If so, create a the
notification.

Create another function to handle the "Yes, show me" link click. This function
should accept a Notification variable and pop the associated items page.

ASI1.1.2
========

[Bug Fix]Missing Upgrade Instructions
-------------------------------------

TAB37

ASI1.1.5
========

[Bug Fix]Event Subscriptions Triggering on Temporary Records
------------------------------------------------------------

Ensure that all event subscription EXIT if they are triggered by a temporary
record event (Rec.ISTEMPORARY).

ASI1.1.3
========

[Bug Fix]RunModal Error When Deleting Associated Lines
------------------------------------------------------

If a change to the parent line e.g. validating the No. to a different item
results in the deletion of the associated lines. If the validation of the new
parent line results in a credit warning or stockout warning a RunModal error
will result.
