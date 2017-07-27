SFDC Development Guide
======================

This document is for developers who wish to extend the functionality of the Shop
Floor Data Capture addition from Technology Management.

Functionality should only be extended through subscription to integration events
included in the product. No changes should be made to the SFDC objects
themselves.

Production Groups
-----------------

SFDC can be customised with the concept of “Production Groups”. This provides a
way of grouping prod. order lines that need to be worked on together other than
by the prod. order no.

The user will be presented with a list of production groups to select from,
rather than a list of prod. orders / prod. order routing lines.

### Setup

SFDC can be configured to use production groups globally i.e. all machine
centres are assigned work by production groups, by including the PROD\_GROUP
data type in one of the three data selection fields on the SFDC Setup page.

Alternatively, if some machine centres work with production groups, but not all,
the “Use Production Groups” field can be set on the SFDC tab of the Machine
Centre card.

**Note:** in the Process Type must be set to All Lines (rather than Single
Lines) for Machine Centres that use production groups.

### Implementation

The standard SFDC product has no definition of what a production group is, or
the lines that it contains. This definition must be supplied by event
subscriptions in custom code in the project.

Two key pieces of information are required by SFDC at various points during the
workflow:

1.  What are the valid production groups that the user can select from?

2.  Which prod. order routing line(s) are included in each production group?

These questions are answered by subscribing to the following events:

### COD9059236. OnBuildProdGroupSelectionBuffer

This event is used to define the valid production group(s) that the user can
select. This event should be subscribed to in a custom codeunit. The parameters
to the function are as follows:

-   PortalFwkApplicationDevice – the record corresponding to the device that has
    made the call to the Portal Framework Web Service.

    -   This can be used to determine extra information about the call e.g. the
        Machine Centre or Operators that have been selected.

-   PortalFrameworkControl – the selection list representing the list of
    production groups that will be presented to the user.

    -   This record should be populated with the appropriate row/column data to
        show to the user.

-   PortalFrameworkControlData – this record represents the collection of
    row/column data that has been populated from the production groups.

-   Handled – a Boolean that should be set to true to indicate that this event
    has been handled.

Example:

In this example a bespoke field has been added to the Prod. Order Line table to
indicate which production group each line belongs to. This code loops through
the prod. order lines to determine the distinct set of production groups related
to the Machine Centre that the user has already selected.

**Note:** the first row of data added to PortalFrameworkControl will be used as
the headings for the selection list that is presented to the user.

SFDCManagement.GetDeviceMachineCenter(PortalFwkApplicationDevice,MachineCenter);

PortalFrameworkControl.AddData(PortalFrameworkControlData,1,1,'Code',FALSE);

PortalFrameworkControl.AddData(PortalFrameworkControlData,1,2,'Description',FALSE);

RowNo := 1;

ProdOrderLine.SETCURRENTKEY("Production Group Code");

ProdOrderLine.SETFILTER("Production Group Code",'\>%1','');

IF ProdOrderLine.FINDSET THEN

REPEAT

ProdOrderRoutingLine.SETRANGE(Status,ProdOrderLine.Status);

ProdOrderRoutingLine.SETRANGE("Prod. Order No.",ProdOrderLine."Prod. Order
No.");

ProdOrderRoutingLine.SETRANGE("Routing Reference No.",ProdOrderLine."Routing
Reference No.");

ProdOrderRoutingLine.SETRANGE("Routing No.",ProdOrderLine."Routing No.");

ProdOrderRoutingLine.SETRANGE(Type,ProdOrderRoutingLine.Type::"Machine Center");

ProdOrderRoutingLine.SETRANGE("No.",MachineCenter."No.");

ProdOrderRoutingLine.SETFILTER("Qty. Available to Start (Base)",'\>%1',0);

IF NOT ProdOrderRoutingLine.ISEMPTY THEN BEGIN

ProdOrderLine.SETRANGE("Production Group Code",ProdOrderLine."Production Group
Code");

RowNo += 1;

PortalFrameworkControl.AddData(PortalFrameworkControlData,RowNo,1,ProdOrderLine."Production
Group Code",FALSE);

PortalFrameworkControl.AddData(PortalFrameworkControlData,RowNo,2,ProdOrderLine."Production
Group Code",FALSE);

ProdOrderLine.FINDLAST;

ProdOrderLine.SETRANGE("Production Group Code");

END;

UNTIL ProdOrderLine.NEXT = 0;

Handled := TRUE;

### COD9059236.OnMarkProdOrderRtngLineForProdGroup

This event allows a custom subscriber to mark the prod. order routing lines that
are related to a given production group. It will be called several times as the
user navigates around the SFDC portal to display details appropriate to the
selected production group.

The event has the following paramters:

-   PortalFwkApplicationDevice - the record corresponding to the device that has
    made the call to the Portal Framework Web Service.

    -   This can be used to determine extra information about the call e.g. the
        Machine Centre or Operators that have been selected.

-   ProdGroup – the production group which has been selected.

-   ProdOrderRoutingLine – a record where the lines related to the production
    group should be MARKed.

-   Handled – a Boolean to indicate that this event has been handled.

Example:

As above, a bespoke field has been added to the prod. order line to indicate
which production group they belong to. The prod. order lines belonging to the
production group are iterated through and those lines which have a prod. order
routing line for the current Machine Centre, with a quantity available to start,
are MARKed.

SFDCManagement.GetDeviceMachineCenter(PortalFwkApplicationDevice,MachineCenter);

ProdOrderLine.SETRANGE("Production Group Code",ProdGroup);

IF ProdOrderLine.FINDSET THEN

REPEAT

ProdOrderRoutingLine.SETRANGE(Status,ProdOrderLine.Status);

ProdOrderRoutingLine.SETRANGE("Prod. Order No.",ProdOrderLine."Prod. Order
No.");

ProdOrderRoutingLine.SETRANGE("Routing Reference No.",ProdOrderLine."Routing
Reference No.");

ProdOrderRoutingLine.SETRANGE("Routing No.",ProdOrderLine."Routing No.");

ProdOrderRoutingLine.SETRANGE(Type,ProdOrderRoutingLine.Type::"Machine Center");

ProdOrderRoutingLine.SETRANGE("No.",MachineCenter."No.");

ProdOrderRoutingLine.SETFILTER("Qty. Available to Start (Base)",'\>%1',0);

IF ProdOrderRoutingLine.FINDSET THEN

REPEAT

ProdOrderRoutingLine.MARK := TRUE;

UNTIL ProdOrderRoutingLine.NEXT = 0;

UNTIL ProdOrderLine.NEXT = 0;

Handled := TRUE;

Instructions
------------

SFDC includes the ability to display PDF instruction sheets to users related to
the operation that they are working on. The standard application allows the user
to upload a PDF document against a Routing Line record.

When this routing line record is inherited by a released production order the
user will see an “Instructions” button in the navigation pane in the SFDC
portal. Clicking this button will display the PDF content in the portal window.

This functionality can be overridden by subscribing to the follow events:
