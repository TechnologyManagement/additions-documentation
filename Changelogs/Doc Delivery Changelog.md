
DD2.9
=========

Interoperability with Document Links
------------------------------------

<font size=3>Support for:</font><div><ul><li><span style="font-size:medium;">Determining that Document Links is available in the database</span></li><li><span style="font-size:medium;">Providing setup options for using DL as the storage mechanism (in place of the DD Document Store table)</span></li><li><span style="font-size:medium;">Calling new DL functionality to store attachments that have been generated against the source record (this will mean they are visible in the DL factbox against the source record)</span></li><li><span style="font-size:medium;">Calling new DL functionality to retrieve the attachment when it is required,</span></li></ul></div>

DD2.8.1
=========

Doc Per Record Selection When Grouping by Field
------------------------------------

On the Document Type Selection page the user has the option to choose between &quot;One document per record&quot; and &quot;One document for all records&quot;. This option isn't relevant if the document type already specifies grouping by one of the primary table fields.<div><br></div><div>If when the page opens a document type which groups by a field is already selected the above option is still enabled. This is wrong.</div>

DD2.7.9
=========

Remove Obsolete Fields
------------------------------------

Remove obsolete fields from DD tables.

Make available on issued finance charge memos
------------------------------------

<div class=ExternalClassFCE3EFD2F4894CA3A6884E1F60C03C9D><p><span id=ms-rterangecursor-start></span>Add emailing options on page - Issued Fin. Charge Memo List (452)</p><p>This will be require for the BMA?<span id=ms-rterangecursor-end></span></p></div>

Remove ADC Subscription
------------------------------------

The subscription to the ADC event means that DD must be installed the same way as ADC (either as objects or extension). Reworking this dependency to be more loosely coupled would give us more flexibility when installing.

DD Custom Values Table 
------------------------------------

When trying to add table 52102244 to Linked Tables page I get an error saying &quot;there is no Object within that filter&quot;. This is when modifying DD as an Extension. We need to lookup to All Object table not just Object table.

DD2.7.8.3
=========

Document Type Parameters reset
------------------------------------

When submitting a record via the Document Delivery Mgt codeunit, the DeleteSessionRecords function reset the filter applied. All document types are returned instead of the one filtered and passed through.&nbsp;

DD2.7.8.1
=========

Payment Journal is treated as a card page
------------------------------------

DD actions on Payment Journal should prompt DD Record Selection page

DD2.7.7
=========

Dynamically Determined Attachment Paths
------------------------------------

Doc Type Attachment records allow the user to attach either 1) a report or 2) a static attachment uploaded to the attachments page to an email generated for the document type.<div><br></div><div>We need to support the ability to attach a file whose path is determined dynamically.</div>


Expose New Event to Customise Document Log Details
------------------------------------

New integration event exposing the DD Document Log records that have been created for a given source record but prior to them being processed.

DD2.7.8
=========

Get Current Document Log Record
------------------------------------

New function in Document Delivery Mgt. codeunit to retrieve the Document record that is currently being processed.

Overflow Error on SetDocumentType
------------------------------------

Pass a &gt;10 length string to SetDocumentType.<div><br></div><div>Overflow error.</div>

DD2.7.6.2
=========

Doc. Type Attachment Options Unreliable
------------------------------------

TAB52102237.UpdateRequestPageOptions

DD2.7.6.1
=========

[Bug Fix]Watermark document
---------------------------

Currently doing a FINDFIRST on the DocumentTypeAttachment for the wrong filters.
Change function WatermarkDocument in COD52102242 to get the DD Doc. Type
Attachment required for watermarking

DD2.7.6
=======

Lookup for Custom Layout Description
------------------------------------

The Custom Layout Description field in the Doc Type Attachment list/card pages
doesn't provide any lookup to help the user to choose some appropriate data.

The lookup should display a list of the appropriate custom layouts to choose
from.

DD2.7.5.2
=========

[Bug Fix]GetServicePassword cannot get Cust/Vend
------------------------------------------------

COD52102235.GetServicePassword does a GET on Customer/Vendor without checking if
it exists.

DD2.7.5.1
=========

Move DD Actions to Correct Sales Return Page
--------------------------------------------

Should be on PAG9304 rather than PAG6633.

DD2.7.5
=======

"Test Mode" functionality
-------------------------

Add a "Test Mode" checkbox and "Test Email Recipient" fields to the Doc Delivery
Setup page.

Change the logic that evaluates the To, Cc and Bcc document fields. The only
recipient of emails while the system is in "Test Mode" should be the recipient
defined on the setup page. Allow the user to defined more than one address,
separated with semi-colons still.

DD2.7.3.1
=========

[Bug Fix]Doc Delivery Setup Does Not Exist Error on Customer List
-----------------------------------------------------------------

-   Install Doc Delivery, but do not get default config.

-   Open customer list.

-   Error message about Doc Delivery Setup not existing.

DD2.7.3
=======

Make Import/Export Config More Flexible
---------------------------------------

Rather than exporting all of the config and forcing the user to delete the
existing config before importing, allow the user to export specified document
type codes and their related records. If the user has filtered to certain
document types only export records that are related to those document types.

[Bug Fix]All document types listed when queuing or opening email
----------------------------------------------------------------

All Document Types would be listed when Opening/Queuing an email. The Document
Type code needs after each request.

Picked up this bug when integrating with CH, as we pass it a specific Document
Type Code instead of looking for one. Apply a check to see if you can get the
specified Document Type Code, else go to search through the other doc types.

DD2.7.2
=======

[Bug Fix]Record is Already Open Error
-------------------------------------

Clear the FilterRef variable before determining the record view to use for the
submission.

Add Document Type Code to Document Log Page
-------------------------------------------

DD2.7.0
=======

Optionally Watermark Only the Final Page of the Attachment
----------------------------------------------------------

There may be scenarios where the user wants to apply a watermark to only the
final page of the document (the example to hand is that the customer wants to
have watermarks with users' signatures that should be applied to certain
documents e.g. purchase orders).

DD2.6.9.2
=========

[Bug Fix]Edit Body Page
-----------------------

This page allows the user to navigate to next/previous records but doesn't
handle it properly. The user should only be able to edit the body of a single
document type at once on that page. They can work in different languages by
changing the language code field, but otherwise they should close and reopen the
page on a different document type code to edit another.

DD2.6.9.1
=========

[Bug Fix]Report Parameter Calculation on Resubmitting Documents
---------------------------------------------------------------

Need to calculate to Report Parameter BLOB field on the detail records, not on
the Document record.

DD2.6.9
=======

Doc Delivery Password Field
---------------------------

On Customer and Vendor cards (Communication tab)

DD2.6.8
=======

[Bug Fix]Overflow Retrieving Attachment from Store
--------------------------------------------------

The store table has a record view (text 250) which is filtered on to determine
whether the report that is being run is already available in the document store.

Sometimes the record view can be longer than 250 in which case an overflow
occurs when attempting to filter on that field.

[Bug Fix]Error Processing Attachment Type Document Fields
---------------------------------------------------------

Attachments can be defined on the Attachments tab of the document type card or
with document fields of type Attachment. In the latter case the field value
should be the path to a file accessible by the NAV service account from the
middle tier server.

These attachments are not being processed correctly. The DD Document records are
being created with an attachment type of Report (rather than Attachment). This
is causing the CreateAttachments function to fail when the document is
processed.

[Bug Fix]Custom Value Overflow Error
------------------------------------

When document type is longer than 10 characters get an overflow error message
when trying to open the list of custom values from the document type card.

DD2.6.7
=======

 [Bug Fix]Xml Serialization Error when Editing Request Page
-----------------------------------------------------------

Function UpdateRequestPageOptions in TAB52102237. Ensure that all of the DotNet
variables are RunOnClient = No. This error is because one of the variables is
set to Yes.

DD2.6.6
=======

[Bug Fix]Selecting Single Document for All Records Includes Records Not Selected by the User
--------------------------------------------------------------------------------------------

DD2.6.5
=======

Support for Service Document Types - Dev
----------------------------------------

Add the doc delivery actions to the Actions group of each of the following
pages.

Posted service Invoice list - 5977

Posted service invoice - 5978

Posted service credit list - 5971

Posted service credit - 5972

Service quote list - 9317

Service quote - 5964

Service order list - 9318

Service order - 5900

Service contract quote list - 9322

Service contract quote - 6053

Version List: DD2.6.5

Versions: 2015, 2016

DD2.6.4
=======

[Bug Fix]Incorrect Linked Table Record Found for Records with Ampersand
-----------------------------------------------------------------------

The & in a filter expression e.g. A&C001 customer no. is replaced with a ? to
avoid an error message about the filter that is applied being illegal. This can
mean that the wrong record is found e.g. AJC001 is found instead of A&C001.

DD2.6.3
=======

[Bug Fix]Check for Outlook Running is Performed on Server Not Client
--------------------------------------------------------------------

DD Document Type table checks whether outlook is running before allowing the
body of a document type to be edited in Outlook. This check is being performed
on the server instead of the client.

The .Net component needs to be run on the client.

 [Bug Fix]Attachments Not Processed When No Email Add & Selecting "Open Email"
------------------------------------------------------------------------------

When there is no recipient email address and the user selects "Open Email", the
email is opened in Outlook despite there being no email to send it to.

In this case the attachments are no included on the email.

DD2.6.1
=======

[Bug Fix]Performance of Recalculating Document Fields on Selecting new Document Type
------------------------------------------------------------------------------------

The initial calculation of document fields when records are submitted is good.
Selecting a new document type for the batch of records forcing a recalculation
of all the fields. This is unacceptably slow.

DD2.6
=====

Unnecessary Attachments
-----------------------

Efficiency improvement to prevent unnecessary (re)creation of attachments for
print and email copies. Reuse the existing attachments unless the print and
email copies should actually be different e.g. because a Document Type
Attachment record only applies to one but not the other.

Default Configuration Action
----------------------------

Add support for the Default Configuration framework into Doc Delivery.

DD2.5.7
=======

Request Page Options
--------------------

Support for setting options on the Request Page of reports (other than table
filters – which are already supported). Developed with statement reports in mind
that need to set Start Date and End Date options. Supports the ability to
calculate these options with DateFormula calculations.

[Bug Fix]RPC Error on Editing Body in Outlook
---------------------------------------------

Edit the body of a document type in Outlook.

Edit the email and save

Close the email, an error message is received "RPC error..."

Progress Bar of Submitting Records - Dev
----------------------------------------

If GUIALLOWED, open a dialog window in SubmitRecords2 (of COD52102235). Show the
'Calculating\@1\@\@\@\@\@\@\@\@\@\@\@' in the window and update with the
percentage of RecRefs that have been submitted to the InsertRecord function.

 [Bug Fix]Email Address Reverts Incorrectly
-------------------------------------------

Select multiple records, say posted sales invoices, and choose Open Email.

Choose to send the records in a single document

Change the TO address on the first record in the batch

When the email is opened it is addressed to the original To address, not the
updated one.

Static Value - Check Correct Usage - Dev
----------------------------------------

TESTFIELD OnValidate of Static Value for the correct Static Value.

DD2.5.6
=======

Add DD Actions to Sales Return Orders
-------------------------------------

DD2.5.5
=======

Prompt for Missing Request Page Parameters - Dev
------------------------------------------------

Create a new function, CheckDocTypeAttachmentParameters to do the following:

Loop through Doc. Type Attachment records for the current DocumentType (global
variable), of attachment type Report.

For each Doc Type Attachment, check whether a TempDocTypeAttachment record
exists (having the same primary key).

If a TempDocTypeAttachment record does not exist, or the request page parameters
for that record cannot be read (that is, BlobToText returns an empty string)
then continue,

If the DocTypeAttachment record does not have request page parameters that can
be read (that is, BlobToText returns an empty string) then call
DocTypeAttachment.SetRequestPageParameters to prompt the user to save the
request page.

If the request page parameters are still not set (because the user clicked
Cancel on the request page) then throw an error "Your documents have not been
sent as there are no request page parameters saved for %1 %2, report %3" where
%1 is the table caption for Document Type, %2 is the document type code, %3 is
the report ID.

Primary Table Document Type Field - Dev
---------------------------------------

OnValidate of the Primary Table Document Type field check if the primary table
filter includes either table 36 or table 38. If it doesn't throw the following
error message:

"%1 should not be set unless the %2 includes either %3 or %4." where %1 is the
field caption for Primary Table Document Type, %2 is the field caption for
Primary Table No. Filter, %3 is the table caption for the Sales Header table and
%4 is the table caption for the Purchase Header table.

DD2.5.4
=======

DD Translation Table Relation - Dev
-----------------------------------

The table relation for the Primary Key Integer is wrong in the DD Translation
table. The relationship to the DD Doc. Type Attachment should be on the Entry
No. field instead of the Report ID.

Enumerate available printer trays - Dev
---------------------------------------

A new version of the PrintOrPreview dll has been loaded into the Add-ins folder.
This contains some new methods that will allow us to enumerate the trays for a
given printer.

Error Message on Document Log - Dev
-----------------------------------

Add code on assist edit of the error message on the document log to pop a
message box with the error message.

[Bug Fix]Bug with Resubmit Functionality
----------------------------------------

Ensure that when the new DD Document records are created that all of the BLOB
fields in the table are calculated (includes Body, Report Parameters).

Don't lock DD Document table while generating attachments
---------------------------------------------------------

Currently the DD Document table is locked while attachments are generated from
reports. Depending on the number of attachments that are being generated and the
reports that are being executed, this can take a long time and prevent other
users from submitting records to doc delivery at the same time.

The DD Process Documents codeunit should be reworked so that the table is not
locked while the attachments are being generated.
