PRO1.0.6.1
==========

[Bug Fix]Promotion Entry created with incorrect Source Line No.
---------------------------------------------------------------

PRO1.0.6
========

Create Promotion Entry Records Prior to Applying the Promotion Benefit
----------------------------------------------------------------------

Currently promotion entry records are created at the end of the process to apply
promotions to a given sales header.  If we create the entries before the
benefits of the promotions are applied customer bespoke code will be able to
tell from Sales Line field validation code whether a promotion has been / is
being applied to the sales line and respond accordingly.

Break Promotions Mgt. Codeunit Up
---------------------------------

Create new, smaller codeunits with a more limited scope per codeunit.

PRO1.0.3.1
==========

[Bug Fix]Remove Test Coduenit
-----------------------------

PRO1.0.3
========

Split Benefit Across Target Lines
---------------------------------

Change default behaviour of applying promotion benefit to apply the full amount
to all lines. Add new field to Benefit table to allow the user to specify that
the benefit should be split (as per existing functionality).

Enabled/Disable Actions
-----------------------

Add new actions to the Promotions Card.

Allow Benefit Multiplication Should Not Be Editable When Promotion Enabled
--------------------------------------------------------------------------

Make the Allow Benefit Multiplication field uneditable when the promotion is
enabled.

PRO1.0.2
========

[Bug Fix]Promotion Summary on Posted Document
---------------------------------------------

Create a promotion that has a qualifying criteria which uses a promotion group.

Create a sales order and assign a promotion from that group and apply the
promotion.

The promotion summary on the order is correct.

Post the order

The promotion summary on the invoice is incorrect / blank.

[Bug Fix]Promotion in Use Check
-------------------------------

When disabling a promotion the system should check whether the promotion is used
on any unposted documents and warn the user that changes to the promotion will
not be reflected on the documents.

This seems to work when there is a qualification criteria using a promotion
code, but not otherwise.

Check Qualification Criteria Valid when Enabling Promotion
----------------------------------------------------------

TESTFIELD on the Item No. when enabling the promotion, for qualification
criteria lines that have a Criteria Type of Minimum Qty. or Minimum Amount.

[Bug Fix]Applying Promotions Where Discount % \> 100
----------------------------------------------------

If a promotion is applied that would result in the line discount % being more
than 100 i.e. original line discount % + promotion discount % \> 100 then the
resulting line discount % after applying the discount is wrong.

It should be set to 100%, but it appears to be set to 100 - promotion discount %

Promotion Code Where Used Page
------------------------------

Rename "Order No." to "Document No." (the document isn't always an order).

PRO1.0.1
========

Reapply Promotions
------------------

Add code to COD9030249 to show the confirmation message (if GUIALLOWED) and on
confirmation, unapply existing promotions before applying the promotions again.

Available Promotions Factbox
----------------------------

Create a new listpart page, Available Promotions Factbox, id = 9030265

Add new flowfilter fields to the Promotion table TAB9030249

Source Type filter

Source Subtype filter

Source No. filter

These filters can be used to pass the details of the current document to the
factbox e.g. the factbox can be included as a part on the sales order page where
Source Type = 36, Source Subtype = 1, Source No. = [sales order no.]

The factbox can use these filters to identify the source document and the
customer and therefore the promotions that may be available to the customer.

[Bug Fix]Promotion Entry Already Exists Error
---------------------------------------------

Example: Sales Order 1017

When more than one promotion is applied to a document the following error is
received:

Microsoft Dynamics NAV

\---------------------------

The Promotion Entry Promotion Code already exists. Identification fields and
values: Source Type='36',Source Subtype='1',Source No.='1017',Source Line
No.='0',Promotion No.='',Promotion Benefit Line No.='0',Promotion
Code='APRIL-SALE'

\---------------------------

OK

\---------------------------

[Bug Fix]Unapplying Promotions Asks for Confirmation of Editing Line
--------------------------------------------------------------------

Example: Sales Order 1016

When unapplying the promotion the user is prompted with the message:

Microsoft Dynamics NAV

\---------------------------

This line has been affected by Promotion TEST2. If you edit it manually the
related Promotion Entries will be deleted.

Do you want to continue?

\---------------------------

No   Yes

\---------------------------

When clicking Yes, the following error is shown:

Microsoft Dynamics NAV

\---------------------------

The Promotion Entry does not exist. Identification fields and values: Source
Type='37',Source Subtype='1',Source No.='1016',Source Line No.='20000',Promotion
No.='TEST2',Promotion Benefit Line No.='10000'

\---------------------------

OK

\---------------------------

Handling Editing on Sales Lines Affected by Promotions
------------------------------------------------------

Create a new event handler in COD9030249 to subscribe to sales line
modifications. It may be better to subscribe to the events on the sales document
pages themselves? The sales line table events may fire in circumstances where we
don't want the promotions functionality to apply.

Check whether the line that is being modified has any related Promotion Entry
records. If it does, ask the user for confirmation before proceeding and if they
choose to continue delete the related Promotion Entry records.

Optionally Multiply Promotion Benefits
--------------------------------------

Add new field, Allow Benefit Multiplication to TAB9030249 and the General tab of
the Promotion Card page (PAG9030250).

Change COD9030249.GetPromotionBufferMinMultiplierRate to respect Allow Benefit
Multiplication.

PRO1.0
======

[Bug Fix]Promotion Code - No. of Uses
-------------------------------------

No. of Uses flowfield isn't incremented correctly when a promotion code is used
on a sales document.

The user should be able to drilldown from the No. of Uses field to see the
promotion entry records that detail where the promotion code has been previously
used.

The Max. No. of Uses option on the Promotion Code Group is not enforced e.g. if
Max. No. of Uses is 1 and the promotion code is used on a document it should not
be possible to use the same promotion code again. At the moment the same promo
code can be reused.

[Bug Fix]Promotion Applied When Promotion Code is Deleted
---------------------------------------------------------

A promotion is created which has a criteria requiring a promotion code to be
quoted on the sales document

A sales invoice is created and a valid document promotion code entered

The promotion is (correctly) applied to the document

Unapply the promotion

Delete the document promotion code

The promotion code is applied to the document again (incorrectly) even though
the document promotion code has been deleted.

Apply Discounts to New Line type Benefits
-----------------------------------------

Ensure that discounts are applied as per the setup of the benefit line to New
Line type benefits.
