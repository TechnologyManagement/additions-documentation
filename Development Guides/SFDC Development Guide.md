SFDC Development Guide
======================

This document is for developers who wish to extend the functionality of the Shop
Floor Data Capture addition from Technology Management.

Functionality should only be extended through subscription to integration events
included in the product. No changes should be made to the SFDC objects
themselves.

Contents
========

-   [Page Customisation](#page-customisation)

    -   [Adding Controls](#adding-controls)

    -   [Metadata Names and Values](#metadata-names-and-values)

-   [Handling Device Activity](#handling-device-activity)

-   [Production Groups](#adding-controls)

-   [Instructions](#instructions)

Page Customisation
==================

Most of the logic for defining the pages that are displayed in the web portal by
SFDC is contained in the “SFDC – Page Definition Mgt.” codeunit.

This codeunit builds populates data in the following temporary tables:

-   **Portal Framework Control** - each record defines a control (button, grid,
    textbox, image etc.) that should be displayed on the page.

-   **Portal Framework Control Data** – data that is associated with a control
    e.g. the data to be shown in cells of a grid control.

-   **Portal Framework Control Metadata** – the metadata associated with a
    control, e.g. its type, colour, caption

The standard SFDC logic will populate these tables with records to define each
page in the portal. The following integration events provided opportunity to
override the standard logic.

-   [OnBeforeBuildPage](#onbeforebuildpage) – called before any of the standard
    logic is applied

-   [OnAfterBuildPage](#onafterbuildpage) – called after the standard logic has
    been applied

-   [OnAddControlToPage](#onaddcontroltopage) – called immediately before each
    control is ‘added’ to the page i.e. before each record is included in the
    set.

These events can be used to manipulate the data contained in these temporary
tables, which in turn will reflect the controls that are built by the portal.

OnBeforeBuildPage
-----------------

This event is called before any of the standard logic for defining the page is
called. The following parameters are included:

-   ActivityCode – a code that denotes the type of activity for which a page
    needs building e.g. DataSelection, OutputScreen, ComponentSubform – see the
    *xyz*Code functions in the “SFDC – Activity Mgt.” codeunit.

-   PortalFwkApplicationDevice2 – a record denoting the current Portal Framework
    Application device.

-   DataTypeCode – when the requested activity is DataSelection, this code
    indicates the type of data that should be presented to the user for
    selection.

-   PortalFrameworkControl – the temporary table representing the set of
    controls

-   PortalFrameworkControlData – the temporary table containing data associated
    with the controls

-   PortalFwkControlMetadata – the temporary table containing metadata
    associated with the controls.

-   ActivityLogEntryNo – the entry no. of the log record associated with this
    activity.

-   Handled – a Boolean indicating whether the event has been handled (true
    indicates that the standard logic should not be called).

OnAfterBuildPage
----------------

This event is called after the standard SFDC logic has been applied. The same
parameters are included as for [OnBeforeBuildPage](#onbeforebuildpage), except
for the Handled parameter:

-   ActivityCode – a code that denotes the type of activity for which a page
    needs building e.g. DataSelection, OutputScreen, ComponentSubform – see the
    *xyz*Code functions in the “SFDC – Activity Mgt.” codeunit.

-   PortalFwkApplicationDevice2 – a record denoting the current Portal Framework
    Application device.

-   DataTypeCode – when the requested activity is DataSelection, this code
    indicates the type of data that should be presented to the user for
    selection.

-   PortalFrameworkControl – the temporary table representing the set of
    controls

-   PortalFrameworkControlData – the temporary table containing data associated
    with the controls

-   PortalFwkControlMetadata – the temporary table containing metadata
    associated with the controls.

-   ActivityLogEntryNo – the entry no. of the log record associated with this
    activity.

OnAddControlToPage
------------------

This event is called for each control that is included in the temporary tables
that is set by the standard logic. This gives the opportunity for the data
associated with the control to be manipulated, the control “skipped” i.e. not
included on the page and [new controls to be injected before the current
control](#adding-controls).

The following parameters are included:

-   ActivityCode – a code that denotes the type of activity for which a page
    needs building e.g. DataSelection, OutputScreen, ComponentSubform – see the
    *xyz*Code functions in the “SFDC – Activity Mgt.” codeunit.

-   PortalFwkApplicationDevice2 – a record denoting the current Portal Framework
    Application device.

-   PortalFrameworkControl – the temporary table representing the set of
    controls

-   PortalFrameworkControlData – the temporary table containing data associated
    with the controls

-   PortalFwkControlMetadata – the temporary table containing metadata
    associated with the controls.

-   SkipControl – a Boolean indicating whether the control should be skipped. If
    this is set to true the current control will not be included on the page
    that is built by the portal.

Adding Controls
---------------

New controls can be added though use of the functions on the
PortalFrameworkControl parameter passed by the above integration events.

The following functions can be used to create new controls:

-   AddNewEntry – creates a new control below the current one and moves the
    pointer to the new control.

-   AddNewEntryBefore – creates a new control before the current one and moves
    the pointer to the new control.

These functions require a ControlID. This should be a unique code that is
associated with the control. This ID will be used in subsequent web service
calls to NAV when activity occurs that is associated with this control. See
[Handling Device Activity](#handling-device-activity).

### Metadata

Having creating the control, metadata must be added to determine its type and
appearance. Metadata records are added with the AddMetadata function. The
following parameters are required:

-   PortalFwkControlMetadata – a Portal Framework Control Metadata record. This
    will be populated with the appropriate values defined by the other
    parameters and inserted.

-   Name – the name of the metadata. See [Metadata Names and
    Values](#metadata-names-and-values) for a list of valid metadata names and
    values.

-   Type – the type of the metadata. This is an option: 0 = Element, 1 =
    Attribute. The correct value for this parameter will be determined by the
    metadata name.

-   Value – the value of the metadata. See [Metadata Names and
    Values](#metadata-names-and-values) for a list of valid metadata names and
    values.

If you use these functions to create a new control you will need to implement an
event subscriber to [handle the activity](#handling-device-activity) associated
with the control. For example, if you add a new button control you need to
provide a handler function for to determine the correct response to that button
being clicked.

Metadata Names and Values
-------------------------

Below is a list of the valid metadata names and their corresponding values.

| Name                 | Type | Values                                                          |
|----------------------|------|-----------------------------------------------------------------|
| type                 | 0    | button, text, selectiontiles, selectionlist, image              |
| caption              | 0    | The text of the caption                                         |
| value                | 0    | The value to display in the control                             |
| colour               | 1    | Any value that is valid for HTML i.e. hex value, name of colour |
| placement            | 1    | ‘right’/’left’                                                  |
| [subform](#subforms) | 1    | true/false                                                      |
| [layout](#layouts)   | 1    | ‘nav’/’1-column’/’2-columns’                                    |

Subforms
--------

Layouts
-------

Handling Device Activity
========================

‘Activity’ refers to anything that happens in the web portal which results in a
call to the NAV web service. The context of the activity (the control that was
used, the current state of the page) is passed to NAV and the portal expects a
response containing the definition of the page that should be shown next.

You can use the following integration events in the “SFDC – Post Mgt.” codeunit
to execute your code when ‘activity’ takes place on the portal.

OnBeforeHandleActivity
----------------------

This event is called before any standard logic is executed. You can use this
event to override the response to standard SFDC activity or define the response
to your own controls. The event has the following parameters:

-   PortalFrameworkControl – the temporary table containing the controls on the
    page

-   PortalFrameworkControlData – the temporary table containing the data
    associated with the controls on the page

-   PortalFwkApplicationDevice – a record indicating the device that the
    activity came from

-   ActivityCode – a code denoting the type of activity that has occurred i.e.
    the code corresponding to the portal page that the activity took place in.

-   Command – a code denoting the control on the above page that raised the
    activity. This code corresponds to the ControlID assigned to the control by
    the Page Definition codeunit. See [Adding Controls](#adding-controls).

-   PortalFrameworkActivityLog – the activity log record associated with this
    activity

-   Handled – a Boolean indicating whether this activity has been handled. A
    value of true will bypass any standard logic that would apply to this
    activity.

OnAfterDeviceActivity
---------------------

This event is raised after any standard logic has been applied in response to
the activity. It has the same parameter as OnBeforeDeviceActivity except for the
Handled parameter, which is omitted.

-   PortalFrameworkControl – the temporary table containing the controls on the
    page

-   PortalFrameworkControlData – the temporary table containing the data
    associated with the controls on the page

-   PortalFwkApplicationDevice – a record indicating the device that the
    activity came from

-   ActivityCode – a code denoting the type of activity that has occurred i.e.
    the code corresponding to the portal page that the activity took place in.

-   Command – a code denoting the control on the above page that raised the
    activity. This code corresponds to the ControlID assigned to the control by
    the Page Definition codeunit. See [Adding Controls](#adding-controls).

-   PortalFrameworkActivityLog – the activity log record associated with this
    activity

Production Groups
=================

SFDC can be customised with the concept of “Production Groups”. This provides a
way of grouping prod. order lines that need to be worked on together other than
by the prod. order no.

The user will be presented with a list of production groups to select from,
rather than a list of prod. orders / prod. order routing lines.

Setup
-----

SFDC can be configured to use production groups globally i.e. all machine
centres are assigned work by production groups, by including the PROD\_GROUP
data type in one of the three data selection fields on the SFDC Setup page.

Alternatively, if some machine centres work with production groups, but not all,
the “Use Production Groups” field can be set on the SFDC tab of the Machine
Centre card.

**Note:** in the Process Type must be set to All Lines (rather than Single
Lines) for Machine Centres that use production groups.

Implementation
--------------

The standard SFDC product has no definition of what a production group is, or
the lines that it contains. This definition must be supplied by event
subscriptions in custom code in the project.

Two key pieces of information are required by SFDC at various points during the
workflow:

1.  What are the valid production groups that the user can select from?

2.  Which prod. order routing line(s) are included in each production group?

These questions are answered by subscribing to the following events:

COD9059236. OnBuildProdGroupSelectionBuffer
-------------------------------------------

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

COD9059236.OnMarkProdOrderRtngLineForProdGroup
----------------------------------------------

This event allows a custom subscriber to mark the prod. order routing lines that
are related to a given production group. It will be called several times as the
user navigates around the SFDC portal to display details appropriate to the
selected production group.

The event has the following parameters:

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
============

SFDC includes the ability to display PDF instruction sheets to users related to
the operation that they are working on. The standard application allows the user
to upload a PDF document against a Routing Line record.

When this routing line record is inherited by a released production order the
user will see an “Instruction” button in the navigation pane in the SFDC portal.
Clicking this button will display the PDF content in the portal window.

This functionality can be overridden by subscribing to the follow events: \#\#\#
COD9059234.OnBeforeProdOrderRtngLineHasInstruction This event allows a custom
subscriber to indicate if the prod. order routing line has an instruction. This
is used to determine whether to display the “Instruction” button on the output
screen. The event has the following parameters: - ProdOrderRtingLine - This
might be a set of record if the machine center is an All Line process type -
HasInstruction – set the Boolean if there is an instruction available - Handled
– a Boolean to indicate that this event has been handled.

COD9059234.OnBeforeGetProdOrderRtingLineResource
------------------------------------------------

This event can be used to set the resource that will be displayed on the
instruction page. The event has the following parameters: - ProdOrderRtingLine –
single record in question for resource - ResourceValue – resource identifier
path, should be unique for each file. - i.e. the value consist of an identifier
directory and a file name with the primary keys of the record which the resource
is set against. - ResourceFileExtension – the file extension of resource file -
Handled – a Boolean to indicate that this event has been handled.

Example:

Setting the resource from a sales order after getting the sales header of the
ProdOrderRtingLine

ResourceValue :=
STRSUBSTNO('%1/%2\_%3',SalesOrderInstructionDirectoryName,SalesHeader."Document
Type",SalesHeader."No.");

ResourceFileExtension := 'pdf';

Handled := TRUE;

\*SalesOrderInstructionDirectoryName is a function that returns
‘/SFDC/SalesOrder/Instruction’

COD9059234.OnBuildPortalResourceResponse
----------------------------------------

This event can be used to return the resource file content. The resource file is
cached on retrieve so the file content is only required if the file has been
updated since last response. The event has the following parameters: -
ResourceDirectoryName – the resource identifier directory - ResourceValue –
resource file name - LastRetrieved – the downloaded DateTime -
PortalFrameworkControl - record to add resouce control -
PortalFwkControlMetadata - record to add resouce control metadata - Handled – a
Boolean to indicate that this event has been handled.

Example:

CASE ResourceDirectoryName OF

SalesOrderInstructionDirectoryName:

BEGIN

PortalFwkApplicationDevice.SplitData(ResourceValue,ResourceKeyArray,'\_');

SalesHeader.SETFILTER("Document Type",ResourceKeyArray.GetValue(0));

SalesHeader.SETRANGE("No.",ResourceKeyArray.GetValue(1));

SalesHeader.FINDFIRST;

GetSalesOrderResource(SalesHeader,LastRetrieved,TempBlob,LastUpdatedDateTime);

SFDCSetup.GET;

PortalFrameworkControl.AddNewEntry(SFDCSetup."Portal Fwk. Application
ID",ResourceTxt);

PortalFrameworkControl.AddMetadata(PortalFwkControlMetadata,'modifiedOn',0,FORMAT(LastUpdatedDateTime,0,''));

PortalFrameworkControl.AddMetadata(PortalFwkControlMetadata,'mimeType',0,SFDCPageDefinitionMgt.GetMimeType('pdf'));

IF (LastRetrieved = 0DT) OR (ROUNDDATETIME(LastUpdatedDateTime,1000,'\<') \>
LastRetrieved) THEN

PortalFrameworkControl.AddMetadataBlob(PortalFwkControlMetadata,'content',0,SFDCPageDefinitionMgt.BlobToBase64String(TempBlob));

Handled := TRUE;

END;

END;

\*PortalFwkApplicationDevice: Record 9059291

\*SFDCPageDefinitionMgt: Codeunit 9059234

\*GetSalesOrderResource is function which will return the resource content in
TempBlob if the file has been updated since the last download. Set
LastUpdatedDateTime to CURRENTDATETIME if not available.
