WPM1.1
======

Pallet Numbering Functionality
------------------------------

Pallet Numbering functionality is currently kept in its own separate branch.
This should be moved (and renumbered) to be included in this addition.

SSCC Label
----------

We need to have a generic SSCC label that can be printed from the Posted Pallet
Content records.

Initially these labels will only be printed manually by the user going to a
posted / registered document and clicking a "Print SSCC Labels" action. The
action will be required on the following pages:

Posted Sales Shipment\*

Posted Invt. Pick

Posted Whse. Shipment

\*when labels are printed from a posted sales shipment the system should look
for posted pallet content records for registered invt. pick lines and posted
warehouse shipment lines that are related to the sales shipment lines - not just
for posted pallet content records that are related to the sales shipment lines
themselves.

WPM1.0
======

Whse Shipment creation automatically on Sales Order release
-----------------------------------------------------------

Add boolean options to the Warehouse Process Mgt. Setup page:

Create Whse. Shpt. on SO Release

Create Whse. Shpt. on TO Release

Create Whse. Shpt. on PRO Release

These options will determine whether warehouse shipments are automatically
created when sales orders, transfer order and purchase return orders are
released.

Each of these pages has a Create Whse. Shipment action - this functionality
should be called on release of the source document.

Whse Shipment status "Released" automatically on creation
---------------------------------------------------------

Add a new boolean option to the Warehouse Process Mgt. Setup page - Auto Release
Whse. Shipments

This option will determine whether warehouse shipments are automatically
released when they are created - either because they have been created by the
user with the "Create Whse. Shipment" actions or because they have been created
automatically on release of the source document.

Adding lines manually to Phys Inv & whse Phys Inv Journals
----------------------------------------------------------

Add a new action the Phys Inv Journal page. (Add New Line)

This action will open a StandardDialog type page prompting the user to enter
details for the new line. The details required are:

Item No., Location Code, Bin Code, Qty. (Phys. Inventory)

The new line will always be of Entry Type Positive Adjustment

Once the new line is added and item tracking needs to be applied, don't move
down to the line below, stay on current line

Warehouse Process Mgt. Setup Page
---------------------------------

We should have our own Warehouse Process Mgt. Setup page to switch on or off the
various pieces of functionality that will be included in the addition.

Add this card page to the menu in the same place as the standard Warehouse Setup
page.

Automating Pick Creation on Release of Whse. Shipment
-----------------------------------------------------

Add a new field to the setup page, Create Pick on Whse. Shpt. Release.

When the warehouse shipment is released call the create pick functionality (same
functionality that is called from the create pick action). This should work both
when the whse shipment is released automatically (by the addition functionality)
and when it is released manually by the user.

Do not attempt to create a pick if there is at least one pick line or registered
pick line against the whse shipment - otherwise the user will receive a "nothing
to handle" error.
